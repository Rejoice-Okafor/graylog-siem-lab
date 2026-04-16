# Docker Setup for Graylog SIEM Lab

## Overview

This document outlines the deployment of a SIEM lab environment using Docker.

The stack includes:
- Graylog – Log management and analysis platform  
- Elasticsearch – Log storage and search engine  
- MongoDB – Configuration and metadata storage  

---

## Why Docker?

Docker was used for this setup to:

- Eliminate manual installation overhead  
- Ensure service isolation through containerization  
- Enable reproducibility across environments  
- Align with modern DevSecOps practices  

---

## Architecture


Graylog → Elasticsearch → MongoDB


- Graylog: Processes, analyzes, and visualizes logs  
- Elasticsearch: Stores and indexes log data  
- MongoDB: Stores application configurations and metadata  

---

## Graylog Root Password Configuration

Graylog requires the root password to be stored as a SHA-256 hash instead of plain text.

---

## Generating SHA-256 Password Hash

### Option 1: Linux / Git Bash / WSL

```bash
echo -n "Password123" | sha256sum
Option 2: PowerShell (Windows)
"Password123" | Out-File -Encoding ascii temp.txt
Get-FileHash temp.txt -Algorithm SHA256
Option 3: Online Generator (Testing Only)

Use only in non-production environments.

Example Output
ef92b778bafe771e89245b89ecbc08a44a4e166c06659911881f383d4473e94f
Docker Compose Configuration
version: '3'

services:
  mongo:
    image: mongo:5.0
    container_name: graylog-mongo
    restart: always

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch-oss:7.10.2
    container_name: graylog-elasticsearch
    environment:
      - discovery.type=single-node
    restart: always

  graylog:
    image: graylog/graylog:5.0
    container_name: graylog
    environment:
      GRAYLOG_PASSWORD_SECRET: "supersecretstring"
      GRAYLOG_ROOT_PASSWORD_SHA2: "your_generated_hash_here"
      GRAYLOG_HTTP_EXTERNAL_URI: "http://127.0.0.1:9000/"
    depends_on:
      - mongo
      - elasticsearch
    ports:
      - "9000:9000"
      - "1514:1514/udp"
      - "12201:12201/udp"
    restart: always
Starting the Environment
docker-compose up -d
Accessing Graylog

http://localhost:9000

Default Credentials
Username: admin
Password: Password123
Security Considerations
Always store passwords as SHA-256 hashes
Avoid exposing secrets in configuration files
Use environment variables or secret managers in production
This setup is intended for lab and educational purposes only
Status
Docker environment configured
Graylog stack deployed
Ready for log ingestion

---
