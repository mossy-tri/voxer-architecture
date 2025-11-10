# Voxer Clients

This section documents the various client applications that connect to the Voxer platform. Each client implements the Voxer messaging protocol and provides platform-specific user experiences.

## Client Applications

### [iOS Client](ios.md)
**Repository**: https://github.com/voxer/ios

Native iOS application for iPhone and Apple Watch. Built with Swift/Objective-C using Xcode and CocoaPods. Published to the App Store as "Voxer Walkie Talkie Messenger" (Bundle ID: com.rebelvox.voxer-lite). Supports iOS devices and watchOS 8.7+. Uses fastlane for deployment automation and includes features like message recall, push-to-talk voice messaging, and the signature walkie-talkie interface.

**Key Technologies:**
- Swift/Objective-C
- CocoaPods dependency management
- fastlane for CI/CD
- Native iOS frameworks
- watchOS support

---

### [Web Client](webclient.md)
**Repository**: https://github.com/voxer/webclient

Modern cross-platform client built with React Native and Expo, supporting web, iOS, and Android platforms. This is the current generation web client with a modern tech stack including expo-router for navigation, WatermelonDB for local persistence, and Tailwind CSS for styling. Uses native `fetch` API for HTTP communication and implements real-time streaming via long-lived connections.

**Key Technologies:**
- React Native & Expo
- expo-router for navigation
- WatermelonDB (SQLite) for offline storage
- NativeWind (Tailwind CSS)
- Material-UI components
- expo-audio for audio playback
- Jest for testing

**Supported Platforms:**
- Web browsers
- iOS (via Expo)
- Android (via Expo)

---

### [Unison](unison.md)
**Repository**: https://github.com/voxer/unison

Legacy web client described as "the unified web product." This is an older generation web application built with Backbone.js/Marionette framework. Uses classic web technologies including RequireJS for module loading, Grunt for build automation, and Bower for dependency management. Includes components like video.js for media playback and BinaryJS for real-time communication.

**Key Technologies:**
- Backbone.js & Marionette
- RequireJS (AMD modules)
- Grunt build system
- jQuery & Bootstrap
- Bower package management
- SoundManager2 for audio
- BinaryJS for WebSocket communication

**Status**: Legacy - being replaced by the modern [Web Client](webclient.md)

---

## Client Architecture Overview

### Communication Protocol

All clients communicate with the Voxer backend using:

1. **HTTP/HTTPS REST API**
   - Authentication and session management
   - Message sending and retrieval
   - User profile and settings
   - Contact management
   - See [../Api/README.md](../Api/README.md) for complete API documentation

2. **Real-time Streaming**
   - Long-lived HTTP connection for server-pushed updates
   - Endpoint: `/updates?mode=continuous`
   - Automatic reconnection logic
   - Health monitoring

3. **WebSocket (Legacy)**
   - BinaryJS used in Unison (legacy client)
   - Being phased out in favor of streaming HTTP

### Authentication

- Session-based authentication
- `Rv_session_key` parameter for authenticated requests
- Login via `/cs/login` endpoint
- Session refresh mechanisms

### Data Persistence

- **iOS**: Core Data / SQLite
- **Web Client**: WatermelonDB (SQLite-based)
- **Unison**: Browser localStorage / IndexedDB

### Media Handling

- **Audio Recording**: Platform-native APIs
- **Audio Playback**: expo-audio (Web Client), SoundManager2 (Unison), AVFoundation (iOS)
- **Media Upload**: Direct to Body Store (BS) via REST API
- **Media Download**: Streaming from Body Store with caching

### Offline Support

Clients implement offline capabilities:
- Local message queue for sending when online
- Cached message history
- Synchronization on reconnection
- Optimistic UI updates

## Development Status

| Client | Status | Primary Platform | Technology Stack |
|--------|--------|------------------|------------------|
| iOS | Active | iPhone, Apple Watch | Swift/Objective-C, Native iOS |
| Web Client | Active | Web, iOS, Android | React Native, Expo |
| Unison | Legacy | Web | Backbone.js, jQuery |

## Related Documentation

- [API Documentation](../Api/README.md) - Complete API endpoint reference
- [Server Architecture](../Server/README.md) - Backend services documentation
- [Reference Materials](../Reference/) - Additional technical documents
