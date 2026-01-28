## Performance Optimization

### Caching Strategy

#### Cache Layers

1. **CDN Cache**
    - Static assets (checkout page HTML/CSS/JS)
    - Global edge caching for low latency
    - Cache-Control headers optimized per asset type

2. **API Gateway Cache**
    - Common API responses
    - TTL: 30 seconds
    - Suitable for idempotent GET requests
    - Reduces backend load during traffic spikes

3. **Application Cache (Redis)**
    - User sessions
    - Access tokens & refresh tokens
    - Fraud detection feature vectors
    - Rate limiting counters
    - Low-latency, in-memory access

4. **Database Cache**
    - PostgreSQL shared buffers
    - OS-level page cache
    - Frequently accessed indexes and hot rows

#### Cache Invalidation

- **Time-based**
  - TTL expiration per cache layer
  - Short TTLs for volatile data

- **Event-based**
  - Transaction completion
  - Payment status changes
  - Settlement and refund events

- **Manual Purge**
  - Admin-triggered invalidation
  - Emergency cache clear (e.g. configuration issues)

<br/>

### Database Optimization

#### Read-Write Splitting

- Writes → Primary PostgreSQL instance
- Reads → Read replicas (load balanced)
- Automatic replica selection
- Replication lag monitoring with alerts
- Fallback to primary if replicas lag beyond threshold

#### Query Optimization

- Query plan analysis using `EXPLAIN ANALYZE`
- Index optimization (B-tree, GIN, partial indexes)
- Removal of unused or redundant indexes
- Materialized views for complex analytical reports
- Table partitioning:
  - `transactions` table partitioned by month
  - Improved query performance and maintenance

#### Connection Pooling

- PgBouncer in transaction pooling mode
- Max connections: 500
- Connection timeout: 30 seconds
- Idle connection timeout: 10 minutes
- Prevents database connection exhaustion under load

<br/>

### API Rate Limiting

#### Algorithm

- Token Bucket

#### Limits by Tier

- **Free Tier**: 10 requests/minute
- **Standard Tier**: 100 requests/minute
- **Premium Tier**: 1,000 requests/minute
- **Enterprise Tier**: Custom (10,000+ requests/minute)

#### Rate Limit Headers

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 823
X-RateLimit-Reset: 1704294000

```
