# DevOps Training Notes

**Date:** **17 July 2026**

---

# Linux Basics

- Introduction to Linux
- Basic Linux Commands
  - `touch`
  - `mkdir`
  - `pwd`
  - `cd`
  - `cd ..`
  - `ls`
  - `ll`
  - `ls -la`
  - `ls -lrth`
  - `cp`
  - `mv`
  - `rm -rf`
  - `cat`
  - `head`
  - `tail`
  - `tail -n`
  - `du -sh`
- Basic Vim/Vi Editor
  - `Esc`
  - `:wq` (Save & Exit)
  - `:q!` (Exit Without Saving)

---

# Docker Basics

- What is Docker?
- Why Docker?
- Virtualization vs Containerization
- Docker Architecture
- Docker Desktop Installation
- WSL Installation
- Docker Hub
- Pull Docker Images
- Run Docker Containers
- Create Docker Hub Account

### Common Commands

```bash
docker version
docker info
docker images
docker pull <image>
docker run -d <image>
docker ps
docker ps -a
docker stop <container>
docker rm <container>
docker rmi <image>
docker login
```

---

# Python Application

- Run Python Application (Manual)
- Access in Browser
- Dockerize Application
- Create Dockerfile
- Build Docker Image
- Run Docker Container
- Access in Browser
- Push Image to Docker Hub
- Pull & Run Image in Another Environment

### Commands

```bash
python app.py

docker build -t python-app .

docker run -d -p 5000:5000 python-app

docker tag python-app <username>/python-app:v1

docker push <username>/python-app:v1

docker pull <username>/python-app:v1
```

---

# Apache2 Application

- Run Apache2 (Manual)
- Access in Browser
- Dockerize Apache2
- Build Docker Image
- Run Docker Container
- Access in Browser
- Push Image to Docker Hub
- Pull & Run Image in Another Environment

### Commands

```bash
sudo apt install apache2

sudo systemctl start apache2

docker build -t apache-app .

docker run -d -p 8080:80 apache-app

docker tag apache-app <username>/apache-app:v1

docker push <username>/apache-app:v1

docker pull <username>/apache-app:v1
```

---

# Docker Features

## Port Mapping

```bash
docker run -p HOST_PORT:CONTAINER_PORT <image>
```

Example

```bash
docker run -d -p 8080:80 nginx
```

---

## Volume Mapping

```bash
docker run -v HOST_PATH:CONTAINER_PATH <image>
```

Example

```bash
docker run -d -v /home/user/data:/var/www/html nginx
```

---

## Docker Networking

```bash
docker network create mynetwork

docker network ls

docker network inspect mynetwork

docker network rm mynetwork
```

Run Container in Network

```bash
docker run -d --network mynetwork mysql

docker run -d --network mynetwork wordpress
```

---

# Quick Revision

## Linux

- Linux Commands
- Vim Editor

## Docker

- Images
- Containers
- Docker Hub
- Dockerfile

## Applications

- Python
- Apache2

## Docker Concepts

- Port Mapping
- Volume Mapping
- Networking

---








# Docker, Docker Compose & Kubernetes (K8s) Notes

# YAML Basics

YAML (YAML Ain't Markup Language) is a human-readable data serialization format used to represent configuration data.

In Kubernetes, YAML files are used to define Kubernetes objects such as Pods, Deployments, Services, ConfigMaps, etc.

---

## Example Data

Server Details

| Name | Owner | Created | Status |
|------|--------|----------|--------|
| insta_server | Madhu | 18-07-2026 | Active |
| facebook | Kiran | 18-07-2026 | Active |

---

# XML Representation

```xml
<servers>
    <server>
        <name>insta_server</name>
        <owner>Madhu</owner>
        <created>18-07-2026</created>
        <status>active</status>
    </server>

    <server>
        <name>facebook</name>
        <owner>Kiran</owner>
        <created>18-07-2026</created>
        <status>active</status>
    </server>
</servers>
```

---

# JSON Representation

```json
{
  "servers": [
    {
      "name": "insta_server",
      "owner": "Madhu",
      "created": "18-07-2026",
      "status": "active"
    },
    {
      "name": "facebook",
      "owner": "Kiran",
      "created": "18-07-2026",
      "status": "active"
    }
  ]
}
```

---

# YAML Representation

```yaml
servers:
  - name: insta_server
    owner: Madhu
    created: 18-07-2026
    status: active

  - name: facebook
    owner: Kiran
    created: 18-07-2026
    status: active
```

---

# YAML Data Types

## 1. Key-Value Pair

```yaml
name: server1
```

---

## 2. List / Array

```yaml
fruits:
  - grape

  - banana

  - orange:
      carbs: 5g
      fat: 0.1g
      calories: 50

  - apple:
      carbs: 5g
      fat: 0.1g
      calories: 50
```

---

## 3. Dictionary (Map)

```yaml
orange:
  calories: 50
  fat: 0.1g
  carbs: 5g
```

---

# YAML Rules

- YAML is indentation-sensitive.
- Use spaces only (avoid tabs).
- Maintain equal indentation for the same level.
- Keys and values are separated using `:`.
- Lists begin with `-`.

Example

```yaml
servers:
  - name: whatsapp
    owner: Kiran
    created: 2025-10-13
    status: active

  - name: insta
    owner: Madhu
    created: 2025-10-13
    status: active
```

---

# Docker Compose

## What is Docker Compose?

Docker Compose is a tool used to define and manage multiple Docker containers using a single YAML file (`docker-compose.yml`).

Instead of executing multiple `docker run` commands, everything is defined in one configuration file.

---

## Why Docker Compose?

Without Docker Compose

```bash
docker run frontendimg
docker run backendimg
docker run db_image
```

Difficult to manage multiple containers.

With Docker Compose

```bash
docker compose up -d
```

One command starts all required containers.

---

# Real-Time Example

A WordPress application requires:

- WordPress
- MySQL Database

Docker Compose

- Starts both containers
- Creates the network
- Creates persistent volumes
- Connects services automatically

---

# Docker Compose Installation

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose version
```

---

# docker-compose.yml

```yaml
services:
  db:
    image: mysql:5.7
    container_name: wordpress_db
    restart: always

    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app

    depends_on:
      - db

    restart: always

    ports:
      - "8000:80"

    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress

    volumes:
      - wordpress_data:/var/www/html

volumes:
  db_data:
  wordpress_data:
```

---

# Docker Compose Commands

Start containers

```bash
docker compose up -d
```

Stop containers

```bash
docker compose down
```

Remove containers and volumes

```bash
docker compose down --volumes
```

View running containers

```bash
docker compose ps
```

View logs

```bash
docker compose logs
```

Restart services

```bash
docker compose restart
```

Build images

```bash
docker compose build
```

---

# Advantages

- Manage multiple containers
- Single configuration file
- Easy deployment
- Automatic networking
- Volume management
- Easy scaling
- Ideal for local development

---

# Limitations

- Mainly designed for a single host
- Not suitable for enterprise production
- No advanced orchestration
- No automatic self-healing
- Limited auto-scaling

---

# Summary

Docker

→ Manages a single container

Docker Compose

→ Manages multiple related containers

```bash
docker compose up -d
docker compose down --volumes
```

---

# Kubernetes (K8s)

## What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

Docker creates containers.

Kubernetes manages containers.

---

# Why Kubernetes?

Managing hundreds or thousands of containers manually is difficult.

Kubernetes automates

- Deployment
- Scaling
- Load Balancing
- Self Healing
- Rolling Updates

---

# Features

- Automatic Deployment
- Self Healing
- Auto Scaling
- Load Balancing
- Service Discovery
- Rolling Updates
- Rollback
- High Availability

---

# Advantages

- Production Ready
- Easy Scaling
- Zero Downtime Deployment
- Automatic Recovery
- Better Resource Utilization
- Cloud Independent

---

# Real-Time Example

Amazon receives one million users during a sale.

Kubernetes automatically

- Starts additional containers
- Balances traffic
- Replaces failed containers
- Removes extra containers after traffic decreases

---

# Summary

Docker

↓

Creates Containers

Kubernetes

↓

Manages Containers

---

# Problems with Docker & Docker Compose

## Docker Limitations

Docker is excellent for

- Building Images
- Running Containers

However, Docker lacks

- Auto Scaling
- Advanced Scheduling
- Enterprise Orchestration
- Automatic Load Balancing
- Multi-node Management

---

## Docker Compose Limitations

Docker Compose works well for

- Development
- Small Applications

Not suitable for

- Large Production
- Multiple Servers
- Enterprise Clusters

---

# Container Orchestration

Container orchestration means automatically managing containers.

Examples

- Deployment
- Scaling
- Restart Failed Containers
- Load Balancing
- Scheduling

---

# Why Kubernetes Became the Industry Standard

Because it provides

- High Availability
- Self Healing
- Auto Scaling
- Rolling Updates
- Better Scheduling
- Large Ecosystem

---

# Kubernetes Architecture

```
                     Control Plane
                           │
       -----------------------------------------
       │         │          │          │
   API Server  Scheduler Controller    ETCD
                          Manager
       -----------------------------------------
                Worker Nodes
       -----------------------------------------
       │           │             │
    kubelet    kube-proxy   Container Runtime
           │
          Pods
```

---

# Control Plane

The brain of Kubernetes.

Responsibilities

- Cluster Management
- Scheduling
- Maintaining Desired State

---

# Worker Node

Runs

- Pods
- Containers

Applications execute here.

---

# API Server

Main entry point of Kubernetes.

Example

```bash
kubectl get pods
```

↓

API Server

---

# Scheduler

Responsible for selecting the best worker node.

Checks

- CPU
- Memory
- Available Resources

---

# Controller Manager

Continuously monitors the cluster.

If a Pod fails,

A replacement Pod is automatically created.

Maintains the desired state.

---

# ETCD

Distributed key-value database.

Stores

- Cluster Information
- Nodes
- Pods
- Secrets
- Configurations

---

# Kubelet

Runs on every worker node.

Responsibilities

- Communicates with API Server
- Starts Pods
- Monitors Pods

---

# Kube-Proxy

Responsible for networking.

Functions

- Service Communication
- Load Balancing

---

# Container Runtime

Software responsible for running containers.

Examples

- containerd
- CRI-O

Earlier

- Docker Engine

---

# Kubernetes Workflow

```
Developer

      │

kubectl apply

      │

API Server

      │

Scheduler

      │

Worker Node Selected

      │

Kubelet

      │

Container Runtime

      │

Pod Created
```

---

# Quick Revision

## Docker

- Build Images
- Run Containers

## Docker Compose

- Manage Multiple Containers
- YAML-Based Configuration
- Best for Local Development

## Kubernetes

- Container Orchestration
- Auto Scaling
- Self Healing
- High Availability
- Production Ready

---
