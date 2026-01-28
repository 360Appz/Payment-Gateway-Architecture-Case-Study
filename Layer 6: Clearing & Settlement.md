## Clearing Service

### Process

- Batch authorized transactions (typically end-of-day)
- Send batch to PSP/Acquirer
- PSP forwards to Card Network
- Card Network performs clearing
- Issuer confirms amounts

### Batch Schedule

- Real-time batching for high-value (>$10,000)
- Hourly batching for standard transactions
- End-of-day batch for low-value

<br/>

## Settlement Engine
### Settlement Flow

```mermaid
graph LR
    AUTH[Authorized Transactions]
    BATCH[Batch Creation]
    CLEARING[Clearing Process]
    SETTLEMENT[Settlement]
    FUNDING[Funding]
    
    AUTH -->|Daily| BATCH
    BATCH --> CLEARING
    CLEARING -->|T+1 to T+3| SETTLEMENT
    SETTLEMENT -->|Net Settlement| FUNDING
    
    subgraph FEES [Fee Deductions]
        INTERCHANGE[Interchange Fees]
        ASSESSMENT[Assessment Fees]
        PROCESSOR[Processor Fees]
    end
    
    SETTLEMENT --> INTERCHANGE
    SETTLEMENT --> ASSESSMENT
    SETTLEMENT --> PROCESSOR
    
    INTERCHANGE --> FUNDING
    ASSESSMENT --> FUNDING
    PROCESSOR --> FUNDING
```

### Settlement Timeframes

- **T+0 (Instant):** Real-time payments, some PSPs (premium service)
- **T+1 (Next day):** Most card transactions
- **T+2 (2 days):** Standard for some acquirers
- **T+3 (3 days):** International transactions

### Net Settlement

- Aggregate all transactions
- Deduct fees (interchange, assessment, processing)
- Transfer net amount to merchant account

<br/>

## Reconciliation Service

### Three-Way Reconciliation

- Internal Records (our transaction database)
- PSP Reports (settlement reports from Stripe, Adyen, etc.)
- Bank Statements (actual deposits)
  
```mermaid
graph TB
    subgraph DS [Data Sources]
        INTERNAL_DB[(Internal Transaction DB)]
        PSP_REPORTS[PSP Settlement Reports]
        BANK_FEED[Bank Statement Feed]
    end
    
    subgraph RE [Reconciliation Engine]
        DATA_INGESTION[Data Ingestion]
        NORMALIZATION[Data Normalization]
        MATCHING[Matching Algorithm]
        EXCEPTION[Exception Handling]
    end
    
    subgraph OUT [Outputs]
        RECONCILED[Reconciled Transactions]
        DISCREPANCIES[Discrepancy Reports]
        ALERTS[Real-time Alerts]
    end
    
    INTERNAL_DB --> DATA_INGESTION
    PSP_REPORTS --> DATA_INGESTION
    BANK_FEED --> DATA_INGESTION
    
    DATA_INGESTION --> NORMALIZATION
    NORMALIZATION --> MATCHING
    
    MATCHING -->|Match| RECONCILED
    MATCHING -->|Mismatch| EXCEPTION
    EXCEPTION --> DISCREPANCIES
    EXCEPTION --> ALERTS

    style EXCEPTION fill:#ffcccc
```
### Matching Rules

- **Exact match:** Amount, date, reference ID
- **Fuzzy match:** Amount ±0.01 (for rounding), date ±1 day
- **Partial match:** Partial payments, chargebacks
- **Manual review:** Complex discrepancies

**Automated Reconciliation Rate Target:** >98%

### Exception Types

- Missing transactions (in PSP but not in DB)
- Amount discrepancies
- Fee calculation errors
- Duplicate settlements
- Chargebacks not recorded

<br/>

## Chargeback Management
### Chargeback Flow

```mermaid
sequenceDiagram
    participant Customer
    participant Issuer
    participant CardNetwork
    participant Acquirer
    participant Merchant
    participant ChargebackSvc

    Customer->>Issuer: Dispute Transaction
    Issuer->>CardNetwork: Chargeback Request
    CardNetwork->>Acquirer: Chargeback Notification
    Acquirer->>Merchant: Chargeback Debit
    Merchant->>ChargebackSvc: Chargeback Alert
    
    Note over ChargebackSvc: Internal Review
    ChargebackSvc->>ChargebackSvc: Gather Evidence
    ChargebackSvc->>Merchant: Representment Package
    Merchant->>Acquirer: Respond with Evidence
    Acquirer->>CardNetwork: Representment
    CardNetwork->>Issuer: Review Evidence
    
    alt Evidence Accepted
        Issuer-->>CardNetwork: Reverse Chargeback
        CardNetwork-->>Acquirer: Funds Returned
        Acquirer-->>Merchant: Credit Account
    else Evidence Rejected
        Issuer-->>CardNetwork: Chargeback Stands
        CardNetwork-->>Acquirer: Confirm Chargeback
        Merchant->>Merchant: Loss Recorded
    end
```

### Chargeback Prevention

- Real-time fraud detection
- 3D Secure authentication
- Clear merchant descriptors
- Proactive refunds
- Customer communication

### Chargeback Alerts

- Verifi/Ethoca integration
- Pre-chargeback notifications
- Automatic refund if alert received
- Reduces chargeback ratio

**Target Chargeback Ratio:** <0.9% (industry threshold 1%)
