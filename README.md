# Mercury AI Chat Widget — Embed Testbed

A single-page harness for integrators and QA to poke at the Mercury AI chat embed widget from a real third-party origin.

**Live:** https://mikolajserediuk.github.io/chat-ui-embed-testbed/ *(enable GitHub Pages on this repo to activate)*

## What it does

Loads `<baseUrl>/embed/chat/chat-widget.js` dynamically and exposes UI controls for the full public API:

- `init` / `destroy`
- `open` / `close` / `toggle`
- `updateJwt` (forces an iframe-side re-auth)
- `setTheme` (`light` / `dark`)
- Event log for all of `ready | error | open | close | expand | collapse`

Every option from the integration guide (`theme`, `width`, `height`, `zIndex`, `allowFullscreen`, `bubbleIconOnly`, offsets, gap) is a form field, editable live before each `init`.

## Running locally

Any static HTTP server works — there is no build step.

```sh
npx http-server -p 8081 -c-1
```

Then open http://localhost:8081/ and:

1. Set **baseUrl** to your mercury-ui deployment (e.g. `https://localhost:8080` for local dev, or `https://dev.cobundu.com` for the deployed env).
2. Paste a valid AI JWT into the **jwt** field (the `sub` claim is used as the userId; signature/expiry are not validated client-side). Never commit a real token.
3. Click **init**.

### Testing against a local mercury-ui dev server

If `baseUrl` is `https://localhost:8080` (self-signed cert), browsers silently refuse to load cross-origin `<script>` resources until the cert is trusted:

- Open `https://localhost:8080/embed/chat/chat-widget.js` in a new tab once.
- Click through the "Your connection is not private" warning.
- Come back to this page and click **init**.

## Integration docs

See the `EMBED_INTEGRATION.md` draft in the mercury-ui repo for the full reference (options, postMessage protocol, JWT handling, CSP, troubleshooting).

## Purpose vs scope

This repo is:
- ✅ A quick, hostable demo for integrating teams.
- ✅ A place to reproduce reported bugs on a real cross-origin page.

It is **not**:
- A production-grade test suite.
- A CSP-compliance harness (GitHub Pages cannot set CSP response headers). For that, mirror this HTML on a site where you control the headers.
