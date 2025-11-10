# Node Router (NR)

## Overview

The Node Router is the front-line service that handles all client connections and routes messages between clients and backend services. It serves as the primary entry point for all mobile and web client traffic.

## Entry Point

- **Main File**: `node_router/node_router.js`
- **Service Type**: `nr`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Client Connection Management**
   - WebSocket connections for real-time communication
   - HTTP/HTTPS endpoints for client requests
   - Session management and authentication
   - Rate limiting and IP blocklisting

2. **Message Routing**
   - Routes messages between clients
   - Coordinates with Header Store for message metadata
   - Coordinates with Body Store for media content
   - Maintains message delivery state

3. **Client API**
   - Provides client-facing REST and WebSocket APIs
   - Version compatibility enforcement
   - Feature flag management

## Key Components

### Core Modules

- **`client_router.js`** (89KB) - WebSocket/HTTP client connection handling
- **`message_router.js`** (88KB) - Message routing and delivery logic
- **`client_api.js`** (114KB) - Client-facing API endpoints
- **`session_manager.js`** - Client session lifecycle management
- **`session.js`** - Individual session implementation

### Supporting Modules

- **`endpoints.js`** - API endpoint definitions
- **`feature_flags.js`** - Feature flag management
- **`rate_limit.js`** / **`rate_limit_second.js`** - Rate limiting logic
- **`refresh_join_voxer.js`** - User onboarding flows
- **`start_session.js`** - Session initialization

## Configuration Requirements

The Node Router requires extensive configuration to connect to all backend services:

- `hs_pool` - Header Store pool
- `bs_pool` - Body Store pool
- `nmn_pool` - Notification service pool
- `auth_pool` - Authentication service pool
- `cs_pool` - Contact service pool
- `pbr_pool` - Purchase/Premium Broker pool
- `shorturl_pool` - Short URL service pool
- `transcribe_pool` - Transcription service pool
- `sd_pool` - Spam detector pool
- `md_pool` - Message deleter pool
- `ba_pool` - Business API pool
- `em_pool` - Email pool
- `tp_pool` - Third-party services pool
- `pm_pool` - People matcher pool
- `ss_pool` - Signal service pool
- `datadeletion_pool` - Data deletion service pool

## Network Configuration

- **`secure`** - Port for secure (HTTPS/WSS) client traffic
- **`public`** - Port for public (HTTP/WS) client traffic
- **`ring`** - Port for inter-router communication
- **`external_secure`** - Advertised secure endpoint
- **`external_public`** - Advertised public endpoint

## Client Version Management

Enforces minimum client versions for different platforms:
- `min_version_iphone` - iOS app minimum version
- `min_version_android` - Android app minimum version
- `min_version_web_browser` - Web client minimum version
- `min_version_windows_phone` - Windows Phone minimum version

## Dependencies

The Node Router depends on nearly all other services in the system, making it the central orchestration point for client interactions.

## Scaling Characteristics

- Horizontally scalable (multiple NR instances: nr1001, nr1002, etc.)
- Stateful (maintains active client connections)
- Load balanced at network layer
- Can handle thousands of concurrent WebSocket connections per instance

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `node_router/`
**Entry Point**: `node_router/node_router.js`
