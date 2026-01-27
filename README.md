# mlx-audio Skill for Claude Code

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that gives Claude specialized knowledge for working with [MLX-Audio](https://github.com/Blaizzy/mlx-audio) — the audio processing library optimized for Apple Silicon.

## What is this?

Claude Code skills are modular packages that extend Claude's capabilities with domain-specific knowledge. When this skill is installed, Claude automatically knows how to:

- **Generate speech from text** (TTS) using models like Kokoro, CSM, Qwen3-TTS, Chatterbox, and more
- **Transcribe audio to text** (STT) using Whisper, Parakeet, VibeVoice-ASR, and others
- **Run speech-to-speech pipelines** combining STT, LLM, and TTS
- **Start an OpenAI-compatible REST API** server for TTS and STT
- **Convert and quantize models** for optimized inference on Apple Silicon
- **Choose the right model and voice** for a given task

Without this skill, Claude would need to look up documentation each time. With it, Claude has instant access to correct import paths, API patterns, model IDs, voice presets, and CLI commands.

## Prerequisites

- Apple Silicon Mac (M1/M2/M3/M4)
- Python 3.10+
- [mlx-audio](https://github.com/Blaizzy/mlx-audio) installed (`pip install mlx-audio`)
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
    ├── models_and_voices.md        # Catalog of TTS/STT/STS models and voice presets
    └── server_api.md               # REST API endpoint documentation
```

| File | Purpose |
|------|---------|
| `SKILL.md` | Core instructions: quick start code, task workflows, module map, installation |
| `references/models_and_voices.md` | Full list of supported models (21 TTS, 8 STT, 3 STS) and Kokoro voice naming conventions |
| `references/server_api.md` | OpenAI-compatible API endpoints for TTS (`/v1/audio/speech`) and STT (`/v1/audio/transcriptions`) |

## Example prompts

Once the skill is installed, you can ask Claude things like:

- *"Generate speech from this text using Kokoro"*
- *"Transcribe this audio file to SRT subtitles"*
- *"Start an MLX-Audio server and show me how to call it"*
- *"What's the best model for voice cloning?"*
- *"Convert this HuggingFace model to 4-bit quantized MLX format"*
- *"Set up a speech-to-speech pipeline with Whisper and CSM"*

## Supported models

The skill covers the full MLX-Audio model ecosystem:

**TTS (21 models)**: Kokoro, Qwen3-TTS, CSM/Sesame, Chatterbox, OuteTTS, Spark, Soprano, Dia, Bark, IndexTTS, and more

**STT (8 models)**: Whisper, Parakeet, Voxtral, VibeVoice-ASR, GLM-ASR, Wav2Vec, and more

**STS (3 models)**: SAM-Audio, Liquid2.5-Audio, MossFormer2 SE

See [references/models_and_voices.md](references/models_and_voices.md) for the complete list.

## Credits

- [MLX-Audio](https://github.com/Blaizzy/mlx-audio) by Prince Canuma — the underlying library this skill documents
- [Apple MLX](https://github.com/ml-explore/mlx) — the machine learning framework for Apple Silicon

## License

MIT
