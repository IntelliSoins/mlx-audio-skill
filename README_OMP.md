# mlx-audio Skill for Oh My Pi

Skill pack pour [Oh My Pi (omp)](https://omp.sh/) ajoutant le support de **mlx-audio** : synthèse vocale (TTS), transcription (STT), séparation/enhancement audio (STS), détection de voix (VAD), pipelines voix en temps réel et API compatible OpenAI sur Apple Silicon via MLX.

## Installation

### Option 1 : Installation locale (développement)

```bash
cd /chemin/vers/mlx-audio-omp-skill
omp plugin link .
```

### Option 2 : Installation via marketplace (si publié)

```bash
/marketplace add votre-org/mlx-audio-omp-skill
/marketplace install mlx-audio-skill@votre-marketplace
```

### Option 3 : Copie manuelle

Copiez le dossier `skills/mlx-audio` dans l'un des emplacements suivants :

- Projet : `<votre-projet>/.omp/skills/mlx-audio/`
- Utilisateur : `~/.omp/agent/skills/mlx-audio/`

Puis rechargez les skills :
```bash
/reload-plugins
```

## Structure du package

```
mlx-audio-omp-skill/
├── package.json              # Manifeste OMP avec omp.skills
├── skills/
│   └── mlx-audio/
│       └── SKILL.md          # Définition du skill (contenu + métadonnées)
├── README.md                 # Ce fichier
└── LICENSE                   # Licence MIT
```

## Utilisation

Une fois installé, le skill est automatiquement découvert par OMP au démarrage.

### Commandes interactives

Le skill expose une commande slash `/skill:mlx-audio` qui injecte la documentation complète dans la conversation.

### Accès programmatique

Le contenu du skill est accessible via l'URL interne :
```
skill://mlx-audio           → SKILL.md complet
skill://mlx-audio/references/models_and_voices.md → Documentation des modèles
skill://mlx-audio/references/server_api.md → API du serveur
```

### Fonctionnalités disponibles

Le skill fournit des exemples et workflows pour :

- **TTS (Text-to-Speech)** : Synthèse vocale avec Qwen3-TTS, Kokoro, CSM, etc.
- **STT (Speech-to-Text)** : Transcription avec Whisper, Qwen3-ASR, Voxtral, etc.
- **STS (Speech-to-Speech)** : Séparation de sources, enhancement, noise removal
- **VAD (Voice Activity Detection)** : Détection de parole avec Silero, Smart Turn
- **Voice Pipeline** : Assistant vocal temps réel (mic → STT → LLM → TTS)
- **Server API** : Serveur FastAPI compatible OpenAI (+ UI Studio)
- **Conversion de modèles** : Quantification et conversion depuis Hugging Face

## Prérequis

- **Matériel** : Mac Apple Silicon (M1/M2/M3+)
- **Python** : 3.10+
- **ffmpeg** : `brew install ffmpeg` (pour MP3/FLAC/OGG/Opus/WebM)
- **mlx-audio** : `pip install mlx-audio[all]`

## Configuration OMP

Assurez-vous que les skills sont activés dans votre configuration OMP :

```json
{
  "skills": {
    "enabled": true,
    "enableSkillCommands": true
  }
}
```

## Publication sur un Marketplace

Pour publier ce skill sur un marketplace OMP :

1. Créez un dépôt Git avec cette structure
2. Ajoutez un fichier `.omp-plugin/marketplace.json` :

```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "mlx-audio-marketplace",
  "owner": { "name": "Votre Nom", "email": "vous@exemple.com" },
  "metadata": {
    "description": "Skills audio MLX pour Apple Silicon",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "mlx-audio-skill",
      "description": "TTS, STT, STS, VAD pour mlx-audio",
      "source": "./",
      "category": "development",
      "homepage": "https://github.com/votre-org/mlx-audio-omp-skill"
    }
  ]
}
```

3. Publiez le dépôt sur GitHub
4. Les utilisateurs peuvent l'ajouter avec :
   ```bash
   /marketplace add votre-org/mlx-audio-omp-skill
   /marketplace install mlx-audio-skill@votre-org
   ```

## License

MIT — voir le fichier LICENSE
