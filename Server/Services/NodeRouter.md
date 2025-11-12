# Node Router (NR)

**Repository**: `https://github.com/voxer/server`
**Location**: `node_router/node_router.js`

## Overview

The Node Router handles all client connections and routes messages between clients and backend services. It serves as the primary entry point for mobile and web client traffic.

## Client Connection Management

The service manages WebSocket connections for real-time communication and HTTP/HTTPS endpoints for client requests. It handles session management, authentication, rate limiting, and IP blocklisting.

## Message Routing

The service routes messages between clients and coordinates with the Header Store for message metadata and the Body Store for media content. It maintains message delivery state during transmission.

## Client API

The service provides client-facing REST and WebSocket APIs. It enforces version compatibility and manages feature flags for different client platforms.

## Core Modules

The service consists of several core modules: `client_router.js` handles WebSocket and HTTP client connections, `message_router.js` implements message routing and delivery logic, and `client_api.js` defines client-facing API endpoints. `session_manager.js` manages client session lifecycle while `session.js` implements individual sessions.

## Supporting Modules

Supporting modules include `endpoints.js` for API endpoint definitions, `feature_flags.js` for feature flag management, `rate_limit.js` and `rate_limit_second.js` for rate limiting logic, `refresh_join_voxer.js` for user onboarding flows, and `start_session.js` for session initialization.

## Configuration Requirements

The service requires connection pools for all backend services: Header Store, Body Store, Notification service, Authentication service, Contact service, Purchase/Premium Broker, Short URL service, Transcription service, Spam detector, Message deleter, Business API, Email, Third-party services, People matcher, Signal service, and Data deletion service.

## Network Configuration

The service listens on multiple ports: `secure` for HTTPS/WSS client traffic, `public` for HTTP/WS client traffic, and `ring` for inter-router communication. It advertises external endpoints through `external_secure` and `external_public` configuration.

## Client Version Management

The service enforces minimum client versions for iOS, Android, web browsers, and Windows Phone through version configuration parameters.

## Dependencies

The service integrates with nearly all other services in the system, serving as the central orchestration point for client interactions.

## Scaling Characteristics

The service is horizontally scalable with multiple instances and maintains active client connections. Load balancing occurs at the network layer. Each instance handles thousands of concurrent WebSocket connections.
