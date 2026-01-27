# MLX-Audio Models and Voices Reference

## Contents
- TTS Models
- STT Models
- STS Models
- Audio Codec Models
- Kokoro Voice Presets

## TTS Models

| Model | HuggingFace ID Pattern | Features |
|-------|----------------------|----------|
| **Kokoro** | `mlx-community/Kokoro-82M-bf16` | 54 voice presets, 8 languages, real-time streaming |
| **Qwen3-TTS** | `Qwen/Qwen3-TTS` | Voice cloning, emotion control, voice design |
| **CSM (Sesame)** | `mlx-community/csm-1b-fp16` | Voice cloning from reference audio |
| **Chatterbox** | `mlx-community/Chatterbox-*` | 16 languages, emotional expression |
| **OuteTTS** | `mlx-community/OuteTTS-*` | 500M model, efficient |
| **Spark** | `mlx-community/SparkTTS-*` | English + Chinese |
| **Soprano** | `mlx-community/Soprano-*` | High-quality voices |
| **Dia** | `mlx-community/Dia-*` | Dialogue-focused TTS |
| **Bark** | `suno/bark` | Multi-language, sound effects |
| **IndexTTS** | `mlx-community/IndexTTS-*` | Index-based TTS |

## STT Models

| Model | HuggingFace ID Pattern | Features |
|-------|----------------------|----------|
| **Whisper** | `mlx-community/whisper-large-v3-turbo-asr-fp16` | 99+ languages, robust |
| **Parakeet** | `mlx-community/parakeet-*` | English, NVIDIA, accurate |
| **Voxtral** | `mistralai/Voxtral-*` | Mistral speech model |
| **VibeVoice-ASR** | `mlx-community/VibeVoice-*` | 9B, diarization + timestamps |
| **GLM-ASR** | `mlx-community/GLM-ASR-*` | Chinese specialist |
| **Wav2Vec** | `facebook/wav2vec2-*` | Self-supervised, multiple languages |

## STS Models

| Model | Key Feature |
|-------|-------------|
| **SAM-Audio** | Text-guided source separation |
| **Liquid2.5-Audio (LFM2.5)** | All-in-one speech-to-speech, TTS, STT |
| **MossFormer2 SE** | Speech enhancement & noise removal |

## Audio Codec Models

BigVGAN, EnCodec, DAC/DACVAE, Vocos, SNAC, Descript, Mimi, S3

## Kokoro Voice Presets

Kokoro voices follow the pattern `{lang}_{gender}_{name}`:
- Language prefix: `a` (American English), `b` (British English), `j` (Japanese), `z` (Chinese), `k` (Korean), `f` (French), `h` (Hindi), `p` (Portuguese)
- Gender: `f` (female), `m` (male)
- Examples: `af_heart`, `af_bella`, `am_adam`, `bf_emma`, `bm_george`

Use `af_heart` as a reliable default voice for Kokoro.
