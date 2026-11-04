# Shopify Admin API Token Generator FAQ

[Open Token Generator](https://shopify-token-tool-kohl.vercel.app) · [Setup guide](SETUP.md) · [API examples](USAGE.md) · [Security](../SECURITY.md) · [README](../README.md)

Answers about Shopify custom apps, OAuth tokens, scopes, privacy, installation and API testing.

- [Getting started and choosing a workflow](#getting-started-and-choosing-a-workflow)
- [Token lifetime and authentication](#token-lifetime-and-authentication)
- [Permissions and scope lists](#permissions-and-scope-lists)
- [URLs, credentials and privacy](#urls-credentials-and-privacy)
- [Installation errors and support](#installation-errors-and-support)

## Getting started and choosing a workflow

### Why use this website?

It handles the authorization redirect, callback checks, and code exchange so you do not need to build and host a backend just to acquire a token.

### Is using it necessary?

No. You can use your own backend, Shopify's libraries, or a manual OAuth workflow. This tool is an optional convenience.

### Is it a Shopify product?

No. It is an independent project without Shopify endorsement.

### What do I enter?

Your store's permanent myshopify.com domain, your own app's Client ID, and its Client Secret.

### Is that all the setup?

Those are the three website fields. You must still create and configure the Shopify app, set the supplied URLs, release permissions, and install the app appropriately.

### Do I need to code or deploy anything?

Not to use the hosted website. Your actual integration may still need its own backend.

### Does the token belong to my app?

Yes. The exchange uses your entered app credentials and the authorized store installation.

### Will this create a custom app for me?

No. App creation, distribution and Shopify permission approvals remain in Shopify.

### Do I need a Shopify account or store access?

Yes. You need an account authorized to manage and install the app. App credentials are not a replacement for merchant authorization.

### Does this need my Shopify account password?

The tool asks for app credentials, not your Shopify password. Sign in and approve permissions on Shopify's own pages.

## Token lifetime and authentication

### Is every token permanent?

No. Lifetime depends on app eligibility and Shopify's authentication rules. The website cannot override those rules.

### Is this the 24-hour client-credentials flow?

No. The website uses the OAuth authorization-code flow. Other grants have different lifetime rules.

### What does “no expiry returned” mean?

Shopify did not provide an expiry field in that exchange response. It is not a promise that the token can never become invalid.

### Can a non-expiring token stop working?

Yes. Uninstall, revocation and applicable Shopify security or policy changes can invalidate access.

### Does this service automatically renew tokens?

No. Your application must manage renewal when required. Copying a token once does not establish automatic renewal in another application.

### Are Client ID, Client Secret and access token the same?

No. The Client ID identifies an app, the Client Secret authenticates server-side exchanges, and the access token authorizes API requests for the granted store installation.

### Is this an Admin API or Storefront API token?

The helper obtains an Admin API token. Storefront API access uses a different authentication model.

### Can I use the same token on another store?

Do not assume so. The token is tied to the authorized app installation and store. Obtain a token for each authorized store separately.

### Will reinstalling preserve my token?

Do not rely on a previous token. Uninstall revokes installation access; complete authorization again and update your backend.

### Should I regenerate a token every time I open the page?

No. Save credentials securely and follow the appropriate lifecycle. Reauthorization can affect existing tokens or refresh credentials depending on the flow.

## Permissions and scope lists

### Why is read_products absent when write_products is present?

The matching write scope includes read access. Shopify may omit the separate read scope from its exchange response.

### Where does the permission list come from?

The tool queries the current installation's granted access scopes. If unavailable, it shows the token exchange's scope list with a notice.

### Does the list reproduce my dashboard configuration?

It shows granted permissions, not every configured but unapproved or optional permission. Those are different things.

### Can the service grant more scopes?

No. Shopify is the authority for granted permissions. Protected-data requirements also remain in effect.

### Will this work with every possible scope?

No universal compatibility claim is made. Shopify controls approval and the current request includes read_products. Other configurations need testing; protected data can require additional approval.

### Does it collect product, customer or order records?

The shown callback implementation exchanges credentials and queries installation scope handles; it does not query those business records. A granted token can still authorize sensitive access, so limit scopes appropriately.

### How do I add inventory or orders permissions?

Configure required scopes in Shopify, release changes and obtain store approval. Reconnect as appropriate and verify granted scopes before calling the relevant API.

### Are configured, optional and granted scopes identical?

No. Configuration declares requested access, optional scopes can require separate approval, and granted scopes reflect actual installation access.

## URLs, credentials and privacy

### Can I change the app URLs later?

Yes. Update the app configuration and release it. Future OAuth requests must use a valid callback supported by your new backend.

### Is the Client Secret sent to this service?

Yes. The backend needs it for the code exchange. Use the service only if you trust its operator. HTTPS does not remove that trust requirement.

### Is the source available for self-hosting?

Not yet in this repository. This repository currently documents the hosted tool. Do not assume the deployed implementation has been independently audited.

### Can I put the token in Liquid or public JavaScript?

No. An Admin API token belongs in trusted server-side code or secret configuration.

### What if I accidentally share a credential?

Treat it as exposed. Follow Shopify's revocation or credential-rotation process and update affected integrations. Deleting a screenshot alone does not revoke a credential.

### Can you guarantee the service is fully secure?

No responsible service can promise zero risk. The implementation uses encryption and callback verification, but infrastructure review, abuse prevention and independent testing remain separate requirements.

### Why is an encrypted cookie used?

It carries a short-lived OAuth session across the redirect without placing the Client Secret in the redirect URL. The encryption key must stay private. Cookie flags do not protect against a compromised server.

### Can browser extensions or screen sharing expose my token?

Potentially. The token is visible in the result page and can be copied to the clipboard. Use a trusted browser and avoid unredacted screenshots and recordings.

### Does no-store prevent retention everywhere?

No. It instructs compliant caches not to store the response. It does not control screenshots, extensions, clipboard history, hosting telemetry or administrator logging.

### Can I self-host this repository?

Not yet. Application source is not published here. Documentation alone is not a runnable Next.js application.

### Can I use localhost?

Local rendering can be tested, but the registered callback and cookie session must match the deployed flow. Use the live HTTPS website for this hosted helper's complete OAuth test.

## Installation errors and support

### Why does installation say the link is invalid?

Use the current complete install link generated in Shopify's custom distribution settings for the intended store. Check that the domain and app match.

### Why did the callback expire?

The helper's temporary OAuth session lasts ten minutes. Start from the homepage again in the same browser. An old authorization code cannot be reused.

### Where do I ask for support?

Use repository Issues for redacted bug reports. Never post credentials, token values, complete OAuth callback URLs or sensitive store data.

### Why does read_products appear in the code?

It is the current authorization request scope. Managed installation can affect grants through released and approved configuration. Inspect the result rather than assuming permissions from the request.

### Why did copying fail?

Clipboard permissions differ by browser. Select the token and use your operating system's copy shortcut. Never paste it into a public issue.

### Does successful OAuth prove every API operation works?

No. It proves the exchange returned a token. Test an authorized read-only request, inspect GraphQL errors, and verify the operation's required scopes.

### Does the helper bypass API rate limits?

No. Shopify applies API limits independently. Hosted-service abuse protection remains on the roadmap.

### Where do I report a security vulnerability?

Use private reporting if enabled, or ask for a private contact route without exploit details. Follow SECURITY.md and do not post credentials publicly.

### How can I support development?

If the tool helped you, star the repository, share the public link or contribute a documentation improvement. Stars are optional and never unlock access.
