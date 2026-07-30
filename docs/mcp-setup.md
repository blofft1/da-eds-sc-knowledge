# MCP setup — connecting DA + AEM to Claude

These MCP servers are what let Claude **read and write DA content and AEM Content Fragments** directly. If they're not connected, Claude can't edit DA — but that's a *connection* problem, not a capability gap (see the "Capabilities & MCP" section in CLAUDE.md).

## The two servers
| Server | Tool prefix | What it enables |
|---|---|---|
| **DA content** | `da_*` (e.g. `da_get_source`, `da_update_source`, `da_create_source`, `da_move_content`, `da_upload_media`, `da_lookup_fragment`, `da_get_versions`) | Read/write DA sources (pages, docs, **config sheets**), fragments, media, version history on `content.da.live` |
| **AEM Author** | `*-aem-*` (e.g. `create-aem-fragment`, `patch-aem-page-content`, `list-aem-fragments`, `create-aem-launch`, `publish-aem-content`) | Content Fragments + models, pages, launches, publish on AEM Cloud Service |

## Connecting
1. Add the MCP server in your Claude client (Claude Desktop/Code MCP settings, or the connector your team uses).
2. Complete the **Adobe IMS sign-in** when prompted (browser OAuth).
3. Confirm the tools are live — a safe read like listing sources for your site (`da_list_sources` on your `org`/`repo`) should return the content tree.

## It timed out — reconnect
- DA/IMS auth is **~1 hour**. If edits start failing or the `da_*` / `*-aem-*` tools disappear mid-session, the session **timed out** — reconnect the MCP (or re-auth) and continue. Don't conclude "editing isn't possible."
- The **da-auth** skill handles this too: it checks for a cached token (`~/.aem/da-token.json`, ~1 hr) and re-runs the IMS flow if needed. If a DA MCP is active, da-auth defers to the MCP's own auth tool.

## Verify checklist
- [ ] `da_*` tools present → DA content read/write available
- [ ] `*-aem-*` tools present → AEM CF/page tools available
- [ ] A test `da_list_sources` on your site returns files → connection good
- [ ] If not: reconnect the MCP / run `da-auth`, then retry
