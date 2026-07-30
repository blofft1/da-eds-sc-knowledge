# DA / EDS reference

## The two layers
- **DA (Document Authoring)** = the authoring experience (business-user, config-driven).
- **EDS (Edge Delivery Services)** = delivery/rendering (developer-owned, in the GitHub repo).

## Customization surface

**DA (authoring)** — mostly config/business-user
- **Config sheets** (`da.live/config`): `apps`, `library`, `templates`, etc. control what shows up.
- **Custom Apps** (`da.live/apps`): full web apps via the DA SDK (List/Source APIs). e.g. content browser, importer, Fragments console.
- **Library extensions** (`library` sheet): Blocks, Templates, Placeholders, Icons, Media Library, AEM Assets, + plugins.
- **Editor/Sidekick plugins** (`tools/sidekick/config.json`).
- **Fragments** (native reuse), **Templates**, **Placeholders/Icons**, **permissions**.

**EDS (delivery)** — developer-owned
- **Blocks** (`blocks/`) — custom components (JS+CSS). **`scripts/`** — the JS pipeline (auto-blocking, `schema.js` for JSON-LD, delayed logic). **`styles/`** — design system/tokens.
- **`head.html`** — martech/Alloy, experimentation, analytics, fonts, CSP, preconnects.
- **Plugins** (`plugins/`) — martech (personalization/analytics), experimentation.
- **Workers** — auth/gated, forms, html2json/json2html, feeds, redirects, tokens.
- **`helix-query.yaml`** → `query-index.json` (search/listings/feeds). **Metadata & sheets.** **UE config** (`component-definition/models/filters.json`). Site chrome (nav/footer/404), redirects/robots/sitemap.

**Integrations:** Target · Analytics/CJA · Experimentation · AEM Assets · Dynamic Media · AEM Content Fragments · Forms · Auth/IMS · Search · RUM.

**Who does what:** business users/SCs stay in DA (config, apps, library, fragments, content); developers own the EDS repo; the sweet spot is a **DA app** that exposes a developer capability to business users. Adobe manages the core pipeline/CDN/editor — you extend *around* it.

## Experience Workspace extensibility
The editor's right-rail dropdown = built-in panels (Editor/Outline/Files/Versions/Library) **+ `library`-sheet rows**. To add extensions (Media Library, Rollout, Request Publish, Preflight, Generate Variations, Fragment Picker, Tagger…), add rows to the `library` sheet pointing at the hosted plugin HTML (`aemsites/da-blog-tools/tools/plugins/…`) with `experience: dialog`/`full-dialog`. Fastest way to match a reference site: copy its `library` sheet rows.

## Reusable / multi-channel content — recommendation
Depends on **where content should live**:
1. **DA-native, no AEM** → **DA Fragments** (+ optionally a custom DA app for models/variations/visual editing). Cleanest, no CORS, no coupling.
2. **AEM-committed, want CF-model UX** → **Universal Editor / x-walk** (author in AEM, deliver via EDS). *Not* CF-overlay.
3. **True omnichannel (web + app + …)** → **CF in AEM + GraphQL headless**; EDS consumes for web.
4. **CF Overlay (`json2html`)** → only to surface existing AEM CFs on an EDS site read-only; it re-couples EDS to AEM Author (auth forwarding, re-preview on edit, worker maintenance).

**CORS reality:** delivery is fine (server-side worker → CDN). But a **browser** DA app fetching AEM-author CF JSON hits auth + CORS — live CF preview needs a server-side proxy. Argues for staying same-origin (DA-native) or UE.
