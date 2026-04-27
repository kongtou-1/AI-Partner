# AI-Partner Backend

Django backend for AI-Partner. It provides user authentication, character management, friend relationships, chat history, AI chat orchestration, ASR, TTS, and memory updates.

## Configuration

Copy the example environment file before running locally:

```powershell
Copy-Item .env.example .env
```

Fill in the required values:

```env
DJANGO_SECRET_KEY=change-me
DJANGO_DEBUG=True
DJANGO_ALLOWED_HOSTS=127.0.0.1,localhost

API_KEY=
API_BASE=
WSS_URL=
VOICE_URL=
```

Never commit `.env` or real service keys.

## Local Development

```powershell
python -m venv ..\.venv
..\.venv\Scripts\Activate.ps1
pip install -r ..\requirements.txt
python manage.py migrate
python manage.py runserver
```

Or with uv:

```powershell
uv sync
uv run python manage.py migrate
uv run python manage.py runserver
```

## Notes

- `db.sqlite3`, `media/`, generated static files, local vector stores, and `.env` files are ignored by Git.
- `DJANGO_SECRET_KEY`, model credentials, and voice service URLs should be provided through environment variables.
- Frontend configuration lives in `../frontend/src/js/config/config.js`.
