# Secure Container Build Pipeline

A GitHub Actions pipeline that runs **secret scanning, dependency scanning and container scanning** before a Docker image is allowed to reach Docker Hub. If any stage finds a HIGH or CRITICAL issue, the image is never pushed.

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?logo=nodedotjs&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

## Stages

| # | Stage | Tool | Fails build on |
|---|---|---|---|
| 1 | Secret detection | Gitleaks | Any leaked credential |
| 2 | Dependency scan | Snyk | HIGH+ vulnerabilities |
| 3 | Filesystem scan | Trivy FS | HIGH / CRITICAL |
| 4 | Build | Docker Buildx (`load: true`, local only) | Build errors |
| 5 | Image scan | Trivy Image | HIGH / CRITICAL in the OS or libraries |
| 6 | Publish | `docker push` | Runs only if 1 to 5 pass |

The image is built **locally first** and scanned before it is pushed, so a vulnerable image never reaches the registry.

## Required secrets

`DOCKERHUB_USERNAME`, `DOCKERHUB_PASSWORD`, `SNYK_TOKEN`

## Evolution

This was an early single-repo pipeline. It has since grown into the reusable, multi-scanner [devsecops-workflows](https://github.com/chandru05092001/devsecops-workflows).
