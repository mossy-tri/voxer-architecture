# Server

GitHub repository: https://github.com/voxer/server

## Overview

Node.js-based backend server infrastructure for the Voxer platform. Implements the core messaging services including the Node Router (NR), Header Store (HS), Body Store (BS), and notification services. Built with Express 3.x, uses Redis (ioredis) for caching/pub-sub, PostgreSQL for persistent data, and integrates with third-party services like Stripe for payments. Recent work includes Stripe integration, message search improvements, conversation summarization features, and message recall functionality.
