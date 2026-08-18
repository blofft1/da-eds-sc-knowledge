# DA UI & Admin — UX lessons (field-observed)

Real friction observed while standing up a WKND demo on DA/EDS (governance/brand setup, localization config, a full FIRE→WKND content rebrand, and app/config work). Written as an input to a **full DA UI/Admin UX audit**. Each item = the concrete incident → the UX lesson → an audit prompt.

## 1. Naming is inconsistent between docs and UI
- **Incident:** The brand-governance feature is "Governance Context" in Adobe docs but **"Enterprise Context"** in the product nav. We couldn't find it and assumed it wasn't provisioned.
- **Lesson:** Doc↔UI label drift makes provisioned features look missing.
- **Audit:** Inventory every feature's name across docs, nav, and config sheets; flag mismatches.

## 2. Activation is a hidden, multi-step gate
- **Incident:** A new Brand (and separately, its Domain) defaults to **Draft** and does nothing until Active. Activating is buried behind a **pencil/edit icon top-right → Status → Active → Save** — no obvious "Activate" CTA. Domain activates *separately* and is easy to forget.
- **Lesson:** State gates (Draft→Active) with no primary CTA and per-object repetition cause dead-end "why isn't this working" moments.
- **Audit:** Surface status + a one-click activate; cascade or clearly prompt for dependent objects (domain).

## 3. Async work has no clear system status
- **Incident:** Uploading a brand-policy PDF extracts checks **asynchronously**. Sections read blank for a while, so we concluded it failed — then it began populating ("still processing").
- **Lesson:** Missing progress/status for background jobs reads as failure. (Nielsen #1 — visibility of system status.)
- **Audit:** Add explicit processing/percent/last-updated states to every async operation (policy parse, rollout, publish).

## 4. Unclear data provenance per section
- **Incident:** We assumed one PDF upload would fill Brand Voice + Design System + Checks. Provenance (what's extracted vs authored vs inherited) was unclear.
- **Lesson:** When a screen aggregates data from multiple sources, users can't tell what fills what.
- **Audit:** Label each section's source ("from policy PDF", "authored", "inherited from org").

## 5. Configuration lives in spreadsheets — powerful but opaque and unvalidated
- **Incident:** MSM, translation, library, apps, and **MCP servers** are all configured as **key/value rows in config-sheet tabs** (`da.live/config`). A single missing slash in a library path silently broke it; no validation.
- **Lesson:** Sheet-as-control-plane is flexible but error-prone, undiscoverable, and unforgiving (no schema, no validation, no inline help).
- **Audit:** Consider guided editors/validation for high-impact sheets (mcp-servers, msm, library); at minimum inline examples + error checks.

## 6. Config is split across two stores with no signposting
- **Incident:** Config sheets live in the **config service** (`da.live/config`), not the content bus — so they 404 via the content/Source API and aren't editable the same way as pages. Nothing tells you which store a setting is in.
- **Lesson:** Two storage locations for "settings" with no UI cue → users (and tools) look in the wrong place. An assistant even misattributed the governance config location.
- **Audit:** Make the storage boundary visible; unify or clearly cross-link config vs content.

## 7. Forked/cloned demos silently inherit wrong-org wiring
- **Incident:** The governance MCP resolved the **wrong Adobe org** (a shared demo org's brands, not ours) because the site was forked and inherited a shared worker endpoint (`aem-agentic-plugins…workers.dev`). Not editable in the visible sheet; org was baked into the worker.
- **Lesson:** Clone/fork carries hidden environment bindings (orgs, endpoints, domains) with no "these point elsewhere" warning.
- **Audit:** A post-clone "connections point to: {org/domain/endpoints}" review screen.

## 8. The localization model has two English trees that silently diverge
- **Incident:** A **live root English** site (`/start-here`, `/learn`) AND an **`/en` localization master** (translation source). They drifted — root was rebranded to travel, `/en` was still the old finance content. A rollout would have propagated **stale** content.
- **Lesson:** "Language master vs live site" is non-obvious; nothing shows drift or which tree is canonical.
- **Audit:** Visualize the language-master↔locale↔live relationship and surface drift/staleness before rollout.

## 9. Reskin/clone leaves dangling references — no link integrity
- **Incident:** An AEM-Coder reskin updated labels/content but left **internal links pointing at the old demo domain** (`scdemos`) and at **slugs that don't exist** (homepage cards → `where-to-begin`, `trip-planning`). Images still loaded from the old host.
- **Lesson:** No link/asset integrity check after bulk content operations.
- **Audit:** A broken-link/cross-origin-asset report scoped to a site.

## 10. Renaming a slug has no reference-update — the ripple is manual
- **Incident:** Renaming financial slugs → travel slugs meant manually updating nav, footer, every internal link, the query index, and locale parity. No "rename with references."
- **Lesson:** Path is identity; renaming is high-risk with no tooling to keep references coherent.
- **Audit:** "Rename + update references" (and redirects) as a first-class action.

## 11. Where content "appears" depends on the surface (source vs preview vs live)
- **Incident:** Edits show immediately in the editor/Library (which read **source** on `content.da.live`) but not on `aem.page`/`aem.live` until Preview/Publish. And there's no DA/EDS **publish** action via the automation (MCP) tools — publishing is a separate UI/agent step.
- **Lesson:** The source→preview→live pipeline isn't visible; users can't tell why a change is/ isn't showing on a given URL.
- **Audit:** Per-page status showing source vs previewed vs published, with the action to advance.

## 12. DA app/tool hosting has silent footguns
- **Incident (earlier work):** App card 404'd because `nx` appends `.html` (registering with `.html` → `.html.html`); apps cache ~2 hrs (stale UI); the app panel scales down (must open in a new tab); relative asset paths break.
- **Lesson:** Extensibility conventions are undocumented in-context and fail silently.
- **Audit:** Validate app registration paths; show cache/version; document panel sizing.

## 13. The core gap: no per-site "CMS home" / admin landing
- **Incident:** The reason we built a prototype at all — DA drops you into a file browser, not a **site-specific control center** (status, structure, the key "apps" like MSM/permissions/fragments/governance one click away, AI visibility).
- **Lesson:** Powerful capabilities exist but are scattered and unsurfaced; there's no orienting "front door" per site.
- **Audit:** Evaluate a site dashboard/landing as the primary admin surface (see the `da-prototype` / `tools/da-redesign` work).

---
### Heuristic lens for the audit
Score DA UI/Admin against: **visibility of system status** (#3, #11), **match to real-world/consistency** (#1), **user control & error prevention** (#5, #10), **recognition over recall / discoverability** (#2, #6, #13), and **help users recognize/recover from errors** (#7, #9). The recurring theme: **capability is high, but state, provenance, and wiring are invisible.**

---

# Audit synthesis (Aug 2026)

Turns the 13 field observations + RFP evidence into a product-team ask. Leave-behind deck/doc: `~/Documents/DA_UX_Audit_Leave-Behind.docx`.

## The problem, precisely
DA opens the author into a **file browser**, not a **workspace**. The first screen answers none of the four questions a new author asks: *Where do I start? What can this do? How do I reach the tool that solves my problem? What happens between "I typed something" and "it's live"?* Timing note: **Experience Workspace is still Early Access** (canvas AI editor at `da.live/canvas#`, gated by `da.live/config`) — default-landing and IA decisions are cheap to change *now*.

## The model that separates UI from product — sort every gap into three tiers
Two buckets (UI vs product) is why prioritization stalls. Use three; tag every ask with its tier.
- **Tier 1 — Presentation.** Data already exists; laid out/labeled badly. Fix = re-arrange/re-skin/re-word. *Owner: design + front-end. Cost: low.*
- **Tier 2 — Instrumentation.** System *knows* the state (or cheaply could) but never emits/shows it. Fix = backend exposes state + UI renders it. *Owner: product + eng + design. Cost: medium.* **← most of DA's worst moments live here; it *feels* like UI but *needs* product.**
- **Tier 3 — Capability.** Functionality doesn't exist yet. Fix = build a new engine/feature. *Owner: product + eng. Cost: high.*

## UX guidelines DA should be held to (mapped to author goals)
- **Ready to author now:** open to a purposeful screen, never a raw directory; one primary action per screen (real "Activate" CTA, not pencil→Status→Active→Save — #2).
- **Understand the scope:** make capability *navigable*, not buried in config sheets (#5, #13); consistent naming across docs/nav/sheets (#1).
- **Find your solution:** task-based entry ("Translate this site") over object-based file trees; guided/validated config for anything high-stakes (#5, #6).
- **End-to-end workflow:** content lifecycle (draft → **source → preview → live**) always visible per page (#11); system status on every async job (#3); reversibility + integrity — rename-with-references, broken-link reports, drift warnings before rollout (#8, #9, #10).

## Top 5 changes (tier / ask to product / evidence)
1. **Give every site a "front door"** — `Tier 1 (mostly)`. Ship a site-scoped home surface as the default landing, not the file browser (reference design = `da-prototype`). *Evidence: landing = raw file browser; EA so defaults are cheap now.*
2. **Make system state + the publish pipeline visible** — `Tier 2`. Expose lifecycle + job state via API so UI shows source/preview/live per page and whether async work is running. *Evidence: async reads as failure; SC tooling already works around false 404s + ack-not-done publish.*
3. **Turn the spreadsheet control-plane into guided config** — `Tier 1 + 2`. Validated guided editors for high-impact sheets; activation as a real CTA; unify feature naming; post-clone "connections point to {org/domain/endpoints}" review. *Evidence: a missing slash silently breaks a site; Governance/Enterprise label drift hides provisioned features.*
4. **Complete the workflow: approval + governance before publish** — `Tier 3`. Native review/approval workflow, RBAC, audit trail as a gate before publish. *Evidence: approval = #1 non-security RFP theme (34/61); SSO/MFA/RBAC = most common ask overall (38/61).*
5. **Make localization & multi-site coherent — and visible** — `Tier 3 + 2`. Resolve inherited fragments/links per locale at delivery; locale asset substitution; field-level MSM override; visualize inheritance + drift before rollout. *Evidence: fragment localization is an engineering-confirmed gap blocking a live eval (Stryker); 29/61 RFPs raise localization.*

## Deliberately deferred (keep the five sharp)
Personalization/segmentation (24/61 — a delivery story, not authoring-surface); broad translation-connector ecosystem (fold under #5); MSM↔Translation composability gap (investment-planning context, not a headline line-item).
