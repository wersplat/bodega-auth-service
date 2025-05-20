# bodega-auth-service

A FastAPI-based authentication service for esports-platform, providing Discord OAuth2 login and JWT-based authentication.

---

## Table of Contents
- [Features](#features)
- [Setup](#setup)
- [Environment Variables](#environment-variables)
- [Dependencies](#dependencies)
- [Docker](#docker)
- [Endpoints](#endpoints)
- [Project Structure](#project-structure)

---

## Features
- Discord OAuth2 authentication
- JWT token issuance
- User management with SQLAlchemy
- Health check endpoint

## Setup
1. Copy `.env.example` to `.env` and fill in the values:
    - `DATABASE_URL`
    - `DISCORD_CLIENT_ID`
    - `DISCORD_CLIENT_SECRET`
    - `DISCORD_REDIRECT_URI`
    - `JWT_SECRET`
2. Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3. Run the application locally:
    ```bash
    uvicorn app.main:app --reload
    ```

## Environment Variables
- `DATABASE_URL` - PostgreSQL connection string
- `DISCORD_CLIENT_ID` - Discord application's client ID
- `DISCORD_CLIENT_SECRET` - Discord application's client secret
- `DISCORD_REDIRECT_URI` - Redirect URI for Discord OAuth2
- `JWT_SECRET` - Secret key for JWT token signing

## Dependencies
Key dependencies from `requirements.txt`:
- fastapi
- uvicorn
- sqlalchemy
- psycopg2-binary
- authlib
- python-jose[cryptography]
- python-dotenv
- httpx
- asyncpg
- pydantic[email]
- sentry-sdk
- fastapi.security

## Docker
You can run the service with Docker:
```bash
docker build -t bodega-auth-service .
docker run -p 8000:8000 --env-file .env bodega-auth-service
```

## Endpoints
- `GET /auth/login/discord` - Get Discord OAuth2 login URL
- `GET /auth/callback/discord` - Discord OAuth2 callback, returns JWT
- `GET /auth/me` - Get current user info (mocked for now)
- `GET /auth/logout` - (Not implemented)
- `GET /health` - Health check

## Project Structure
```
app/
├── main.py         # FastAPI app and router registration
├── config.py       # Loads environment variables
├── database.py     # SQLAlchemy setup and DB session
├── models.py       # SQLAlchemy models (User)
├── schemas.py      # Pydantic schemas (UserOut)
├── routes/
│   ├── auth.py     # Auth endpoints (Discord OAuth, JWT)
│   └── health.py   # Health check endpoint
├── oauth/
│   └── discord.py  # Discord OAuth helper functions
```

---

## Notes
- The service expects a PostgreSQL database.
- Discord OAuth2 is used for authentication; after login, a JWT is issued.
- Health check endpoint is available at `/health`.
- For production, configure all secrets securely and use HTTPS for all endpoints.