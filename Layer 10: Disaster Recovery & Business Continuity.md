```mermaid
graph TB
    subgraph SG1 ["Region: US-East-1 (Primary)"]
        LB1[Load Balancer]
        APP1[App Instances]
        DB1[(PostgreSQL Primary)]
        REDIS1[(Redis Primary)]
    end
    
    subgraph SG2 ["Region: US-West-2 (Secondary)"]
        LB2[Load Balancer]
        APP2[App Instances]
        DB2[(PostgreSQL Read Replica)]
        REDIS2[(Redis Replica)]
    end
    
    subgraph SG3 ["Region: EU-West-1 (Tertiary)"]
        LB3[Load Balancer]
        APP3[App Instances]
        DB3[(PostgreSQL Read Replica)]
        REDIS3[(Redis Replica)]
    end
    
    GLOBAL_LB[Global Load Balancer<br/>Route53 / CloudFlare]
    
    GLOBAL_LB --> LB1 & LB2 & LB3
    DB1 -->|Async Replication| DB2 & DB3
    REDIS1 -->|Replication| REDIS2 & REDIS3

    style SG1 fill:#e1f5ff

```
<br/>

## Failover Strategy

- **Automatic Health Checks**: Every 30 seconds  
- **DNS Failover**: Route53 health-based routing (TTL: 60s)  
- **Database Promotion**: Automatic read replica promotion  
- **RTO (Recovery Time Objective)**: < 5 minutes  
- **RPO (Recovery Point Objective)**: < 1 minute  

<br/>

## Backup Strategy

### Database Backups

- Continuous WAL archiving to S3  
- Daily full backups (automated)  
- Retention: 30 days online, 7 years in Glacier  

### Application Backups

- Infrastructure as Code (Terraform state in S3)  
- Docker images in ECR (versioned)  
- Configuration in Git (version controlled)  

<br/>

## Testing

- Quarterly disaster recovery drills  
- Automated backup restoration tests  
- Chaos engineering (random instance termination)  
