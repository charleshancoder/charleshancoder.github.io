# Scenario: Consolidate Customer Data

Our company uses two systems:

1. CRM System - Stores customer details, interactions, and correspodences. 
2. Billing System - Generates invoices and stores payment details.

Currently, our system needs to make separate API calls to both both system to display customer data and their billing information, causing latency issues and inconsistent data availability.

## Task:

Design an API that consolidates customer data from both systems into a single, unified API for internal and customer-facing applications.

Expected data model:

```json
{
  "customer_id": "123",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@hotmail.com",
  "phone": "123-456-7890",
  "billing_info":[
   {
       "invoice_id": "INV-123",
       "amount": 100.00,
       "status": "paid"
   },
   {
       "invoice_id": "INV-124",
       "amount": 200.00,
       "status": "unpaid"
   }
  ]
}
```

## System Details:

![unified-api](unified-api.drawio.svg)

