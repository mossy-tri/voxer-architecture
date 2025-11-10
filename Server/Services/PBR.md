# Purchase/Premium Broker (PBR)

## Overview

The Purchase/Premium Broker service handles all payment processing, subscription management, and premium feature access. It integrates with Stripe for payment processing and manages both consumer and enterprise billing.

## Entry Points

- **Main Service**: `pbr/pbr.js`
- **Invoicing**: `pbr/invoicer.js`
- **Service Type**: `pbr`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Payment Processing**
   - Credit card processing via Stripe
   - Payment method storage
   - Transaction processing
   - Payment failure handling
   - Refund processing

2. **Subscription Management**
   - Subscription creation and cancellation
   - Plan changes and upgrades
   - Billing cycle management
   - Trial period handling
   - Proration calculations

3. **Premium Features**
   - Feature access control
   - Premium tier management
   - Add-on purchases
   - One-time purchases

4. **Invoicing**
   - Generate invoices
   - Invoice delivery
   - Payment reminders
   - Receipt generation

## Subscription Plans

### Consumer Plans
- **Monthly SKU** - Standard monthly subscription
- **Yearly SKU** - Annual subscription (discounted)
- **Monthly VoxerAI SKU** - AI features monthly
- **Yearly VoxerAI SKU** - AI features annual

Plan configurations stored in:
- `configs/monthly_sku`
- `configs/yearly_sku`
- `configs/monthly_voxerAI_sku`
- `configs/yearly_voxerAI_sku`

### Enterprise Plans
- Custom pricing
- Seat-based licensing
- Volume discounts
- Annual contracts

## Stripe Integration

### Dependencies
- `stripe` npm package (v4.12.0)
- Stripe API keys
- Webhook endpoints

### Stripe Objects
- **Customers** - Stripe customer records
- **Payment Methods** - Saved cards/payment methods
- **Subscriptions** - Recurring billing
- **Invoices** - Billing documents
- **Charges** - Individual payments
- **Refunds** - Payment reversals

### Webhook Handling
Processes Stripe webhooks for:
- Successful payments
- Failed payments
- Subscription updates
- Customer updates
- Dispute notifications

## API Surface

HTTP endpoints for:
- Create subscription
- Update subscription
- Cancel subscription
- Add payment method
- Process one-time payment
- Get billing history
- Download invoice
- Update billing information

## Payment Flows

### New Subscription
1. Client selects plan
2. PBR creates Stripe customer
3. Attach payment method
4. Create Stripe subscription
5. Activate premium features
6. Send confirmation

### Subscription Renewal
1. Stripe attempts charge
2. Webhook notifies PBR
3. Update subscription status
4. Extend premium access
5. Send receipt

### Failed Payment
1. Stripe webhook on failure
2. Update subscription to past_due
3. Send payment failure notice
4. Retry payment per policy
5. Downgrade features after grace period

## Feature Access Control

The PBR service provides APIs for other services to check premium feature access:
- Is user subscribed?
- What plan is user on?
- Which features are enabled?
- Trial status and expiration

## Business Logic

1. **Trial Management**
   - Free trial periods
   - Trial to paid conversion
   - Trial expiration handling

2. **Proration**
   - Calculate prorated charges
   - Plan upgrade credits
   - Plan downgrade handling

3. **Revenue Recognition**
   - Deferred revenue tracking
   - Revenue reporting
   - Tax calculation support

## Configuration

Key settings:
- Stripe API keys (production/test)
- Webhook signing secrets
- Plan definitions and pricing
- Trial period duration
- Grace period for failed payments

## Scaling Characteristics

- Moderate traffic volume
- Critical for revenue
- Must handle payment spikes (e.g., new feature launches)
- Requires high availability
- Idempotency for payment operations

## Dependencies

- **Stripe API** - Payment processing
- **Header Store** - User subscription status
- **Email Service** - Receipts and notifications
- **Business API** - Enterprise billing
- **Metrics Service** - Revenue analytics

## Security Considerations

1. **PCI Compliance**
   - Never store raw card data
   - Use Stripe for card storage
   - Secure API key management

2. **Webhook Verification**
   - Verify Stripe webhook signatures
   - Prevent replay attacks
   - Idempotent processing

3. **Sensitive Data**
   - Encrypt payment metadata
   - Audit all payment operations
   - Restrict access to financial data

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `pbr/`
**Main Service**: `pbr/pbr.js`
**Invoicer**: `pbr/invoicer.js`
