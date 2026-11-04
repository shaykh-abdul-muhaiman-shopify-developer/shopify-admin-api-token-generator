# Test a Shopify Admin API token

[Open Token Generator](https://shopify-token-tool-kohl.vercel.app) · [Setup](SETUP.md) · [FAQ](FAQ.md) · [README](../README.md)

Successful OAuth means Shopify returned a token. Verify a read-only API request before relying on it in an integration. This guide does not require a product write or inventory change.

## Postman request

Create a POST request to your own store:

```text
https://YOUR-STORE.myshopify.com/admin/api/2026-10/graphql.json
```

Headers:

| Header | Value |
| --- | --- |
| Content-Type | application/json |
| X-Shopify-Access-Token | Your private Admin API access token |

Keep the token in a private local secret variable. Do not export it, share a workspace containing it, or include it in screenshots.

Use a raw JSON body:

```json
{
  "query": "query TokenPermissions { currentAppInstallation { accessScopes { handle } } }"
}
```

The response should contain the granted scope handles under:

```text
data.currentAppInstallation.accessScopes
```

This query checks the token's current installation permissions without modifying store records.

## Optional product-read check

Only if the installation has product read access, send:

```json
{
  "query": "query ProductReadTest { products(first: 1) { nodes { id title } } }"
}
```

A valid token can still receive an access-denied error if the requested operation needs scopes the app does not have.

## Interpret the response

- HTTP success alone is not enough: inspect GraphQL's top-level errors and operation-specific errors.
- Check the store domain if Shopify rejects the token.
- Review granted scopes if a resource is denied.
- Empty product results can mean the store has no accessible products; they do not necessarily mean authentication failed.
- Respect API throttling and inspect Shopify error details.
- If you change permissions, release and obtain any needed merchant approval before assuming the token's access changed.

## Put the token in your actual app

Store it in trusted backend secret configuration. Never hardcode it into public JavaScript, Shopify theme assets, a public repository or client-exposed environment variables.

Implement renewal when your token type requires it. The helper does not update your other applications automatically.

An inventory or auto-restock app also needs its own validated business logic, inventory permissions, scheduler, error recovery and uninstall handling. Acquiring a token is only the authentication step.

## Official reference

[Shopify currentAppInstallation query](https://shopify.dev/docs/api/admin-graphql/latest/queries/currentappinstallation)

[Manage access scopes](https://shopify.dev/docs/apps/build/authentication-authorization/manage-access-scopes)
