# Header Store (HS)

**Repository**: `https://github.com/voxer/server`
**Location**: `header_store.js` and `header_store/`

## Overview

The Header Store stores and retrieves message metadata, conversation state, and user presence information. It provides the primary database for all non-media message data.

## Message Metadata Storage

The service stores message headers and attributes, conversation and thread metadata, message state and delivery status, and read receipts and acknowledgments.

## User Data Management

The service manages user profiles and settings, contact relationships, user presence and online status, and device tokens for push notifications.

## Conversation Management

The service handles thread and conversation creation, participant management, conversation history, and muting and notification preferences.

## Search and Indexing

The service provides message search capabilities, conversation lookups, and user directory queries.

## Storage Backend

The service uses PostgreSQL as its primary database for persistent storage of message metadata, relational queries, and transaction support. Redis provides a caching layer for frequently accessed data, pub/sub for real-time updates, and session state caching.

## API Surface

The service provides HTTP endpoints for message CRUD operations, conversation management, user profile queries, search operations, and analytics and reporting queries.

## Performance Characteristics

The service implements connection pooling to PostgreSQL and extensive caching to reduce database load. It is optimized for low-latency lookups and high read/write throughput.

## Configuration

Key configuration parameters include PostgreSQL connection settings, Redis connection pool settings, cache TTL configurations, and rate limiting thresholds.

## Scaling Characteristics

The service is horizontally scalable with multiple instances and stateless request handling. The database is the scaling bottleneck. Caching is used to improve performance.

## Dependencies

The service uses PostgreSQL database, Redis cache cluster, the Metrics service for monitoring, and the Message Indexer service for search.

## Related Services

The Message Indexer (`message_indexer.js`) indexes messages for search. The Message Saver (`message_saver.js`) provides asynchronous message persistence.
