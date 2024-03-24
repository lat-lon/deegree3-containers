[![Docker Image CI](https://github.com/deegree/deegree3-docker/actions/workflows/docker-image.yaml/badge.svg)](https://github.com/lat-lon/deegree3-containers/blob/main/.github/workflows/docker-image.yaml)

# Supported tags and respective `Dockerfile` links

- deegree 3.5.x (JDK 17/Tomcat 9): `3.5.6`, `3.5`, `latest`) - [Dockerfile](https://github.com/lat-lon/deegree3-containers/blob/main/3.5/Dockerfile)

# Quick reference

## deegree web services on Docker
Dockerfile for [deegree web services](https://www.deegree.org/). This repository contains a ```Dockerfile``` for building Docker images containing ready-to-use deegree webservices.

Please consult the [deegree documentation](https://download.deegree.org/documentation/current/html/) for further information how to
configure and use deegree webservices. The [Docker web site](https://www.docker.com/) provides all information about Docker.

## Docker images on GitHub Container Registry

https://hub.docker.com/r/deegree/deegree3-docker/

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

### How to access deegree administration console

To access the deegree webservices console start a browser of your choice and open the URL:

http://<container_ip>:8080/deegree-webservices/, the `<container_ip>` depends on the docker networking mode.
http://localhost:8080/deegree-webservices/ should work with bridge mode.

Continue with configuration of deegree as described in the [getting started guide](https://download.deegree.org/documentation/current/html/#anchor-lightly) of the deegree webservices handbook.
