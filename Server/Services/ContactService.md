# Contact Service (CS)

## Overview

The Contact Service manages user contact lists, contact synchronization, and contact discovery. It handles the import, storage, and matching of contacts from various sources.

## Entry Point

- **Main File**: `contact_service/contact_server.js`
- **Service Type**: `cs`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Contact List Management**
   - Store user contact lists
   - Contact CRUD operations
   - Contact organization and grouping
   - Contact metadata

2. **Contact Synchronization**
   - Import contacts from phone address books
   - Import from email providers (Gmail, Outlook, etc.)
   - Import from social networks
   - Incremental sync with client devices
   - Conflict resolution

3. **Contact Discovery**
   - Match phone numbers to Voxer users
   - Match email addresses to Voxer users
   - Privacy-preserving contact matching
   - Suggest Voxer contacts

4. **Privacy & Permissions**
   - Contact sharing preferences
   - Block/unblock contacts
   - Privacy settings enforcement
   - GDPR compliance for contact data

## Contact Matching

The service performs privacy-preserving contact matching:
- Hash-based matching to protect privacy
- Phone number normalization (E.164 format)
- Email address normalization
- Fuzzy matching for name-based lookup

## Data Storage

- User contact lists
- Contact-to-user mappings
- Import history and metadata
- Blocked contacts
- Sharing preferences

## API Surface

HTTP endpoints for:
- Upload contacts
- Sync contacts
- Get contact list
- Search contacts
- Block/unblock contacts
- Contact suggestions

## Integration Points

### Import Sources
- Phone address book (iOS/Android)
- Gmail contacts
- Outlook/Exchange
- Facebook friends
- Twitter followers

### External Services
- Phone number validation services
- Email verification
- Social network APIs

## Privacy Considerations

1. **Data Minimization**
   - Only store necessary contact data
   - Hash sensitive identifiers
   - Regular data cleanup

2. **User Consent**
   - Explicit permission for contact access
   - Granular sharing controls
   - Opt-out mechanisms

3. **Data Protection**
   - Encrypted storage
   - Access controls
   - Audit logging

## Performance Characteristics

- Read-heavy workload (contact lookups)
- Periodic sync bursts
- Large batch imports need throttling
- Caching for frequent lookups

## Scaling Characteristics

- Horizontally scalable
- Partitionable by user ID
- Cache-friendly access patterns
- Can shard database by user ranges

## Dependencies

- Header Store (user data)
- People Matcher service (contact suggestions)
- Third-party contact import APIs
- Phone number/email validation services

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `contact_service/`
**Entry Point**: `contact_service/contact_server.js`
