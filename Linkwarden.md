# Configuration
Env (obligatoire)
```
NEXTAUTH_SECRET=VERY_SENSITIVE_SECRET
NEXTAUTH_URL=http://localhost:3000/api/v1/auth
POSTGRES_PASSWORD=YOUR_POSTGRES_PASSWOR
```
### Env (optionnel)
```
NEXT_PUBLIC_DISABLE_REGISTRATION=false
```
# Compose

```yml
services:
  postgres:
    image: postgres:16-alpine
    env_file: .env
    restart: always
    volumes:
      - ./postgres/data:/var/lib/postgresql/data
    networks:
      - linkwarden-network

  linkwarden:
    env_file: .env
    environment:
      - DATABASE_URL=postgresql://postgres:${POSTGRES_PASSWORD}@postgres:5432/postgres
    restart: always
    image: ghcr.io/linkwarden/linkwarden:latest 
    ports:
      - 3000:3000
    volumes:
      - ./linkwarden/data:/data/data
    depends_on:
      - postgres
    networks:
      - linkwarden-network 

networks:
  linkwarden-network:
    driver: bridge
```

# Architecture
```
usr/local/docker-compose/
├─ linkwarden_service/
│  ├─ data/
│  │  ├─ linkwarden/
│  │  ├─ postgres/
│  ├─ .env
│  ├─ docker-compose.yml
```