# AGENTS.md

## Source Of Truth
Single-file static site. `index.html` is product and source of truth.
No framework, package manager, build step, or external assets.
`Inter` in font stack is a name, not loaded — no CDN/font preconnects.

## Repo State
Branch `master`, no commits, no remote. `https://web.asfmedia.org` is live;
this repo is the intended git-backed replacement.

## Editing Rules
- Small direct edits in `index.html`; no tooling unless requested.
- Align business data (email, phone, links) between `index.html` and `README.md`.
- German umlauts transliterated: `ae`/`oe`/`ue`/`ss` (fix real umlauts as drift).
- HTML entities: always `&amp;` not bare `&`.

## Deploy
Alfahosting billing → DNS → GitLab → static host.
Read `ALFAHOSTING_DEPLOY_CHECKLIST.md` before DNS changes.

## Verify
`python3 -m http.server` → `http://localhost:8000/index.html`.
Check: broken links, invalid entities, copy drift.
