# Notification Service (NMN)

**Repository**: `https://github.com/voxer/server`
**Location**: `notification/notification_server.js`

## Overview

The Notification Service manages push notifications to mobile and desktop clients. It handles APNS for iOS and FCM/GCM for Android platforms.

## Push Notification Delivery

The service sends notifications to iOS clients via APNS, Android clients via FCM/GCM, web browsers, and desktop applications. It processes notifications asynchronously and maintains connection pools to platform providers.

## Device Token Management

The service registers device tokens, updates device token mappings, removes invalid or expired tokens, and tracks tokens per user and device.

## Notification Formatting

The service formats payloads for each platform, localizes notification text, manages badge counts, and customizes sounds and alerts.

## Delivery Tracking

The service tracks delivery status, processes feedback from APNS and FCM, retries failed deliveries, and removes invalid tokens based on platform feedback.

## Push Notification Platforms

APNS operates in production and sandbox environments using certificate-based authentication over HTTP/2. FCM uses token-based authentication and provides topic and group messaging with analytics integration.

## Notification Types

Message notifications include new message alerts, sender information, message previews, and conversation context. System notifications include contact requests, group invitations, mentions, replies, and account alerts. Engagement notifications include promotional messages, feature announcements, and re-engagement campaigns.

## API Surface

The service provides HTTP endpoints for sending notifications, registering device tokens, unregistering device tokens, updating notification preferences, and batch notification sending.

## Error Handling

The service detects and removes invalid device tokens, handles token migration, and notifies upstream services. It retries failed deliveries with exponential backoff, implements circuit breakers for platform outages, and validates payload sizes to respect platform rate limits.

## Configuration

Key configuration parameters include APNS certificates and keys, FCM API keys, connection pool sizes, retry policies, and rate limits per platform.

## Dependencies

The service integrates with APNS HTTP/2 API, Firebase Cloud Messaging API, Header Store for user and device data, localization service, and metrics service.

## Scaling Characteristics

The service is horizontally scalable and stateless. Processing can be partitioned by platform. Queues smooth load distribution.
