# @polar-sh/fastify

Payments and Checkouts made dead simple with Fastify.

`pnpm install @polar-sh/fastify zod`

## Checkout

Create a Checkout handler which takes care of redirections.

```typescript
import fastify from "fastify";
import { Checkout } from "@polar-sh/fastify";

fastify().get(
  "/checkout",
  Checkout({
    accessToken: "xxx", // Or set an environment variable to POLAR_ACCESS_TOKEN
    successUrl: process.env.SUCCESS_URL,
    server: "sandbox", // Use sandbox if you're testing Polar - omit the parameter or pass 'production' otherwise
    theme: "dark" // Enforces the theme - System-preferred theme will be set if left omitted
  }),
);
```

### Query Params

Pass query params to this route.

- products `?products=123`
- customerId (optional) `?products=123&customerId=xxx`
- customerExternalId (optional) `?products=123&customerExternalId=xxx`
- customerEmail (optional) `?products=123&customerEmail=janedoe@gmail.com`
- customerName (optional) `?products=123&customerName=Jane`
- metadata (optional) `URL-Encoded JSON string`

## Customer Portal

Create a customer portal where your customer can view orders and subscriptions.

```typescript
import fastify from "fastify";
import { CustomerPortal } from "@polar-sh/fastify";

fastify().get(
  "/portal",
  CustomerPortal({
    accessToken: "xxx", // Or set an environment variable to POLAR_ACCESS_TOKEN
    getCustomerId: (event) => "", // Fuction to resolve a Polar Customer ID
    server: "sandbox", // Use sandbox if you're testing Polar - omit the parameter or pass 'production' otherwise
  }),
);
```

## Webhooks

A simple utility which resolves incoming webhook payloads by signing the webhook secret properly.

```typescript
import fastify from 'fastify'
import { Webhooks } from "@polar-sh/fastify";

fastify.post('/polar/webhooks', Webhooks({
  webhookSecret: process.env.POLAR_WEBHOOK_SECRET!,
  onPayload: async (payload) => /** Handle payload */,
}))
```

#### Payload Handlers

The Webhook handler also supports granular handlers for easy integration.

- onCheckoutCreated: (payload) =>
- onCheckoutUpdated: (payload) =>
- onOrderCreated: (payload) =>
- onOrderUpdated: (payload) =>
- onOrderPaid: (payload) =>
- onSubscriptionCreated: (payload) =>
- onSubscriptionUpdated: (payload) =>
- onSubscriptionActive: (payload) =>
- onSubscriptionCanceled: (payload) =>
- onSubscriptionRevoked: (payload) =>
- onProductCreated: (payload) =>
- onProductUpdated: (payload) =>
- onOrganizationUpdated: (payload) =>
- onBenefitCreated: (payload) =>
- onBenefitUpdated: (payload) =>
- onBenefitGrantCreated: (payload) =>
- onBenefitGrantUpdated: (payload) =>
- onBenefitGrantRevoked: (payload) =>
- onCustomerCreated: (payload) =>
- onCustomerUpdated: (payload) =>
- onCustomerDeleted: (payload) =>
- onCustomerStateChanged: (payload) =>
