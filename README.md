# Voxer Architecture Overview

This repository contains comprehensive documentation of the Voxer platform architecture, including client applications, backend services, and API specifications.

## Documentation Sections

### [Clients](Clients/README.md)

Client applications that connect to the Voxer platform across different platforms:

- **[iOS Client](Clients/ios.md)** - Native iPhone and Apple Watch application built with Swift/Objective-C
- **[Web Client](Clients/webclient.md)** - Modern cross-platform client using React Native and Expo (web, iOS, Android)
- **[Unison](Clients/unison.md)** - Legacy web client built with Backbone.js/Marionette

All clients implement real-time messaging with offline support, local data persistence, and seamless synchronization.

### [Server](Server/README.md)

Node.js-based backend infrastructure implementing the core Voxer messaging services. The server uses a microservices architecture with 17+ independently deployable services including:

- **Node Router (NR)** - Client connection handling and message routing
- **Header Store (HS)** - Message metadata storage using PostgreSQL and Redis
- **Body Store (BS)** - Media storage and streaming with Riak
- **Authentication, Notification, Contact, Growth, and Business services**
- **Payment processing (PBR)** - Stripe integration for subscriptions

See [Server/Services/README.md](Server/Services/README.md) for detailed service architecture documentation.

### [API](Api/README.md)

Complete REST API documentation organized by functional area:

- **Authentication & Session** - User registration, login, and session management
- **Messaging** - Send/receive messages, reactions, search, and AI features
- **Chat Management** - Thread creation, participants, and settings
- **User Profile & Contacts** - Profile management and contact lists
- **Teams & Billing** - Team administration and subscription management
- **Timeline & Sync** - Real-time updates and data synchronization

The API uses versioned endpoints (e.g., `/2/cs/login`) with session-based authentication via `Rv_session_key` parameters.

## Reference

### Source Code Repositories

- **[voxer/server](https://github.com/voxer/server)** - Node.js backend server infrastructure
- **[voxer/ios](https://github.com/voxer/ios)** - Native iOS client application
- **[voxer/webclient](https://github.com/voxer/webclient)** - Modern React Native/Expo client (web, iOS, Android)
- **[voxer/unison](https://github.com/voxer/unison)** - Legacy web client (Backbone.js)

### Architecture Documents

- [Server Architecture, Design Principles, and Coding Style](Reference/Server%20Architecture%2C%20Design%20Principles%2C%20and%20Coding%20Style.md)
  Documents the production operation for the Voxer service, including scalability principles, data storage strategies (Riak, Data Store, Redis), service-oriented architecture, and detailed coding standards for the Node.js codebase. Emphasizes building a telecommunications infrastructure designed for 24/7 availability with tolerance for single machine/process failures.

- [How_Voxer_Works.pptx.pdf](Reference/How_Voxer_Works.pptx.pdf)
  Explains Voxer's core innovation as a communication protocol combining synchronous (live) and store-and-forward messaging without their respective drawbacks. Details how the system enables live message delivery without requiring end-to-end connections, using a store-and-stream protocol with conversation-based routing.

- [Server Architecture.pdf](Reference/Server%20Architecture.pdf)
  Presents the high-level technology choices (Node.js, Riak, Redis, SmartOS) and visual diagrams of the live messaging flow, consistent hashing implementation, and component interactions between NR, HS, BS, and NS services. Illustrates how the system achieves scalability through horizontal scaling and distributed architecture.

- [Voxer architecture and feature list.docx.pdf](Reference/Voxer%20architecture%20and%20feature%20list.docx.pdf)
  Comprehensive feature list describing Voxer's capabilities including live/messaged voice integration, offline operation, multi-device support, and advanced incoming call management (screen, join, catch-up-to-live). Details the ring-based cluster architecture and data storage across Router Ring, Header Store Ring, Body Store Ring, and Riak clusters.

- [Voxer Architecture Discovery.pdf](Reference/Voxer%20Architecture%20Discovery.pdf)
  Architecture discovery report from Oct 2021 providing detailed analysis of the current system and recommendations for migration to Google Cloud. Identifies scalability issues with JSON-based configuration and Redis compaction, proposing solutions using etcd/Redis for configuration management, Spanner/BigTable for database replacement, and GCS optimization for media storage.
