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

## WebClient Implementation Details

### Real-time Streaming Implementation
The WebClient implements the real-time streaming protocol with automatic reconnection logic (1-second delay on normal disconnect, 5-second on errors) and 30-second health monitoring.

**Entry point**: [src/services/sync.service.ts:142](https://github.com/voxer/webclient/blob/main/src/services/sync.service.ts#L142) - `startRealtimeUpdates()`

### Session Storage and Lifecycle
The session key is stored in AsyncStorage and automatically appended as a query parameter to all subsequent HTTP requests. Session expiration is tracked and validated before operations.

**Entry point**: [src/services/session.service.ts:39](https://github.com/voxer/webclient/blob/main/src/services/session.service.ts#L39) - `startSession()`

### File Upload Implementation
Files are temporarily stored in IndexedDB with local URLs (`indexeddb://{messageId}`) for immediate display before upload completes. The HttpClient detects binary data (ArrayBuffer/Uint8Array) and sends it with Content-Type: application/octet-stream without JSON stringification.

**Entry point**: [src/services/messaging.service.ts:690](https://github.com/voxer/webclient/blob/main/src/services/messaging.service.ts#L690) - `uploadAndSendFile()`

### Error Handling & Retries
The WebClient implements exponential backoff retry logic with 3 attempts (1s, 2s, 4s delays) for server errors (5xx) and network failures. Client errors (4xx) are not retried. Failed messages are marked in the database with `failureReceipts` containing sender ID, timestamp, and error details. The HttpClient parses error responses into ApiError format with status, error, and message fields.

**Entry point**: [src/services/messaging.service.ts:323](https://github.com/voxer/webclient/blob/main/src/services/messaging.service.ts#L323) - `performRetryAttempts()`

### Audio Message Implementation
The WebClient uses WASM for Vox encoding and WritableStream for live audio streaming. Live streams send headers with the first audio chunk, followed by continuous chunks via WritableStream.

**Entry points**:
- Standard: [src/services/messaging.service.ts:1002](https://github.com/voxer/webclient/blob/main/src/services/messaging.service.ts#L1002) - `sendAudioMessage()`
- Live: [src/services/messaging.service.ts:1763](https://github.com/voxer/webclient/blob/main/src/services/messaging.service.ts#L1763) - `startLiveAudioStream()`
- Encoding: [src/services/audio-encoder.service.ts](https://github.com/voxer/webclient/blob/main/src/services/audio-encoder.service.ts)
