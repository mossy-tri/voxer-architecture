# Authentication Service (Auth)

**Repository**: `https://github.com/voxer/server`
**Location**: `auth/auth.js`

## Overview

The Authentication Service handles user authentication, authorization, and session management across the Voxer platform. It provides centralized identity and access control.

## User Authentication

The service validates username and password credentials, implements multi-factor authentication, and integrates OAuth for third-party authentication including Facebook, Google, and Apple Sign-In. It generates and validates JWT tokens for authenticated sessions.

## Session Management

The service creates and validates user sessions, tracks active sessions, and implements session expiration and renewal policies. Sessions can be revoked when required.

## Authorization

The service verifies user permissions, implements role-based access control, and enforces enterprise and team access policies. It manages API keys for programmatic access.

## Authentication Methods

Password-based authentication uses bcrypt hashing with configurable work factors and implements password reset flows. Token-based authentication uses JWT with refresh token support and configurable expiration policies. Third-party authentication supports Facebook, Google, Apple Sign-In, and SAML for enterprise environments.

## Security

The service implements bcrypt password hashing, JWT token signing and verification, and JWKS key rotation. It provides rate limiting for login attempts, account lockout policies after failed attempts, and IP-based rate limiting.

## API Surface

The service provides HTTP endpoints for user login, user logout, token refresh, password reset, account verification, session validation, and permission checks.

## Dependencies

The service uses bcrypt for password hashing, jsonwebtoken for JWT creation and validation, jwks-rsa for JWKS key management, and xml-crypto for SAML signature verification. It integrates with the Header Store for user data, the Metrics service, and the Email service for password resets.

## Configuration

Key configuration parameters include JWT signing keys, token expiration times, OAuth client credentials, password policy settings, and rate limiting thresholds.

## Scaling Characteristics

The service is horizontally scalable and stateless through JWT-based authentication. Validation results can be cached.
