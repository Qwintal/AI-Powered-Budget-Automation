## Data Schema
| Field | Type | Description |
|-------|------|------------|
| amount | number | Transaction amount |
| currency | string | Currency code |
| transaction_type | debit/credit | Recieve/Send |
| recipient_or_merchant | string | Their name |
| upi_ref | string | UPI reference ID |
| bank | string | Name of the Bank |
| date | ISO 8601 date | Date & Time of Transaction |
| category | LLM classified category | Type of Transaction |
