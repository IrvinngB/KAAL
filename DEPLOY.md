# KAAL — Deployment

## VPS (un solo comando)

Todo corre en un solo contenedor. Sin configurar URLs, sin DNS, sin nada.

```bash
# 1. Clonar
git clone <tu-repo> kaal && cd kaal

# 2. Levantar
docker compose up -d --build
```

Listo. Abrí `http://tu-ip-vps` en el navegador.

Nginx sirve el frontend y hace proxy automático de `/game/` al backend. No hay que configurar nada.

### Con HTTPS (dominio propio)

Si tenés un dominio apuntando al VPS, agregá Caddy a `docker-compose.yml`:

```yaml
services:
  backend:
    build: { context: ., dockerfile: Dockerfile.backend }
    container_name: kaal-backend
    restart: unless-stopped

  frontend:
    build: { context: ., dockerfile: Dockerfile.frontend }
    container_name: kaal-frontend
    restart: unless-stopped
    expose: ["80"]

  caddy:
    image: caddy:alpine
    container_name: kaal-caddy
    restart: unless-stopped
    ports: ["80:80", "443:443"]
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
    depends_on: [frontend]

volumes:
  caddy_data:
```

Creá `Caddyfile` en el root del repo:
```
tu-dominio.com {
    reverse_proxy frontend:80
}
```

Luego: `docker compose up -d --build`. Caddy obtiene el certificado HTTPS automático.

---

## Vercel (frontend) + Railway (backend)

Si querés separar las cosas:

### Backend en Railway
1. Deploy desde GitHub → root: `mors-backend`
2. Start command: `uvicorn app.main:api --host 0.0.0.0 --port $PORT`
3. Copiá la URL que te da Railway (ej: `https://kaal-backend.railway.app`)

### Frontend en Vercel
1. Deploy desde GitHub → root: `mors-frontend`
2. Environment variable: `VITE_API_BASE=https://kaal-backend.railway.app`
3. Deploy

---

## Actualizar

```bash
git pull
docker compose up -d --build
```
