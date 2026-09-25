# Security Policy

## Scope

This repository is the public face of the **uCoz remote MCP** endpoint: connection docs, client configs, and Official MCP Registry metadata (`server.json`).

Agent Skills and IDE plugins live in a separate repository: [ucoz-skills/agent-skills](https://github.com/ucoz-skills/agent-skills).

## How access works

Connect your AI client to the remote MCP URL:

`https://www.ucoz.com/mcp`

Authentication is handled by the uCoz Control Panel flow. You do not need to paste API keys into this repository or into the configs under `clients/`.

Revoke or limit access from your uCoz account / Control Panel settings at any time.

## Secrets in this repository

There are no secrets here and none should be committed. Client configs contain only the public MCP URL.

If you find a real token, password, or personal data in this repository, report it via [uCoz support](https://www.ucoz.com/) and do not open a public issue with the secret value.