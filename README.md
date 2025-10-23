# Jenkins Blue Ocean Docker Setup

This guide provides instructions to set up Jenkins Blue Ocean using Docker.

## Prerequisites
- Docker installed on your system.
- A Docker network named `jenkins`.

## Build the Docker Image

Before running the Jenkins Blue Ocean container, build the Docker image using the following command:

```bash
docker build -t myjenkins-blueocean:2.528.1-1 .
```

This command creates a Docker image tagged as `myjenkins-blueocean:2.528.1-1` using the `Dockerfile` in the current directory.

## Steps to Run Jenkins Blue Ocean

1. **Run the Jenkins Blue Ocean Docker Container**

   ```bash
   docker run \
     --name jenkins-blueocean \
     --restart=on-failure \
     --detach \
     --network jenkins \
     --env DOCKER_HOST=tcp://docker:2376 \
     --env DOCKER_CERT_PATH=/certs/client \
     --env DOCKER_TLS_VERIFY=1 \
     --publish 8080:8080 \
     --publish 50000:50000 \
     --volume jenkins-data:/var/jenkins_home \
     --volume jenkins-docker-certs:/certs/client:ro \
     myjenkins-blueocean:2.528.1-1
   ```

   - `--name`: Assigns a name to the container.
   - `--restart`: Automatically restarts the container on failure.
   - `--detach`: Runs the container in the background.
   - `--network`: Connects the container to the `jenkins` network.
   - `--env`: Sets environment variables for Docker host and TLS configuration.
   - `--publish`: Maps ports 8080 and 50000 to the host machine.
   - `--volume`: Mounts volumes for Jenkins data and Docker certificates.

2. **Retrieve the Initial Admin Password**

   After the container is running, retrieve the initial admin password to set up Jenkins:

   ```bash
   docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
   ```

   This command outputs the password required for the initial setup of Jenkins.

## Notes
- Replace `myjenkins-blueocean:2.528.1-1` with the desired Jenkins Blue Ocean image tag if different.
- Ensure the `jenkins` network and required volumes are created before running the container.

## References
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Docker Documentation](https://docs.docker.com/)