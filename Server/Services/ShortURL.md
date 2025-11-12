# Short URL Service

**Repository**: `https://github.com/voxer/server`
**Location**: `shorturl/`

## Overview

The Short URL Service generates and manages shortened URLs and dynamic links for sharing content, tracking campaigns, and deep linking into the Voxer application.

## URL Shortening

The service generates short URLs from long URLs, stores mappings in a database, and handles redirection. Custom slugs can be specified. URLs support expiration policies and can be deactivated. When a short URL is accessed, the service looks up the mapping, tracks the click event, and redirects to the destination.

## Dynamic Links

The service provides cross-platform deep linking with platform detection for iOS, Android, and web. It implements deferred deep linking for app install attribution and provides fallback URLs for non-app users. The service preserves deep link parameters during redirection and supports link preview customization. Firebase Dynamic Links integration is implemented through `dynamic_link/firebase_links_pool.js` and `dynamic_link/short_link.js`.

## Link Tracking

The service captures timestamp, IP address and geolocation, user agent and device information, referrer URL, and platform data for each click. Click-through rates and conversion tracking are recorded.

## Campaign Management

The service generates campaign-specific links and preserves UTM parameters. A/B test link variants are supported. QR codes can be generated for links.

## API Surface

The service provides HTTP endpoints for creating short URLs, retrieving link analytics, deactivating links, checking custom slug availability, and bulk link creation.

## Pool Client

The service provides a pool client in `shorturl/pool.js` for connecting to Short URL service instances. The pool client is used by Growth, Node Router, and other services with load balancing across instances.

## Configuration

Key configuration parameters include the short URL domain, Firebase Dynamic Links credentials, default expiration times, custom slug rules, and rate limiting per user.

## Dependencies

The service integrates with Firebase Dynamic Links API for dynamic link generation, Growth Service for invite link generation, Analytics Service for click tracking, and Node Router for deep link handling.

## Performance Characteristics

The service handles high read volume for link redirects and lower write volume for link creation. Redirects are user-facing and cached heavily for popular links. The database stores link mappings and represents the primary bottleneck.

## Scaling Characteristics

The service is horizontally scalable and stateless for redirects. Links are cache-friendly and can use CDN for redirects.
