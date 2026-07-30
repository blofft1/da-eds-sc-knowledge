# MCP setup — connecting DA + AEM to Claude

These MCP servers are what let Claude **read and write DA content and AEM Content Fragments** directly. If they're not connected, Claude can't edit DA — but that's a *connection* problem, not a capability gap (see the "Capabilities & MCP" section in CLAUDE.md).

## The two servers
| Server | Tool prefix | What it enables |
|---|---|---|
| **DA content** | `da_*` (e.g. `da_get_source`, `da_update_source`, `da_create_source`, `da_move_content`, `da_upload_media`, `da_lookup_fragment`, `da_get_versions`) | Read/write DA sources (pages, docs, **config sheets**), fragments, media, version history on `content.da.live` |
| **AEM Author** | `*-aem-*` (e.g. `create-aem-fragment`, `patch-aem-page-content`, `list-aem-fragments`, `create-aem-launch`, `publish-aem-content`) | Content Fragments + models, pages, launches, publish on AEM Cloud Service |
| **hlx-admin / helix-mcp** | `da_write`, bulk preview/publish | Write DA content + CDN-bust in one call; bulk **preview/publish** via `admin.hlx.page` (the piece the `da_*` source tools lack). Run locally: `npx @adobe/hlx-admin-mcp` |

## Fastest setup — SuperSkills installer
The SuperSkills `setup.sh` wires the MCP servers for you (DA MCP, hlx-admin MCP `da_write`, helix-mcp bulk preview/publish) + AEM CLI + Playwright:
```
bash <(curl -s https://raw.githubusercontent.com/AEMXSC/SuperSkills/main/setup.sh)
```
Then complete the manual steps it prints:
1. **claude.ai → Settings → Integrations** — connect the connectors **`AEM Content - Prod`** and **`AEM DA - Prod`**.
2. **`gh auth login`** (GitHub).
3. Install the **aem-code-sync** GitHub App on your org — <https://github.com/apps/aem-code-sync>.
4. Confirm your **DA org** exists at <https://da.live>.
5. For BUILDs, run the hlx-admin server in a separate terminal first: `npx @adobe/hlx-admin-mcp`.

## Connecting manually
1. Add the MCP server in your Claude client (MCP settings / connector).
2. Complete the **Adobe IMS sign-in** when prompted (browser OAuth).
3. Confirm the tools are live — `da_list_sources` on your `org`/`repo` should return the content tree.

## It timed out — reconnect
- DA/IMS auth is **~1 hour**. If edits start failing or the `da_*` / `*-aem-*` tools disappear mid-session, the session **timed out** — reconnect the MCP (or re-auth) and continue. Don't conclude "editing isn't possible."
- The **da-auth** skill handles this too: it checks for a cached token (`~/.aem/da-token.json`, ~1 hr) and re-runs the IMS flow if needed. If a DA MCP is active, da-auth defers to the MCP's own auth tool.

## Verify checklist
- [ ] `da_*` tools present → DA content read/write available
- [ ] `*-aem-*` tools present → AEM CF/page tools available
- [ ] A test `da_list_sources` on your site returns files → connection good
- [ ] If not: reconnect the MCP / run `da-auth`, then retry
