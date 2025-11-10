# Admin Tool

## Overview

The Admin Tool is an internal web-based interface for Voxer operations, support, and engineering teams. It provides administrative controls, user management, debugging tools, and operational dashboards.

## Entry Point

- **Main File**: `admin_tool.js`
- **Service Type**: `admin`
- **Base Class**: Extends `VoxerService`
- **File Size**: 161KB (extensive functionality)
- **Framework**: Express 3.x with EJS templating

## Key Responsibilities

1. **User Management**
   - View and edit user profiles
   - User account operations (disable, delete, etc.)
   - Reset passwords
   - Manage user permissions
   - View user activity logs

2. **Support Tools**
   - Customer support case management
   - User impersonation (for debugging)
   - Message history viewing
   - Account recovery
   - Abuse and moderation tools

3. **System Operations**
   - Service health monitoring
   - Configuration management
   - Feature flag control
   - Cache invalidation
   - Database maintenance tools

4. **Analytics & Reporting**
   - User statistics
   - System metrics
   - Revenue reports
   - Growth analytics
   - Error reporting

## Web Interface

### Technology Stack
- **Express 3.x** - Web framework
- **EJS** - Template engine (via `ejs` package)
- **express-partials** - Partial template support
- **connect-flash** - Flash messages
- **Bootstrap** - UI framework (likely)

### View Templates
Located in `admin_tool_views/` directory with 36 view files including:
- User management pages
- Dashboard views
- Report generators
- Configuration editors
- Debug tools

## Authentication & Authorization

### Role-Based Access
Three role levels (defined in `admin_tool.js`):
- **user** (level 10) - Basic access
- **admin** (level 100) - Admin operations
- **legaladmin** (level 1000) - Full access including legal/compliance

### Security
- Password-based authentication
- Session management
- Role enforcement
- Audit logging of admin actions
- IP restrictions (production)

### Environment-Specific
- Development: Simple password auth with default `dev_role`
- Production: Password file-based authentication

## Key Features

### User Operations
- Search users
- View user details
- Edit user profiles
- Manage user settings
- View message history
- Badge count management (via `BadgeCount` model)

### Content Moderation
- Review reported content
- User blocking/suspension
- Message deletion
- Abuse pattern detection

### System Management
- Service configuration
- Feature flags
- A/B test setup
- Cache management
- Database queries

### Analytics
- User metrics
- System health
- Revenue tracking
- Growth funnel
- Error rates

## Dependencies

### Internal Services
- **Header Store** (US client) - User data
- **Node Router** (NR client) - Session management
- **Contact Service** - Contact data
- **Riak** - Raw data access
- **Relay Client** - Riak communication

### npm Packages
- `express` - Web framework
- `ejs` - Templating
- `bcrypt` - Password hashing
- `request` - HTTP client
- `validator` - Input validation
- `underscore` - Utility functions

## API Integrations

- **GCP Components** - Google Cloud Platform integration
- **CloudRun Client** - Cloud Run service management
- **Stripe** (via PBR) - Payment information viewing
- **Analytics Sender** - Event tracking

## Access Control

Production access controlled via:
- Password file authentication
- Role-based permissions
- Restricted IP ranges
- Session timeout
- Audit logging

## Operational Use Cases

### Customer Support
- Investigate user issues
- Recover accounts
- Debug message delivery
- Check payment status

### Engineering
- Debug production issues
- Test feature flags
- Validate configurations
- Performance investigation

### Legal/Compliance
- Data export for legal requests
- Account deletion
- Compliance reporting
- Audit trail access

## Configuration

Service configuration includes:
- Web server port
- Authentication settings
- Database connections
- Service pool configurations
- Feature flags

## Security Considerations

1. **Access Control**
   - Strong authentication required
   - Role-based permissions
   - Action audit logging
   - Session management

2. **Data Protection**
   - Sensitive data masking
   - Encrypted connections
   - Limited data retention in UI
   - No credential storage

3. **Abuse Prevention**
   - Rate limiting
   - IP restrictions
   - Action logging
   - Alerting on suspicious activity

## Code Location

**Repository**: `https://github.com/voxer/server`
**Main File**: `admin_tool.js`
**Views Directory**: `admin_tool_views/`
