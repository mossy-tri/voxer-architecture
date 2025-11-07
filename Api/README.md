# Voxer API Documentation

This directory contains comprehensive documentation of the Voxer API endpoints as implemented in the WebClient.

## API Categories

### [Authentication](authentication.md)
User registration, password management, email verification, and SSO integration.

### [Session](session.md)
User authentication and session management.

### [User Profile](user-profile.md)
User profile management, search, and account settings.

### [Contacts](contacts.md)
Contact list management and contact tagging.

### [Chat Management](chat-management.md)
Chat thread creation, participant management, and chat settings.

### [Messaging](messaging.md)
Message sending, reading, reactions, search, and AI-powered features.

### [Timeline & Sync](timeline-sync.md)
Data synchronization and real-time update acknowledgment.

### [Teams](teams.md)
Team creation, management, and member administration.

### [Subscription & Billing](subscription-billing.md)
Subscription lifecycle, payment methods, and billing management.

### [Utility](utility.md)
Server configuration, support requests, and integrations.

## API Patterns

- **Versioning**: Endpoints use version prefixes (e.g., `1/`, `2/`, `3/`)
- **Authentication**: Session key passed via `Rv_session_key` query parameter
- **Content-Type**: Typically `text/plain` with JSON payloads
- **Real-time**: Long-lived streaming connection via `/updates?mode=continuous`
