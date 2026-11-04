# Frequently asked questions

[Open Token Generator](https://shopify-token-tool-kohl.vercel.app) · [Setup guide](SETUP.md) · [README](../README.md)

## Why use this website?

It handles the authorization redirect, callback checks, and code exchange so you do not need to build and host a backend just to acquire a token.

## Is using it necessary?

No. You can use your own backend, Shopify's libraries, or a manual OAuth workflow. This tool is an optional convenience.

## Is it a Shopify product?

No. It is an independent project without Shopify endorsement.

## What do I enter?

Your store's permanent myshopify.com domain, your own app's Client ID, and its Client Secret.

## Is that all the setup?

Those are the three website fields. You must still create and configure the Shopify app, set the supplied URLs, release permissions, and install the app appropriately.

## Do I need to code or deploy anything?

Not to use the hosted website. Your actual integration may still need its own backend.

## Does the token belong to my app?

Yes. The exchange uses your entered app credentials and the authorized store installation.

## Will this create a custom app for me?

No. App creation, distribution and Shopify permission approvals remain in Shopify.

## Is every token permanent?

No. Lifetime depends on app eligibility and Shopify's authentication rules. The website cannot override those rules.

## Is this the 24-hour client-credentials flow?

No. The website uses the OAuth authorization-code flow. Other grants have different lifetime rules.

## What does “no expiry returned” mean?

Shopify did not provide an expiry field in that exchange response. It is not a promise that the token can never become invalid.

## Can a non-expiring token stop working?

Yes. Uninstall, revocation and applicable Shopify security or policy changes can invalidate access.

## Does this service automatically renew tokens?

No. Your application must manage renewal when required. Copying a token once does not establish automatic renewal in another application.

## Why is read_products absent when write_products is present?

The matching write scope includes read access. Shopify may omit the separate read scope from its exchange response.

## Where does the permission list come from?

The tool queries the current installation's granted access scopes. If unavailable, it shows the token exchange's scope list with a notice.

## Does the list reproduce my dashboard configuration?

It shows granted permissions, not every configured but unapproved or optional permission. Those are different things.

## Can the service grant more scopes?

No. Shopify is the authority for granted permissions. Protected-data requirements also remain in effect.

## Can I change the app URLs later?

Yes. Update the app configuration and release it. Future OAuth requests must use a valid callback supported by your new backend.

## Is the Client Secret sent to this service?

Yes. The backend needs it for the code exchange. Use the service only if you trust its operator. HTTPS does not remove that trust requirement.

## Is the source available for self-hosting?

Not yet in this repository. This repository currently documents the hosted tool. Do not assume the deployed implementation has been independently audited.

## Can I put the token in Liquid or public JavaScript?

No. An Admin API token belongs in trusted server-side code or secret configuration.

## What if I accidentally share a credential?

Treat it as exposed. Follow Shopify's revocation or credential-rotation process and update affected integrations. Deleting a screenshot alone does not revoke a credential.

## Why does installation say the link is invalid?

Use the current complete install link generated in Shopify's custom distribution settings for the intended store. Check that the domain and app match.

## Why did the callback expire?

The helper's temporary OAuth session lasts ten minutes. Start from the homepage again in the same browser. An old authorization code cannot be reused.

## Where do I ask for support?

Use repository Issues for redacted bug reports. Never post credentials, token values, complete OAuth callback URLs or sensitive store data.
