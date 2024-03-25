[![Docker Image CI](https://github.com/deegree/deegree3-docker/actions/workflows/docker-image.yaml/badge.svg)](https://github.com/lat-lon/deegree3-containers/blob/main/.github/workflows/docker-image.yaml)

# Supported tags and respective `Dockerfile` links

- deegree 3.5.x (JDK 17/Tomcat 9): `3.5.6`, `3.5`, `latest` - [Dockerfile](https://github.com/lat-lon/deegree3-containers/blob/main/3.5/Dockerfile)

# Quick reference

## lat/lon container with deegree webservices
This repository contains Dockerfiles for [deegree web services](https://www.deegree.org/) to build container images with ready-to-use deegree webservices based on the Bitnami's Apache Tomcat package.

- The containers are continuously updated when new versions are made available
- Security scan is enabled and automatically executed with every update
- Ready to use on production
- Ready to use with Kubernetes

Do you want to move your container to a Kubernetes infrastructure? We are providing Helm charts for deegree as well! Please get in contact with us.

Please consult the [deegree documentation](https://download.deegree.org/documentation/current/html/) for further information how to
configure and use deegree webservices.

## Docker images on GitHub Container Registry

[https://hub.docker.com/r/deegree/deegree3-docker/](https://github.com/lat-lon/deegree3-containers/pkgs/container/deegree3-containers)

## How to use a non-root container

Non-root container images add an extra layer of security and are generally recommended for production environments. Because they run as a non-root user, privileged tasks are typically off-limits.

## How to use it

Use the following command to pull the latest image:

```
docker pull ghcr.io/lat-lon/deegree3-containers:latest
```

To start a docker container with the name `deegree` on port 8080 run the following command:

```
docker run -d --name deegree -p 8080:8080 ghcr.io/lat-lon/deegree3-containers:latest
```
Running the image with `-d` runs the container in detached mode, leaving the container running in the background.
The `--name` flag is setting the name for the container. The `-p` flag redirects a public port to a private port inside the container.
