# MLX-Audio Server API Reference

## Contents
- Starting the Server
- REST Endpoints
- WebSocket Endpoints
- Configuration Options
- OpenAI Client Usage

## Starting the Server

```bash
# API server only
mlx_audio.server --host 0.0.0.0 --port 8000

# API server with Studio UI (Next.js at http://localhost:3000)
mlx_audio.server --host 0.0.0.0 --port 8000 --start-ui
```

Install server dependencies first:
```bash
pip install "mlx-audio[server]"
```

The server is FastAPI-based with OpenAI-compatible endpoints, CORS support, and continuous TTS batching.

## REST Endpoints

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/` | Welcome |
| GET/POST/DELETE | `/v1/models` | List / load / unload models |
| POST | `/v1/audio/speech` | Text-to-speech (OpenAI-compatible) |
| GET | `/v1/audio/voices?model=` | List voice presets for a TTS model |
| POST | `/v1/audio/transcriptions` | Speech-to-text |
| POST | `/v1/audio/separations` | SAM-Audio source separation |

### TTS Endpoint

`POST /v1/audio/speech`

Request body (JSON):
```json
{
  "model": "mlx-community/Kokoro-82M-bf16",
  "input": "Text to speak",
  "voice": "af_heart",
  "speed": 1.0,
  "lang_code": "a",
  "instruct": null,
  "response_format": "mp3",
  "stream": false,
  "streaming_interval": 2.0,
  "temperature": 0.7,
  "max_tokens": 1200,
  "ref_audio": null,
  "ref_text": null
}
```

Key parameters:
- `model` (required): HuggingFace model ID
- `input` (required): Text to synthesize
- `voice` (optional): Voice preset name (model-dependent)
- `speed` (optional): Playback speed multiplier (default: 1.0)
- `lang_code` (optional): Language code (default: `"a"` for Kokoro)
- `response_format` (optional): `mp3`, `wav`, `flac`, `ogg`, `opus` (default: **`mp3`**)
- `stream` (optional): Stream audio chunks (default: false)
- `ref_audio` / `ref_text` (optional): Reference audio for voice cloning

Returns: Audio file in the requested format.

### STT Endpoint

`POST /v1/audio/transcriptions`

Request (multipart form):
- `file` (required): Audio file (WAV, MP3, FLAC, OGG, M4A)
- `model` (required): HuggingFace model ID
- `language` (optional): Language code
- `max_tokens` (optional): Maximum output tokens (default: 1024)
- `stream` (optional): Stream results as NDJSON
- `context` (optional): Hotwords or metadata to guide transcription
- `verbose` (optional): Include extra details

```bash
curl -X POST http://localhost:8000/v1/audio/transcriptions \
  -F "file=@audio.wav" \
  -F "model=mlx-community/whisper-large-v3-turbo-asr-fp16"
```

### Source Separation Endpoint

`POST /v1/audio/separations`

Request (multipart form):
- `file`: Mixed audio file
- `model`: SAM-Audio model ID (e.g. `mlx-community/sam-audio-large-fp16`)
- `description`: Text prompt describing the target sound (e.g. `"speech"`)

Returns: JSON with base64-encoded `target` and `residual` WAV buffers.

### Model Management

```bash
# List loaded models
curl http://localhost:8000/v1/models

# Load a model
curl -X POST "http://localhost:8000/v1/models?model_name=mlx-community/Kokoro-82M-bf16"

# Unload a model
curl -X DELETE "http://localhost:8000/v1/models?model_name=mlx-community/Kokoro-82M-bf16"
```

## WebSocket Endpoints

### `/v1/audio/transcriptions/realtime` (VAD-based streaming)

```
ws://localhost:8000/v1/audio/transcriptions/realtime
```

Send raw 16-bit PCM frames as binary WebSocket messages. Server performs VAD, chunks on silence, and emits transcription JSON. Preload an STT model via `POST /v1/models` first.

### `/v1/realtime` (OpenAI Realtime-compatible)

```
ws://localhost:8000/v1/realtime?model=<model-id>
```

Implements a subset of the OpenAI Realtime API wire protocol. Recommended model: `mlx-community/Voxtral-Mini-4B-Realtime-2602-4bit`.

Model selection order:
1. `?model=` query parameter on connect
2. `session.update.model` event after connect
3. `--realtime-model` CLI flag / `MLX_AUDIO_REALTIME_MODEL` env var

Key client events: `session.update`, `input_audio_buffer.append`, `input_audio_buffer.commit`

Key server events: `session.created`, `conversation.item.input_audio_transcription.delta`, `conversation.item.input_audio_transcription.completed`

Enable server-side turn detection:
```json
{
  "type": "session.update",
  "session": {
    "audio": {
      "input": {
        "turn_detection": {
          "type": "server_vad",
          "threshold": 0.5,
          "silence_duration_ms": 500
        }
      }
    }
  }
}
```

## Configuration Options

| Flag | Default | Description |
|------|---------|-------------|
| `--host` | `localhost` | Bind address |
| `--port` | `8000` | Port number |
| `--reload` | `false` | Auto-reload on code changes |
| `--start-ui` | `false` | Start Studio UI alongside API |
| `--allowed-origins` | `*` | CORS allowed origins (space-separated) |
| `--log-dir` | `logs` | Server log directory |
| `--realtime-model` | `null` | Default model for `/v1/realtime` |
| `--realtime-transcription-delay-ms` | `null` | Latency/quality knob for streaming models |
| `--vad-model` | `mlx-community/silero-vad` | VAD model for server-side turn detection |
| `--tts-max-batch-size` | `8` | Max TTS requests per continuous batch session |

Environment variables (CLI flags take precedence):
- `MLX_AUDIO_ALLOWED_ORIGINS`
- `MLX_AUDIO_REALTIME_MODEL`
- `MLX_AUDIO_REALTIME_TRANSCRIPTION_DELAY_MS`
- `MLX_AUDIO_VAD_MODEL`
- `MLX_AUDIO_TTS_MAX_BATCH_SIZE`

Models are cached after first load for fast subsequent requests.

## OpenAI Client Usage

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="not-needed",
)

response = client.audio.speech.create(
    model="mlx-community/Kokoro-82M-bf16",
    voice="af_heart",
    input="Hello from the OpenAI client!",
)
response.stream_to_file("output.mp3")

with open("audio.wav", "rb") as f:
    transcript = client.audio.transcriptions.create(
        model="mlx-community/whisper-large-v3-turbo-asr-fp16",
        file=f,
    )
print(transcript.text)
```
