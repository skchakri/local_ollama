# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Docker Compose-based setup for running AI models locally with GPU acceleration. The setup includes:
- **ollama**: The core Ollama service for running language models locally
- **open-webui**: A web interface for interacting with Ollama models
- **ollama-init**: A one-time initialization service that pulls Qwen2.5:14b and nomic-embed-text models
- **stable-diffusion-webui**: AUTOMATIC1111 WebUI for AI image generation (browser interface)
- **stable-diffusion-api**: REST API service for programmatic image generation

## Key Commands

**IMPORTANT**: This project uses Docker Compose V2 syntax. Use `docker compose` (space, not hyphen) instead of the deprecated `docker-compose` command.

### Starting Services
```bash
docker compose up -d
```

### Stopping Services
```bash
docker compose down
```

### Viewing Logs
```bash
docker compose logs ollama
docker compose logs open-webui
docker compose logs stable-diffusion-webui
docker compose logs stable-diffusion-api
```

### Checking Service Status
```bash
docker ps
```

### Testing Ollama API
```bash
curl http://localhost:11434/api/tags
```

## Architecture

- **Port Mappings**:
  - Ollama API: `11434` (host) → `11434` (container)
  - Open WebUI: `4000` (host) → `8080` (container)
  - Stable Diffusion WebUI: `7860` (host) → `7860` (container)
  - Stable Diffusion API: `8000` (host) → `8000` (container)

- **Volumes**:
  - `ollama_data`: Persists Ollama model data at `/root/.ollama`
  - `openwebui_data`: Persists Open WebUI data at `/app/backend/data`
  - `sd_models`: Stores Stable Diffusion WebUI models
  - `sd_outputs`: Stores generated images from WebUI
  - `sd_api_models`: Stores Stable Diffusion API models (HuggingFace cache)

- **GPU Support**: All services are configured to use NVIDIA GPUs
  - Ollama: Uses `gpus: all` flag
  - Stable Diffusion: Uses Docker Compose deploy.resources with nvidia driver

- **Service Dependencies**:
  - `open-webui` depends on `ollama`
  - `ollama-init` depends on `ollama` and runs once to pull initial models

- **Models Included**:
  - **qwen2.5:14b**: For content generation and multilingual translation (~9 GB)
  - **nomic-embed-text**: For text embeddings (~274 MB)
  - **Stable Diffusion WebUI**: Downloads default models on first run
  - **Stable Diffusion API**: Uses stabilityai/stable-diffusion-2-1 by default

- **Initialization**: The `ollama-init` service waits for Ollama to be ready, then pulls both Qwen2.5:14b and nomic-embed-text models automatically

## Accessing Services

- Ollama API: http://localhost:11434
- Open WebUI: http://localhost:4000
- Stable Diffusion WebUI: http://localhost:7860
- Stable Diffusion API: http://localhost:8000

## API Usage Examples

### Stable Diffusion API (Text-to-Image)

**curl:**
```bash
curl -X POST http://localhost:8000/ \
  -H "Content-Type: application/json" \
  -d '{"modelInputs": {"prompt": "a beautiful sunset", "num_inference_steps": 50, "width": 512, "height": 512}, "callInputs": {}}'
```

**Python:**
```python
import requests
response = requests.post("http://localhost:8000/",
    json={"modelInputs": {"prompt": "a beautiful sunset", "num_inference_steps": 50, "width": 512, "height": 512}, "callInputs": {}})
# Returns base64-encoded images in response.json()['images']
print(response.json())
```

## GPU Requirements

- NVIDIA GPU with CUDA support required
- NVIDIA drivers must be installed (580.65.06 or later recommended)
- NVIDIA Container Toolkit must be installed for Docker GPU access
