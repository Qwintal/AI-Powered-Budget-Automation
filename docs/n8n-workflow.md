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
![Refer](https://docs.n8n.io/hosting/installation/docker/)

## Core Workflow Design
### Step 1 – Webhook Node

Method: POST
Path: /sms
Response Mode: On Received (or after workflow)
Example public endpoint:
https://your-domain/webhook/sms
Payload expected from MacroDroid:
{
  "message": "Rs. 350 debited from A/C XXXX via UPI to Swiggy Ref 123456",
  "timestamp": "2025-02-10T14:32:00"
}

### Step 2 – Validation Node (Function or IF Node)

Purpose:
Ensure required fields exist
Reject malformed payloads
Optionally validate secret token
Example validation logic:
message must exist
timestamp must exist

If validation fails:
Return HTTP 400
Do not continue workflow

### Step 3 – LLM Classification Node

Use HTTP Request node to call local Ollama instance.
Example:
Endpoint: http://localhost:11434/api/generate
Model: llama3.1:8b
Prompt design:
Extract amount
Identify debit/credit
Extract merchant
Classify category
Important:
Enforce structured JSON output
Validate JSON before proceeding
If LLM fails:
Set category = "Uncategorized"
Set llm_status = failed

### Step 4 – Normalization Layer (Function Node)

Purpose:
Parse extracted values
Convert amount to decimal
Convert date to ISO timestamp
Generate transaction fingerprint (hash)
Example hash logic:
hash = SHA256(amount + timestamp + merchant + reference)
This supports deduplication.

### Step 5 – Storage Node
Option A – Google Sheets Node
Append Row
Map structured fields to columns
Option B – PostgreSQL Node
Insert into transactions table
Use parameterized query
Prevent duplicates via unique constraint
