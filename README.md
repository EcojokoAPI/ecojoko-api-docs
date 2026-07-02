# Ecojoko API — Unofficial Documentation

A community-made reference for the private HTTP API used by the **Ecojoko** mobile app
(`https://service.ecojoko.com`). It is written as a single [OpenAPI 3.1](./openapi.yaml)
spec and rendered as a website with [Scalar](https://github.com/scalar/scalar).

## ⚠️ Disclaimer

- **Unofficial & unaffiliated.** This project is not affiliated with, endorsed by, or
  connected to Ecojoko / homesys in any way.
- It was produced by **observing traffic** from the official app. The real API can
  **change at any time without notice**, and this documentation may become inaccurate or
  stop working. **Use at your own risk.**
- Every example value here is **anonymized** (fake IDs, tokens, e-mails, MAC addresses,
  meter serials, etc.).

## Authentication (in a nutshell)

Almost every endpoint requires an authenticated session. Auth is **cookie-based**:

1. `POST /login` with your account e-mail (`l`) and password (`p`) — the only
   endpoint that works without a session.
2. The response sets two cookies: **`LKSA`** (session token, sent as a session cookie)
   and **`tdw`** (sent with a `Max-Age` of ~30 days in the observed response — that's the
   cookie's advertised lifetime; the server may invalidate the session sooner).
3. Send **both** cookies back on every other request via the `Cookie` header.

See the rendered docs for full details and per-endpoint examples.

## View the docs locally

No build step is required — it's a static site.

```bash
# from the repo root
python3 -m http.server 8080
# then open http://localhost:8080
```

Or validate / lint the spec:

```bash
npx @redocly/cli lint openapi.yaml
```

## Deploy to GitHub Pages

A workflow is included at [`.github/workflows/deploy.yml`](./.github/workflows/deploy.yml).
Once you push this repo to GitHub:

1. Go to **Settings → Pages** and set **Source** to **GitHub Actions**.
2. Push to `main`. The workflow publishes the repo root (which serves `index.html`
   pointing at `openapi.yaml`).

## Project structure

| File | Purpose |
|------|---------|
| `openapi.yaml` | The OpenAPI 3.1 spec — the single source of truth. |
| `index.html` | Scalar renderer that loads `openapi.yaml`. |
| `.github/workflows/deploy.yml` | GitHub Pages deployment. |

The original request/response capture files (`*.txt`) are the raw material the spec was
built from and are not required to serve the site.
