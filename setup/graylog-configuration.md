# Graylog Configuration Guide

## Overview

This document explains how Graylog was configured after deployment using Docker. It covers inputs, streams, user setup, and log ingestion.

---

## What is Graylog?

Graylog is a centralized log management and SIEM platform used to:

- Collect logs from multiple sources
- Normalize and analyze log data
- Create detection rules (events)
- Generate alerts and dashboards

---

## Core Components Configured

### 1. Inputs (Log Collection Points)

Inputs are how Graylog receives logs.

### Configured Inputs:
- GELF UDP Input (Port 12201) → used for application logs
- Syslog UDP Input (Port 514) → used for system logs

### How to create:
1. Go to: `System → Inputs`
2. Select input type (GELF UDP / Syslog UDP)
3. Click “Launch new input”
4. Set:
   - Bind address: `0.0.0.0`
   - Port: `12201` or `514`
5. Save

---

## 2. Log Streams

Streams are used to categorize logs into logical groups.

### Created Stream:
- Detection Stream
  - Filters security-related logs

### Example Stream Rule:

message contains "Failed password"


---

## 3. Indexing (Elasticsearch)

Logs are stored in Elasticsearch.

Purpose:
- Store logs efficiently
- Enable fast search queries
- Support dashboards and event queries

---

## 4. Configuration Database (MongoDB)

MongoDB is used to store:

- User settings
- Streams configuration
- Dashboards
- Event definitions

---

## 6. Log Ingestion Test

### Test Log Command (PowerShell)

```powershell
$udpClient = New-Object System.Net.Sockets.UdpClient
$endpoint = New-Object System.Net.IPEndPoint ([System.Net.IPAddress]::Parse("127.0.0.1"),12201)

$message = '{
  "version":"1.1",
  "host":"windows",
  "short_message":"Failed password for admin from 192.168.1.50",
  "level":4
}'

$bytes = [System.Text.Encoding]::UTF8.GetBytes($message)
$udpClient.Send($bytes, $bytes.Length, $endpoint)
$udpClient.Close()
```
Result

After sending logs:

Logs appear in Search

Logs are routed into Streams

Logs can be used for Events and Alerts
