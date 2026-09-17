# Docker + Nginx CI/CD Pipeline

## Overview
A simple Nginx web application containerized with Docker and integrated with GitHub Actions for automated CI/CD. The pipeline validates project files, builds the Docker image, runs 5 automated tests, and generates a pipeline report.

## Project Structure
```
├── Dockerfile                    # Docker image definition
├── index.html                    # Static web page served by Nginx
├── docker-compose.yml            # Docker Compose for local development
├── .dockerignore                 # Files excluded from Docker build
├── README.md                     # Project documentation
└── .github/
    └── workflows/
        └── ci.yml                # GitHub Actions CI pipeline (4 jobs)
```

## Steps to Run the Application Locally

### Prerequisites
- Docker Desktop installed and running
- Git installed

### 1. Clone the Repository
```bash
git clone https://github.com/Adityuh1/docker-nginx-ci.git
cd docker-nginx-ci
```

### 2. Build the Docker Image
```bash
docker build -t nginx-app:latest .
```

### 3. Run the Docker Container
```bash
docker run -d -p 8080:80 --name my-nginx nginx-app:latest
```

### 4. Verify in Browser
Open your browser and navigate to: **http://localhost:8080**

You should see: _"Hello from Nginx + Docker!"_

### 5. Verify via Command Line
```bash
docker ps                    # Container should show status "Up"
curl http://localhost:8080   # Should return HTML content
```

### 6. Stop and Remove the Container
```bash
docker stop my-nginx
docker rm my-nginx
```

### Alternative: Run with Docker Compose
```bash
docker-compose up -d         # Start in detached mode
docker-compose ps            # Check status
docker-compose down          # Stop and remove
```

## GitHub Actions CI Pipeline

The CI pipeline (`.github/workflows/ci.yml`) runs automatically on every push to `main` and consists of **4 sequential jobs**:

| Job | What It Does | Depends On |
|-----|-------------|------------|
| **Validate Project Files** | Checks all required files exist and validates Dockerfile instructions (FROM, COPY, EXPOSE) | — |
| **Build Docker Image** | Builds the Docker image, reports image size, and uploads it as an artifact | Validate |
| **Test Docker Container** | Downloads the built image, runs 5 automated tests on the container | Build |
| **Pipeline Report** | Generates a summary report with pass/fail status for all jobs | All jobs |

### Automated Tests Performed
1. Container is running
2. HTTP 200 response check
3. HTML content validation
4. Response time under 2 seconds
5. Content-Type header verification

## Technologies Used
- **Nginx** (Alpine) — Lightweight web server
- **Docker** — Containerization
- **Docker Compose** — Local container orchestration
- **GitHub Actions** — CI/CD automation
