# Short URL Service

## Overview

The Short URL Service generates and manages shortened URLs and dynamic links for sharing content, tracking campaigns, and enabling deep linking into the Voxer application.

## Entry Point

- **Main File**: `shorturl/shorturl.js`
- **Service Type**: `shorturl`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **URL Shortening**
   - Generate short URLs
   - Custom short link slugs
   - URL expiration policies
   - Link deactivation

2. **Dynamic Links**
   - Deep linking to app content
   - Platform detection (iOS/Android/Web)
   - App install attribution
   - Fallback URLs for non-app users

3. **Link Tracking**
   - Click tracking
   - Referrer tracking
   - Geographic data
   - Device/platform analytics

4. **Campaign Management**
   - Campaign-specific links
   - UTM parameter preservation
   - A/B test link variants
   - QR code generation

## Dynamic Link Implementations

### Firebase Dynamic Links
- **`dynamic_link/firebase_links_pool.js`** - Firebase integration pool
- **`dynamic_link/short_link.js`** - Short link generation

### Features
- Cross-platform deep linking
- Deferred deep linking (install attribution)
- Link preview customization
- Analytics integration

## URL Shortening Flow

1. **Link Creation**
   - Receive long URL
   - Generate short code
   - Store mapping in database
   - Return short URL

2. **Link Resolution**
   - Receive short URL request
   - Lookup in database
   - Track click event
   - Redirect to destination

3. **Smart Redirect**
   - Detect platform (iOS/Android/Web)
   - Check if app is installed
   - Redirect to app or fallback URL
   - Preserve deep link parameters

## Use Cases

### Invite Links
- Generate unique invite URLs
- Track invite attribution
- Deep link to sign-up flow

### Content Sharing
- Share messages/media
- Share conversation links
- Share profile links

### Marketing Campaigns
- Campaign-specific URLs
- Track campaign performance
- A/B testing different creatives

### QR Codes
- Generate QR codes for events
- In-app QR scanning
- Physical marketing materials

## API Surface

HTTP endpoints for:
- Create short URL
- Get link analytics
- Deactivate link
- Custom slug availability
- Bulk link creation

## Pool Client

- **`shorturl/pool.js`** - Pool client for connecting to Short URL service
- Used by Growth, Node Router, and other services
- Load balanced across instances

## Analytics & Tracking

Captures for each click:
- Timestamp
- IP address and geolocation
- User agent and device
- Referrer URL
- Platform (iOS/Android/Web/Desktop)
- Click-through rate
- Conversion tracking

## Configuration

Key settings:
- Short URL domain
- Firebase Dynamic Links credentials
- Default expiration times
- Custom slug rules
- Rate limiting per user

## Integration Points

- **Firebase Dynamic Links API** - Dynamic link generation
- **Growth Service** - Invite link generation
- **Analytics Service** - Click tracking
- **Node Router** - Deep link handling

## Performance Characteristics

- High read volume (link redirects)
- Lower write volume (link creation)
- Must be very fast (user-facing redirects)
- Heavy caching for popular links
- Database for link mappings

## Scaling Characteristics

- Horizontally scalable
- Stateless redirects
- Cache-friendly (links don't change)
- Can use CDN for redirects
- Database is primary bottleneck

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `shorturl/`
**Main Service**: `shorturl/shorturl.js`
**Pool Client**: `shorturl/pool.js`
