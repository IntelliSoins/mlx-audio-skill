# MLX-Audio Server API Reference

## Contents
- Starting the Server
- TTS Endpoint
- STT Endpoint
- Configuration Options

## Starting the Server

```bash
python -m mlx_audio.server --host 0.0.0.0 --port 8000
```

The server is FastAPI-based with OpenAI-compatible endpoints and CORS support.

## TTS Endpoint

`POST /v1/audio/speech`

Request body (JSON):
```json
{
  "model": "mlx-community/Kokoro-82M-bf16",
  "input": "Text to speak",
  "voice": "af_heart",
  "speed": 1.0,
  "response_format": "wav"
}
```

Parameters:
- `model` (required): HuggingFace model ID
- `input` (required): Text to synthesize
- `voice` (optional): Voice preset name (model-dependent)
- `speed` (optional): Playback speed multiplier (default: 1.0)
- `response_format` (optional): Output format (`wav`, `mp3`, `flac`)

Returns: Audio file in the requested format.

## STT Endpoint

`POST /v1/audio/transcriptions`

Request (multipart form):
- `file`: Audio file (WAV, MP3, FLAC, OGG, M4A)
- `model`: HuggingFace model ID
- `response_format` (optional): `json`, `text`, `srt`, `vtt`
- `language` (optional): Language code

```bash
curl -X POST http://localhost:8000/v1/audio/transcriptions \
  -F "file=@audio.wav" \
  -F "model=mlx-community/whisper-large-v3-turbo-asr-fp16"
```

Returns: Transcription in the requested format.

## Configuration Options

Server CLI flags:
- `--host`: Bind address (default: `0.0.0.0`)
- `--port`: Port number (default: `8000`)
- `--workers`: Number of worker processes

Models are cached after first load for fast subsequent requests.
