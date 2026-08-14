# DevOps My App

A containerized static website with a full CI/CD pipeline, free cloud hosting, and uptime monitoring.

## Links

| Deliverable | Link |
|---|---|
| GitHub Repo (with Actions) | https://github.com/Awter12/devops-my-app |
| Docker Hub Image | https://hub.docker.com/r/bret77/devops-my-app |
| Live URL (Render) | https://devops-my-app.onrender.com |
| UptimeRobot Dashboard | https://stats.uptimerobot.com/sv1Wu8fsNz |

## How it works

The site is a static HTML/CSS page served by an `nginx:alpine` container. The Dockerfile copies the site files into nginx's default serving directory and exposes port 80.

## CI/CD Explanation

Every push to the `main` branch triggers a GitHub Actions workflow defined in `.github/workflows/deploy.yml`. 
The workflow checks out the repository, logs in to Docker Hub using credentials stored as encrypted GitHub secrets
 (`DOCKER_USERNAME` and `DOCKER_PASSWORD`), then builds the Docker image and pushes it to Docker Hub tagged as both 
 `latest` and the commit SHA. This means any change merged into `main` is automatically packaged into a new container image 
 without manual building or pushing. Render is configured to deploy from that same Docker Hub image, so once the new image is available,
  redeploying the live site is just a matter of triggering (or waiting for) Render's next pull of the `latest` tag. Combined with UptimeRobot polling the live URL every few minutes, 
  the pipeline gives an end-to-end flow from code change to a monitored, publicly running deployment — with no manual Docker commands required after the initial setup.

## Local development

\`\`\`bash
docker build -t bret77/devops-my-app .
docker run -d -p 8080:80 bret77/devops-my-app
\`\`\`
Visit `http://localhost:8080`.

## Git workflow

Changes are made on feature branches, opened as pull requests, and merged into `main` via GitHub's web UI — this keeps a clean PR history.
