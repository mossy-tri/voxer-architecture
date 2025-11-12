# Purchase/Premium Broker (PBR)

**Repository**: `https://github.com/voxer/server`
**Location**: `pbr/`

## Overview

The Purchase/Premium Broker service handles payment processing, subscription management, and premium feature access. It integrates with Stripe for payment processing and manages consumer and enterprise billing.

## Payment Processing

The service processes credit card payments through Stripe, stores payment methods, handles transaction processing, manages payment failures, and processes refunds.

## Subscription Management

The service creates and cancels subscriptions, handles plan changes and upgrades, manages billing cycles, implements trial periods, and calculates prorated charges.

## Premium Features

The service controls feature access, manages premium tiers, processes add-on purchases, and handles one-time purchases.

## Invoicing

The service generates and delivers invoices, sends payment reminders, and generates receipts.

## Subscription Plans

Consumer plans include monthly and yearly subscriptions with standard and VoxerAI variants. Enterprise plans use custom pricing with seat-based licensing, volume discounts, and annual contracts.

## Stripe Integration

The service uses the Stripe npm package and manages Stripe customers, payment methods, subscriptions, invoices, charges, and refunds. It processes webhooks for payment events, subscription updates, customer updates, and dispute notifications.

## API Surface

The service provides HTTP endpoints for subscription operations, payment method management, one-time payments, billing history retrieval, invoice downloads, and billing information updates.

## Feature Access Control

The service provides APIs for checking subscription status, identifying user plans, determining enabled features, and verifying trial status and expiration.

## Business Logic

The service manages trial periods and conversion to paid plans. It calculates prorated charges for plan changes. It tracks deferred revenue and supports tax calculation.

## Configuration

Key configuration parameters include Stripe API keys for production and test environments, webhook signing secrets, plan definitions and pricing, trial period duration, and grace periods for failed payments.

## Dependencies

The service integrates with the Stripe API for payment processing, Header Store for subscription status, Email Service for receipts and notifications, Business API for enterprise billing, and Metrics Service for revenue analytics.

## Security Considerations

The service maintains PCI compliance by using Stripe for card storage and never storing raw card data. It verifies webhook signatures to prevent replay attacks and implements idempotent processing. Payment metadata is encrypted, all payment operations are audited, and access to financial data is restricted.
