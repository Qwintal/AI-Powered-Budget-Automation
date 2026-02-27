# AI-Powered SMS Budget Automation (MacroDroid + n8n + Cloudflare Tunnel)
A self-hosted AI-powered personal finance ingestion pipeline.

## Overview
Automated budgeting pipeline that:
- Parses SMS transaction data
- Uses LLM to categorize transactions
- Stores structured financial data in PostgreSQL / Google Sheets
- Generates dynamic dashboard (Pie Chart by Category)

## Architecture
Webhook → LLM Categorization → Data Normalization → Google Sheets Append → Summary Dashboard

## Tech Stack
- n8n
- OpenAI API
- Google Sheets API
- PostgreSQL (optional)

## Data Schema
| Field | Type | Description |
|-------|------|------------|
| amount | number | Transaction amount |
| currency | string | Currency code |
| transaction_type | debit/credit |
| recipient_or_merchant | string |
| upi_ref | string |
| bank | string |
| date | ISO 8601 date |
| category | LLM classified category |

## Improvements Planned
- Monthly filtering
- Bank-level aggregation
- PostgreSQL backend
- Authentication layer
- Add monthly trend charts
- Add spending alerts

SMS → MacroDroid → n8n Webhook (Cloudflare Tunnel) 
→ LLM Categorization → Google Sheets

/n8n/workflow.json
/docs/architecture.md
/docs/data-flow.md
/README.md
/screenshots/

Problem
  Most budgeting apps:
  Require invasive permissions
  Lock data behind proprietary systems
  Lack automation flexibility

This system:
  Captures transaction SMS locally
  Sends only structured payload to webhook
  Self-hosted
  AI-categorizes transactions
  Stores data in structured format

Android SMS
   ↓
MacroDroid (Trigger + HTTP POST)
   ↓
Cloudflare Tunnel (Secure Public Endpoint)
   ↓
n8n Webhook
   ↓
LLM Categorization
   ↓
Google Sheets / PostgreSQL
   ↓
Dashboard

Tech Stack
  MacroDroid (Android automation)
  n8n (workflow orchestration)
  Cloudflare Tunnel (secure reverse proxy)
  OpenAI API (classification)
  Google Sheets API

Why Cloudflare Tunnel?
  Avoids port forwarding
  No public IP exposure
  Encrypted connection
  Free tier
  
Architecture Document (docs/architecture.md)
Why webhook instead of polling
Why tunnel instead of ngrok
Security considerations
Failure points

Tradeoffs of a privacy-first, self-hosted automation system.
1. Cloudflare Tunnel (Free Tier Constraints)
Ephemeral public URL when not using a custom domain.
Manual reconfiguration required if tunnel restarts.
No guaranteed uptime SLA.
Adds external dependency layer to a self-hosted system.
Mitigation:
Use a custom domain + persistent tunnel configuration, or deploy on a VPS with fixed endpoint.
2. MacroDroid Complexity & Battery Impact
Rule configuration is non-trivial.
Continuous SMS monitoring + background HTTP calls may increase battery usage.
Android background restrictions may interrupt execution depending on OEM policies.
Mitigation:
Whitelist app from battery optimization.
Long-term alternative: replace with a lightweight Android app using SMS BroadcastReceiver.
3. Cash Transactions Not Captured
System only captures digital transactions (SMS-based).
Cash payments require manual entry.
Results in incomplete spending analytics.
Mitigation:
Add a lightweight manual entry form (Google Form or simple frontend).
4. 24/7 Runtime Dependency
n8n must remain active.
Ollama + Llama 3.1 (8B) must be running continuously.
Hardware resource consumption (RAM/CPU).
System fails silently if local server goes down.
This is a self-hosted tradeoff.
Mitigation:
Deploy on:
Low-cost VPS
Home server with monitoring
Add health checks + auto-restart
5. LLM Classification Errors
Category accuracy depends on prompt quality.
Ambiguous merchant names reduce reliability.
Model may misclassify edge cases.
Using Ollama + Llama 3.1 8B improves privacy but reduces classification accuracy compared to larger models
Mitigation Approaches:
Add deterministic rules before LLM (regex for known merchants)
Store corrections and build a feedback loop
Add few-shot examples in prompt
Fine-tune smaller model (advanced)
Add One More Critical Con (You Missed It)
6. Webhook Security Risk
If webhook is publicly accessible without verification:
Anyone can inject fake transactions.
System can be spammed.
No request signature validation.
This is a serious issue.
You should at minimum:
Add secret token header validation in n8n
Or validate request body signature
Or restrict IP
Without this, your system is insecure.

