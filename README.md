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
