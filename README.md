
# INDUPHARMA Fusion Workflows

## Overview

This repository contains the complete Fusion AI workflow definition for the **INDUPHARMA IIoT** pharmaceutical monitoring system. The workflow orchestrates real-time sensor data ingestion, GMP threshold validation, anomaly detection, alert escalation, and daily compliance reporting across the production facility.

## Architecture

The workflow is composed of 5 integrated pipelines (WF-01 through WF-05):

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| **WF-01** | MQTT Subscriber (`indupharma/mesures`) | Ingests sensor data, validates thresholds, stores measurements |
| **WF-02** | Webhook (`/wf-02-anomalie`) | Analyzes trends over 30min, classifies GMP severity via AI |
| **WF-03** | Webhook (`/wf-03-notification`) | Routes alerts by severity (Email + Slack) |
| **WF-04** | CRON (23:00 daily) | Generates GMP compliance report |
| **WF-05** | Webhook (`/api/production/status`) | Real-time dashboard API |

## Technology Stack

- **Broker**: HiveMQ Cloud (MQTT over TLS)
- **Database**: Supabase (PostgreSQL)
- **AI Classification**: Google Gemini 3.1 Flash
- **Notifications**: Gmail API + Slack API
- **Hardware**: ESP32_PHARMA_01 (BME280, IR, MPU6050)

## Key Features

- Real-time GMP threshold monitoring with configurable `seuils_config`
- Persistent anomaly tracking with `alertes` and `interventions` tables
- AI-powered severity classification (`GMP_critique`, `majeur`, `mineur`)
- Automated lot impact tracing and root cause logging
- Daily aggregated KPI reports with conformity rates

## File Structure

```
indupharma-workflows/
├── workflow.json          # Complete Fusion AI workflow export
├── README.md              # This documentation
└── docs/
    (optional)             # Additional architecture diagrams
```

## Installation

1. Import `workflow.json` into your Fusion AI workspace
2. Configure environment variables:
   - `MQTT_USER`, `MQTT_PASS`
   - `SUPABASE_HOST`, `SUPABASE_PASSWORD`
   - `GEMINI_API_KEY`
   - `GMAIL_REFRESH_TOKEN`, `SLACK_BOT_TOKEN`
3. Deploy webhooks and verify MQTT topic subscription

## License

Proprietary — INDUPHARMA Pharmaceutical Systems
