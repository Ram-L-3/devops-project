# DevOps Project: Flask App with CI/CD

A Flask web app with two CI/CD pipelines (GitHub Actions and Jenkins),
containerized with Docker and developed on WSL2 (Ubuntu).

## Architecture

(insert draw.io diagram here)

Developer → GitHub → Jenkins → Docker image → Deploy → Health check

## Jenkins pipeline stages

1. **Checkout**: pull the latest code from GitHub
2. **Install**: install Python dependencies
3. **Test**: run the automated tests
4. **Build image**: build the Docker image
5. **Deploy**: run the container
6. **Health check**: verify the app responds

## GitHub Actions

Runs on every push: installs dependencies and runs tests.

## Run locally

    git clone https://github.com/Ram-L-3/devops-project.git
    cd devops-project
    docker build -t devops-project .
    docker run -p 5000:5000 devops-project

## Tech stack

Python, Flask, Docker, Jenkins, GitHub Actions, WSL2
