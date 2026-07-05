---
name: mlx-audio
description: Generate speech (TTS), transcribe audio (STT), separate/enhance audio (STS), detect speech (VAD), run realtime voice pipelines, and serve an OpenAI-compatible API on Apple Silicon via MLX. Use for voice synthesis, transcription, cloning, diarization, noise removal, or local audio API work on Mac.
---

# MLX-Audio

Audio library for Apple Silicon (MLX): TTS, STT, STS, VAD, voice cloning, streaming, Studio UI, and OpenAI-compatible REST + Realtime WebSocket APIs.

**Requires**: Python 3.10+, Apple Silicon Mac, ffmpeg for MP3/FLAC/OGG/Opus/Vorbis/WebM (`brew install ffmpeg`)

**Version reference**: 0.4.4

## Quick Start

### Text-to-Speech (TTS)

```python
from mlx_audio.tts.utils import load_model

model = load_model("mlx-community/Qwen3-TTS-12Hz-1.7B-Base-8bit")
for result in model.generate(
    "Hello, world!",
    voice="Chelsie",
    lang_code="English",
):
    audio = result.audio  # mx.array
```

CLI equivalent:
```bash
mlx_audio.tts.generate \
  --model mlx-community/Qwen3-TTS-12Hz-1.7B-Base-8bit \
  --text "Hello, world!" \
  --voice Chelsie \
  --lang_code English \
  --play
```

**Kokoro (fast/lightweight alternative):**
```python
model = load_model("mlx-community/Kokoro-82M-bf16")
for result in model.generate(
    text="Welcome to MLX-Audio!",
    voice="af_heart",
    speed=1.0,
    lang_code="a",
):
    audio = result.audio
```
Kokoro requires `pip install misaki` (add `misaki[ja]` or `misaki[zh]` for Japanese/Mandarin).

### Speech-to-Text (STT)

```python
from mlx_audio.stt.generate import generate_transcription

result = generate_transcription(
    model="mlx-community/whisper-large-v3-turbo-asr-fp16",
    audio="audio.wav",
    output_path="transcript",
    format="json",
    verbose=True,
)
print(result.text)
```

**Unified loader (preferred for programmatic use):**
```python
from mlx_audio.stt import load

model = load("mlx-community/Qwen3-ASR-0.6B-8bit")
result = model.generate("audio.wav", language="English")
print(result.text)
```

CLI equivalent:
```bash
mlx_audio.stt.generate \
  --model mlx-community/whisper-large-v3-turbo-asr-fp16 \
  --audio audio.wav \
  --output-path transcript
```

### Speech-to-Speech (STS)

**Noise removal / enhancement:**
```bash
mlx_audio.sts.generate \
  --model mlx-community/DeepFilterNet-mlx \
  --audio noisy.wav \
  --version 2 \
  --output-path clean.wav
```

**Source separation (SAM-Audio):**
```python
from mlx_audio.sts import SAMAudio, SAMAudioProcessor, save_audio

model = SAMAudio.from_pretrained("mlx-community/sam-audio-large")
processor = SAMAudioProcessor.from_pretrained("mlx-community/sam-audio-large")
batch = processor(descriptions=["A person speaking"], audios=["mixed.wav"])
result = model.separate_long(batch.audios, descriptions=batch.descriptions)
save_audio(result.target[0], "voice.wav")
```

### Realtime Voice Pipeline

```python
from mlx_audio.sts import VoicePipeline, VoicePipelineConfig

config = VoicePipelineConfig(
    latency_profile="balanced",  # fast | balanced | quality
    stt_model="mlx-community/Voxtral-Mini-4B-Realtime-2602-4bit",
    tts_model="mlx-community/pocket-tts",
    tts_voice="cosette",
    response_model="mlx-community/NVIDIA-Nemotron-3-Nano-30B-A3B-4bit",
    vad_model="mlx-community/silero-vad",
    turn_model="mlx-community/smart-turn-v3",
    barge_in=True,
    play_audio=True,
)
pipeline = VoicePipeline(config)
await pipeline.start()
```

CLI equivalent:
```bash
python -m mlx_audio.sts.voice_pipeline \
  --stt_model mlx-community/Voxtral-Mini-4B-Realtime-2602-4bit \
  --tts_model mlx-community/pocket-tts \
  --llm_model mlx-community/NVIDIA-Nemotron-3-Nano-30B-A3B-4bit \
  --vad_model mlx-community/silero-vad \
  --turn_model mlx-community/smart-turn-v3 \
  --latency_profile balanced \
  --voice cosette
```

## Task Workflows

### Generate Speech from Text

1. Choose a TTS model based on requirements (see [references/models_and_voices.md](references/models_and_voices.md))
   - **Default/recommended**: Qwen3-TTS (`mlx-community/Qwen3-TTS-12Hz-1.7B-Base-8bit`)
   - **Fast/lightweight**: Kokoro (`mlx-community/Kokoro-82M-bf16`)
   - **Voice cloning**: CSM (`mlx-community/csm-1b`), Qwen3-TTS, Higgs Audio, OmniVoice
   - **Multilingual**: Chatterbox (16 languages), KugelAudio (24 EU langs), Voxtral TTS (9 langs)
2. Load the model with `load_model()` or `from mlx_audio.tts import load`
3. Call `model.generate()` with text, voice, and output parameters
4. Key CLI flags: `--text`, `--voice`, `--lang_code`, `--speed`, `--play`, `--output_path`, `--stream`, `--save`, `--join_audio`, `--ref_audio`, `--ref_text`

Multi-segment output saves `audio_000.wav`, `audio_001.wav` by default; use `--join_audio` for a single file.

### Transcribe Audio

1. Choose an STT model:
   - **General purpose**: Whisper (`mlx-community/whisper-large-v3-turbo-asr-fp16`)
   - **Multilingual (Alibaba)**: Qwen3-ASR
   - **European languages**: Parakeet v3 (25 langs)
   - **Streaming/low latency**: Voxtral Realtime
   - **With diarization**: VibeVoice-ASR, MOSS-Transcribe-Diarize
   - **Word alignment**: Qwen3-ForcedAligner
2. Call `generate_transcription()` or `model.generate()` with audio path and model
3. Output formats: `txt`, `srt`, `vtt`, `json`

### Enhance or Separate Audio (STS)

1. **Noise removal**: DeepFilterNet (`mlx_audio.sts.generate`) or MossFormer2 SE
2. **Source separation**: SAM-Audio with text prompts
3. **All-in-one speech model**: Liquid2.5-Audio (LFM2.5)

### Voice Activity Detection (VAD)

```python
from mlx_audio.vad import load

vad = load("mlx-community/silero-vad")
# Used by VoicePipeline and server-side turn detection on /v1/realtime
```

### Convert and Quantize Models

```bash
python -m mlx_audio.convert \
  --hf-path <huggingface_model_id> \
  --mlx-path ./output \
  --quantize \
  --q-bits 4 \
  --q-mode affine
```

Supported quantization modes: `affine`, `mxfp4`, `mxfp8`, `nvfp4`. Data types: `float16`, `bfloat16`, `float32`. Optional: `--model-domain tts|stt|sts|lid`, `--upload-repo`.

### Run the REST API Server

```bash
mlx_audio.server --host 0.0.0.0 --port 8000

# With Studio UI
mlx_audio.server --host 0.0.0.0 --port 8000 --start-ui
```

OpenAI-compatible endpoints plus Realtime WebSocket. See [references/server_api.md](references/server_api.md) for full API reference.

## Key Modules

| Module | Import | Purpose |
|--------|--------|---------|
| TTS | `from mlx_audio.tts import load` | Load TTS models |
| TTS CLI | `mlx_audio.tts.generate` | Generate + save/play audio |
| STT | `from mlx_audio.stt import load` | Load STT models |
| STT CLI | `mlx_audio.stt.generate` | Transcribe to txt/srt/vtt/json |
| STS | `from mlx_audio.sts import load, SAMAudio, save_audio` | Separation, enhancement |
| STS CLI | `mlx_audio.sts.generate` | DeepFilterNet / MossFormer2 enhancement |
| Voice assistant | `from mlx_audio.sts import VoicePipeline, VoicePipelineConfig` | Mic → STT → LLM → TTS loop |
| VAD | `from mlx_audio.vad import load` | Silero, Sortformer, Smart Turn |
| Audio I/O | `from mlx_audio.audio_io import read, write` | WAV/MP3/FLAC/OGG/Opus/WebM/M4A |
| Server | `mlx_audio.server` | FastAPI + Realtime WebSocket |
| Convert | `python -m mlx_audio.convert` | Quantize / dtype convert |

All domains use `load("hf-repo-id")` which reads `config.json` and routes to the correct model implementation.

## Installation

```bash
pip install mlx-audio                # Core
pip install "mlx-audio[tts]"         # + TTS dependencies
pip install "mlx-audio[stt]"         # + STT dependencies
pip install "mlx-audio[sts]"         # + Speech-to-speech
pip install "mlx-audio[server]"      # + FastAPI server (includes python-multipart)
pip install "mlx-audio[all]"         # Most features (missing python-multipart — also install [server] for uploads)
pip install "mlx-audio[docs]"        # Documentation build tools
```

CLI-only via uv:
```bash
uv tool install --force mlx-audio --prerelease=allow
```

For development from source:
```bash
git clone https://github.com/Blaizzy/mlx-audio.git
cd mlx-audio
pip install -e ".[dev, server]"
```

## Audio Format Support

- **Read (miniaudio)**: WAV, MP3, FLAC, OGG/Vorbis
- **Read (ffmpeg)**: M4A/AAC, OGG/Opus, WebM
- **Write (miniaudio)**: WAV
- **Write (ffmpeg)**: MP3, FLAC, OGG, Opus, Vorbis, WebM

## Resources

- **[references/models_and_voices.md](references/models_and_voices.md)**: Complete list of supported TTS, STT, STS, VAD models and voice presets
- **[references/server_api.md](references/server_api.md)**: REST API, WebSocket, and server configuration
