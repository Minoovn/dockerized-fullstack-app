# Dockerized Full-Stack Application

A containerized full-stack application built to demonstrate practical Docker workflows, multi-container architecture, environment-specific configuration, and automated Docker image builds using GitHub Actions.

The application consists of an Nginx-based frontend, a Python Flask REST API, and a MySQL database. Docker Compose is used to orchestrate the services in both development and production environments.

## Overview

This project explores how the different layers of a web application can be containerized and managed as independent services.

The architecture consists of three main components:

```text
Client
  |
  v
Nginx Frontend
  |
  v
Flask REST API
  |
  v
MySQL Database
```

Each component runs in its own container, allowing the application to be developed, built, and operated consistently across environments.

## Tech Stack

### Backend

- Python
- Flask
- MySQL Connector
- REST API

### Frontend

- HTML
- Nginx

### Database

- MySQL

### DevOps & Infrastructure

- Docker
- Docker Compose
- Docker Hub
- GitHub Actions
- Docker Buildx
- GitHub Secrets

## Architecture

The application follows a simple multi-container architecture:

```text
┌─────────────────────┐
│      Frontend       │
│       Nginx         │
│      Port 8080      │
└──────────┬──────────┘
           │
           v
┌─────────────────────┐
│       Backend       │
│    Python / Flask   │
│      Port 8000      │
└──────────┬──────────┘
           │
           v
┌─────────────────────┐
│      Database       │
│       MySQL         │
│  Persistent Volume  │
└─────────────────────┘
```

Docker Compose manages the services and their dependencies.

The backend communicates with MySQL using environment-based database configuration, while the frontend is served through Nginx.

## Docker Configuration

The project contains separate Docker Compose configurations for development and production.

### Development

`docker-compose.dev.yml` builds the frontend and backend images locally.

The development environment includes:

- MySQL container
- Flask backend container
- Nginx frontend container
- Persistent database volume
- MySQL health check
- Service dependency management
- Environment-based database configuration

The backend waits for the database health check before starting.

### Production

`docker-compose.prod.yml` uses pre-built frontend and backend Docker images instead of building them locally.

This demonstrates the separation between local development and image-based production configuration.

## Automated Image Build Pipeline

The repository includes a GitHub Actions workflow that runs when changes are pushed to the `main` branch.

```text
Push to main
      |
      v
GitHub Actions
      |
      v
Checkout Repository
      |
      v
Set Up Docker Buildx
      |
      v
Authenticate with Docker Hub
      |
      +-------------------+
      |                   |
      v                   v
Build Backend        Build Frontend
Docker Image         Docker Image
      |                   |
      v                   v
Push to Docker Hub   Push to Docker Hub
```

Docker Hub credentials are provided through GitHub repository secrets rather than being stored directly in the workflow.

The workflow automatically builds and publishes separate Docker images for the frontend and backend.

## Backend API

The backend is implemented using Flask.

### Health Check

```http
GET /api/health
```

Example response:

```json
{
  "status": "ok"
}
```

### Database Endpoint

```http
GET /api
```

The endpoint connects to the MySQL database, executes a query, and returns the result as a JSON response.

This verifies communication between the backend and database containers.

## Running Locally

### Prerequisites

Make sure the following are installed:

- Docker
- Docker Compose

Clone the repository:

```bash
git clone https://github.com/Minoovn/dockerized-fullstack-app.git
cd dockerized-fullstack-app
```

Configure the required environment variables for the database:

```text
MYSQL_ROOT_PASSWORD
DB_HOST
DB_USER
DB_PASSWORD
DB_NAME
```

Start the development environment:

```bash
docker compose -f docker-compose.dev.yml up --build
```

Docker Compose will build and start the application services.

Once running, the frontend is exposed on:

```text
http://localhost:8080
```

The backend is exposed on:

```text
http://localhost:8000
```

To stop the containers:

```bash
docker compose -f docker-compose.dev.yml down
```

To also remove the persistent database volume:

```bash
docker compose -f docker-compose.dev.yml down -v
```

## Production Configuration

The production Compose configuration uses published Docker images for the frontend and backend.

Run it with:

```bash
docker compose -f docker-compose.prod.yml up -d
```

This configuration pulls the required application images and starts the multi-container environment.

## Project Structure

```text
dockerized-fullstack-app/
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── backend/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
│
├── db/
│   └── init/
│
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   └── nginx.conf
│
├── docker-compose.dev.yml
├── docker-compose.prod.yml
├── .gitignore
└── README.md
```

## What This Project Demonstrates

This project demonstrates hands-on experience with:

- Containerizing frontend and backend services
- Building multi-container applications
- Docker Compose orchestration
- Creating Dockerfiles for different application services
- Connecting Flask applications to MySQL
- Configuring Nginx inside a containerized architecture
- Managing persistent database storage
- Using container health checks
- Managing service dependencies
- Separating development and production configurations
- Using environment variables for configuration
- Building Docker images with GitHub Actions
- Authenticating CI workflows using GitHub Secrets
- Publishing Docker images to Docker Hub

## Learning Outcomes

Through this project, I gained practical experience with containerizing and orchestrating a multi-service application rather than running each component directly on the host machine.

I also gained experience with automated Docker image builds and learned how GitHub Actions can be integrated into a container-based development workflow.

The project helped strengthen my understanding of how frontend, backend, database, containerization, and automation fit together in a modern application environment.

## Project Context

This project was developed as part of my studies at Oulu University of Applied Sciences and has been maintained as part of my software engineering portfolio.

Its primary focus is containerization, service orchestration, and build automation rather than application-level feature complexity.

## Author

**Minou Vahedinezhad**

Software Engineering | Full-Stack Development | AI Engineering

GitHub: https://github.com/Minoovn  
LinkedIn: https://www.linkedin.com/in/minou-vahedinezhad-10a852268
