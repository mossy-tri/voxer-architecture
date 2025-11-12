# Contact Service (CS)

**Repository**: `https://github.com/voxer/server`
**Location**: `contact_service/`

## Overview

The Contact Service manages user contact lists, contact synchronization, and contact discovery. It handles the import, storage, and matching of contacts from various sources.

## Contact List Management

The service stores user contact lists and performs CRUD operations on contacts. It provides contact organization and grouping functionality and manages contact metadata.

## Contact Synchronization

The service imports contacts from phone address books, email providers including Gmail and Outlook, and social networks. It performs incremental synchronization with client devices and implements conflict resolution.

## Contact Discovery

The service matches phone numbers and email addresses to Voxer users through privacy-preserving contact matching. It provides contact suggestions based on existing contact data.

## Privacy & Permissions

The service manages contact sharing preferences and implements block/unblock functionality. It enforces privacy settings and maintains GDPR compliance for contact data.

## Contact Matching

The service performs hash-based matching to protect privacy. It normalizes phone numbers to E.164 format and standardizes email addresses. Fuzzy matching is implemented for name-based lookup.

## Data Storage

The service stores user contact lists, contact-to-user mappings, import history and metadata, blocked contacts, and sharing preferences.

## API Surface

The service provides HTTP endpoints for uploading contacts, syncing contacts, retrieving contact lists, searching contacts, blocking and unblocking contacts, and generating contact suggestions.

## Integration Points

The service imports from phone address books on iOS and Android, Gmail contacts, Outlook/Exchange, Facebook friends, and Twitter followers. It integrates with phone number validation services, email verification services, and social network APIs.

## Data Protection

The service stores only necessary contact data and hashes sensitive identifiers. Regular data cleanup is performed. User consent is required for contact access with granular sharing controls and opt-out mechanisms. Storage is encrypted with access controls and audit logging.

## Performance Characteristics

The service handles read-heavy workloads with frequent contact lookups. It processes periodic sync bursts and throttles large batch imports. Caching is implemented for frequent lookups.

## Scaling Characteristics

The service is horizontally scalable and partitionable by user ID. Access patterns are cache-friendly and the database can be sharded by user ranges.

## Dependencies

The service integrates with the Header Store for user data and the People Matcher service for contact suggestions. It uses third-party contact import APIs and phone number/email validation services.
