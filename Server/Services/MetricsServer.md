# Metrics Server (MS)

**Repository**: `https://github.com/voxer/server`
**Location**: `metrics_server.js` and `metrics_server/`

## Overview

The Metrics Server aggregates, stores, and serves system metrics, performance data, and operational statistics from all Voxer services. It provides monitoring, alerting, and analytics capabilities.

## Metrics Collection

The service receives metrics from all services through a client library and stores them in a time-series database. Services send metrics via UDP for high-frequency data and HTTP for guaranteed delivery. The client library supports counters for event counting, histograms for distributions, and gauges for point-in-time values. Metrics are batched for transmission efficiency and aggregated in memory before database storage.

## Metrics Storage

The service uses PostgreSQL with time-series optimized schemas through the Zag metrics pipeline. Data is partitioned by time and downsampled according to retention policies. Recent data is stored at high resolution, while historical data is aggregated into lower resolutions. Old data is compressed and archived according to configurable retention tiers.

## Metrics Serving

The service provides query APIs for dashboards and monitoring tools. It supports time range queries, metric filtering, aggregation functions, and real-time metrics streaming via WebSocket. Historical data queries can apply downsampling for display purposes. Integration points include external dashboards and custom monitoring views.

## Alerting

The service implements threshold-based alerts, rate-of-change alerts, and anomaly detection. Alerts are routed through email, SMS, PagerDuty, and Slack integrations. Alert configurations include threshold values, escalation policies, and delivery preferences.

## Metrics Types

The service collects system metrics including CPU usage, memory usage, disk I/O, network traffic, and process metrics. Application metrics include request rates, response times, error rates, and queue depths. Business metrics include active users, message volume, media transfer rates, and subscription data.

## Supporting Components

The service includes a monitoring daemon for health checks and alerting logic. A metrics channel component provides real-time distribution to dashboards. A stuck time detector monitors event loops and identifies deadlocks. The system metrics collector gathers CPU, memory, swap, disk, and network statistics.

## Performance Characteristics

The service handles high write volume through batch writes and in-memory aggregation. Database writes are performed periodically to reduce load. Query operations are optimized for time-series data access patterns.

## Dependencies

The service uses the metrics, zag, zag-agent, zag-backend-pg, zag-daemon, and llquantize npm packages. It requires PostgreSQL for storage and receives data from all Voxer services. The service itself emits metrics on collection rate, storage latency, query performance, alert delivery, and database health.

## Configuration

Key configuration parameters include metrics collection ports, database connections, retention policies, downsampling rules, alert thresholds, and storage quotas.

## Scaling Characteristics

The service is horizontally scalable for metrics collection. The database can be partitioned and sharded by metric namespace. Query optimization is required for read performance.
