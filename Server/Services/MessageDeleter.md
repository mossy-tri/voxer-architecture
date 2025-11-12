# Message Deleter (MD)

**Repository**: `https://github.com/voxer/server`
**Location**: `md/`

## Overview

The Message Deleter service handles asynchronous deletion and cleanup of messages, media, and related data. It provides user-initiated message deletion and automated data retention policies.

## Message Deletion

The service deletes individual messages, entire conversations, and related data through cascading operations. It creates tombstones to mark deleted messages and validates permissions before processing deletion requests.

## Media Cleanup

The service removes media from the Body Store, cleans disk caches, and removes data from distributed storage. It updates storage metrics after cleanup operations.

## Metadata Cleanup

The service removes message headers from the Header Store and cleans up indexes. It updates conversation state and removes search index entries.

## Data Retention

The service implements configurable retention policies for messages, media, and metadata. It supports compliance-driven deletion including GDPR data erasure requests. Grace periods provide soft delete periods with recovery capabilities and delays before hard deletion.

## Deletion Types

User-initiated deletions include deleting for self, deleting for everyone with appropriate permissions, and account deletion cleanup. System-initiated deletions include expired messages, retention policy enforcement, spam and abuse content removal, and deactivated account cleanup. Compliance deletions include GDPR data erasure requests, legal hold expiration, and regulatory compliance requirements.

## Deletion Process

The service receives deletion requests, validates permissions, and queues deletion jobs for asynchronous processing. Cascade operations mark messages as deleted in the Header Store, queue media deletion from the Body Store, remove entries from search indexes, update conversation metadata, notify participants when applicable, and create audit trails.

## Deletion States

Messages transition through active, soft deleted with data remaining, hard deleted with data physically removed, and tombstone states that mark deleted messages.

## Workers

The deletion worker processes the deletion queue with batch processing and retry logic. The promoter worker moves old data to cold storage, prepares data for deletion, and archives before deletion.

## API Surface

The service provides HTTP endpoints for deleting messages and conversations, deleting user data for GDPR compliance, checking deletion status, and querying the deletion queue.

## Integration Points

The service integrates with the Header Store for message metadata deletion, the Body Store for media file deletion, the Message Indexer for search index cleanup, the Contact Service for contact data cleanup, and the Notification Service for deletion notifications.

## Compliance

The service supports complete user data deletion with confirmation and audit trails. It implements legal hold functionality to prevent deletion during holds and releases data after hold expiration.

## Configuration

Key configuration parameters include deletion queue size, batch size, retry policies, retention periods, and grace periods.

## Scaling Characteristics

The service is horizontally scalable through queue-based coordination. Workers can be partitioned by data type and operations are idempotent for retry safety.
