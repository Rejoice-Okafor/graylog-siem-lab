# Graylog SIEM Lab

## Overview

This project documents the design, deployment, and operation of a SIEM solution using Graylog in a controlled lab environment.

The objective is to emulate a Security Operations Center (SOC) workflow by ingesting log data, applying detection logic, identifying suspicious activity, and presenting actionable insights through dashboards and alerts. The lab focuses on common attack patterns and demonstrates how they can be detected using structured log analysis.

---

## Scope

This lab covers the following core SIEM functions:

- Log collection and centralization  
- Log parsing and normalization  
- Detection engineering using rule-based logic  
- Alerting and notification workflows  
- Security event visualization  

The implementation is intentionally simplified to support learning and reproducibility while maintaining alignment with real-world SOC practices.

---

## Architecture

The environment follows a standard SIEM data pipeline:

Log Sources → Graylog → Elasticsearch → Dashboards → Alerts

- Log sources generate or simulate security events  
- Graylog ingests, processes, and routes logs  
- Elasticsearch indexes and stores log data  
- Dashboards provide visibility into events  
- Alerts notify on defined detection conditions  

---

## Technology Stack

### Graylog (SIEM Platform)

Graylog is used as the central SIEM platform. Its responsibilities include:

- Log ingestion via inputs  
- Stream and pipeline processing  
- Field extraction and normalization  
- Detection rule configuration (events and alerts)  
- Dashboard creation and visualization  

---

### Elasticsearch (Data Store)

Elasticsearch is responsible for:

- Storing structured log data  
- Enabling fast search and aggregation  
- Supporting dashboard queries and analytics  

---

### MongoDB (Configuration Database)

MongoDB stores Graylog application data, including:

- System configurations  
- User accounts  
- Dashboards and metadata  

---

### Docker (Deployment)

Docker is used to deploy all services in a consistent and reproducible environment. This approach:

- Simplifies installation and setup  
- Ensures environment consistency  
- Reduces dependency conflicts  

---

## Environment Setup

### 1. Clone Repository

```bash
git clone https://github.com/YOUR-USERNAME/graylog-siem-lab.git
cd graylog-siem-lab
```

---

### 2. Deploy SIEM Stack

Set up Graylog, Elasticsearch, and MongoDB using Docker:

[Docker Setup Guide](./setup/docker-setup.md)

---

### 3. Configure Graylog

Configure inputs, streams, and basic processing:

[Graylog-Configuration](./setup/graylog-configuration.md)

---

### 4. Ingest Log Data

Simulate log generation using PowerShell scripts:

[Data-Ingestion](./data-ingestion/log-simulation.md)

---

### 5. Parse and Normalize Logs

Extract relevant fields such as IP addresses, usernames, and event types:

[Parsed Logs](./parsing/field-extraction.md)

---

### 6. Implement Detection Rules

Detection logic is implemented for the following scenarios:

- Brute force authentication attempts  
- Suspicious PowerShell execution  
- Privilege escalation activity  
- Lateral movement behavior  

Detection rule definitions:

[Detection Rules](./detection-rules/)

---

### 7. Configure Alerts

Define alert conditions and notification channels:

[Alerts](./alerts/notifications.md)

---

### 8. Build Monitoring Dashboard

Create dashboards to monitor security events in near real-time:

[Dahboards](./dashboards/soc-dashboard.md)

---

## Attack Simulation Scenarios

To validate detection capabilities, the following attack techniques are simulated:

- Repeated failed authentication attempts (brute force)  
- Unauthorized privilege escalation  
- Execution of suspicious PowerShell commands  
- Lateral movement between systems  

These simulations generate log data used to test detection accuracy and alerting behavior.

---

## Detection Strategy

The detection approach in this lab includes:

- Threshold-based rules (e.g., multiple failed logins within a time window)  
- Aggregation and grouping (e.g., events grouped by source IP)  
- Time-based correlation of related events  

This reflects foundational detection engineering practices used in SOC environments.

---

## Dashboard and Monitoring

The SOC dashboard provides visibility into key metrics, including:

- Top source IP addresses generating events  
- Authentication failure trends over time  
- Event severity distribution  
- Active alerts and triggered detections  

---

## Validation

Detection rules were validated using simulated attack data to ensure:

- Alerts trigger under expected conditions  
- False positives are minimized  
- Relevant fields are correctly extracted and usable  

---

## Future Enhancements

Planned improvements to extend the lab include:

- Mapping detections to MITRE ATT&CK techniques  
- Integration with threat intelligence feeds  
- GeoIP enrichment for enhanced context  
- Automated response using SOAR workflows  

---

## Author

Rejoice Okafor
