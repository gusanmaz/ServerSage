# Docker on VPS Guide

This guide covers using Docker for containerization on a VPS.

## Table of Contents

1. [Installation](#installation)
2. [Basic Docker Commands](#basic-docker-commands)
3. [Docker Compose](#docker-compose)
4. [Docker Networking](#docker-networking)
5. [Docker Volumes](#docker-volumes)
6. [Best Practices](#best-practices)

## Installation

1. Update packages:
   ```bash
   sudo apt update
   ```

2. Install prerequisites:
   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

3. Add Docker's official GPG key:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. Add Docker repository:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. Install Docker:
   ```bash
   sudo apt update
   sudo apt install docker-ce
   ```

6. Verify installation:
   ```bash
   sudo docker run hello-world
   ```

## Basic Docker Commands

- List running containers:
  ```bash
  docker ps
  ```

- List all containers:
  ```bash
  docker ps -a
  ```

- Run a container:
  ```bash
  docker run -d --name mycontainer nginx
  ```

- Stop a container:
  ```bash
  docker stop mycontainer
  ```

- Remove a container:
  ```bash
  docker rm mycontainer
  ```

- List images:
  ```bash
  docker images
  ```

## Docker Compose

1. Install Docker Compose:
   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   ```

2. Create a `docker-compose.yml` file:
   ```yaml
   version: '3'
   services:
     web:
       image: nginx
       ports:
         - "80:80"
     db:
       image: mysql
       environment:
         MYSQL_ROOT_PASSWORD: example
   ```

3. Run with Docker Compose:
   ```bash
   docker-compose up -d
   ```

## Docker Networking

- Create a network:
  ```bash
  docker network create mynetwork
  ```

- Run a container on a network:
  ```bash
  docker run -d --name mycontainer --network mynetwork nginx
  ```

## Docker Volumes

- Create a volume:
  ```bash
  docker volume create myvolume
  ```

- Use a volume:
  ```bash
  docker run -d --name mycontainer -v myvolume:/app nginx
  ```

## Best Practices

1. Use official images when possible
2. Minimize the number of layers in your Dockerfile
3. Use multi-stage builds for smaller final images
4. Don't run containers as root
5. Use Docker Compose for multi-container applications
6. Regularly update your Docker images
7. Use health checks in your Dockerfiles

Remember to secure your Docker installation by following Docker's security best practices.