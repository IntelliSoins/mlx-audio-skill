---
name: mlx-audio
description: Generate speech from text (TTS), transcribe audio to text (STT), and run speech-to-speech pipelines using MLX on Apple Silicon. Use when working with audio generation, voice synthesis, speech transcription, voice cloning, or audio processing on Mac with MLX framework.
---

# MLX-Audio

Audio processing library optimized for Apple Silicon using Apple's MLX framework. Provides TTS (text-to-speech), STT (speech-to-text), and STS (speech-to-speech) capabilities with streaming support, voice cloning, and an OpenAI-compatible REST API.

**Requires**: Python 3.10+, Apple Silicon Mac, ffmpeg (`brew install ffmpeg`)

## Quick Start

### Text-to-Speech (TTS)

```python
from mlx_audio.tts.utils import load_model
from mlx_audio.tts.generate import generate_audio

model = load_model("mlx-community/Kokoro-82M-bf16")
generate_audio(
    model=model,
    text="Hello, world!",
    voice="af_heart",
    speed=1.0,
    lang_code="a",
    play=True,
    output_path="./output"
)
```

CLI equivalent:
```bash
python -m mlx_audio.tts.generate \
  --model mlx-community/Kokoro-82M-bf16 \
  --text "Hello, world!" \
  --voice af_heart \
  --play
```

### Speech-to-Text (STT)

```python
from mlx_audio.stt.generate import generate_transcription

result = generate_transcription(
    model="mlx-community/whisper-large-v3-turbo-asr-fp16",
    audio="audio.wav",
    output_path="transcript",
    format="json",
    verbose=True
)
print(result.text)
```

CLI equivalent:
```bash
python -m mlx_audio.stt.generate \
  --model mlx-community/whisper-large-v3-turbo-asr-fp16 \
  --audio audio.wav \
  --output-path transcript
```

### Speech-to-Speech Pipeline

```python
from mlx_audio.sts.voice_pipeline import VoicePipeline

pipeline = VoicePipeline(
    stt_model="mlx-community/whisper-large-v3-turbo-asr-fp16",
    llm_model="Qwen/Qwen2.5-0.5B-Instruct-4bit",
    tts_model="mlx-community/csm-1b-fp16"
)
await pipeline.start()
```

## Task Workflows

### Generate Speech from Text

1. Choose a TTS model based on requirements (see [references/models_and_voices.md](references/models_and_voices.md))
   - **Fast/lightweight**: Kokoro (`mlx-community/Kokoro-82M-bf16`)
   - **Voice cloning**: CSM (`mlx-community/csm-1b-fp16`) or Qwen3-TTS
   - **Multilingual**: Chatterbox (16 languages) or Bark
2. Load the model with `load_model()`
3. Call `generate_audio()` with text, voice, and output parameters
4. Key parameters: `text`, `voice`, `speed`, `lang_code`, `play`, `output_path`, `streaming_interval`

### Transcribe Audio

1. Choose an STT model:
   - **General purpose**: Whisper (`mlx-community/whisper-large-v3-turbo-asr-fp16`)
   - **English-only high accuracy**: Parakeet
   - **With diarization**: VibeVoice-ASR
2. Call `generate_transcription()` with audio path and model
3. Output formats: `txt`, `srt`, `vtt`, `json`

### Convert and Quantize Models

```bash
python -m mlx_audio.convert \
  --hf-path <huggingface_model_id> \
  --mlx-path ./output \
  --quantize \
  --q-bits 4
```

Supported quantization: 3-bit, 4-bit, 6-bit, 8-bit. Data types: float16, bfloat16, float32.

### Run the REST API Server

```bash
python -m mlx_audio.server --host 0.0.0.0 --port 8000
```

OpenAI-compatible endpoints. See [references/server_api.md](references/server_api.md) for full API reference.

## Key Modules

| Module | Import Path | Purpose |
|--------|-------------|---------|
| TTS generation | `mlx_audio.tts.generate` | `generate_audio()` |
| TTS model loading | `mlx_audio.tts.utils` | `load_model()` |
| STT generation | `mlx_audio.stt.generate` | `generate_transcription()` |
| STS pipeline | `mlx_audio.sts.voice_pipeline` | `VoicePipeline` |
| Audio I/O | `mlx_audio.audio_io` | Read/write WAV, MP3, FLAC, OGG, M4A |
| DSP utilities | `mlx_audio.dsp` | STFT, mel filterbanks, window functions |
| Server | `mlx_audio.server` | FastAPI OpenAI-compatible API |
| Conversion | `mlx_audio.convert` | Model quantization and format conversion |

## Installation

```bash
pip install mlx-audio                # Core
pip install "mlx-audio[tts]"         # + TTS dependencies
pip install "mlx-audio[stt]"         # + STT dependencies
pip install "mlx-audio[sts]"         # + Speech-to-speech
pip install "mlx-audio[server]"      # + FastAPI server
pip install "mlx-audio[all]"         # Everything
```

For development from source:
```bash
git clone https://github.com/Blaizzy/mlx-audio.git
cd mlx-audio
pip install -e ".[dev]"
```

## Audio Format Support

- **Read**: WAV, MP3, FLAC, OGG/Vorbis, M4A/AAC (auto-detected via magic bytes)
- **Write**: WAV, MP3, FLAC (MP3/FLAC require ffmpeg)

## Resources

- **[references/models_and_voices.md](references/models_and_voices.md)**: Complete list of supported TTS, STT, STS models and Kokoro voice presets
- **[references/server_api.md](references/server_api.md)**: REST API endpoint documentation
