# Server

GitHub repository: https://github.com/voxer/server

## Overview

Node.js-based backend server infrastructure for the Voxer platform. Implements the core messaging services including the Node Router (NR), Header Store (HS), Body Store (BS), and notification services. Built with Express 3.x, uses Redis (ioredis) for caching/pub-sub, PostgreSQL for persistent data, and integrates with third-party services like Stripe for payments. Recent work includes Stripe integration, message search improvements, conversation summarization features, and message recall functionality.

## Dependencies

### Data Storage & Caching
- **RIAK** - Primary distributed data store for profiles, messages, and core application data
- **Redis** (ioredis) - Caching layer and pub/sub messaging for real-time communication between services
- **PostgreSQL** - Metrics and analytics data storage (via zag-backend-pg)

### Push Notifications
- **APNS** (Apple Push Notification Service) - Push notifications for iOS clients
- **FCM** (Firebase Cloud Messaging) - Push notifications for Android clients

### Third-Party Services
- **Stripe** - Payment processing and subscription management
- **SendGrid** - Transactional email delivery
- **Google Cloud Platform** - Cloud services including Cloud Run, Cloud Spanner, and Cloud Storage

### Web Server
- **Express 3.x** - HTTP server framework for web and API endpoints
