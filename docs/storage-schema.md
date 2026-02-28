## Data Schema
| Field                 | Type          | Description                     |
| --------------------- | ------------- | ------------------------------- |
| transaction_id        | UUID / hash   | Unique transaction identifier   |
| amount                | decimal(12,2) | Transaction amount              |
| currency              | varchar(3)    | ISO currency code               |
| transaction_type      | enum          | debit / credit                  |
| merchant_name         | varchar       | Merchant or recipient           |
| upi_ref               | varchar       | UPI reference ID                |
| bank                  | varchar       | Bank name                       |
| transaction_timestamp | timestamp     | Date & time of transaction      |
| category              | varchar       | Classified transaction category |
| raw_sms               | text          | Original SMS content            |
| llm_status            | varchar       | success / failed                |
| created_at            | timestamp     | Ingestion time                  |
