# Growth Service

**Repository**: `https://github.com/voxer/server`
**Location**: `growth/growth.js`

## Overview

The Growth Service manages user acquisition, viral features, invite systems, and growth-related analytics. It extends the VoxerService base class and implements mechanisms to drive user growth and engagement.

## Invite System

The service generates invite links, tracks invite sends and conversions, implements referral attribution, and manages invite rewards and incentives.

## Viral Features

The service provides friend recommendations, contact import and matching, social sharing mechanics, and viral loops.

## User Onboarding

The service implements new user welcome flows, tutorials and feature discovery, initial contact suggestions, and first message prompts.

## Growth Analytics

The service tracks conversion funnels, measures viral coefficients, supports A/B testing, and implements attribution modeling.

## Invite Channels

The service delivers invites through SMS, email, social media sharing, dynamic links via the Short URL service, and QR codes. Each user receives unique invite codes for tracking sends, clicks, and conversions. The service implements multi-touch attribution.

## Session Management

The service implements session start logic through `handlers/start_session.js` and `start_session.js`. It routes user onboarding flows and checks feature flags for new users.

## Referral Programs

The service tracks referrer-referee relationships, distributes rewards, detects fraud, and manages campaigns.

## Social Features

The service provides friend finding, contact suggestions, group recommendations, and trending content.

## Engagement Campaigns

The service implements re-engagement notifications, feature announcements, promotional campaigns, and targeted messaging.

## API Surface

The service provides HTTP endpoints for invite link generation, invite sending, invite event tracking, invite status retrieval, referral program management, and A/B test assignment.

## Dependencies

The service integrates with the Short URL Service for trackable invite links, the Contact Service for contact import and matching, the Friend-of-Friend Service for social graph analysis, the Email Service for email delivery, SMS gateways for SMS delivery, and analytics systems for event tracking and reporting. The pool client in `growth/pool.js` connects the Node Router and other services to the Growth Service with load balancing across multiple instances.

## Configuration

Key configuration parameters include invite templates for SMS and email, reward structures, A/B test configurations, campaign settings, and fraud detection thresholds.

## Performance Characteristics

The service handles medium traffic volume with burst traffic during viral events. Processing is mostly asynchronous with event tracking via queue.

## Scaling Characteristics

The service is horizontally scalable with stateless design. It uses a database for persistent tracking and a queue for async event processing.

## Special Considerations

The service disables growth features for the VXR variant of Voxer:
```javascript
if (config.site === "vxr") {
    // No growth features for VXR
    return callback(new Error("no growth for vxr"));
}
```
