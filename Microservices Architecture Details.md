# Microservices Architecture Details

## Service Decomposition

### Core Services

1. **API Gateway Service**  
  Request routing, authentication

2. **Payment Orchestration Service**  
  Transaction coordination

3. **Authorization Service**  
  Payment authorization

4. **Fraud Detection Service**  
  Real-time fraud analysis

5. **Tokenization Service**  
  PAN tokenization

6. **Notification Service**  
  Email, SMS, webhooks

7. **Reporting Service**  
  Analytics & reports

8. **Settlement Service**  
  Clearing & settlement

9. **Reconciliation Service**  
  Transaction matching

10. **Admin Service**  
  Merchant portal

<br/>

## Service Communication

### Synchronous (REST / gRPC)

- API Gateway ↔ Services  
- Payment Orchestrator ↔ PSP Adapters  
- Authorization ↔ Fraud Detection  

### Asynchronous (Kafka)

- Payment events ↔ Analytics  
- Transaction events ↔ Reconciliation  
- Fraud alerts ↔ Notification Service  


### Service Mesh (Istio)

- Mutual TLS (mTLS) between services  
- Traffic management (circuit breaking, retries)  
- Observability (distributed tracing)  

<br/>

## Technology Stack

### Programming Languages

- **Go**  
  Payment Orchestrator, Authorization Service (high performance)

- **Java (Spring Boot)**  
  Fraud Detection, Settlement Service

- **Python**  
  ML models, Analytics pipelines

- **Node.js**  
  API Gateway, Notification Service

<br/>

### Databases

- **PostgreSQL** — Transactional data  
- **ClickHouse** — Analytics  
- **Redis** — Cache, session management  
- **Elasticsearch** — Logs, search  

<br/>

### Message Queue

- **Apache Kafka** — Event streaming  

<br/>

### Container Orchestration

- **Kubernetes (EKS)** — Container orchestration  
- **Docker** — Containerization  

<br/>

### CI/CD

- **GitHub Actions** — CI/CD pipelines  
- **ArgoCD** — GitOps deployment  
- **Terraform** — Infrastructure as Code  
