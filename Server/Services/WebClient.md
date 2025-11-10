# Web Client Service

## Overview

The Web Client Service serves the browser-based Voxer web application. It provides the static assets, HTML, JavaScript, and CSS for the web interface, as well as server-side rendering and web-specific APIs.

## Entry Point

- **Main File**: `webclient/webclient.js`
- **Service Type**: `webclient`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Static Asset Serving**
   - Serve HTML pages
   - JavaScript bundles
   - CSS stylesheets
   - Images and icons
   - Web fonts

2. **Web Application**
   - Single-page application (SPA)
   - Client-side routing
   - WebSocket connections to Node Router
   - Real-time messaging in browser

3. **Server-Side Features**
   - Session management
   - Authentication redirect
   - Deep link handling
   - Share link previews

4. **Deployment Management**
   - Git-based deployment
   - Version updates
   - Cache busting

## Web Client Architecture

### Frontend
- Modern JavaScript framework (React/Vue/Angular likely)
- WebSocket for real-time communication
- Local storage for offline support
- Service workers for PWA features

### Backend (This Service)
- Express.js web server
- Static file serving
- Proxy to API services
- WebSocket upgrade handling

## Deployment Features

### Git-Based Deployment
- **`git_pull_service.js`** - Automated git pull for updates
- Pull latest web client code
- Trigger rebuild if needed
- Zero-downtime deployments

### Version Management
- Cache busting for assets
- Version-based URLs
- Rollback capabilities
- A/B testing support

## Integration with Node Router

The web client connects to Node Router for:
- User authentication
- WebSocket connections
- Message sending/receiving
- Media upload/download
- API calls

## Browser Support

Likely supports:
- Modern Chrome
- Firefox
- Safari
- Edge
- Mobile browsers (iOS Safari, Chrome Mobile)

## Features

### Core Messaging
- Send/receive messages
- Voice message playback
- Image/video viewing
- Message history

### Web-Specific Features
- Desktop notifications
- Keyboard shortcuts
- Multi-tab synchronization
- Copy/paste media
- Drag-and-drop uploads

### Progressive Web App (PWA)
- Offline support
- Add to home screen
- Background sync
- Push notifications

## API Surface

HTTP endpoints for:
- Serve web application
- Asset delivery
- Authentication callbacks
- Deep link routing
- Share previews

## Configuration

Key settings:
- Static asset paths
- CDN configuration
- WebSocket endpoints
- API proxy settings
- Feature flags for web

## Third-Party Integrations

### Clearbit Proxy
- **`webserver/proxy_to_clearbit.js`** - Proxy for Clearbit API
- Company data enrichment
- Logo fetching
- Contact information

## Performance Optimization

1. **Asset Optimization**
   - Minification
   - Compression (gzip/brotli)
   - Code splitting
   - Lazy loading

2. **Caching**
   - Browser caching headers
   - CDN caching
   - Service worker caching
   - Version-based cache invalidation

3. **Loading Performance**
   - Critical CSS inlining
   - Async script loading
   - Preloading key resources
   - Image optimization

## Security

1. **Content Security Policy**
   - CSP headers
   - XSS protection
   - Frame ancestors policy

2. **HTTPS**
   - TLS/SSL required
   - HSTS headers
   - Secure cookies

3. **Authentication**
   - OAuth flows
   - Session cookies
   - CSRF protection
   - Token refresh

## Monitoring

Track:
- Page load times
- JavaScript errors
- WebSocket connection stability
- API latency from browser
- User engagement metrics

## Scaling Characteristics

- Stateless (can scale horizontally)
- CDN for asset delivery
- Multiple instances for redundancy
- Health checks for load balancing

## Dependencies

- Node Router (WebSocket and API)
- Authentication Service
- CDN for asset delivery
- Static asset build pipeline

## Development Workflow

1. Frontend code in separate repository (likely)
2. Build process generates static assets
3. Assets deployed to webclient service
4. Git pull service updates code
5. Restart or hot reload

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `webclient/`
**Main Service**: `webclient/webclient.js`
**Git Deployment**: `webclient/git_pull_service.js`
**Related**: `webserver/` directory for additional web serving utilities
