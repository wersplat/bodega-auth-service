# Bodega Auth Service

![Python](https://img.shields.io/badge/python-3.9%2B-blue?logo=python)
![Node.js](https://img.shields.io/badge/node.js-18%2B-green?logo=node.js)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)

This repository provides authentication microservices for the Bodega platform. It includes two implementations:
- **Python (FastAPI)**: `auth-service`
- **TypeScript (Node.js/Express/Prisma)**: `auth-service-ts`

Both services handle user registration, login, token issuance, and validation, and are designed for integration with Bodega's backend, frontend, and Discord bot.

--- 

## Directory Structure

```
bodega-auth-service/
├── auth-service/      # Python FastAPI implementation
├── auth-service-ts/   # TypeScript/Node.js implementation
└── README.md
```

---

## 1. Python Auth Service (`auth-service`)

### Setup
```bash
cd auth-service
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt
```

### Environment Variables
Copy `.env.example` to `.env` and fill in required values.

### Running
```bash
uvicorn app.main:app --reload
```

### Docker
```bash
docker build -t bodega-auth-service:py .
docker run --env-file .env -p 8000:8000 bodega-auth-service:py
```

---

## 2. TypeScript Auth Service (`auth-service-ts`)

### Setup
```bash
cd auth-service-ts
npm install
```

### Environment Variables
Copy `.env.example` to `.env` and fill in required values.

### Running
```bash
npm run dev
```

### Docker
```bash
docker build -t bodega-auth-service:ts .
docker run --env-file .env -p 8000:8000 bodega-auth-service:ts
```

---

## Environment Variables
Both services require environment variables for secrets and configuration. See the respective `.env.example` files for details. Common variables include:
- `JWT_SECRET` — Secret key for signing JWTs
- `DATABASE_URL` — Database connection string
- `PORT` — Port to run the service on

---

## API Overview
- **POST /register** — Register a new user
- **POST /login** — Authenticate user and return JWT
- **GET /me** — Get current user info (requires auth)
- **POST /token/validate** — Validate a JWT token

(Endpoints may vary slightly between implementations. See code for details.)

---

## Integration
- Used by Bodega backend, frontend, and Discord bot for authentication and token management.
- Designed to be run as a microservice in your deployment.

---

## License
MIT
