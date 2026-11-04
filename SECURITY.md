# Security and credential handling

## Hosted-service trust

The hosted website receives the Client Secret you submit. Use it only if you trust the operator and have permission to manage the app.

This repository currently contains documentation, not the deployed application source. Descriptions below reflect the implementation shared during development; they are not an independent audit of the deployed service.

## Current implementation design

- Credentials are submitted by POST.
- OAuth session credentials are temporarily encrypted with AES-256-GCM in an HTTP-only cookie.
- The cookie lasts ten minutes, uses SameSite=Lax, and is Secure in production.
- The callback validates the browser-bound state, store, timestamp and Shopify HMAC.
- The callback clears the session cookie and uses no-store response headers.
- Tokens are not intentionally persisted to a database or browser local storage by application code.
- A result-page action copies the token to the user's clipboard.

Encrypted session data is still temporarily present in the browser. Clipboard contents and user-installed browser extensions are outside the application's control. Hosting-provider logging, backups and telemetry require separate configuration and review.

## Never publish

- Client Secrets.
- Access or refresh tokens.
- OAuth authorization codes or complete callback URLs.
- OAUTH_SESSION_KEY.
- Real .env files.
- Screenshots containing unredacted credentials.

Do not enable session recording, request-body logging or analytics that capture credential and token pages.

## If credentials are exposed

Follow Shopify's documented token revocation or app credential rotation process. Update affected integrations and confirm the previous credential no longer works. Deleting an issue or screenshot does not invalidate a leaked credential.

## Reporting a vulnerability

Do not disclose an exploitable vulnerability or secret through a public issue. Use GitHub's private vulnerability reporting option if the repository provides it. If unavailable, open a minimal issue asking for a private contact route without revealing the vulnerability.

Never send real credentials as proof.

## Status

The hosted tool is under active development. Broader compatibility testing, abuse controls and independent security review remain work before a general public launch. No security certification or warranty is claimed.
