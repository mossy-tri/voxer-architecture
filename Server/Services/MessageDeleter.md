# Message Deleter (MD)

## Overview

The Message Deleter service handles asynchronous deletion and cleanup of messages, media, and related data. It provides both user-initiated message deletion and automated data retention policies.

## Entry Point

- **Main File**: `md/md.js`
- **Service Type**: `md`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Message Deletion**
   - Delete individual messages
   - Delete entire conversations
   - Cascade deletion to related data
   - Handle deletion tombstones

2. **Media Cleanup**
   - Remove media from Body Store
   - Clean up disk caches
   - Remove from distributed storage
   - Update storage metrics

3. **Metadata Cleanup**
   - Remove message headers from Header Store
   - Clean up indexes
   - Update conversation state
   - Remove search index entries

4. **Retention Policies**
   - Automated data retention
   - Compliance-driven deletion
   - GDPR "right to be forgotten"
   - Data lifecycle management

## Deletion Types

### User-Initiated Deletion
- Delete for self (remove from own view)
- Delete for everyone (if owner/admin)
- Conversation deletion
- Account deletion cleanup

### System-Initiated Deletion
- Expired messages (if feature enabled)
- Retention policy enforcement
- Spam/abuse content removal
- Deactivated account cleanup

### Compliance Deletion
- GDPR data erasure requests
- Legal hold expiration
- Regulatory compliance
- Data residency requirements

## Deletion Process

### Queue-Based Processing
1. Receive deletion request
2. Validate permissions
3. Queue deletion job
4. Async processing
5. Confirm completion

### Cascade Operations
When deleting a message:
1. Mark message as deleted in Header Store
2. Queue media deletion from Body Store
3. Remove from search indexes
4. Update conversation metadata
5. Notify participants (if applicable)
6. Create audit trail

## Supporting Processes

### Deleter Worker
- **`run_deleter.js`** - Background deletion worker
- Processes deletion queue
- Batch processing for efficiency
- Retry failed deletions

### Promoter
- **`run_promoter.js`** - Data promotion/archival
- Move old data to cold storage
- Prepare data for deletion
- Archive before delete

## API Surface

HTTP endpoints for:
- Delete message
- Delete conversation
- Delete user data (GDPR)
- Check deletion status
- Query deletion queue

## Deletion States

Messages go through states:
1. **Active** - Normal state
2. **Deleted (soft)** - Marked deleted, data remains
3. **Deleted (hard)** - Data physically removed
4. **Tombstone** - Marker of deleted message

## Data Retention

### Configurable Policies
- Message retention period
- Media retention period
- Metadata retention
- Compliance overrides

### Grace Periods
- Soft delete period (recoverable)
- Hard delete delay
- User account deletion delay

## Performance Characteristics

- Asynchronous processing (not blocking)
- Queue-based workload
- Batch deletions for efficiency
- Off-peak scheduling for heavy operations
- Idempotent operations

## Safety Mechanisms

1. **Confirmation**
   - Multi-step confirmation for bulk deletes
   - Audit trail before deletion
   - Backup before major deletions

2. **Recovery**
   - Soft delete period
   - Undo capability (within grace period)
   - Backup restoration procedures

3. **Validation**
   - Permission checks
   - Ownership verification
   - Scope validation

## Integration Points

- **Header Store** - Message metadata deletion
- **Body Store** - Media file deletion
- **Message Indexer** - Search index cleanup
- **Contact Service** - Contact data cleanup
- **Notification Service** - Delete notifications

## Compliance Features

### GDPR Support
- Complete user data deletion
- Deletion confirmation
- Deletion audit trail
- Data export before deletion

### Legal Hold
- Prevent deletion during holds
- Mark data as protected
- Release after hold expiration

## Monitoring & Alerting

Track:
- Deletion queue depth
- Processing rate
- Failed deletions
- Deletion latency
- Storage reclaimed

## Configuration

Key settings:
- Deletion queue size
- Batch size
- Retry policies
- Retention periods
- Grace periods

## Scaling Characteristics

- Horizontally scalable workers
- Queue-based coordination
- Can partition by data type
- Idempotent for retry safety

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `md/`
**Main Service**: `md/md.js`
**Workers**: `md/run_deleter.js`, `md/run_promoter.js`
