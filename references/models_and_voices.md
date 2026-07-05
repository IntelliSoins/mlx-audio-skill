# MLX-Audio Models and Voices Reference

## Contents
- TTS Models
- STT Models
- VAD Models
- STS Models
- Audio Codec Models
- Voice Presets

## TTS Models

| Model | HuggingFace ID | Features |
|-------|---------------|----------|
| **Qwen3-TTS** | `mlx-community/Qwen3-TTS-12Hz-1.7B-Base-8bit` | Voice cloning, emotion, voice design (recommended default) |
| **Kokoro** | `mlx-community/Kokoro-82M-bf16` (+ 8/6/4bit) | 54 voice presets, fast, 6 languages; needs `misaki` |
| **KittenTTS** | `mlx-community/kitten-tts-nano-0.8` | Compact edge-friendly TTS, EN only |
| **Higgs Audio v3** | `bosonai/higgs-audio-v3-tts-4b` | Voice cloning, 100 languages |
| **Higgs Audio v2** | `mlx-community/higgs-audio-v2-3B-mlx-q8` | Real-time voice cloning, EN/ZH/KO/DE/ES |
| **OmniVoice** | `mlx-community/OmniVoice-bf16` | 646+ langs, zero-shot cloning, nonverbal tags |
| **CSM / MisoTTS** | `mlx-community/csm-1b`, `MisoLabs-MisoTTS-bf16` | Voice cloning from reference audio |
| **Chatterbox** | `mlx-community/chatterbox-fp16` | 16 languages, emotional expression |
| **KugelAudio** | `kugelaudio/kugelaudio-0-open` | 24 European languages, AR+Diffusion |
| **Voxtral TTS** | `mlx-community/Voxtral-4B-TTS-2603-mlx-bf16` | 20 voices, 9 languages |
| **LongCat-AudioDiT** | `mlx-community/LongCat-AudioDiT-1B-bf16` | Diffusion TTS, voice cloning |
| **MOSS-TTS** | `OpenMOSS-Team/MOSS-TTS-v1.5` | 31 languages, voice cloning |
| **MOSS-TTS-Nano** | `mlx-community/MOSS-TTS-Nano-100M` | Tiny multilingual voice cloning |
| **Ming Omni TTS** | `mlx-community/Ming-omni-tts-16.8B-A3B-bf16` | Style/music/SFX, voice cloning |
| **OuteTTS** | `mlx-community/OuteTTS-1.0-0.6B-fp16` | Efficient 500M model |
| **Spark** | `mlx-community/Spark-TTS-0.5B-bf16` | English + Chinese |
| **Soprano** | `mlx-community/Soprano-1.1-80M-bf16` | High-quality voices |
| **Dia** | `mlx-community/Dia-1.6B-fp16` | Dialogue-focused TTS |
| **MeloTTS** | `mlx-community/MeloTTS-English-MLX` | Lightweight VITS2, streaming |
| **Pocket TTS** | `mlx-community/pocket-tts` | Used by VoicePipeline (default TTS) |
| **Bark** | `suno/bark` | Multi-language, sound effects |
| **IndexTTS** | `mlx-community/IndexTTS-*` | Index-based TTS |

## STT Models

| Model | HuggingFace ID | Features |
|-------|---------------|----------|
| **Whisper** | `mlx-community/whisper-large-v3-turbo-asr-fp16` | 99+ languages, robust |
| **Distil-Whisper** | `distil-whisper/distil-large-v3` | Fast English-only |
| **Qwen3-ASR** | `mlx-community/Qwen3-ASR-0.6B-8bit` | Multilingual (Alibaba) |
| **Qwen3-ForcedAligner** | `mlx-community/Qwen3-ForcedAligner-0.6B-8bit` | Word-level alignment |
| **Mega-ASR** | Routed Qwen3 backbone | Auto clean/degraded switching |
| **Parakeet** | `mlx-community/parakeet-tdt-0.6b-v3` | 25 European languages (v3) |
| **Nemotron 3.5 ASR** | `mlx-community/nemotron-3.5-asr-streaming-0.6b` | Streaming, 40 locales |
| **Voxtral** | `mlx-community/Voxtral-Mini-3B-2507-bf16` | Mistral speech model |
| **Voxtral Realtime** | `mlx-community/Voxtral-Mini-4B-Realtime-2602-4bit` | Streaming STT, low latency |
| **VibeVoice-ASR** | `mlx-community/VibeVoice-ASR-bf16` | 9B, diarization + timestamps |
| **MOSS-Transcribe-Diarize** | `OpenMOSS-Team/MOSS-Transcribe-Diarize` | Speaker labels + timestamps |
| **Qwen2-Audio** | `mlx-community/Qwen2-Audio-7B-Instruct-4bit` | Multimodal audio understanding |
| **MedASR** | `mlx-community/medasr` | Medical transcription |
| **Canary** | NVIDIA multilingual ASR | 25 EU + RU, UK |
| **Moonshine** | Lightweight ASR | English |
| **MMS** | Meta massively multilingual | 1000+ languages |
| **Granite Speech** | IBM ASR + translation | EN, FR, DE, ES, PT, JA |
| **GLM-ASR** | `mlx-community/GLM-ASR-*` | Chinese specialist |
| **Wav2Vec** | `facebook/wav2vec2-*` | Self-supervised, multiple languages |

## VAD Models

| Model | HuggingFace ID | Features |
|-------|---------------|----------|
| **Silero VAD** | `mlx-community/silero-vad` | Speech/non-speech detection, streaming |
| **Sortformer v1** | `mlx-community/diar_sortformer_4spk-v1-fp32` | Speaker diarization (up to 4 speakers) |
| **Sortformer v2.1** | `mlx-community/diar_streaming_sortformer_4spk-v2.1-fp32` | Streaming diarization |
| **Smart Turn v3** | `mlx-community/smart-turn-v3` | Turn-end detection (VoicePipeline) |

## STS Models

| Model | HuggingFace ID | Use Case |
|-------|---------------|----------|
| **SAM-Audio** | `mlx-community/sam-audio-large` | Text-guided source separation |
| **Liquid2.5-Audio (LFM2.5)** | `mlx-community/LFM2.5-Audio-1.5B-8bit` | All-in-one speech-to-speech, TTS, STT |
| **MossFormer2 SE** | `starkdmi/MossFormer2_SE_48K_MLX` | Speech enhancement @ 48kHz |
| **DeepFilterNet (1/2/3)** | `mlx-community/DeepFilterNet-mlx` | Noise suppression |

## Audio Codec Models

BigVGAN, EnCodec, DAC/DACVAE, Vocos, SNAC, Descript, Mimi, S3

## Voice Presets

### Kokoro (54 voices)

Pattern: `{lang}{gender}_{name}`

| Code | Language | Extra dependency |
|------|----------|-----------------|
| `a` | American English | `pip install misaki` |
| `b` | British English | `pip install misaki` |
| `j` | Japanese | `pip install misaki[ja]` |
| `z` | Mandarin Chinese | `pip install misaki[zh]` |
| `e` | Spanish | `pip install misaki` |
| `f` | French | `pip install misaki` |

Examples: `af_heart`, `af_bella`, `af_nova`, `af_sky`, `am_adam`, `am_echo`, `bf_alice`, `bf_emma`, `bm_daniel`, `bm_george`, `jf_alpha`, `jm_kumo`, `zf_xiaobei`, `zm_yunxi`

Use `af_heart` as a reliable default. Discover voices via `GET /v1/audio/voices?model=mlx-community/Kokoro-82M-bf16`.

### Qwen3-TTS

Preset speakers: `Chelsie`, `Ethan`, `Vivian`, etc.

Use full language names: `lang_code="English"`, `"Chinese"`, `"Japanese"` (not Kokoro single-letter codes).

### Voxtral TTS (20 voices)

`casual_male`, `casual_female`, `cheerful_female`, `neutral_male`, `neutral_female`, `fr_male`, `fr_female`, `es_male`, `es_female`, `de_male`, `de_female`, `it_male`, `it_female`, `pt_male`, `pt_female`, `nl_male`, `nl_female`, `ar_male`, `hi_male`, `hi_female`

### Pocket TTS (VoicePipeline default)

Default voice: `cosette`
