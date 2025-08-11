# Project Setup and Usage Guide

## Prerequisites
- Docker and Docker Compose installed

## Running the Project
To start all services defined in `docker-compose.yml`:

```bash
docker-compose up -d
```
This will run the containers in detached mode.

## Stopping the Project
To stop and remove all containers:

```bash
docker-compose down
```

## Checking Ollama
If Ollama is included in your compose setup, check its status with:

```bash
docker ps | grep ollama
```
Or access its logs:
```bash
docker-compose logs ollama
```

## Checking OSS Chat
If OSS Chat is included, check its status with:

```bash
docker ps | grep oss-chat
```
Or access its logs:
```bash
docker-compose logs oss-chat
```

## Additional Tips
- To view all running containers: `docker ps`
- To view logs for all services: `docker-compose logs`

---
Update service names (`ollama`, `oss-chat`) if your `docker-compose.yml` uses different names.
