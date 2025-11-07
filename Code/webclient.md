# Web Client

GitHub repository: https://github.com/voxer/webclient

## Overview

Modern cross-platform client built with React Native and Expo, supporting web, iOS, and Android platforms. Uses expo-router for navigation, WatermelonDB for local data persistence, and Tailwind CSS (via NativeWind) for styling. Includes Material-UI components for web UI and expo-audio for audio handling. The codebase is actively being developed with recent focus on unit testing setup (Jest), file refactoring, and MessageList/Sidebar component improvements.

## API Architecture

### Network Layer
The WebClient uses native browser `fetch` API wrapped in a custom `HttpClient` class for all HTTP communications. Key characteristics:

- **Central HTTP Client**: `src/core/http-client.ts` - Singleton providing GET/POST/PUT/DELETE methods
- **SDK Facade**: `src/core/voxer.ts` - Main orchestration layer that provides unified access to all services
- **Service Layer**: `src/services/` - Individual service modules (auth, session, messaging, sync, user, chat, contacts, etc.)
- **Streaming**: Uses native `fetch` with `ReadableStream` for real-time updates via `/updates?mode=continuous`

### API Patterns Observed
- RESTful endpoints with versioned paths (e.g., `2/cs/signup`, `3/cs/login`, `2/rb/put_body`)
- Session authentication via `Rv_session_key` query parameter
- Content-Type typically `text/plain` with JSON payloads
- Long-lived streaming connection for real-time server updates

For comprehensive API endpoint documentation, see [../Api/README.md](../Api/README.md).

## Key API Mechanisms

### Real-time Streaming Protocol
The real-time update system uses a long-lived HTTP GET request to `/updates?mode=continuous` that sends newline-delimited JSON objects as events occur on the server. The connection includes automatic reconnection logic (1-second delay on normal disconnect, 5-second on errors) and 30-second health monitoring. Each JSON object represents an update event (new messages, profile changes, etc.) which are parsed and processed individually by the client.

**Entry point**: `src/services/sync.service.ts` - `startRealtimeUpdates()` (line 142)

### Session Management
Session authentication uses an `Rv_session_key` obtained via the `3/cs/start_session` endpoint by posting username, hashed password, and device fingerprint. The server returns the session key along with `session_max_lifetime`, `rebelvox_user_id`, and `home_router`. This session key is stored in AsyncStorage and automatically appended as a query parameter to all subsequent HTTP requests. Session expiration is tracked and validated before operations.

**Entry point**: `src/services/session.service.ts` - `startSession()` (line 39)

### File Upload Handling
Files are uploaded using a multipart format: JSON metadata followed by CRLF and binary data. The metadata includes message_id, content_type (image/video/audio), thread_id, and format information. Files are temporarily stored in IndexedDB with local URLs (`indexeddb://{messageId}`) for immediate display before upload completes. The HttpClient detects binary data (ArrayBuffer/Uint8Array) and sends it with Content-Type: application/octet-stream without JSON stringification.

**Entry point**: `src/services/messaging.service.ts` - `uploadAndSendFile()` (line 690)

### Error Handling & Retries
The system implements exponential backoff retry logic with 3 attempts (1s, 2s, 4s delays) for server errors (5xx) and network failures. Client errors (4xx) are not retried. Failed messages are marked in the database with `failureReceipts` containing sender ID, timestamp, and error details. The HttpClient parses error responses into ApiError format with status, error, and message fields.

**Entry point**: `src/services/messaging.service.ts` - `performRetryAttempts()` (line 323)

### Audio Message Types
The system supports two audio message types: **standard audio** via `3/cs/post_message` for pre-recorded messages (audio is Vox-encoded via WASM, sent as JSON metadata + CRLF + binary data), and **live streaming audio** via `3/cs/post_message_live` for walkie-talkie style real-time communication using HTTP/2 streaming with duplex mode. Live streams send headers with the first audio chunk, followed by continuous chunks via WritableStream. Both use Vox-encoded format; the difference is delivery method (complete vs. streaming).

**Vox Encoding**: Voxer's proprietary audio format based on the SILK codec (originally developed by Skype). It compresses PCM audio into a highly efficient format optimized for voice transmission, processing audio in 20ms frames with variable bitrate compression. This provides better compression than standard formats while maintaining voice quality suitable for walkie-talkie communication.

**Entry points**:
- Standard: `src/services/messaging.service.ts` - `sendAudioMessage()` (line 1002)
- Live: `src/services/messaging.service.ts` - `startLiveAudioStream()` (line 1763)
- Encoding: `src/services/audio-encoder.service.ts`
