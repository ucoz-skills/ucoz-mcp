# Tools

Live schemas come from the remote MCP at `https://www.ucoz.com/mcp`. Prefer the live schemas in your IDE when calling tools.

Each area is split into read, write, and delete tools. Map the call to the tool that matches the action. Deletes and other irreversible actions need `confirm: true`.

**Server tools (41):** `list_sites`, `select_site`, `create_site`, `templates_read`, `templates_write`, `templates_delete`, `tmaker_read`, `tmaker_write`, `tmaker_publish`, `tmaker_delete`, `content_read`, `content_write`, `content_delete`, `forum_read`, `forum_write`, `forum_delete`, `shop_read`, `shop_write`, `shop_delete`, `subscriptions_read`, `subscriptions_write`, `subscriptions_delete`, `users_read`, `users_write`, `users_delete`, `modules_read`, `modules_write`, `modules_uninstall`, `files_read`, `files_write`, `files_delete`, `settings_read`, `settings_write`, `ftp_read`, `ftp_write`, `ftp_delete`, `apps_read`, `apps_write`, `apps_delete`, `skills_tool`, `docs_tool`.

## Account

| Tool | Purpose |
|------|---------|
| `list_sites` | List sites owned by the authorized uCoz account |
| `select_site` | Set the current site for this session (`site_id` from `list_sites`) |
| `create_site` | Create a new site under the account, activate it, and select it |

Most site tools require `select_site` (or a successful `create_site`) first.

## Site tools

| Tools | Purpose |
|------|---------|
| `templates_read` / `templates_write` / `templates_delete` | Templates, global blocks, menus, mail forms, pages, backups. `patch_template` is a surgical edit; `update_template` replaces a template and makes a backup |
| `tmaker_read` / `tmaker_write` / `tmaker_publish` / `tmaker_delete` | Template Maker: validate, edit, and publish a whole-site skeleton. `skeleton_publish` needs `confirm: true` |
| `content_read` / `content_write` / `content_delete` | Content modules (news, blog, photo, board, directory, polls, …) |
| `forum_read` / `forum_write` / `forum_delete` | Forum sections, topics, and posts |
| `shop_read` / `shop_write` / `shop_delete` | Online shop: categories, goods, variants, stock, basket, checkout, orders |
| `subscriptions_read` / `subscriptions_write` / `subscriptions_delete` | Subscription plans, access, payments |
| `users_read` / `users_write` / `users_delete` | Users, groups, ranks, profile fields |
| `modules_read` / `modules_write` / `modules_uninstall` | Install and enable modules, uAPI toggles, quarantine. Uninstall is `modules_uninstall` and needs `confirm: true` |
| `files_read` / `files_write` / `files_delete` | Site file manager via `/uapi/files` (list, read/write text, upload, mkdir, rename, move, delete, quota) |
| `settings_read` / `settings_write` | Module settings. Secret values are masked |
| `ftp_read` / `ftp_write` / `ftp_delete` | Site FTP password only. `ftp_password_get` returns host, port, user, and whether a password is set. `ftp_password_set` and `ftp_password_change` need `confirm: true`; the reply does not include the password. `ftp_password_reset` removes it and needs `confirm: true`. Not file transfer |
| `apps_read` / `apps_write` / `apps_delete` | Scripts and apps: a separate server environment for PHP, Node.js, and Python apps, with its own files, MySQL databases, cron jobs, FTP accounts, and backups |
| `skills_tool` | Resolve / catalog / install official agent skills; usage accounting |
| `docs_tool` | Fallback OpenAPI lookup on `api.ucoz.net` |

### Binary files

`files_write` `action=upload_direct` takes `filename` and the exact size in bytes (1..16777216). The reply is a one-time `upload_url` and `upload_id` (900 seconds). POST the raw body to that URL with `Content-Type: application/octet-stream`. The file is not attached yet.

Bind the `upload_id` on the next call:

- file manager: `files_write` `action=upload` with `path` and `upload_id`, or `upload_ids` for a batch (at most 20)
- material images: `content_write` `image_upload_id` / `image_upload_ids` (at most 50). Cover is a separate `cover_upload_id`
- product photos: `shop_write` `action=img_add` with `image_upload_id` / `image_upload_ids`

`content_base64` / `image_base64` still exist, but a long base64 body is truncated. Prefer `upload_direct`.

### Files vs FTP

- **File operations** → `files_read` / `files_write` / `files_delete`.
- **`ftp_read` / `ftp_write` / `ftp_delete`** only manage the FTP password for the selected site. They do **not** list, read, write, or upload files over FTP. The password is never returned. FTP accounts inside the scripts environment are `apps_read` `ftp_list` and `apps_write` / `apps_delete`.

### Scripts and apps

`apps_read` / `apps_write` / `apps_delete` work with the scripts environment attached to the selected site, not with the site's own files (use `files_*` for those) and not with panel modules (use `modules_*`).

- **Start:** `get_guide` → `status` → `ensure_ready`. The environment is created only after the user approves.
- **Apps:** PHP version selection; Node.js and Python apps with start/stop/restart and dependency install. An app at `/hello` is served at `https://{site}/apps/hello`.
- **Data:** environment files, MySQL databases and users (export/import via phpMyAdmin), cron jobs, FTP accounts for the scripts environment, backups.
- **Confirmation:** activation, key rotation or deletion, deleting files, databases, apps, cron jobs or FTP accounts, creating backups, and legacy file edits require explicit user approval.
