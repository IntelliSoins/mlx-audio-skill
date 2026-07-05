# mlx-audio Skill for Claude Code

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that gives Claude specialized knowledge for working with [MLX-Audio](https://github.com/Blaizzy/mlx-audio) — the audio processing library optimized for Apple Silicon.

## What is this?

Claude Code skills are modular packages that extend Claude's capabilities with domain-specific knowledge. When this skill is installed, Claude automatically knows how to:

- **Generate speech from text** (TTS) using models like Qwen3-TTS, Kokoro, CSM, Chatterbox, KugelAudio, and more
- **Transcribe audio to text** (STT) using Whisper, Qwen3-ASR, Parakeet, Voxtral Realtime, VibeVoice-ASR, and others
- **Enhance and separate audio** (STS) with DeepFilterNet, MossFormer2, and SAM-Audio
- **Detect speech and diarize speakers** (VAD) with Silero, Sortformer, and Smart Turn
- **Run realtime voice pipelines** combining VAD, STT, LLM, and TTS
- **Start an OpenAI-compatible REST API** server with Realtime WebSocket support
- **Convert and quantize models** for optimized inference on Apple Silicon
- **Choose the right model and voice** for a given task

Without this skill, Claude would need to look up documentation each time. With it, Claude has instant access to correct import paths, API patterns, model IDs, voice presets, and CLI commands.

## Prerequisites

- Apple Silicon Mac (M1/M2/M3/M4)
- Python 3.10+
- [mlx-audio](https://github.com/Blaizzy/mlx-audio) v0.4.4+ installed (`pip install mlx-audio`)
- ffmpeg (`brew install ffmpeg`)

## Installation

### Option 1: Add as a Claude Code skill directory

Clone this repo into your Claude Code skills directory:

```bash
git clone https://github.com/IntelliSoins/mlx-audio-skill.git ~/.claude/skills/mlx-audio
```

### Option 2: Add to a project

Clone into your project's `.claude/skills/` directory:

```bash
mkdir -p .claude/skills
git clone https://github.com/IntelliSoins/mlx-audio-skill.git .claude/skills/mlx-audio
```

## What's included

```
mlx-audio/
├── SKILL.md                        # Main skill instructions
└── references/
    ├── models_and_voices.md        # Catalog of TTS/STT/STS/VAD models and voice presets
    └── server_api.md               # REST API, WebSocket, and server configuration
```

| File | Purpose |
|------|---------|
| `SKILL.md` | Core instructions: quick start code, task workflows, module map, installation |
| `references/models_and_voices.md` | Full list of supported models and voice naming conventions |
| `references/server_api.md` | OpenAI-compatible REST + Realtime WebSocket endpoints |

## Example prompts

Once the skill is installed, you can ask Claude things like:

- *"Generate speech from this text using Qwen3-TTS"*
- *"Transcribe this audio file to SRT subtitles"*
- *"Start an MLX-Audio server with the Studio UI"*
- *"Set up a realtime voice pipeline with Voxtral and Pocket TTS"*
- *"Remove noise from this recording with DeepFilterNet"*
- *"What's the best model for voice cloning?"*
- *"Convert this HuggingFace model to 4-bit quantized MLX format"*

## Supported models

The skill covers the full MLX-Audio model ecosystem (v0.4.4):

**TTS (20+ models)**: Qwen3-TTS, Kokoro, KittenTTS, Higgs Audio, OmniVoice, CSM/MisoTTS, Chatterbox, KugelAudio, Voxtral TTS, LongCat-AudioDiT, MOSS-TTS, Ming Omni, and more

**STT (18+ models)**: Whisper, Qwen3-ASR, Parakeet, Voxtral Realtime, VibeVoice-ASR, Nemotron ASR, MOSS-Transcribe-Diarize, MedASR, and more

**VAD (4 models)**: Silero VAD, Sortformer, Smart Turn v3

**STS (4 models)**: SAM-Audio, Liquid2.5-Audio, MossFormer2 SE, DeepFilterNet

See [references/models_and_voices.md](references/models_and_voices.md) for the complete list.

## Credits

- [MLX-Audio](https://github.com/Blaizzy/mlx-audio) by Prince Canuma — the underlying library this skill documents
- [Apple MLX](https://github.com/ml-explore/mlx) — the machine learning framework for Apple Silicon

## License

MIT
