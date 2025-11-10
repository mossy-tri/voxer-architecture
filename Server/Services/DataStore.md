# Data Store Service

## Overview

The Data Store Service provides a general-purpose abstraction layer for data storage and retrieval. It offers a unified interface to various backend storage systems and handles data encoding, caching, and access patterns.

## Entry Point

- **Main File**: `data_store/data_store.js`
- **Service Type**: `datastore`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Storage Abstraction**
   - Unified API for different storage backends
   - Pluggable storage adapters
   - Consistent data access patterns
   - Error handling and retries

2. **Data Encoding/Decoding**
   - Custom binary encoding formats
   - Compression
   - Serialization/deserialization
   - Schema evolution

3. **Caching Layer**
   - Multi-tier caching
   - Cache invalidation
   - Read-through/write-through patterns
   - Cache warming

4. **Data Access Patterns**
   - CRUD operations
   - Batch operations
   - Range queries
   - Atomic operations

## Storage Backends

The Data Store can interface with:
- **Riak** - Distributed key-value store
- **PostgreSQL** - Relational database
- **Redis** - In-memory cache
- **Local filesystem** - Disk storage

## Data Encoding

### Binary Encoder
- **`encoder/bin/decode.js`** - Binary decoding utility
- Custom binary formats for efficiency
- Backward compatibility
- Schema versioning

### Features
- Compact binary representation
- Fast encoding/decoding
- Type preservation
- Null handling

## Use Cases

### Generic Data Storage
Services use Data Store for:
- User-generated content
- Application state
- Configuration data
- Cached computations
- Session data

### Data Models
Located in `data_models/` directory, the Data Store may handle:
- User profiles
- Message metadata
- Media references
- Relationships
- Settings

## API Surface

Provides APIs for:
- **Put** - Store data
- **Get** - Retrieve data
- **Delete** - Remove data
- **Batch operations** - Multiple operations in one call
- **Query** - Search and filter data
- **Atomic updates** - Compare-and-swap operations

## Caching Strategy

### Read Path
1. Check local cache
2. Check distributed cache (Redis)
3. Read from primary storage
4. Populate caches on miss

### Write Path
1. Write to primary storage
2. Invalidate or update caches
3. Optionally write to cache
4. Asynchronous replication

## Performance Characteristics

- Optimized for common access patterns
- Caching reduces backend load
- Batch operations for efficiency
- Connection pooling
- Async I/O

## Consistency Model

- **Strong consistency** - For critical data (via PostgreSQL)
- **Eventual consistency** - For less critical data (via Riak)
- **Cache consistency** - TTL-based or invalidation-based

## Data Partitioning

- Partition by user ID
- Partition by data type
- Consistent hashing
- Automatic rebalancing

## Error Handling

1. **Retries**
   - Automatic retry for transient failures
   - Exponential backoff
   - Circuit breaker pattern

2. **Fallbacks**
   - Degrade gracefully
   - Serve stale cache on backend failure
   - Queue writes for later

3. **Monitoring**
   - Error rates per backend
   - Latency percentiles
   - Cache hit rates

## Configuration

Key settings:
- Backend connection strings
- Connection pool sizes
- Cache TTLs
- Retry policies
- Timeout values
- Consistency levels

## Integration Points

All services can use Data Store:
- Node Router
- Header Store
- Business API
- Growth Service
- Contact Service

## Observability

Metrics tracked:
- Request latency
- Cache hit/miss rates
- Backend error rates
- Throughput
- Queue depths

## Scaling Characteristics

- Horizontally scalable
- Stateless service layer
- Backend-dependent scaling
- Can shard by key ranges
- Read replicas for read-heavy workloads

## Data Migration

Support for:
- Schema migrations
- Data format upgrades
- Backend migrations
- Zero-downtime migrations

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `data_store/`
**Main Service**: `data_store/data_store.js`
**Encoder**: `data_store/encoder/bin/decode.js`
**Models**: `data_models/` directory
