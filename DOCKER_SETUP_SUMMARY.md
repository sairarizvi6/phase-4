# Docker Setup for Todo App

## Overview
This document summarizes the Docker setup for the Todo application with both frontend and backend services.

## Files Created

### 1. Frontend Dockerfile (`frontend/Dockerfile`)
- Multi-stage build for optimized image size
- Uses Node.js 20-alpine base image
- Implements standalone Next.js build output
- Creates non-root user for security
- Includes proper .dockerignore file

### 2. Backend Dockerfile (`backend/Dockerfile`)
- Multi-stage build for optimized image size
- Uses Python 3.11 base image
- Installs necessary build dependencies for Python packages
- Creates non-root user for security
- Includes proper .dockerignore file

### 3. Docker Compose File (`docker-compose.yml`)
- Defines services for frontend, backend, and PostgreSQL database
- Sets up proper networking between services
- Configures environment variables for both services
- Defines volumes for persistent database storage

## How to Run

### Using Docker Compose (Recommended)
```bash
# Build and start all services
docker-compose up --build

# Or run in detached mode
docker-compose up --build -d
```

### Individual Services
```bash
# Build frontend image
cd frontend
docker build -t todo-frontend .

# Build backend image
cd backend
docker build -t todo-backend .

# Run containers
docker run -p 3000:3000 todo-frontend
docker run -p 8000:8000 todo-backend
```

## Environment Variables

### Backend
The backend service requires the following environment variables:
- `DATABASE_URL`: PostgreSQL connection string
- `BETTER_AUTH_SECRET`: Secret key for JWT authentication
- `COHERE_API_KEY`: API key for Cohere integration (optional)

### Frontend
The frontend service uses:
- `NEXT_PUBLIC_API_URL`: URL for the backend API
- `NEXT_PUBLIC_BETTER_AUTH_URL`: URL for authentication service

## Architecture
- **Frontend**: Next.js 16+ application with standalone output
- **Backend**: FastAPI application with SQLModel and PostgreSQL
- **Database**: PostgreSQL 15 for data persistence
- **Network**: Custom bridge network for service communication

## Security Features
- Non-root users in both frontend and backend containers
- Proper environment variable handling
- Isolated network for service communication
- Standalone Next.js build for reduced attack surface