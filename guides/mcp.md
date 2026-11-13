# MCP server (AI agents)

LessCMS exposes a [Model Context Protocol](https://modelcontextprotocol.io) server so AI agents (Claude, ChatGPT, Cursor, custom agents) can build and maintain sites. Unlike the read-only Public API, MCP writes through the same controllers as the admin panel, so versioning, Page Builder validation and cache purges all apply.

## Endpoint

```
POST https://be.lesscms.io/mcp
```

Streamable HTTP transport, JSON-RPC 2.0. Send `Accept: application/json, text/event-stream`.

## Authentication

`Authorization: Bearer <token>`. Two kinds of token are accepted:

| Token | Where it comes from | Scope |
|---|---|---|
| AI key `lcms_mcp_…` | Panel → Project settings → AI keys (MCP), or Workspace settings → AI keys (MCP) | one project, or a whole workspace (a workspace key can also `create_project`); `read` or `write` access; optional expiry |
| LessTools SSO OAuth token | OAuth 2.1 with dynamic client registration — what Claude/ChatGPT connectors use | every workspace the user belongs to |

OAuth discovery: an unauthenticated request returns `401` with `WWW-Authenticate` pointing at `/.well-known/oauth-protected-resource/mcp`. The authorization server is `https://sso.lesstools.cloud` (scope `mcp:use`).

Requests are rate-limited to 120 per minute per client. The `/mcp` and OAuth paths are excluded from Cloudflare's Browser Integrity Check, so non-browser HTTP clients work.

```bash
curl -s https://be.lesscms.io/mcp \
  -H "Authorization: Bearer $LCMS_MCP_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

`tools/list` is paginated: follow `nextCursor` until it is absent.

## Content format

- Multilingual fields: `{ "pl": "…", "en": "…" }`. A plain string is stored in the project's default language.
- Page Builder sections: call `get_widget_catalog` (no params = list of widget types and the section format; `type` = fields, element groups and default data of one widget). `upsert_page` / `upsert_element` validate sections and return errors to the agent.
- Link to a page from a widget: `"url": "page:<page code>"`.

## Preview of write operations

Every write tool accepts `_meta: { "lesstools/preview": true }` in `tools/call`. The tool then returns the planned changes ("before → after") without saving. The LessTools assistant uses this to ask the user for approval.

## Tools

| Tool | Access | Purpose |
|---|---|---|
| `list_projects`, `get_project` | read | projects, languages, homepage, styles, domains, `preview_url` |
| `create_project` | write (workspace key) | new project with languages and route schemas |
| `update_project_settings` | write | languages/default language, homepage, `styles`, `fonts`, `custom_css`, `custom_html`, `custom_css_urls`, `seo_settings` (business data for JSON-LD), `cookie_consent`; returns the list of sections actually updated |
| `list_domains` | read | preview subdomain state, custom domains with verification status, DNS instructions |
| `manage_domains` | write | `action`: `enable_subdomain` / `disable_subdomain` (idempotent), `add_domain` (returns DNS checks and records to set), `verify_domain`, `remove_domain` |
| `purge_cache` | write | refresh CDN/renderer cache — rarely needed, saves purge automatically |
| `list_media`, `upload_media` | read / write | file manager; upload from URL, returns the file URL for widgets |
| `get_widget_catalog` | read | Page Builder widget catalog and section format |
| `list_pages`, `get_page` | read | pages and their content per language |
| `upsert_page`, `publish_page`, `set_published`, `update_page_fields`, `delete_page` | write | create/update pages (builder sections or simple fields, SEO, drafts), publish, visibility, delete |
| `upsert_element` | write | global elements (`header`, `footer`, shared blocks) |
| `list_menus`, `upsert_menu` | read / write | navigation menus (`page` = page code or `url`) |
| `list_collections`, `upsert_collection` | read / write | collections with fields, route schema, entry template |
| `list_collection_entries`, `get_entry` | read | entries with search and pagination |
| `upsert_entries`, `create_entry`, `update_entry_fields`, `delete_entry` | write | single and bulk entries |
| `upsert_form` | write | form fields, consents and settings: `notifications_enabled`, `email_to` (comma-separated recipients), `reply_to_field`, `success_message`, `error_message`; settings are merged with the existing ones |
| `list_form_submissions` | read | submissions of a form (newest first), `status` filter `new`/`read`/`archived`, up to 100 per page |
| `list_redirects`, `upsert_redirects` | read / write | 301/302 redirects; target `page:<code>` |

Write tools need a `write` key (or an OAuth token of a user with the matching panel permission).

## Recommended build order

1. `create_project` → languages and default language.
2. `update_project_settings` → **`styles` first** for custom designs (`text_color`, `background_color`, `h1_color`…`h4_color`, `font_heading`, `font_body`). Default project styles (e.g. text `#212529`) otherwise recolour headings inside `text`/`embed` widgets. Then `fonts`, `custom_css`, `seo_settings`.
3. `upload_media` → logo and images.
4. `upsert_element` → `header`, `footer`.
5. `upsert_page` → each page (`draft: true` to stage changes).
6. `upsert_menu`, `upsert_collection` + `upsert_entries`, `upsert_form`, `upsert_redirects`.
7. `update_project_settings(homepage_page)`, `publish_page` for drafts.
8. `manage_domains(action: enable_subdomain)` → `preview_url`; later `add_domain` + `verify_domain`.

### Custom HTML sections

The `embed` widget renders raw HTML. Scripts inside it run in the browser on the published site, in document order, but never inside the panel editor. Scripts needed on every page belong in `custom_html` (injected into `<head>`; wait for `load`/DOM before touching the page). Sections accept a custom CSS class in their settings, so custom CSS can target native sections, e.g. a two-column section with an `embed` on the left and a `form` widget on the right.

## Related systems

- LessCommerce: `https://be.lesscommerce.io/mcp`
- LessSEO: `https://api.lessseo.io/mcp` — includes `create_site` and `research_keywords` (search volume, CPC, competition and related keyword ideas from DataForSEO; results cached for 7 days and counted against the DataForSEO budget).
- Backoffice (LessTools staff only): workspace setup tools (`create_workspace`, `set_workspace_plan`, `add_workspace_member`, `list_plans`).
