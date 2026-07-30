# SC Knowledge Base — DA / EDS / Modernization Agent (AEM Coder)

Shared starting context for Solution Consultants working with **Document Authoring (DA)**, **Edge Delivery Services (EDS)**, and the **Experience Modernization Agent (AEM Coder / XMOD)**. Clone this repo and run Claude Code from it — this file loads automatically so every session starts from the same basis.

## How to use
1. Clone the repo; open Claude Code in it.
2. Install the official Adobe skills (below).
3. Connect the DA / AEM MCP servers (see [docs/mcp-setup.md](docs/mcp-setup.md)).
4. Ask away — the reference, lessons, and links below are in context.

## Install the skills (once per machine)
**Recommended — SuperSkills (AEM XSC "Super Soldier" pack):** one command installs the XSC advise+build pack **plus** the 17 Adobe EDS dev skills, GSD parallel execution, the DA + hlx-admin MCP servers, AEM CLI, and Playwright:
```
bash <(curl -s https://raw.githubusercontent.com/AEMXSC/SuperSkills/main/setup.sh)
```
Then finish the MCP/GitHub connect steps in [docs/mcp-setup.md](docs/mcp-setup.md). Repo: <https://github.com/AEMXSC/SuperSkills> — advise (competitive intel, demo plays, environment matrix, tech depth) + build (10 demo use-cases across ASO/AI-Agents/LLMO/CSC/EDS/ExMod plays).

**Or just the Adobe EDS skills** (subset — no XSC pack or MCP bundle):
```
/plugin marketplace add adobe/skills
/plugin install aem-edge-delivery-services@adobe-skills
```
Key EDS skills: **da-auth** (get/refresh the DA token — defers to the DA MCP if connected), **da-content** (valid DA HTML + Source API + media + silent-failure traps), `page-import`, `generate-import-html`, `page-decomposition`, `content-modeling`, `building-blocks`, `create-site`, `docs-search`. Source: <https://github.com/adobe/skills/tree/main/plugins/aem/edge-delivery-services/skills>

## Capabilities & MCP — read first
You (Claude) can author/edit **DA content** and **AEM Content Fragments** in this environment **via MCP** — not just the code repo. Treat these as things you CAN do:
- **DA content MCP** (`da_*` tools): read/create/update/move/copy/delete DA sources (pages, docs, sheets), fragments (+ variations/versions), upload media, versions. Target org/repo, e.g. `blofft1/wknd-advanced`.
- **AEM Author MCP** (`*-aem-*` tools): Content Fragments + models, pages, launches, publish (AEM Cloud Service).

**Default posture:** if a user asks you to edit DA content, create/patch a fragment, or work with AEM CFs, **assume you can** — reach for the MCP tools.

**If those tools are NOT available in the session, do NOT tell the user "I can't edit DA."** It means the MCP connection is missing or has **timed out** (auth is ~1 hour). Instead:
1. Tell the user the DA/AEM MCP isn't currently connected (likely timed out).
2. Ask them to reconnect it (re-add the MCP / re-run the connector) or run the **da-auth** skill to obtain a `DA_TOKEN`.
3. Then proceed with the edit.

Before writing DA content, read the **da-content** skill — it documents the block-HTML format, Source API contract, and the silent-failure rules that quietly corrupt content.

## What you edit where (two surfaces)
- **Code repo (GitHub)** → blocks, `scripts/`, `styles/`, `head.html`, `plugins/`, `workers/`, `tools/`, config *files*. Edit via git.
- **DA content (`content.da.live`)** → pages, docs, sheets, fragments, config sheets. Edit via the **DA MCP** (`da_*`).
- Publishing: DA source is authored on `content.da.live`; `aem.page`/`aem.live` is the published copy (Preview → Publish). The DA **Library reads the source**, so a source update shows up without a publish; public pages need Preview/Publish.

## Conventions for contributors
- One reviewed source of truth — add lessons/links/playbooks via **PR**.
- Keep this CLAUDE.md short; put detail in `docs/` and link it.
- Tag facts as **[verified]** (seen in code/observed) vs **[assumed]**.
- Convert relative dates to absolute; cite the source URL for every claim.

## In this repo
- [docs/mcp-setup.md](docs/mcp-setup.md) — connect/reconnect the DA + AEM MCP servers
- [docs/da-eds-reference.md](docs/da-eds-reference.md) — the customization surface, Experience Workspace extensibility, multi-channel/CF guidance
- [docs/lessons-learned.md](docs/lessons-learned.md) — the gotchas (the gold)
- [docs/resources.md](docs/resources.md) — every link
- [docs/playbooks/](docs/playbooks/) — reusable deliverables
