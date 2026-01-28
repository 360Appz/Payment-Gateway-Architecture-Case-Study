
## RESTful API Endpoints

**Base URL:**  
https://api.paymentgateway.com/v1

**Authentication:**  
Bearer Token (JWT)

<br/>

## Create Payment Intent

**Endpoint:**  
POST `/payment-intents`

### Request
```json
{
  "amount": 9999,
  "currency": "usd",
  "payment_method": "card",
  "customer_id": "cust_123",
  "metadata": {
    "order_id": "order_456",
    "product": "Premium Subscription"
  }
}
````

### Response

```json
{
  "id": "pi_abc123",
  "object": "payment_intent",
  "amount": 9999,
  "currency": "usd",
  "status": "requires_payment_method",
  "client_secret": "pi_abc123_secret_xyz789",
  "created": 1704294000
}
```

<br/>

## Confirm Payment

**Endpoint:**
POST `/payment-intents/:id/confirm`

### Request

```json
{
  "payment_method": {
    "type": "card",
    "card": {
      "token": "tok_visa_4242"
    }
  }
}
```

### Response

```json
{
  "id": "pi_abc123",
  "status": "succeeded",
  "amount_received": 9999,
  "charges": {
    "data": [
      {
        "id": "ch_xyz789",
        "amount": 9999,
        "captured": true,
        "paid": true
      }
    ]
  }
}
```

<br/>

## Webhook Events

**Endpoint:**
Merchant-provided URL

**Events:**

* payment_intent.succeeded
* payment_intent.failed
* charge.refunded
* charge.disputed

### Webhook Payload

```json
{
  "id": "evt_abc123",
  "type": "payment_intent.succeeded",
  "data": {
    "object": {
      "id": "pi_abc123",
      "amount": 9999,
      "currency": "usd"
    }
  }
}
```

<br/>

## Webhook Signature Verification

```python
import hmac
import hashlib

def verify_webhook(payload, signature, secret):
    expected = hmac.new(
        secret.encode(),
        payload.encode(),
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(expected, signature)
```


