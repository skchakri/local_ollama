# Project Setup and Usage Guide

## Prerequisites
- Docker and Docker Compose installed

## Running the Project
To start all services defined in `docker-compose.yml`:

```bash
docker compose up -d
```
This will run the containers in detached mode.

**Note**: Use `docker compose` (with a space) instead of the deprecated `docker-compose` (with a hyphen). Docker Compose V2 is now integrated into the Docker CLI.

## Stopping the Project
To stop and remove all containers:

```bash
docker compose down
```

## Checking Ollama
If Ollama is included in your compose setup, check its status with:

```bash
docker ps | grep ollama
```
Or access its logs:
```bash
docker compose logs ollama
```

## Checking OSS Chat
If OSS Chat is included, check its status with:

```bash
docker ps | grep oss-chat
```
Or access its logs:
```bash
docker compose logs oss-chat
```

## Accessing Ollama
Ollama runs on port 11434. You can interact with its API using curl or your browser:

```bash
curl http://localhost:11434/api/tags
```
Or open [http://localhost:11434](http://localhost:11434) in your browser (if a web interface is available).

## Accessing Open WebUI
Open WebUI runs on port 4000. Access it in your browser:

```
http://localhost:4000
```
This provides a web interface to interact with Ollama models.

## Additional Tips
- To view all running containers: `docker ps`
- To view logs for all services: `docker compose logs`

---
Update service names (`ollama`, `oss-chat`) if your `docker-compose.yml` uses different names.
