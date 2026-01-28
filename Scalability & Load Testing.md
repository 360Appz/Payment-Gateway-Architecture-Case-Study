
### Performance Targets

| Metric             |  Target | Current (2026) |
| ------------------ | ------: | -------------: |
| Peak TPS           |  10,000 |          8,500 |
| Avg Response Time  | < 200ms |          145ms |
| P95 Response Time  | < 300ms |          280ms |
| P99 Response Time  | < 500ms |          420ms |
| Uptime             |  99.99% |         99.97% |
| Authorization Rate |   > 95% |          96.2% |

<br/>

### Horizontal Scaling

#### Auto-Scaling Policies

* **Scale up:** CPU > 70% for 5 minutes
* **Scale down:** CPU < 30% for 10 minutes
* **Min instances:** 3 per service
* **Max instances:** 50 per service

### Load Testing

#### Load Testing Tools

* **Apache JMeter** - HTTP load testing
* **Gatling** - Performance testing (Scala-based)
* **K6** - Modern load testing (JavaScript)

#### Load Test Scenarios

* **Normal Load** - 1,000 TPS sustained for 1 hour
* **Peak Load** - 5,000 TPS for 30 minutes
* **Spike Test** - 0 → 10,000 TPS in 1 minute
* **Stress Test** - Increase load until failure
