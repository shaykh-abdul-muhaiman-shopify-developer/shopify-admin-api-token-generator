# Shopify custom app setup

[Open the tool](https://shopify-token-tool-kohl.vercel.app) · [Back to README](../README.md)

## Before you begin

You need permission to develop and install the app on the target store. This is a standalone OAuth flow for your own custom app. It does not create an app or bypass Shopify approvals.

## 1. Create your Shopify app

Open the Shopify Dev Dashboard, choose the correct organization, and create an app. Give it a name that identifies your integration.

## 2. Select API permissions

Select only the access your integration needs. For a product-read test, use `read_products`. Product writes require appropriate write permission.

A matching write scope includes read access. Some scopes also require additional Shopify approval or protected customer data eligibility.

## 3. Configure URLs

App URL:

```text
https://shopify-token-tool-kohl.vercel.app
```

Allowed redirection URL:

```text
https://shopify-token-tool-kohl.vercel.app/api/auth/callback
```

Disable embedding in Shopify admin for this standalone test. Release the app version after saving the configuration.

## 4. Configure distribution and install

For a Partner app requiring distribution selection, open the app's distribution settings. A custom single-store integration normally uses custom distribution.

**Distribution selection cannot be changed later.** Choose based on the intended app audience; do not choose custom distribution for an app you intend to distribute publicly.

Enter the intended store's permanent `myshopify.com` domain, generate the installation link, and open the complete link in your browser. Install the app on the correct store.

Store-organization apps have a different installation path and may not show a Distribution card. Follow Shopify's instructions for that organization. Do not assume all custom app types support identical authentication options.

## 5. Obtain credentials

Copy the Client ID and Client Secret from the same app's settings.

Do not put these in a public repository or screenshots. Do not share a Client Secret through issue reports.

## 6. Use the website

Enter:

1. The store's permanent `myshopify.com` domain.
2. Your app's Client ID.
3. Your app's Client Secret.

Click **Connect Shopify**. Complete Shopify authorization if prompted. Keep the same browser session throughout the flow.

## 7. Save the result

Copy the access token. Review the granted scopes and expiry information. If Shopify returns a refresh token, keep that private too.

Save credentials in your own trusted backend's secret configuration. Do not expose an Admin API token in storefront code.

The result is not a persistent token vault. Save it before leaving. Refreshing the callback does not generate a new result.

## Later: move to your own app backend

You can update the App URL and allowed redirect URLs when your actual application is ready, then release the configuration.

Your backend must handle future OAuth callbacks and any required token renewal. A URL change alone is not a token revocation operation, but it is not a guarantee against other causes of token invalidation.

## Current limitation

The hosted tool currently includes `read_products` in its authorization request. Shopify-managed installation can grant released and approved app configuration scopes. Always inspect the result; do not assume all configured permissions have been granted.

See [Shopify's standalone authentication guide](https://shopify.dev/docs/apps/build/authentication-authorization/authenticate-standalone-apps) and [distribution guide](https://shopify.dev/docs/apps/launch/distribution/select-distribution-method).
