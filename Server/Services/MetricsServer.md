# Metrics Server (MS)

## Overview

The Metrics Server aggregates, stores, and serves system metrics, performance data, and operational statistics from all Voxer services. It provides monitoring, alerting, and analytics capabilities.

## Entry Point

- **Main File**: `metrics_server.js`
- **Service Type**: `ms`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Metrics Collection**
   - Receive metrics from all services
   - Aggregate time-series data
   - Store metrics in database
   - Real-time metrics streaming

2. **Metrics Storage**
   - Time-series database
   - Data retention policies
   - Downsampling for old data
   - Efficient querying

3. **Metrics Serving**
   - Query APIs for dashboards
   - Real-time metrics endpoints
   - Historical data queries
   - Aggregation functions

4. **Alerting**
   - Threshold-based alerts
   - Anomaly detection
   - Alert routing
   - Alert management

## Metrics Architecture

### Collection
Services send metrics via:
- **`metrics_client.js`** - Client library used by all services
- UDP for high-frequency metrics
- HTTP for guaranteed delivery
- Batch sending for efficiency

### Aggregation
- Counter aggregation
- Histogram calculation
- Percentile computation
- Rate calculations

### Storage Backend
- **PostgreSQL** - Via `zag-backend-pg`
- **Zag** - Metrics pipeline (`zag`, `zag-agent`, `zag-daemon`)
- Time-series optimized schema
- Partitioned tables by time

## Metrics Types

### System Metrics
- CPU usage
- Memory usage
- Disk I/O
- Network traffic
- Process metrics

### Application Metrics
- Request rates
- Response times
- Error rates
- Success rates
- Queue depths

### Business Metrics
- Active users
- Message volume
- Media upload/download rates
- Premium subscriptions
- Revenue metrics

## Metrics Client

All services use `metrics_client.js` which provides:

### Counter
```javascript
counter("metric.name", value);
```
Increments counters for event counting.

### Histogram
```javascript
histogram("metric.name", value);
```
Records distributions for latency, sizes, etc.

### Gauge
```javascript
gauge("metric.name", value);
```
Records point-in-time values.

## Supporting Components

### Monitor
- **`metrics_server/monitor/monitor.js`** - Monitoring daemon
- **`metrics/monitor.js`** - Metrics monitoring
- Health checks
- Alerting logic

### Channel
- **`metrics_server/channel/metrics_channel.js`** - Metrics distribution channel
- Real-time metrics streaming
- WebSocket-based updates
- Dashboard integration

### Stuck Time Detector
- **`metrics_server/stuck-time.js`** - Detect stuck processes
- Event loop monitoring
- Deadlock detection

## System Metrics Collection

**`gather_system_metrics.js`** - Collects system-level metrics:
- CPU usage per core
- Memory utilization
- Swap usage
- Disk usage
- Network interface stats
- Process-specific metrics

## Visualization & Dashboards

### Integration Points
- Grafana (likely)
- Custom dashboards via metrics API
- Real-time monitoring views
- Historical trend analysis

### Query API
- Time range queries
- Metric filtering
- Aggregation functions
- Downsampling for display

## Alerting System

### Alert Types
- Threshold alerts (> or < value)
- Rate-of-change alerts
- Anomaly detection
- Pattern matching

### Alert Routing
- Email notifications
- SMS/PagerDuty integration
- Slack/chat integration
- Escalation policies

## Performance Characteristics

- Very high write volume
- Moderate read volume
- Batch writes for efficiency
- In-memory aggregation
- Periodic database flushes

## Data Retention

### Retention Tiers
- **High resolution** - Recent data (hours/days)
- **Medium resolution** - Recent data (weeks)
- **Low resolution** - Historical data (months/years)
- **Archived** - Compressed old data

### Downsampling
- Aggregate high-frequency data
- Reduce storage requirements
- Maintain long-term trends
- Configurable retention policies

## Configuration

Key settings:
- Metrics collection port
- Database connection
- Retention policies
- Downsampling rules
- Alert thresholds
- Storage quotas

## Dependencies

### npm Packages
- `metrics` - Metrics library
- `zag` - Metrics pipeline
- `zag-agent` - Metrics agent
- `zag-backend-pg` - PostgreSQL backend
- `zag-daemon` - Metrics daemon
- `llquantize` - Log-linear quantization

### Services
- PostgreSQL database
- All Voxer services (data sources)
- Monitoring tools (Grafana, etc.)

## Scaling Characteristics

- Horizontally scalable collectors
- Partitioned database
- Can shard by metric namespace
- High write throughput
- Query optimization important

## Monitoring the Monitor

The Metrics Server itself emits metrics:
- Collection rate
- Storage latency
- Query performance
- Alert delivery success
- Database health

## Code Location

**Repository**: `https://github.com/voxer/server`
**Main Service**: `metrics_server.js`
**Directory**: `metrics_server/`
**Client Library**: `metrics_client.js`
**System Metrics**: `gather_system_metrics.js`
