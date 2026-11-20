# Changelog

## 2.4.0

- Document 41 server tools: `ftp_read`, `ftp_write` and `ftp_delete` are removed; FTP access is set up in the Control Panel
- No tool accepts or returns passwords or access keys: user creation and password changes, database users, FTP accounts and DirectAdmin login key creation moved to the Control Panel; secret environment variables and access keys in settings are refused
- Shop: no basket, checkout or balance actions; order replies carry no access keys, payment links or digital goods content
- Subscriptions: no renewal, resume or payment confirmation; tools describe members' access management
- Informers in the template tools: `informer_list`, `informer_get`, `informer_create`, `informer_update`, `informer_delete`

## 2.3.0

- Document 44 server tools
- Add `signs_read`, `signs_write`, and `signs_delete` for standard site captions

## 2.2.0

- Document 41 server tools
- Replace `ftp_tool` with `ftp_read`, `ftp_write`, and `ftp_delete`. The site FTP password is never returned

## 2.1.0

- Document the split site tools (`*_read` / `*_write` / `*_delete`, plus `modules_uninstall` and `tmaker_publish`)
- Document `files_write` `upload_direct` for binary and batch uploads
- Forum is `forum_*`, separate from content modules

## 2.0.0

- Public repository for uCoz remote MCP
- Official MCP Registry listing (`io.github.ucoz-skills/ucoz-mcp`)
- Client connection configs and docs