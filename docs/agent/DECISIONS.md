# Architecture decisions

## 2026-09-12 — dependency-free client dashboard

The iGoogle revival is a static HTML/CSS/JavaScript application served by an unprivileged nginx container. Personal tabs, themes, layouts, gadget configuration, and gadget data are stored in browser local storage. No account data or application database is collected by the service.

Live weather uses Open-Meteo; headline search uses the public Hacker News Algolia endpoint; optional RSS loading uses rss2json with a direct-feed fallback. Retired authenticated Google gadgets launch their current Google service instead of requesting or storing Google credentials.

The container listens on port 8080, has no published host port in Compose, runs with a read-only filesystem and no-new-privileges, and joins an externally managed edge network.

## 2026-09-12 — static-site security boundary

The frontend is constrained by a Content Security Policy to its own scripts/styles and the explicit live-gadget API allowlist. User- and API-provided navigation/image URLs accept only HTTP(S). The nginx runtime drops root and all Linux capabilities, retains a read-only filesystem, and uses the current stable Alpine image line.
