# n8n Workflow Configuration
## Objective
Configure n8n as the orchestration layer that:
- Receives webhook requests from Cloudflare Tunnel
- Validates incoming SMS payloads
- Performs transaction categorization (LLM or rule-based)
- Normalizes data into structured format
- Appends records to storage (Google Sheets / PostgreSQL)
- n8n acts as the processing backbone of the automation system.

![Workflow](/screenshots/workflow.png)

## Install using docker
https://docs.n8n.io/hosting/installation/docker/

## Core Workflow Design
### Step 1 – Webhook Node

Method: POST \ 
Path: /sms \ 
Response Mode: On Received (or after workflow) \ 
Example public endpoint: \ 
https://your-domain/webhook/sms \ 
Payload expected from MacroDroid: \ 
```JSON
{
  "message": "Rs. 350 debited from A/C XXXX via UPI to Swiggy Ref 123456",
  "timestamp": "2025-02-10T14:32:00"
}
```

### Step 2 – LLM Node
Model: llama3.1:8b ( any that runs well ) \
Prompt design: 
- Extract amount 
- Identify debit/credit
- Extract merchant 
- Classify category 
Important: 
- Enforce structured JSON output
- Validate JSON before proceeding
If LLM fails:
- Set category = "Uncategorized"
- Set llm_status = failed

### Step 3 – Storage Node
Option A – Google Sheets Node \ 
Append Row \ 
Map structured fields to columns \ 
Option B – PostgreSQL Node \ 
Insert into transactions table \
Use parameterized query \ 
Prevent duplicates via unique constraint \
