# @polar-sh/nuxt

Payments and Checkouts made dead simple with Nuxt.

## Installation

### Install the package

Choose your preferred package manager to install the module:

`pnpm add @polar-sh/nuxt`

### Register the module

Add the module to your `nuxt.config.ts`:

```typescript
export default defineNuxtConfig({
  modules: ["@polar-sh/nuxt"],
});
```

## Checkout

Create a Checkout handler which takes care of redirections.

```typescript
// server/routes/api/checkout.post.ts
export default defineEventHandler((event) => {
  const {
    private: { polarAccessToken, polarCheckoutSuccessUrl, polarServer },
  } = useRuntimeConfig();

  const checkoutHandler = Checkout({
    accessToken: polarAccessToken,
    successUrl: polarCheckoutSuccessUrl,
    server: polarServer as "sandbox" | "production",
    theme: "dark" // Enforces the theme - System-preferred theme will be set if left omitted
  });

  return checkoutHandler(event);
});
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
// server/routes/api/portal.get.ts
export default defineEventHandler((event) => {
  const {
    private: { polarAccessToken, polarCheckoutSuccessUrl, polarServer },
  } = useRuntimeConfig();

  const customerPortalHandler = CustomerPortal({
    accessToken: polarAccessToken,
    server: polarServer as "sandbox" | "production",
    getCustomerId: (event) => {
      return Promise.resolve("9d89909b-216d-475e-8005-053dba7cff07");
    },
  });

  return customerPortalHandler(event);
});
```

## Webhooks

A simple utility which resolves incoming webhook payloads by signing the webhook secret properly.

```typescript
// server/routes/webhook/polar.post.ts
export default defineEventHandler((event) => {
  const {
    private: { polarWebhookSecret },
  } = useRuntimeConfig();

  const webhooksHandler = Webhooks({
    webhookSecret: polarWebhookSecret,
    onPayload: async (payload: any) => {
      // Handle the payload
      // No need to return an acknowledge response
    },
  });

  return webhooksHandler(event);
});
```

### Payload Handlers

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
