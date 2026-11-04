# Shopify Admin API Token Generator — Custom App OAuth Tool

**Get your own Shopify custom app's Admin API access token without building an OAuth backend from scratch.**

Enter your store domain, Client ID, and Client Secret, complete Shopify authorization, and copy your token. Review the exact granted permissions on the result page.

### [Open Shopify Token Generator →](https://shopify-token-tool-kohl.vercel.app)

[Setup guide](docs/SETUP.md) · [FAQs by category](docs/FAQ.md) · [API usage examples](docs/USAGE.md) · [Security & privacy](SECURITY.md)

**Did this tool save you an OAuth setup? Give the repository a ⭐ using GitHub's Star button.** It helps other Shopify developers discover the project. Starring is optional; it never changes access to the tool.

Built with Next.js by [Shaykh Abdul Muhaiman](https://github.com/shaykh-abdul-muhaiman-shopify-developer).

> Independent developer tool. Not affiliated with or endorsed by Shopify. The hosted tool is under active development. This repository currently contains its user documentation; the application source has not yet been published here.

## Find what you need

- [Why use the tool?](#why-use-this-shopify-token-generator)
- [Who is it for?](#who-is-it-for)
- [Get a Shopify Admin API access token](#quick-start)
- [Security protections](#security-protections-explained)
- [Token expiry and offline access](#does-the-token-expire-after-24-hours)
- [Troubleshooting](#troubleshooting)
- [Support the project](#support-the-project)

## Common integration use cases

An authorized Admin API token can be used by your own backend for product tools, catalog synchronization, inventory integrations, reporting, and custom automations, subject to its granted permissions.

For example, an auto-restock application needs its own business logic, inventory permissions, scheduling, and token lifecycle handling. This website supplies a token; it does not build or operate the restock integration.

[See a read-only GraphQL token test and permissions query →](docs/USAGE.md)

## Why use this Shopify token generator?

You have created a custom app and have its Client ID and Client Secret—but still need an Admin API access token.

For an OAuth-based custom integration, getting that token otherwise means handling authorization redirects, callback validation, and a server-side code exchange. This tool handles those steps through a guided browser flow.

**You configure your Shopify app. The website handles the OAuth exchange.**

| Without a hosted helper | With this tool |
| --- | --- |
| Write authorization and callback code | Enter three credential fields |
| Host a callback and token-exchange endpoint | Use the supplied callback URL |
| Exchange the authorization code manually or through your own backend | Complete Shopify authorization in your browser |
| Inspect raw token responses | Copy the token from a dedicated result screen |
| Query the installation's permissions | Review granted scopes as a list |

Using this website is optional. Shopify's official libraries, your own backend, and tools such as Postman are valid alternatives.

## Who is it for?

- **Shopify developers** testing custom apps and Admin API integrations.
- **Freelancers and agencies** working on stores they are authorized to manage.
- **Store owners working with a developer** who need help completing the token exchange.

If your application already manages Shopify authentication, you may not need this tool.

## Quick start

### 1. Configure and install your app

Create your app in the Shopify Dev Dashboard, select the required API scopes, and configure these URLs:

| Shopify setting | Value |
| --- | --- |
| App URL | `https://shopify-token-tool-kohl.vercel.app` |
| Allowed redirection URL | `https://shopify-token-tool-kohl.vercel.app/api/auth/callback` |

Use standalone app behavior for this flow. Release your configuration, configure distribution where required, and install the app using Shopify's generated install link where applicable.

**[Follow the complete setup guide →](docs/SETUP.md)**

### 2. Enter three values

Open the [hosted token generator](https://shopify-token-tool-kohl.vercel.app).

| Field | What to enter |
| --- | --- |
| Store URL | Your permanent domain, such as `your-store.myshopify.com` |
| Client ID | Client ID from your app's settings |
| Client Secret | Client Secret from the same app |

Only use credentials you own or are explicitly authorized to manage.

### 3. Connect and copy

Click **Connect Shopify**, complete any Shopify authorization prompt, and return to the result screen.

Use **Copy Token** and save it securely. The token belongs to **your app's installation on that store**, not to a shared app owned by this tool.

## What the result screen shows

- The connected store.
- A copyable Admin API access token.
- Granted access scopes, each shown as a separate list item.
- Token expiry information when returned by Shopify.
- A refresh token when returned by Shopify.

The tool queries `currentAppInstallation.accessScopes` for the installation's granted permissions. If that lookup is unavailable, it shows the token exchange's scope list with a fallback notice.

A matching write scope includes read access: for example, `write_products` grants product read and write access.

## Current scope behavior

The current authorization request includes `read_products`. Shopify-managed installation can grant scopes from the app's released and approved configuration.

The result list reports what Shopify actually granted. It does not grant additional permissions, guarantee every configured scope is approved, or bypass protected-data requirements.

Broader app configurations still need testing before a general public launch.

## Does the token expire after 24 hours?

This tool uses the **OAuth authorization-code flow**, not the client-credentials grant.

Token lifetime depends on the app and Shopify's authentication rules. The result shows the expiry information Shopify returns. There is no universal promise of a permanent token.

A non-expiring offline token has no scheduled time expiry, but uninstall, revocation, and relevant platform changes can invalidate it. An expiring token requires renewal in the application that uses it.

**[Read token and permissions FAQs →](docs/FAQ.md)**

## Security protections explained

These controls are implemented in the development code. They reduce specific risks; they are not a claim that the hosted service is risk-free or independently certified.

| Protection | What it does |
| --- | --- |
| HTTPS endpoint | Encrypts browser-to-service transport when a valid HTTPS connection is used |
| POST form submission | Keeps entered credentials out of the form submission URL |
| AES-256-GCM session encryption | Protects temporary OAuth session contents against reading and tampering without the server key |
| HTTP-only cookie | Prevents ordinary page JavaScript from reading the OAuth cookie |
| Secure production cookie | Restricts that cookie to HTTPS connections |
| SameSite=Lax and Origin check | Add protections around cross-site request handling |
| Browser-bound OAuth state | Links the callback to the browser session that started authorization |
| Shopify HMAC check | Verifies the callback signature against the entered app secret |
| Store and timestamp checks | Reject mismatched stores and callbacks outside the accepted time window |
| Session cleanup and no-store headers | Clear the OAuth cookie and tell compliant caches not to retain the response |
| Result-page Content Security Policy | Restricts scripts, resources, and framing on the token result page |

The Client Secret is still processed by the hosted backend. Encryption does not make an untrusted operator trustworthy. Never submit secrets to a service you do not trust.

## Trust and credential handling

Using a hosted helper means trusting its operator with your Client Secret during authorization. HTTPS protects transport; it does not remove that trust requirement.

The implementation shared during development uses a temporary encrypted, HTTP-only OAuth cookie, verifies callback state and Shopify's signature, and clears that cookie while processing the callback.

It does not intentionally save result tokens in a database or browser local storage. Hosting-provider infrastructure and logging settings are separate considerations. The service has not undergone an independent security audit.

Never paste Admin API tokens into theme code, public JavaScript, GitHub issues, or screenshots.

**[Read security and privacy details →](SECURITY.md)**

## Troubleshooting

| Message | Next step |
| --- | --- |
| Distribution method required | Configure distribution if required for this app's organization. |
| Installation link invalid | Use Shopify's current generated install link for the intended store. |
| Callback returns 404 | Check that the registered callback matches the deployed route exactly. |
| Session missing or expired | Start again from the homepage in the same browser. |
| Signature verification failed | Verify the Client Secret and begin a fresh connection. |
| Token exchange rejected | Check app credentials and restart authorization. |
| Missing permissions | Check the released scopes, store approval, and Shopify access requirements. |

Do not refresh an old OAuth callback to repeat the exchange. Authorization codes are temporary and single-use.

## Project status

| Available in the hosted tool | Remaining work |
| --- | --- |
| Three-field form and OAuth exchange | Publish the reviewed application source |
| Token copy screen | Abuse protection and security review |
| Granted scopes list | Broader app and permissions compatibility testing |
| Expiry details when returned | Expanded failure and browser compatibility testing |

This is a token-acquisition helper. It does not run your integration, perform automatic inventory updates, or manage token renewal for your application's backend.

## Official Shopify resources

- [Authenticate a standalone app](https://shopify.dev/docs/apps/build/authentication-authorization/authenticate-standalone-apps)
- [Access tokens](https://shopify.dev/docs/apps/build/authentication-authorization/access-tokens)
- [Manage access scopes](https://shopify.dev/docs/apps/build/authentication-authorization/manage-access-scopes)
- [Select a distribution method](https://shopify.dev/docs/apps/launch/distribution/select-distribution-method)

## Support the project

If this Shopify custom app token generator helped you:

1. **Star this repository** to bookmark it and show support.
2. Share the repository link with developers who need this workflow.
3. Report a reproducible issue with all credentials redacted.
4. Suggest a missing FAQ, clearer setup step, or compatibility test.

Stars are voluntary. No token access, support entitlement, or feature is conditional on starring. This project does not buy stars or claim unverified adoption numbers.

Documentation improvements are welcome through pull requests. See [contribution guidance](CONTRIBUTING.md).

## Support

Found a problem? [Open an issue](https://github.com/shaykh-abdul-muhaiman-shopify-developer/shopify-admin-api-token-generator/issues).

Include reproduction steps and a redacted error message. **Never include a Client Secret, access token, refresh token, authorization code, or complete callback URL.**

Created by [Shaykh Abdul Muhaiman](https://github.com/shaykh-abdul-muhaiman-shopify-developer) · Shopify development and custom integrations.
