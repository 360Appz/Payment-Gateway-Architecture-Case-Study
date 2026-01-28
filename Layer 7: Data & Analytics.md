## Layer 7: Data & Analytics

### Event-Driven Architecture

**Event Streaming:** Apache Kafka / AWS MSK (Managed Streaming for Kafka)

**Event Topics:**
- payment.initiated  
- payment.authorized  
- payment.captured  
- payment.failed  
- payment.refunded  
- fraud.detected  
- chargeback.received  

```
{
  "event_id": "evt_abc123",
  "event_type": "payment.authorized",
  "timestamp": "2026-01-03T14:23:45Z",
  "payload": {
    "transaction_id": "txn_xyz789",
    "amount": 99.99,
    "currency": "USD",
    "psp": "stripe",
    "card_type": "visa",
    "card_last4": "4242",
    "merchant_id": "merch_123",
    "customer_id": "cust_456",
    "risk_score": 0.12,
    "3ds_completed": true
  }
}
```

<br/>

### Analytics Engine

**Real-time Analytics:**
- Transaction volume (TPS)
- Authorization rates by PSP, card type, region
- Average transaction value
- Fraud detection rates
- System latency (p50, p95, p99)

**Batch Analytics:**
- Daily reconciliation reports
- Monthly settlement summaries
- Chargeback analysis
- Revenue forecasting
- Customer lifetime value (CLV)

**Tools:**
- ClickHouse – OLAP database for analytics
- Apache Flink – Stream processing
- Looker / Tableau – Business intelligence dashboards

<br/>

### Unified Ledger

**Purpose:** Single source of truth for all financial transactions

**Ledger Entries:**
- Double-entry bookkeeping
- Immutable transaction records
- Real-time balance updates
- Multi-currency support

**Accounts:**
- Cash-in-Transit
- Accounts Receivable (A/R)
- Accounts Payable (A/P)
- Revenue
- Fees & Commissions

**Compliance:**
- SOX compliance (Sarbanes-Oxley)
- GAAP / IFRS reporting standards
- Audit trails for all entries
