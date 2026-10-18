# Deepfake Detection · Django Application

Web app for uploading a video and classifying it as real or deepfake with a deep
learning model. Faces are located in the browser with face-api, frames are
analysed server-side, and the result is shown with a confidence score.

## Requirements

- Python 3.6+ and Django 3.0+ (full list in `requirements.txt`)
- NVIDIA GPU with CUDA 10.0+ (compute capability above 3.0)

## Configuration

Settings are read from environment variables, so no secrets are stored in the repo:

| Variable | Purpose | Default |
| --- | --- | --- |
| `DJANGO_SECRET_KEY` | Django secret key (required in production) | random per run |
| `DJANGO_DEBUG` | `1` to enable debug mode | `0` |
| `DJANGO_ALLOWED_HOSTS` | Comma-separated host names | `localhost,127.0.0.1` |

## Run locally

```bash
pip install -r requirements.txt
mkdir models uploaded_images uploaded_videos   # place trained model files in models/
python manage.py runserver
```

## Run with Docker

```bash
docker build -t deepfake-app .
docker build -t deepfake-nginx nginx
docker run --rm --gpus all --name deepfake-app -e DJANGO_SECRET_KEY=change-me deepfake-app
docker run -p 80:80 --volumes-from deepfake-app deepfake-nginx
```

## Structure

- `ml_app/`: views, forms and the prediction pipeline
- `project_settings/`: Django settings and WSGI/ASGI entry points
- `static/`: CSS, JS and face-api assets
- `templates/`: HTML templates
- `nginx/`: reverse-proxy image
