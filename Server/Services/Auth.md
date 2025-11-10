# Authentication Service (Auth)

## Overview

The Authentication Service handles user authentication, authorization, and session management across the Voxer platform. It provides centralized identity and access control.

## Entry Point

- **Main File**: `auth/auth.js`
- **Service Type**: `auth`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **User Authentication**
   - Username/password validation
   - Multi-factor authentication
   - OAuth integration
   - Third-party authentication (Facebook, Google, etc.)
   - JWT token generation and validation

2. **Session Management**
   - Session creation and validation
   - Session expiration and renewal
   - Active session tracking
   - Session revocation

3. **Authorization**
   - Permission verification
   - Role-based access control
   - Enterprise/team access policies
   - API key management

4. **Security**
   - Password hashing (bcrypt)
   - Token signing and verification (jsonwebtoken)
   - JWKS key rotation (jwks-rsa)
   - Rate limiting for login attempts
   - Account lockout policies

## Authentication Methods

### Password-Based
- Bcrypt password hashing
- Password strength requirements
- Password reset flows

### Token-Based
- JWT (JSON Web Tokens)
- Refresh token support
- Token expiration policies

### OAuth/Third-Party
- Facebook authentication
- Google authentication
- Apple Sign-In
- SAML for enterprise (via xml-crypto)

## API Surface

HTTP endpoints for:
- User login
- User logout
- Token refresh
- Password reset
- Account verification
- Session validation
- Permission checks

## Dependencies

### npm Packages
- `bcrypt` - Password hashing
- `jsonwebtoken` - JWT creation and validation
- `jwks-rsa` - JWKS key management
- `xml-crypto` - SAML signature verification

### Services
- Header Store (user data)
- Metrics service
- Email service (for password resets)

## Security Features

1. **Password Security**
   - Bcrypt with configurable work factor
   - No plaintext password storage
   - Secure password reset mechanism

2. **Token Security**
   - Signed JWTs
   - Short-lived access tokens
   - Secure token storage recommendations

3. **Rate Limiting**
   - Login attempt throttling
   - Account lockout after failed attempts
   - IP-based rate limiting

## Configuration

Key configuration parameters:
- JWT signing keys
- Token expiration times
- OAuth client credentials
- Password policy settings
- Rate limiting thresholds

## Scaling Characteristics

- Horizontally scalable
- Stateless (JWT-based)
- Can cache validation results
- Lightweight service

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `auth/`
**Entry Point**: `auth/auth.js`
