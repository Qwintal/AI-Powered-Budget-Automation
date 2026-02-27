# AI-Powered SMS Budget Automation
## (MacroDroid + n8n + Cloudflare Tunnel)

## Problem
- Most budgeting apps:
- Require broad device permissions
- Lock data into proprietary ecosystems
- Require manual entry, creating friction

## Goal:
- Build a privacy-first, self-hosted budgeting pipeline with full data control.

## Overview
- Captures transaction SMS locally (Android)
- Sends structured payload to a secure webhook
- Uses a local LLM (Ollama – Llama 3.1 8B) for categorization
- Stores transactions in Google Sheets (or PostgreSQL)
- Generates automatic spending breakdown charts

## Architecture
Webhook → LLM Categorization → Data Normalization → Google Sheets Append → Summary Dashboard

## Tech Stack
- MacroDroid (Android automation)
- n8n (workflow orchestration)
- Cloudflare Tunnel (secure reverse proxy)
- Ollama – Llama 3.1 8B (local LLM)
- Google Sheets (dashboard layer)

## How It Works
1.SMS received from bank. \
2.MacroDroid triggers HTTP POST request. /
3.Cloudflare Tunnel securely exposes local n8n instance.
4.n8n processes payload.
5.LLM extracts and classifies transaction.
6.Structured data appended to storage.
7.Dashboard updates automatically.

## Limitations
- Free Cloudflare Tunnel generates temporary URLs.
- Requires 24/7 runtime (n8n + Ollama).
- LLM categorization may be inaccurate.
- Cash transactions require manual entry.
- Webhook requires proper authentication to prevent abuse.
- MacroDroid setup complexity and potential battery impact.

## Future Improvements
- Replace Google Sheets with PostgreSQL.
-Add indexing and transaction deduplication.
- Integrate with Power BI for advanced analytics.
- Implement webhook authentication and request validation.
- Add rule-based pre-classification before LLM.
- Add monthly trend and anomaly detection.

## Improvements Planned
- Monthly filtering
- Bank-level aggregation
- PostgreSQL backend
- Authentication layer
- Add monthly trend charts
- Add spending alerts
