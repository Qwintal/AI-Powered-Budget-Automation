# Event-Driven Personal Finance Automation System

## Problem
Most budgeting apps:
- Require broad device permissions
- Lock data into proprietary ecosystems
- Require manual entry, creating friction

## Goal:
- Build a privacy-first, self-hosted budgeting pipeline with full data control.

## Overview
- Event-driven SMS capture on Android
- Secure webhook ingestion layer
- Local LLM-based transaction categorization
- Structured data storage (Google Sheets / PostgreSQL)
- Automated summary dashboard

## Architecture
![workflow](screenshots/architecture.png)

## Tech Stack & Limitations
### Android + MacroDroid
Why local trigger?
- Avoid third-party SMS readers
- Preserve privacy
- Reduce attack surface 

Trade-off:
- Device must remain active
- Battery impact possible

### Cloudflare Tunnel
Why not expose local IP directly?
- Avoid port forwarding
- Avoid exposing home network
- Simplify secure remote access 

Trade-off:
- Free plan creates temporary URLs
- Reliance on external service

### n8n
Why orchestration instead of writing custom server?
- Visual workflow control
- Easier debugging
- Faster iteration
- Clear separation of triggers and processing 

Trade-off:
- Requires 24/7 runtime

## Local LLM (Ollama)
Why local inference instead of OpenAI API?
- Privacy (financial data)
- No per-call cost
- Offline capability 

Trade-off:
- Hardware requirements
- Slightly lower model quality
- Slower inference

## Google Sheets (current version)
Why not PostgreSQL initially?
- Rapid prototyping
- Simpler dashboarding
- Lower setup friction 

Trade-off:
- Limited scalability
- No strict schema enforcement
- Weak indexing

## Overall Limitation
- Requires always-on services
- Limited scalability
- Manual setup complexity
- No enterprise-grade security

## Screenshots
![workflow](screenshots/workflow.png)
![workflow](screenshots/dashboard-piechart.png)

## How It Works
1. SMS received from bank. 
2. MacroDroid triggers HTTP POST request. 
3. Cloudflare Tunnel securely exposes local n8n instance. 
4. n8n processes payload. 
5. LLM extracts and classifies transaction. 
6. Structured data appended to storage. 
7. Dashboard updates automatically.

## Reliability & Risk Handling
- **LLM failure:** Defaults to `Uncategorized` while preserving raw SMS data.
- **Webhook abuse:** Mitigated via token-based request validation (authentication planned).
- **Duplicate transactions:** Prevented through unique transaction fingerprinting (planned).
- **Malformed SMS:** Stored with failure flags for later review.

## Improvements Planned
- Power Bi Dashboard 
- PostgreSQL backend 
- Implement webhook authentication and request validation.
- Add spending alerts/limit 

## 📂 Documentation
- [MacroDroid Setup](docs/macrodroid-setup.md)
- [n8n Workflow Configuration](docs/n8n-workflow.md)
- [Cloudflare Tunnel Setup (Free Tier)](docs/cloudflare-tunnel.md)
- [Storage Schema Design](docs/storage-schema.md)
