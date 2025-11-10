# Growth Service

## Overview

The Growth Service manages user acquisition, viral features, invite systems, and growth-related analytics. It implements various mechanisms to drive user growth and engagement.

## Entry Point

- **Main File**: `growth/growth.js`
- **Service Type**: `growth`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Invite System**
   - Generate invite links
   - Track invite sends and conversions
   - Referral attribution
   - Invite rewards and incentives

2. **Viral Features**
   - Friend recommendations
   - Contact import and matching
   - Social sharing mechanics
   - Viral loops and flows

3. **User Onboarding**
   - New user welcome flows
   - Tutorial and feature discovery
   - Initial contact suggestions
   - First message prompts

4. **Growth Analytics**
   - Conversion funnel tracking
   - Viral coefficient measurement
   - A/B test support
   - Attribution modeling

## Invite Mechanisms

### Invite Channels
- SMS invites
- Email invites
- Social media sharing
- Dynamic links (via Short URL service)
- QR codes

### Invite Tracking
- Unique invite codes per user
- Track invite sends
- Track invite clicks
- Track conversions (sign-ups)
- Multi-touch attribution

## Session Management

Implements session start logic:
- **`handlers/start_session.js`** - Session initialization
- **`start_session.js`** - Session routing
- User onboarding flows
- Feature flag checks for new users

## Pool Client

- **`growth/pool.js`** - Pool client for connecting to Growth service
- Used by Node Router and other services
- Load balanced across multiple instances

## API Surface

HTTP endpoints for:
- Generate invite link
- Send invite
- Track invite event
- Get invite status
- Referral program management
- A/B test assignment

## Growth Features

1. **Referral Programs**
   - Track referrer-referee relationships
   - Reward distribution
   - Fraud detection
   - Campaign management

2. **Social Features**
   - Find friends on Voxer
   - Contact suggestions
   - Group recommendations
   - Trending content

3. **Engagement Campaigns**
   - Re-engagement notifications
   - Feature announcements
   - Promotional campaigns
   - Targeted messaging

## Integration Points

- **Short URL Service** - Generate trackable invite links
- **Contact Service** - Contact import and matching
- **Friend-of-Friend Service** - Social graph analysis
- **Email Service** - Email invite delivery
- **SMS gateways** - SMS invite delivery
- **Analytics** - Event tracking and reporting

## Configuration

- Invite templates (SMS, email)
- Reward structures
- A/B test configurations
- Campaign settings
- Fraud detection thresholds

## Performance Characteristics

- Medium traffic volume
- Burst traffic during viral events
- Mostly asynchronous processing
- Event tracking via queue

## Scaling Characteristics

- Horizontally scalable
- Stateless service design
- Database for persistent tracking
- Queue for async event processing

## Special Considerations

**VXR vs. Standard**: The service includes special handling for the VXR variant of Voxer:
```javascript
if (config.site === "vxr") {
    // No growth features for VXR
    return callback(new Error("no growth for vxr"));
}
```

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `growth/`
**Entry Point**: `growth/growth.js`
**Pool Client**: `growth/pool.js`
