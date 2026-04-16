# Docker Setup for Graylog SIEM Lab

## Overview

This document explains how the SIEM environment was deployed using Docker.

The setup includes:

- Graylog (SIEM platform)
- Elasticsearch (log storage & search engine)
- MongoDB (configuration database)

---

## Why Docker?

Docker is used because:

- It removes manual installation complexity
- Ensures all services run in isolated containers
- Makes the environment reproducible
- Matches real-world DevSecOps practices

---

## Architecture

Graylog → Elasticsearch → MongoDB

- Graylog: Processes and analyzes logs
- Elasticsearch: Stores and indexes logs
- MongoDB: Stores configurations and metadata

---

## docker-compose.yml

```yaml
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
      GRAYLOG_ROOT_PASSWORD_SHA2: "<your_password_hash>"
      GRAYLOG_HTTP_EXTERNAL_URI: "http://127.0.0.1:9000/"
    depends_on:
      - mongo
      - elasticsearch
    ports:
      - "9000:9000"
      - "1514:1514/udp"
      - "12201:12201/udp"
    restart: always
