# Lessons learned (real sessions)

Gotchas that cost time. Each tagged **[verified]** (seen/observed) or **[assumed]**.

## DA apps & Experience Workspace
- **Experience Workspace = the DA canvas editor.** Its right-rail dropdown (Editor/Outline/Files/Versions/Library + extensions) is **100% config-driven** — every item beyond the built-ins is a row in the **`library` config sheet**. Two sites differ only by which rows they register. **[verified]**
- **DA app entry paths must be extension-less.** DA's app shell appends `.html` itself. Registering `.../tools/x/x.html` → it fetches `x.html.html` → your site's 404. Register `.../tools/x/x` (matches Adobe's `better-da`). **[verified]**
- **DA app assets/nav should be absolute paths** (`/tools/x/...`), not relative — relative paths don't resolve inside the app context; absolute do (better-da uses absolute). **[verified]**
- **DA embeds apps at a reduced scale** in the app panel; a centered `max-width` looks tiny. Let content fill width, and for full-size, launch the editor/app in a **new tab** (`window.open`). **[assumed/observed]**
- **Extension plugins are hosted centrally** in `aemsites/da-blog-tools` (`/tools/plugins/<name>/<name>.html`): media-library, msm (Rollout), request-for-publish, preflight, jump-links, da-library. Plus Adobe-hosted AI (Generate Variations at `experience.adobe.com/aem/generate-variations`). Register as `library`-sheet rows with `experience: dialog`/`full-dialog`. **[verified]**

## DA content / caching
- **Tool files (`/tools/**`) cache ~2 hrs** (`max-age=7200`) on `aem.page`/`aem.live`. Changes won't show in DA until TTL expires — **bust with a `?v=N` query** on the app path (and version the CSS link too). **[verified]**
- **The DA Library reads the SOURCE** (`content.da.live`), not the published copy. So a `da_update_source` shows up immediately; the `aem.live` copy can lag (stale) — don't be fooled by the published JSON. **[verified]**
- **Library config = `library` sheet** (`da.live/config`): columns `title · path · format · ref · icon · experience`. Content sources (Blocks/Icons/Templates/Placeholders) → `.json` path; plugins → hosted HTML + `experience`. **[verified]**
- **Templates aren't a "New Page" chooser** — they surface in the **Library panel** (add a Templates row → `templates.json`); you create a blank page and insert. **[verified]**

## Import
- **The DA Import app rejects GitHub Pages sources (403)** even though the file is 200 + CORS-open in a browser — it's built for EDS/production sources. **Use AEM Coder** (fetches arbitrary URLs) or host the source on an EDS host. **[verified]**
- **DA Import maps destination = source URL path.** GitHub Pages project sites serve under `/<repo>/`, so imports land with that prefix — plan a move, or host at a root-matching path. **[verified]**

## Modernization Agent (AEM Coder)
- AEM Coder is a **Claude-based agent**; its skills live in `.skills/` (`SKILL.md` format) and are published at **`adobe/skills`** (install via `/plugin marketplace add adobe/skills`). **[verified]**
- It **injects `.skills/` into sites it generates**; the same ~20 EDS skills appear across agent-created/forked repos. **[verified]**

## Project-specific (WKND / bbird)
- `wknd-advanced` is a **fork of the bbird demo** (`scdemos/demo`), so it inherits the full feature set (auth, forms, experimentation, martech/Alloy, workers). **[verified]**
- **Cloudflare Workers were intentionally skipped** for the first WKND demo → Forms/Login/Gated/html2json don't run on wknd-advanced; demo those on live bbird or deploy workers. **[verified]**
- `fstab.yaml` is legacy; **AEM Code Sync auto-sets the content source** (`content.da.live/{org}/{site}`) for a fork — no manual step. **[verified]**
