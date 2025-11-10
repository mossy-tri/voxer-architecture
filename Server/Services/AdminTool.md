# Admin Tool

**Repository**: `https://github.com/voxer/server`
**Location**: `admin_tool.js` and `admin_tool_views/`

## Overview

The Admin Tool is an internal web-based interface providing administrative controls, user management capabilities, debugging tools, and operational dashboards. The service is built on Express 3.x with EJS templating and extends the VoxerService base class.

## User Management

The Admin Tool manages Voxer user accounts. It provides user search, profile viewing and editing, account operations including disabling and deleting accounts, password resets, permission management, and access to user activity logs.

## Support Tools

The Admin Tool provides message history viewing, user impersonation for debugging, account recovery functionality, and abuse and moderation tools.

## System Operations

The Admin Tool provides service health monitoring, configuration management, feature flag control, cache invalidation tools, and database maintenance utilities.

## Analytics & Reporting

The Admin Tool provides user statistics, system metrics, growth patterns, revenue reports, and error reporting.

## Web Interface

The Admin Tool is built using Express 3.x and EJS templating. It uses express-partials for modular components and connect-flash for user feedback messages. The view layer includes user management pages, dashboards, report generators, configuration editors, and debug tools.

## Authentication & Authorization

The Admin Tool implements role-based access control with three permission levels: basic user access, admin access, and legal admin access. It enforces password-based authentication with session management and logs all administrative actions. Production environments include IP restrictions. Development environments use simplified authentication with default role assignment.

## Dependencies

The Admin Tool integrates with the Header Store for user data, the Node Router for session management, and the Contact Service for relationship information. It accesses Riak through the Relay Client for raw data operations. External integrations include Google Cloud Platform components, Stripe for payment information, and analytics tracking. Key npm packages include bcrypt, request, and validator.

## Operational Use Cases

The Admin Tool supports investigating user issues, recovering accounts, debugging message delivery, and checking payment status. It provides debugging for production issues, testing feature flags, validating configurations, and investigating performance problems. It handles data exports, account deletions, compliance reports, and audit trail access.

## Security Considerations

The Admin Tool implements authentication and role-based permissions to restrict access. All administrative actions are logged for audit trails. Sensitive data is protected through masking, encrypted connections, and limited data retention. Abuse prevention includes rate limiting, IP restrictions, and alerting.
