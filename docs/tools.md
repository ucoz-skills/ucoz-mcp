# Tools

Live schemas come from the remote MCP at `https://www.ucoz.com/mcp`. Prefer the live schemas in your IDE when calling tools.

**Server tools (15):** `list_sites`, `select_site`, `create_site`, `templates_tool`, `tmaker_tool`, `content_tool`, `shop_tool`, `subscriptions_tool`, `users_tool`, `modules_tool`, `files_tool`, `ftp_tool`, `apps_tool`, `skills_tool`, `docs_tool`.

## Account

| Tool | Purpose |
|------|---------|
| `list_sites` | List sites owned by the authorized uCoz account |
| `select_site` | Set the current site for this session (`site_id` from `list_sites`) |
| `create_site` | Create a new site under the account, activate it, and select it |

Most site tools require `select_site` (or a successful `create_site`) first.

## Site tools

| Tool | Purpose |
|------|---------|
| `templates_tool` | Templates, global blocks, menus, mail forms, pages, backups (`patch_template` for surgical edits) |
| `tmaker_tool` | Template Maker: validate / archive / publish a whole-site skeleton |
| `content_tool` | Content modules (news, blog, forum, photo, …) |
| `shop_tool` | Online shop: categories, goods, variants, stock, basket, checkout, orders |
| `subscriptions_tool` | Subscription plans, access, payments |
| `users_tool` | Users, groups, ranks, profile fields |
| `modules_tool` | Install/uninstall modules, uAPI toggles, quarantine |
| `files_tool` | Site file manager via `/uapi/files` (list, read/write text, upload, mkdir, rename, move, delete, quota) |
| `ftp_tool` | **FTP password only** (`ftp_password_get/set/change/reset`) — not file transfer |
| `apps_tool` | Scripts and apps: a separate server environment for PHP, Node.js, and Python apps, with its own files, MySQL databases, cron jobs, FTP accounts, and backups |
| `skills_tool` | Resolve / catalog / install official agent skills; usage accounting |
| `docs_tool` | Fallback OpenAPI lookup on `api.ucoz.net` |

### Files vs FTP

- **File operations** → `files_tool` (uAPI file manager).
- **`ftp_tool`** only manages the FTP password for the selected site. It does **not** list, read, write, or upload files over FTP.

### Scripts and apps

`apps_tool` works with the scripts environment attached to the selected site, not with the site's own files (use `files_tool` for those) and not with panel modules (use `modules_tool`).

- **Start:** `get_guide` → `status` → `ensure_ready`. The environment is created only after the user approves.
- **Apps:** PHP version selection; Node.js and Python apps with start/stop/restart and dependency install. An app at `/hello` is served at `https://{site}/apps/hello`.
- **Data:** environment files, MySQL databases and users (export/import via phpMyAdmin), cron jobs, FTP accounts for the scripts environment, backups.
- **Confirmation:** activation, key rotation or deletion, deleting files, databases, apps, cron jobs or FTP accounts, creating backups, and legacy file edits require explicit user approval.