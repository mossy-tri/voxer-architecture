# Web Client Service

**Repository**: `https://github.com/voxer/server`
**Location**: `webclient/`

## Overview

The Web Client Service serves the browser-based Voxer web application. It provides static assets, HTML, JavaScript, and CSS for the web interface, along with server-side rendering and web-specific APIs.

## Static Asset Serving

The service serves HTML pages, JavaScript bundles, CSS stylesheets, images, icons, and web fonts to client browsers.

## Web Application

The service hosts a single-page application with client-side routing. The application establishes WebSocket connections to Node Router for real-time messaging in the browser.

## Server-Side Features

The service manages user sessions, handles authentication redirects, processes deep links, and generates share link previews.

## Deployment

The service uses git-based deployment with automated updates through `git_pull_service.js`. It supports version management, cache busting for assets, and rollback capabilities.

## Architecture

The frontend uses JavaScript with WebSocket for real-time communication, local storage for offline support, and service workers for PWA features. The backend runs an Express.js web server for static file serving, proxying to API services, and WebSocket upgrade handling.

## Integration with Node Router

The web client connects to Node Router for user authentication, WebSocket connections, message sending and receiving, media upload and download, and API calls.

## Features

The service provides core messaging functions including message send and receive, voice message playback, image and video viewing, and message history. Web-specific features include desktop notifications, keyboard shortcuts, multi-tab synchronization, copy and paste media, and drag-and-drop uploads. PWA features include offline support, add to home screen, background sync, and push notifications.

## API Surface

The service provides HTTP endpoints for serving the web application, asset delivery, authentication callbacks, deep link routing, and share previews.

## Third-Party Integrations

The service includes `webserver/proxy_to_clearbit.js` for proxying Clearbit API requests. This integration provides company data enrichment, logo fetching, and contact information.

## Performance

The service implements asset minification, compression, code splitting, and lazy loading. It configures browser caching headers, CDN caching, service worker caching, and version-based cache invalidation. It uses critical CSS inlining, async script loading, preloading for key resources, and image optimization.

## Security

The service implements Content Security Policy headers, XSS protection, and frame ancestors policy. It requires TLS/SSL with HSTS headers and secure cookies. Authentication uses OAuth flows, session cookies, CSRF protection, and token refresh.

## Monitoring

The service tracks page load times, JavaScript errors, WebSocket connection stability, API latency from browser, and user engagement metrics.

## Scaling Characteristics

The service is stateless and can scale horizontally. It uses CDN for asset delivery and supports multiple instances for redundancy with health checks for load balancing.

## Dependencies

The service depends on Node Router for WebSocket and API, the Authentication Service, CDN for asset delivery, and a static asset build pipeline.

## Configuration

Key configuration parameters include static asset paths, CDN configuration, WebSocket endpoints, API proxy settings, and feature flags.
