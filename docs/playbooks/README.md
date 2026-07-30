# Playbooks

Reusable, step-by-step deliverables. Add new ones here via PR.

## SuperSkills use-cases (external, installed)
The **SuperSkills** pack (`AEMXSC/SuperSkills/use-cases`) ships 10 ready XSC scenarios — install it (see CLAUDE.md) rather than copying:
1. Healthcare POC (build) · 2. ExMod migration — Sitecore 4k pages (build) · 3. Skeptical IT/AI architect (advisory) · 4. Omni-channel CF + GraphQL (build) · 5. Competitive RFP · 6. LLMO / AI search · 7. Pre-demo crisis · 8. MCP content push · 9. CSC deal · 10. Gartner MQ.

## Available (team playbooks)
- **Clone + reskin a demo site** (bbird → branded vertical) — fork via template → reskin with AEM Coder → repurpose features. *(Import the team's existing `bbird-clone-reskin-prompt.md` here.)*
- **Adobe Target on EDS** — request access to the shared Target env, add `mbox` + `target:on` metadata, build the activity in Target UI. *(Import `wknd-target-setup.md`.)*
- **Feature lab book** — hands-on labs against a DA/EDS site (authoring, forms/gated, governance, DA apps). *(See the WKND lab book.)*
- **WKND DA library build** — a travel-branded block library (16 blocks) + the curated index. *(See WKND-LIBRARY-BUILD.)*

## Template for a new playbook
```
# <Title>
**Goal:** …
**Prerequisites / config:** …
**Steps:** 1… 2… 3…
**Verify:** …
**Gotchas:** link to docs/lessons-learned.md entries
```
