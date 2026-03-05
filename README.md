# InternAge Internship Demo Repo

A demo project showcasing a containerized HTML weather app (**Skycast**) deployed to the GitHub Container Registry (GHCR) using GitHub Actions.

---

## 🌤️ Project Overview

**Skycast** is a lightweight, single-file weather app built with vanilla HTML, CSS, and JavaScript. It fetches live weather data from the OpenWeatherMap API and displays current conditions plus a 5-day forecast for any city in the world.

This repo demonstrates a simple but complete CI/CD pipeline: every push to `main` automatically builds a Docker image and publishes it to GHCR — no manual steps required.

---

## 🐳 Pull & Run the Docker Image

Make sure you have [Docker](https://www.docker.com/get-started) installed, then run:

```bash
# Pull the latest image
docker pull ghcr.io/astrolight23/internage_internship_demorepo:latest

# Run the container
docker run -d -p 8080:80 ghcr.io/astrolight23/internage_internship_demorepo:latest
```

Then open your browser and visit:

```
http://localhost:8080
```

> **Note for Apple Silicon (M-series) Mac users:** You may see a platform mismatch warning (`linux/amd64` vs `linux/arm64`). The container will still run correctly via emulation.

### Useful Docker Commands

```bash
# List running containers
docker ps

# Stop the container
docker stop <CONTAINER_ID>

# Run and auto-remove on exit
docker run --rm -p 8080:80 ghcr.io/astrolight23/internage_internship_demorepo:latest
```

---

## ⚙️ GitHub Actions CI/CD

Every push to the `main` branch triggers the workflow defined in `.github/workflows/deploy.yml`, which:

1. **Checks out** the repository code
2. **Logs in** to GHCR using the automatically provided `GITHUB_TOKEN` (no secrets setup needed)
3. **Extracts metadata** — generates tags like `latest` and a SHA-based tag for traceability
4. **Builds and pushes** the Docker image to `ghcr.io/astrolight23/internage_internship_demorepo`

```yaml
name: Deploy to GHCR

on:
  push:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: astrolight23/internage_internship_demorepo

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=sha
            type=raw,value=latest

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

### Image Tags

| Tag | Description |
|-----|-------------|
| `latest` | Most recent build from `main` |
| `sha-xxxxxxx` | Pinned to a specific commit SHA |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | HTML, CSS, JavaScript |
| Server | nginx (Alpine) |
| Container | Docker |
| Registry | GitHub Container Registry (GHCR) |
| CI/CD | GitHub Actions |
