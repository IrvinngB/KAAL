# KAAL — Deployment Guide

## Option 1: Docker Compose (VPS)

The simplest way to deploy everything on a single VPS.

### Prerequisites
- A VPS with Docker and Docker Compose installed
- A domain pointing to your VPS IP (optional, for HTTPS)

### Deploy

```bash
# Clone the repo
git clone <your-repo-url> kaal
cd kaal

# Build and start
docker compose up -d --build

# Check logs
docker compose logs -f
```

The app will be available at `http://your-vps-ip`.

### With HTTPS (using Caddy)

Replace the `docker-compose.yml` ports section and add Caddy:

```yaml
services:
  backend:
    build:
      context: .
      dockerfile: Dockerfile.backend
    container_name: kaal-backend
    restart: unless-stopped
    expose:
      - "8000"

  frontend:
    build:
      context: .
      dockerfile: Dockerfile.frontend
    container_name: kaal-frontend
    restart: unless-stopped
    expose:
      - "80"

  caddy:
    image: caddy:alpine
    container_name: kaal-caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - frontend

volumes:
  caddy_data:
  caddy_config:
```

Create `Caddyfile`:
```
your-domain.com {
    reverse_proxy frontend:80
}
```

---

## Option 2: Vercel (Frontend) + Container Platform (Backend)

### Frontend on Vercel

1. Push your repo to GitHub
2. Go to [vercel.com](https://vercel.com) → New Project
3. Select your repo
4. Set **Root Directory** to `mors-frontend`
5. Build settings are auto-detected from `vercel.json`
6. **Important**: Update `vercel.json` rewrites to point to your backend URL

```json
{
  "rewrites": [
    {
      "source": "/game/:path*",
      "destination": "https://your-backend-url.com/game/:path*"
    }
  ]
}
```

### Backend on Railway / Render / Fly.io

#### Railway
1. Push repo to GitHub
2. Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub
3. Set root directory to `mors-backend`
4. Set start command: `uvicorn app.main:api --host 0.0.0.0 --port $PORT`
5. Railway auto-detects Python and installs from `requirements.txt`

#### Render
1. New Web Service → connect GitHub repo
2. Root directory: `mors-backend`
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn app.main:api --host 0.0.0.0 --port $PORT`

#### Fly.io
```bash
# From repo root
fly launch --name kaal-backend --region gru

# Update generated fly.toml:
[build]
  dockerfile = "Dockerfile.backend"

fly deploy
```

---

## Option 3: Docker on VPS (manual)

```bash
# Build images
docker build -t kaal-backend -f Dockerfile.backend .
docker build -t kaal-frontend -f Dockerfile.frontend .

# Run backend
docker run -d --name kaal-backend -p 8000:8000 kaal-backend

# Run frontend (update API_BASE in .env or rebuild with backend URL)
docker run -d --name kaal-frontend -p 80:80 kaal-frontend
```

---

## Environment Variables

### Backend
| Variable | Default | Description |
|---|---|---|
| `PYTHONUNBUFFERED` | `1` | Required for Docker logging |

### Frontend
| Variable | Default | Description |
|---|---|---|
| `VITE_API_BASE` | `http://localhost:8000` | Backend URL (set at build time) |

To change the API URL for the frontend, update `mors-frontend/src/api/game.ts`:
```ts
const API_BASE = import.meta.env.VITE_API_BASE || 'http://localhost:8000'
```

Then rebuild with:
```bash
VITE_API_BASE=https://your-backend.com docker compose up -d --build
```
