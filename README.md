# adfit — Static HTML test page

A self-contained landing page (`index.html`) to test the adfit snippet locally
with the **Static HTML** platform. No build, no dependencies — it runs from a
plain file.

> This page is **outside** the adfit repo on purpose (it simulates a customer
> site). It is not part of the app build or deploy.

## Steps

1. **Run the app** (in the `adfit` repo): `npm run dev` (and make sure the DB
   migration has been applied: `npm run db:migrate`). Log in.
2. **Create a site**: Dashboard → **Sites** → **New site**.
   - Domain: any valid domain, e.g. `example.com`. (Locally the connection check
     does not depend on it — see "How the connection check behaves" below.)
   - Platform: **Static HTML**.
3. **Copy the snippet**: in the wizard's step 2 ("Install the snippet"), click
   **Copy snippet**. It already points at `http://localhost:3000` and contains
   your site token.
4. **Paste it into `index.html`**: replace the placeholder `adfit snippet` block
   in the `<head>` (the one with `REPLACE_WITH_YOUR_SITE_TOKEN`) with what you
   copied. Save.
5. **Open the page**: double-click `index.html` (opens via `file://`).
6. **Watch it connect**: back in the wizard's step 3 (or the site's page), the
   status flips to **Connected** within a few seconds. Reload the test page once
   if needed — the snippet sends one connect-ping per page session.

## How the connection check behaves locally

- The snippet figures out where to send its ping from its own `src`
  (`http://localhost:3000`), so it always reaches your local app — no DNS needed.
- adfit checks that the ping's **origin** matches the site's registered domain
  (a soft anti-spoofing signal). Opening the file directly (`file://`) makes the
  origin opaque, so the check passes automatically and the site shows
  **Connected** regardless of which domain you registered. 👍
- If you instead serve the page over HTTP (e.g. `npx serve` on
  `http://localhost:8080`), the origin `localhost:8080` won't match `example.com`,
  so the site stays **pending** (the page snapshot still arrives — it just isn't
  marked "connected"). For a quick local check, `file://` is the easy path.

## What this tests (W04)

- Snippet generation + the Static HTML install instructions.
- The **connection verification** (A.10): the connect-ping arrives, the first
  page snapshot is captured (the headline, sub-headline, CTA, feature text and
  the form **field definitions** — never the values you type).
- You can also try the manual **"Run a connection check"** on the site (server
  scrape) — note it only works against a publicly reachable URL, so for a local
  `file://` page it will report "not installed"; the connect-ping is the real
  local signal.

DOM personalization (changing the headline/CTA per visitor) is **not** part of
W04 — that's the snippet engine in W05. For now this page is about getting the
site **connected** and the first snapshot captured.
