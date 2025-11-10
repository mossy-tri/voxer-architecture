# Voxer Server Services

This document provides an overview of the Voxer server architecture organized around deployable service units.

## Deployment Mechanism

The server uses a **service-based deployment model** with the following characteristics:

### Service Launcher
- **Script**: `voxer-service` bash script in the server repository
- Each service is started with a service name (e.g., `prod-nr1001`, `sjc1-hs2`)
- Service names follow pattern: `[cluster-]<service-type><number>`
- Configuration loaded from `/voxer/etc/json/<service-name>.json`
- Each config specifies a `main` entry point (the JS file to execute)

### Base Service Framework
- **Location**: `lib/voxer_service.js`
- All services inherit from `VoxerService` base class
- Handles common concerns: logging, metrics, graceful shutdown, REPL, health checks
- Services are started via: `node <main-file> <service-name>`

### Service Discovery
- Pool-based architecture for inter-service communication
- Services communicate via HTTP connection pools (using `poolee` library)
- Pool configurations loaded from `/voxer/etc/services/self/<pool-name>.json`
- Load balancing built-in via `poolee_stats_wrapper.js`

## Service Communication Pattern

```
Client Apps
    ↓
Node Router (NR) ← [Entry point for all client traffic]
    ↓
    ├→ Header Store (HS) ← [Message metadata]
    ├→ Body Store (BS) ← [Media storage]
    ├→ Auth Service ← [Authentication]
    ├→ Notification (NMN) ← [Push notifications]
    ├→ Contact Service (CS) ← [Contacts]
    ├→ Growth Service ← [Invites, viral]
    ├→ Business API (BA) ← [Enterprise features]
    ├→ Short URL ← [Link shortening]
    ├→ Transcribe ← [Audio transcription]
    ├→ PBR ← [Payments]
    └→ Other services...
```

## Key Architectural Patterns

1. **Service Independence**: Each service is independently deployable
2. **HTTP-based Communication**: Services communicate via HTTP/HTTPS pools
3. **Horizontal Scaling**: Multiple instances of each service type (e.g., nr1001, nr1002, nr1003)
4. **Configuration-driven**: Service behavior controlled via JSON configs
5. **Shared Libraries**: Common code in `lib/`, shared utilities across services

## Core Services

### Client-Facing Services

- [**Node Router (NR)**](NodeRouter.md) - Front-line client connection handler and message routing
- [**Web Client**](WebClient.md) - Serves the web client application

### Data Storage Services

- [**Header Store (HS)**](HeaderStore.md) - Stores and retrieves message metadata/headers
- [**Body Store (BS)**](BodyStore.md) - Stores and serves media files (audio, images, video)
- [**Data Store**](DataStore.md) - General-purpose data storage abstraction

### Authentication & User Services

- [**Authentication Service (Auth)**](Auth.md) - User authentication and authorization
- [**Contact Service (CS)**](ContactService.md) - Contact list management and sync
- [**Growth Service**](Growth.md) - User acquisition, invites, and viral features

### Notification & Communication

- [**Notification Service (NMN)**](Notification.md) - Push notifications (APNS, FCM/GCM)
- [**Short URL Service**](ShortURL.md) - URL shortening and dynamic links

### Business & Enterprise

- [**Business API (BA)**](BusinessAPI.md) - Business/enterprise API endpoints
- [**Purchase/Premium Broker (PBR)**](PBR.md) - Payment processing, Stripe integration, subscriptions

### Content & AI Services

- [**Transcription Service**](Transcribe.md) - Audio transcription
- [**Friend-of-Friend Service (FOF)**](FriendOfFriend.md) - Social graph and friend recommendations

### Operations & Maintenance

- [**Admin Tool**](AdminTool.md) - Internal admin interface for operations
- [**Message Deleter (MD)**](MessageDeleter.md) - Message deletion and cleanup operations
- [**Metrics Server (MS)**](MetricsServer.md) - Metrics collection and aggregation

## Service Repository Structure

The server repository is organized as follows:

- **`/<service>/`** - Service-specific directories (e.g., `node_router/`, `auth/`, `notification/`)
- **`<service>.js`** - Standalone service entry points at root (e.g., `header_store.js`, `body_server.js`)
- **`lib/`** - Shared libraries and utilities
- **`clients/`** - Client libraries for connecting to other services
- **`services/`** - Shared service logic
- **`data_models/`** - Data models and schemas
- **`common/`** - Common utilities and helpers
- **`tools/`** - Operational tools and scripts
