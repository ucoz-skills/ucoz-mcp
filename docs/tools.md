# Tools

Live schemas come from the remote MCP at `https://www.ucoz.com/mcp`. Prefer the live schemas in your IDE when calling tools.

Each area is split into read, write, and delete tools. Map the call to the tool that matches the action. Deletes and other irreversible actions need `confirm: true`.

No tool accepts or returns passwords or access keys (user, forum, FTP and database passwords; reCAPTCHA, CleanTalk and shop map keys), and no tool takes a payment, places an order or turns auto-renewal on. The site owner sets passwords and keys in the Control Panel; buyers place orders on the site.

**Server tools (41):** `list_sites`, `select_site`, `create_site`, `templates_read`, `templates_write`, `templates_delete`, `tmaker_read`, `tmaker_write`, `tmaker_publish`, `tmaker_delete`, `content_read`, `content_write`, `content_delete`, `forum_read`, `forum_write`, `forum_delete`, `shop_read`, `shop_write`, `shop_delete`, `subscriptions_read`, `subscriptions_write`, `subscriptions_delete`, `users_read`, `users_write`, `users_delete`, `modules_read`, `modules_write`, `modules_uninstall`, `files_read`, `files_write`, `files_delete`, `settings_read`, `settings_write`, `signs_read`, `signs_write`, `signs_delete`, `apps_read`, `apps_write`, `apps_delete`, `skills_tool`, `docs_tool`.

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
| `templates_read` / `templates_write` / `templates_delete` | Templates, global blocks, menus, mail forms, pages, informers, backups. `patch_template` is a surgical edit; `update_template` replaces a template and makes a backup |
| `tmaker_read` / `tmaker_write` / `tmaker_publish` / `tmaker_delete` | Template Maker: validate, edit, and publish a whole-site skeleton. `skeleton_publish` needs `confirm: true` |
| `content_read` / `content_write` / `content_delete` | Content modules (news, blog, photo, board, directory, polls, …) |
| `forum_read` / `forum_write` / `forum_delete` | Forum sections, topics, and posts |
| `shop_read` / `shop_write` / `shop_delete` | Online shop for the site owner: categories, goods, variants, stock, orders and their statuses, currencies, product comments. `basket_get` reads a user's unfinished basket. No basket, checkout or balance actions: buyers order on the site. Order replies carry no access keys, payment links or digital goods content |
| `subscriptions_read` / `subscriptions_write` / `subscriptions_delete` | Subscriptions module: membership plans, members' access, trial periods, pause, plan change, turning auto-renewal off, unpaid payment records. No renewal, resume, payment confirmation or refund |
| `users_read` / `users_write` / `users_delete` | Users, groups, ranks, profile fields. `users_write` updates profiles, logins and groups; users are created and passwords set in the Control Panel |
| `modules_read` / `modules_write` / `modules_uninstall` | Install and enable modules, uAPI toggles, quarantine. Uninstall is `modules_uninstall` and needs `confirm: true` |
| `files_read` / `files_write` / `files_delete` | Site file manager via `/uapi/files` (list, read/write text, upload, mkdir, rename, move, delete, quota) |
| `settings_read` / `settings_write` | Module settings. Access keys (reCAPTCHA, CleanTalk, shop map keys) are masked and not accepted |
| `signs_read` / `signs_write` / `signs_delete` | Standard site captions (Control Panel "Replace standard captions"): module names, form field labels, buttons, list titles, dates. `signs_reset` needs `confirm: true` when the site owner requires it |
| `apps_read` / `apps_write` / `apps_delete` | Scripts and apps: a separate server environment for PHP, Node.js, and Python apps, with its own files, MySQL databases, cron jobs, and backups. Database users, FTP accounts and their passwords are created in the Control Panel |
| `skills_tool` | Resolve / catalog / install official agent skills; usage accounting |
| `docs_tool` | Fallback OpenAPI lookup on `api.ucoz.net` |

### Binary files

`files_write` `action=upload_direct` takes `filename` and the exact size in bytes (1..16777216). The reply is a one-time `upload_url` and `upload_id` (900 seconds). POST the raw body to that URL with `Content-Type: application/octet-stream`. The file is not attached yet.

Bind the `upload_id` on the next call:

- file manager: `files_write` `action=upload` with `path` and `upload_id`, or `upload_ids` for a batch (at most 20)
- material images: `content_write` `image_upload_id` / `image_upload_ids` (at most 50). Cover is a separate `cover_upload_id`
- product photos: `shop_write` `action=img_add` with `image_upload_id` / `image_upload_ids`

`content_base64` / `image_base64` still exist, but a long base64 body is truncated. Prefer `upload_direct`.

### Informers

`templates_read` `informer_list` and `informer_get` read informers of any kind; `manageable` is `true` when MCP can change the informer. `templates_write` `informer_create` and `informer_update` work with news and blog informers only (settings, sorting, period, own template). `templates_delete` `informer_delete` removes an informer of any kind and needs `confirm: true`. An informer shows where its code `$MYINF_<id>$` is put into a site template.

### Standard captions

`signs_read` `signs_groups` lists the caption groups (`modules`, `users`, `news`, `publ`, `blog`, `shop`, `datetime`, …) with an `active` flag; groups of inactive modules are listed too. `signs_list` returns each caption with `id`, current `value`, `default` and `customized`; filter by `group`, `ids` or `customized: true`.

`signs_write` `signs_update` takes `signs: {"<id>": "<text>"}`. Texts are at most 150 characters, HTML is allowed and output as is, and the change is live at once. The call saves all captions or none: an unknown id or a long text fails the whole call. An empty text restores the default. `signs_delete` `signs_reset` restores defaults by `ids` / `group`, or every caption with `all: true`; the site's own texts are lost.

The names of entry fields in `content_read fields` come partly from these captions (`title`, `other1..5`). The visible module name comes from module settings (`settings_*` `name`), not from the `modules` captions.

### Site files

File operations go through `files_read` / `files_write` / `files_delete`. MCP has no FTP tools: the site owner sets up FTP access in the Control Panel.

### Scripts and apps

`apps_read` / `apps_write` / `apps_delete` work with the scripts environment attached to the selected site, not with the site's own files (use `files_*` for those) and not with panel modules (use `modules_*`).

- **Start:** `get_guide` → `status` → `ensure_ready`. The environment is created only after the user approves.
- **Apps:** PHP version selection; Node.js and Python apps with start/stop/restart and dependency install. An app at `/hello` is served at `https://{site}/apps/hello`.
- **Data:** environment files, MySQL databases (export/import via phpMyAdmin), cron jobs, backups. Database users, FTP accounts and their passwords are created in the Control Panel (Server scripts section).
- **Secrets:** environment variables with secret names (passwords, tokens, API keys) are refused; replies show variable values as `***`. The owner sets secrets in the Control Panel (Server scripts, Node or Python tab, environment variables), and the code reads them from the environment.
- **Confirmation:** activation, key rotation or deletion, deleting files, databases, apps or cron jobs, creating backups, and legacy file edits require explicit user approval.
