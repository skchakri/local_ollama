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

## Checking Stable Diffusion Services
Check the Stable Diffusion services status with:

```bash
docker ps | grep stable-diffusion
```
Or access their logs:
```bash
docker compose logs stable-diffusion-webui
docker compose logs stable-diffusion-api
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

## Accessing Stable Diffusion WebUI
Stable Diffusion WebUI runs on port 7860. Access it in your browser:

```
http://localhost:7860
```
This provides a web interface for AI image generation using Stable Diffusion models.

## Accessing Stable Diffusion API
Stable Diffusion API runs on port 8000. This is a REST API for programmatic image generation.

**API Endpoint:** `http://localhost:8000/`

**Example usage with curl:**
```bash
curl -X POST http://localhost:8000/ \
  -H "Content-Type: application/json" \
  -d '{
    "modelInputs": {
      "prompt": "a beautiful sunset over mountains",
      "num_inference_steps": 50,
      "guidance_scale": 7.5,
      "width": 512,
      "height": 512
    },
    "callInputs": {}
  }'
```

**Example usage with Python:**
```python
import requests
import base64
from io import BytesIO
from PIL import Image

response = requests.post(
    "http://localhost:8000/",
    json={
        "modelInputs": {
            "prompt": "a beautiful sunset over mountains",
            "num_inference_steps": 50,
            "guidance_scale": 7.5,
            "width": 512,
            "height": 512
        },
        "callInputs": {}
    }
)

result = response.json()
# Decode base64 image
image_data = base64.b64decode(result['images'][0])
image = Image.open(BytesIO(image_data))
image.save("output.png")
print("Image saved as output.png")
```

## Models Included

### Ollama Models
- **qwen2.5:14b** - Content generation and translation (~9 GB)
- **nomic-embed-text** - Text embeddings (~274 MB)

### Stable Diffusion
- **WebUI**: On first run, downloads default models automatically
- **API**: Uses stabilityai/stable-diffusion-2-1 model by default
- You can add custom models to the volumes

## Additional Tips
- To view all running containers: `docker ps`
- To view logs for all services: `docker compose logs`
- First startup will take longer as models are downloaded
- GPU support is required for optimal performance
