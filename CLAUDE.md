# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Docker Compose-based setup for running Ollama (a local LLM runtime) with Open WebUI as a web interface. The setup includes:
- **ollama**: The core Ollama service for running language models locally
- **open-webui**: A web interface for interacting with Ollama models
- **ollama-init**: A one-time initialization service that pulls the gpt-oss:20b model

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

- **Volumes**:
  - `ollama_data`: Persists Ollama model data at `/root/.ollama`
  - `openwebui_data`: Persists Open WebUI data at `/app/backend/data`

- **GPU Support**: The Ollama service is configured to use all available GPUs (`gpus: all`)

- **Service Dependencies**:
  - `open-webui` depends on `ollama`
  - `ollama-init` depends on `ollama` and runs once to pull the initial model

- **Initialization**: The `ollama-init` service waits for Ollama to be ready, then pulls the `gpt-oss:20b` model automatically

## Accessing Services

- Ollama API: http://localhost:11434
- Open WebUI: http://localhost:4000
