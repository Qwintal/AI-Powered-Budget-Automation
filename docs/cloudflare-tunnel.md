# Cloudflare Tunnel Setup (Free Tier) for n8n

## Objective
Expose a locally running n8n instance securely to the internet without:
- Port forwarding
- Public IP exposure
- Direct firewall configuration

### Cloudflare Tunnel creates a secure outbound connection from your local machine to Cloudflare’s edge network, enabling HTTPS access to your local webhook endpoint.

## Architecture Overview
Local n8n (localhost:5678) \ 
        ↓ \ 
cloudflared (tunnel client) \
        ↓ \ 
Cloudflare Edge Network \ 
        ↓ \ 
Public HTTPS URL 

## Step 1: Install cloudflared

Windows 
```bash
winget install --id Cloudflare.cloudflared
```
Or download from:
https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/

## Step 2: Start n8n
Ensure it runs at: (http://localhost:5678)
Step 3: Start Tunnel
cloudflared tunnel --url http://localhost:5678
Cloudflare will output something like: (https://random-string.trycloudflare.com)
This is your public HTTPS endpoint.

### Limitations of Temporary Tunnel
- URL changes each restart
- No authentication by default
- Not suitable for production use

### Using with MacroDroid
In MacroDroid:
- Set HTTP POST
- Target URL: (https://your-public-url/webhook/sms)
- Send structured JSON payload
