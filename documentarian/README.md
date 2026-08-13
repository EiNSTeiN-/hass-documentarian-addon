# Documentarian

Documentarian is a documentation assistant for appliance and machine manuals.
It discovers, ingests, indexes, and searches durable documentation sets, then
serves the existing Documentarian web UI through Home Assistant ingress.

Documentarian can optionally connect to the Home Assistant MCP server exposed
by `ha-mcp`. The Documentarian backend calls the internal MCP server, converts
selected MCP tools into ordinary agent function tools, and keeps the MCP URL
out of OpenAI requests. When autodetection finds `ha-mcp`, Documentarian shows
a confirmation prompt in the main UI. Confirming lets Documentarian read the
`ha-mcp` add-on configuration, resolve the internal MCP URL, and save it for
future agent turns. The `home_assistant_mcp_url` option remains available as a
backend-reachable explicit override.

The add-on stores application-owned persistent roots under
`/data/documentarian/` so Home Assistant backups can include documentation
sets, compatibility indexes, conversations, attachments, package templates,
generated packages, saved links, facts, and operational recovery state.
Semantic-search generations are rebuildable. The rotating application log is
transient under `/tmp/documentarian/logs/` and is excluded from Home Assistant
backups; stdout remains available through Supervisor logs. Optional workbench
MCP integration files under `/config` are a separate Home Assistant-owned
backup surface, not application portability data.

<!-- runtime-store-contract: documentarian-runtime-stores-v1 -->

The generated
[runtime-store inventory](https://github.com/EiNSTeiN-/chat-documentarian/blob/main/docs/runtime-store-inventory.md)
is the authoritative root, member, backup, and exclusion policy.

The add-on UI exposes bounded attachment, source-ingestion, archive, memory,
disk-headroom, and concurrency settings. They reserve local reliability
capacity and constrain accidental resource use; they do not expand tool or
user privileges. Attachment responses have a fixed 5 MiB streaming charge.
Materialized plain text is capped at 16 MiB and HTML at 1 MiB before parsing,
below the general 150 MiB path-native source limit.
At the default maximum archive limits, co-located staging and publication can
need at least 27 GiB before filesystem overhead. Keep about 30 GiB of free
add-on storage plus space for existing durable data, and a practical 2 GiB of
host memory headroom, when all configured ceilings are intended to be usable.

Documentarian can also expose its own workbench MCP tools through Home
Assistant's public webhook mechanism. This is disabled by default. When
`workbench_mcp_proxy_enabled` is enabled and the add-on restarts, Documentarian
installs a small Home Assistant custom integration, registers a generated
`/api/webhook/mcp_...` URL, and forwards that webhook to the add-on's internal
`/mcp` endpoint using a private internal header. The generated webhook URL is a
bearer secret; anyone with the full URL can reach the workbench MCP tools.
Home Assistant user authentication does not protect this webhook. A remote MCP
client reaches it through the user's cloudflared HTTPS tunnel, then Home
Assistant Core forwards it over private HTTP with a separate internal token to
the add-on's loopback backend. The full URL appears in logs only after Home
Assistant registers the matching credential generation. Use
`workbench_mcp_proxy_regenerate_webhook` for one successful restart to rotate
the URL and then turn the option back off to rearm future rotation.

The browser surface uses a separate boundary: cloudflared HTTPS reaches Home
Assistant, and Home Assistant authenticates the ingress session before the UI,
API, or Socket.IO request reaches Documentarian. The add-on publishes no host
port. Directly exposing the add-on or a local Documentarian Compose port beyond
loopback is unsupported.

The add-on starts its nginx ingress front door before configuration preflight
and backend recovery. The Documentarian tab therefore shows the startup shell
and a redacted active phase instead of an upstream 502 while durable recovery
is running. Backend startup has no elapsed-time deadline; migration, publication
recovery, index recovery, and saved-conversation audits may take as long as the
stored data requires. The add-on fails startup only when a core process exits,
an explicit startup error occurs, or Home Assistant requests a stop.

`/addon-health` is nginx/container-front-door liveness for Home Assistant
Watchdog. The add-on runner separately supervises backend exit and post-start
liveness. `/startup-status` exposes only controlled phase messages and elapsed
diagnostic values, and the open tab enters the workbench automatically once
backend readiness succeeds.

## Credits

Add-on icon derived from the
[Document icon](https://www.flaticon.com/free-icon/document_8964905) by
Talha Dogar on Flaticon.
