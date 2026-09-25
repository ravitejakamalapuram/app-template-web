# app-template-web

GitHub template repo for the "web" app type in the app factory
(`decisions/0001-app-factory.md`, company-brain). A new web app is created by
using this repo as a GitHub template, then filling in the TODOs below.

## Template changes are never backfilled

**Once an app repo is created from this template, changes made here afterward do
not propagate to it.** This is a snapshot copied once, not a shared dependency.
If you find a bug in a live app that came from this template, fix it in that
app's own repo with an ordinary PR. Only fix it here too if you want *future*
apps to get the fix — and if a template bug has had to be fixed in more than
two live apps, that's the signal to reconsider the template itself (see the ADR's
"Consequences" section).

## What's in here

- `.github/workflows/pages.yml` — deploys to GitHub Pages via `actions/deploy-pages`
  on every push to `main`, after the app's own `npm test` / `npm run build` pass.
  `id-token: write` is scoped to the `deploy` job alone (GitHub's Pages OIDC), never
  at workflow level and never on a job that runs app build code.
- `app-metadata.json` — the shape `scripts/gen-products.mjs` (company-brain) reads to
  generate `products/<slug>.md`. Fill in every `TODO` before this app goes live.
- `privacy-policy.stub.html` — a starting point for this app's privacy policy. It is
  **not served from this repo**: copy it to
  `ravitejakamalapuram.github.io/privacy/<slug>.html`, the same place every other
  product's privacy policy lives, and fill in its TODOs there.
- `package.json` / `index.html` — placeholder app and build/test scripts so the Pages
  workflow runs out of the box. Replace both with the real app.

## Why no `release.yaml` or release-platform workflows

Web apps deploy straight to GitHub Pages and never go through `release-platform`.
`release-platform` exists to hold keyless store-publishing credentials (Chrome Web
Store, Google Play); GitHub Pages needs no credential at all, so routing web through
it would add board-gated surface for no benefit. See D4 in
`decisions/0001-app-factory.md`. Do not add a `release.yaml` to an app created from
this template.

## Setting up a new app from this template

1. Fill in every `TODO` in `app-metadata.json`, `package.json`, and `index.html`.
2. Copy `privacy-policy.stub.html` into
   `ravitejakamalapuram.github.io/privacy/<slug>.html`, fill in its TODOs, and open a
   PR there.
3. In the new repo's Settings → Pages, set the source to "GitHub Actions".
4. Replace the placeholder app with the real one, keeping the `test` and `build`
   npm scripts (or updating `pages.yml` to match your build tool) so the deploy gate
   still works.
5. Run `node scripts/gen-products.mjs` in company-brain to generate
   `products/<slug>.md` from `app-metadata.json`.
