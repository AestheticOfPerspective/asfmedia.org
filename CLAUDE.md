# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Source of truth

`index.html` **is** the product. There is no framework, package manager, build step, CI, or deploy config in-repo. If prose in `README.md` or other docs drifts from `index.html`, trust `index.html` and update the prose to match.

The page is fully self-contained: no `<img>`, no `<script>`, no external `<link>` (no Google Fonts, no CDN). `Inter` is just the first name in a system font stack — do **not** add font/CDN preconnects "to make it work." If the page needs a third-party asset later, that's a real decision, not a tidy-up.

Do not introduce tooling, a build system, or a directory split unless the user explicitly asks for it. Small, direct edits to `index.html` are the expected change shape.

`AGENTS.md` is the short-form peer of this file (for the agents.md spec). When a rule here changes, keep `AGENTS.md` in sync.

## Local preview

No tests. To verify edits:

```bash
python3 -m http.server
# open http://localhost:8000/index.html
```

Manually check: broken links, invalid HTML entities, copy drift between `index.html` and the markdown docs.

## Editing rules

- **HTML entities:** always `&amp;` — never a bare `&` in attributes or text. The existing `Foto &amp; Video` / `Web &amp; Content` headings are the pattern.
- **Umlauts are transliterated** as `ae` / `oe` / `ue` / `ss`. The page is dominantly written this way: `Praesenz`, `fuer`, `Webflaeche`, `Naechster`, `Bildgefuehl`, `spaeter`. The file declares UTF-8, so this is a stylistic choice, not an encoding requirement. New copy should follow the transliterated form. If you spot a real umlaut that slipped in (e.g. `ausgewählte` on line 402), that is drift — fix it to `ausgewaehlte` rather than spreading the inconsistency.
- **Business data must stay aligned** between `index.html` and `README.md`: email (`alexander.solod@web.de`), phone (`0172 8486517`), current site (`https://web.asfmedia.org`), Instagram (`aesthetic_of_perspective`).
- **Branding rule from global config:** never prepend "DE" / "Deutsch" to copy unless explicitly asked.
- The page is German (`<html lang="de">`) with German body copy and English section IDs (`#services`, `#approach`, `#contact`) and English eyebrow tags. Preserve that split.
- Design tokens live in `:root` CSS custom properties at the top of `<style>` (`--bg`, `--accent` = `#79ffe1`, etc.). Reuse them rather than hardcoding new colors or spacing. The accent mint is identity — don't swap it for a "cleaner" color.

## Repo state

- Branch: `master`. **No commits yet, no remote configured.** The first push hasn't happened.
- `https://web.asfmedia.org` is the current live site; this repo is the intended git-backed replacement, not the running production.
- Two staged-but-untracked working docs (`AGENTS.md`, the deploy/setup checklists) are part of the planned first commit per `GITLAB_DEVELOPMENT_SETUP.md`.

## Deploy / DNS

Do **not** suggest DNS or deploy commands without first reading the relevant checklist:

- `ALFAHOSTING_DEPLOY_CHECKLIST.md` — verified order is **fix Alfahosting billing → snapshot DNS → deploy to `web.asfmedia.org` first → move root domain later**. Mail records (MX/SPF/DKIM/DMARC) must be preserved through any cutover.
- `GITLAB_DEVELOPMENT_SETUP.md` — git remote target is undecided between GitLab.com and the self-hosted `git.labs.ooo` (SSH port `30022`). Don't pick one for the user.

`AGENTS.md` is the short-form version of these constraints; the checklists above are the long-form reference.
