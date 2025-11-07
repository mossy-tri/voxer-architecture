# Authentication API

User registration, password management, email verification, and SSO integration.

## Endpoints

- `2/cs/signup` - Register a new user account
- `3/cs/forgot_password` - Send password reset email
- `3/cs/reset_password` - Reset password using email token
- `3/cs/verify_email` - Send email verification
- `/2/cs/resend_verify_email` - Resend verification email
- `3/cs/sso_provider` - Check SSO provider for email
- `tp/google/connect/1` - Link Google account
