# SiteKeeper — our own owner-editable site system

The problem: small-business owners need to change their hours, specials, and menu items without learning code — and without being able to destroy the site's layout.

The answer: the site's **words** live in a plain `content.json` file. The site's **design** lives in `index.html` and only reads those words. The owner gets a private `admin.html` page with simple form fields. They edit fields, hit save, and the site updates in ~2 minutes. They can never touch the layout because the layout isn't in the fields.

## How it works (no backend, no monthly fee)

1. `demo/index.html` fetches `demo/content.json` and renders every editable region from it.
2. `demo/admin.html` is the editor. The owner logs in with a site password, fills in the connection settings once (repo + a GitHub access token), and gets friendly fields: business info, announcement, specials, menu sections/items, hours.
3. **Save** commits the new `content.json` straight to the repo via the GitHub API (from the browser — no server). GitHub Pages rebuilds automatically.

## Security, honestly

- The site password in `admin.html` is a polite gate, not real security — anyone can view-source it. **Change it per client.**
- Real security is the GitHub token: a fine-grained personal access token scoped to **one repo** with **contents read/write only**. It lives in the owner's browser `localStorage` — never in the repo, never on a server. You can revoke/regenerate it anytime.
- `admin.html` is unlisted (not linked from the public site). Obscurity + token-scoping is proportionate for a diner's hours page.

## Setting up a client

1. Copy `demo/` into the client's site as you build it (or wire `content.json` into their existing pages).
2. In `admin.html`, change `SITE_PASSWORD` to something per-client.
3. Create a fine-grained PAT (GitHub → Settings → Developer settings → Personal access tokens → Fine-grained): resource owner = you, repository access = only their repo, permissions = Contents: read & write. Send it to the owner to paste once.
4. Walk them through it once on their phone. Done.

## Demo

- Live site: `demo/` renders from `demo/content.json`
- Editor: `demo/admin.html` (password: `changeme`)

## Why build this instead of using Decap CMS?

Decap is excellent, but it's someone else's project, needs its own auth service for GitHub Pages, and adds concepts the owner never sees. SiteKeeper is one HTML file we control, zero dependencies, zero ongoing cost, and the setup conversation is: "here's your password, here's your token, these are your fields." For low-maintenance clients who change three things a year, that's the whole job.
