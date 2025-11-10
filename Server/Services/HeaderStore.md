# Header Store (HS)

## Overview

The Header Store is responsible for storing and retrieving message metadata, conversation state, and user presence information. It serves as the primary database for all non-media message data.

## Entry Point

- **Main File**: `header_store.js`
- **Service Type**: `hs`
- **Base Class**: Extends `VoxerService`
- **File Size**: 154KB (extensive functionality)

## Key Responsibilities

1. **Message Metadata Storage**
   - Message headers and attributes
   - Conversation/thread metadata
   - Message state and delivery status
   - Read receipts and acknowledgments

2. **User Data Management**
   - User profiles and settings
   - Contact relationships
   - User presence and online status
   - Device tokens for push notifications

3. **Conversation Management**
   - Thread/conversation creation
   - Participant management
   - Conversation history
   - Muting and notification preferences

4. **Search and Indexing**
   - Message search capabilities
   - Conversation lookups
   - User directory queries

## Storage Backend

- **Primary Database**: PostgreSQL
  - Persistent storage for all message metadata
  - Relational queries for complex lookups
  - Transaction support for data consistency

- **Caching Layer**: Redis (ioredis)
  - Fast access to frequently accessed data
  - Pub/sub for real-time updates
  - Session state caching

## API Surface

Provides HTTP endpoints for:
- Message CRUD operations
- Conversation management
- User profile queries
- Search operations
- Analytics and reporting queries

## Performance Characteristics

- High read/write throughput requirements
- Extensive caching to reduce database load
- Connection pooling to PostgreSQL
- Optimized for low-latency lookups

## Configuration

Key configuration parameters:
- PostgreSQL connection settings
- Redis connection pool settings
- Cache TTL configurations
- Rate limiting thresholds

## Scaling Characteristics

- Horizontally scalable (multiple HS instances)
- Stateless (can route to any instance)
- Database is the scaling bottleneck
- Heavy use of caching to improve performance

## Dependencies

- PostgreSQL database
- Redis cache cluster
- Metrics service for monitoring
- Message Indexer service (for search)

## Related Services

- **Message Indexer** (`message_indexer.js`) - Indexes messages for search
- **Message Saver** (`message_saver.js`) - Asynchronous message persistence

## Code Location

**Repository**: `https://github.com/voxer/server`
**Main File**: `header_store.js`
**Directory**: `header_store/` (supporting modules)
