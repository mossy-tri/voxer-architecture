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

## Core API Mechanisms

### Real-time Streaming Protocol
The real-time update system uses a long-lived HTTP GET request to `/updates?mode=continuous` that sends newline-delimited JSON objects as events occur on the server. Each JSON object represents an update event (new messages, profile changes, etc.) which are parsed and processed individually by the client.

### Session Management
Session authentication uses an `Rv_session_key` obtained via the `3/cs/start_session` endpoint by posting username, hashed password, and device fingerprint. The server returns the session key along with `session_max_lifetime`, `rebelvox_user_id`, and `home_router`. This session key is automatically appended as a query parameter to all subsequent HTTP requests.

### File Upload Handling
Files are uploaded using a multipart format: JSON metadata followed by CRLF and binary data. The metadata includes message_id, content_type (image/video/audio), thread_id, and format information. The binary data is sent with Content-Type: application/octet-stream.

### Audio Message Types
The system supports two audio message types:

**Standard Audio Messages** (`3/cs/post_message`): For pre-recorded messages. Audio is Vox-encoded and sent as JSON metadata + CRLF + binary data.

**Live Streaming Audio** (`3/cs/post_message_live`): For walkie-talkie style real-time communication using HTTP/2 streaming with duplex mode. Live streams send headers with the first audio chunk, followed by continuous chunks.

**Vox Encoding**: Voxer's proprietary audio format based on the SILK codec (originally developed by Skype). It compresses PCM audio into a highly efficient format optimized for voice transmission, processing audio in 20ms frames with variable bitrate compression. This provides better compression than standard formats while maintaining voice quality suitable for walkie-talkie communication.
