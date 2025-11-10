# Notification Service (NMN)

## Overview

The Notification Service manages push notifications to mobile and desktop clients, including APNS (Apple Push Notification Service) for iOS and FCM/GCM (Firebase Cloud Messaging/Google Cloud Messaging) for Android.

## Entry Point

- **Main File**: `notification/notification_server.js`
- **Service Type**: `nmn`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Push Notification Delivery**
   - iOS push notifications via APNS
   - Android push notifications via FCM/GCM
   - Web push notifications
   - Desktop notifications

2. **Device Token Management**
   - Register device tokens
   - Update device token mappings
   - Remove invalid/expired tokens
   - Track token per user/device

3. **Notification Formatting**
   - Platform-specific payload formatting
   - Localization of notification text
   - Badge count management
   - Sound and alert customization

4. **Delivery Tracking**
   - Track delivery status
   - Handle feedback from APNS/FCM
   - Retry failed deliveries
   - Invalid token cleanup

## Push Notification Platforms

### Apple Push Notification Service (APNS)
- Production and sandbox environments
- Certificate-based authentication
- HTTP/2 protocol
- Priority and expiration settings

### Firebase Cloud Messaging (FCM)
- Replaces legacy GCM
- Token-based authentication
- Topic and group messaging
- Analytics integration

## Notification Types

1. **Message Notifications**
   - New message alerts
   - Sender information
   - Message preview (if enabled)
   - Conversation context

2. **System Notifications**
   - Contact requests
   - Group invitations
   - Mentions and replies
   - Account alerts

3. **Engagement Notifications**
   - Promotional messages
   - Feature announcements
   - Re-engagement campaigns

## API Surface

HTTP endpoints for:
- Send notification
- Register device token
- Unregister device token
- Update notification preferences
- Batch notification sending

## Performance Characteristics

- High throughput (thousands of notifications per second)
- Asynchronous processing
- Connection pooling to APNS/FCM
- Queue-based delivery

## Configuration

Key configuration parameters:
- APNS certificates and keys
- FCM API keys
- Connection pool sizes
- Retry policies
- Rate limits per platform

## Error Handling

1. **Invalid Tokens**
   - Detect and remove invalid device tokens
   - Handle token migration (iOS/Android)
   - Notify upstream services of invalid tokens

2. **Delivery Failures**
   - Retry with exponential backoff
   - Circuit breaker for platform outages
   - Fallback strategies

3. **Platform Limits**
   - Respect APNS/FCM rate limits
   - Payload size validation
   - Throttling strategies

## Scaling Characteristics

- Horizontally scalable
- Stateless processing
- Can partition by platform
- Queue-based for load smoothing

## Dependencies

- APNS HTTP/2 API
- Firebase Cloud Messaging API
- Header Store (for user/device data)
- Localization service
- Metrics service

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `notification/`
**Entry Point**: `notification/notification_server.js`
