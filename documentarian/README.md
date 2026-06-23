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

The add-on stores all durable state under `/data/documentarian/` so Home
Assistant backups can include documentation sets, indexes, conversations,
attachments, package templates, generated packages, saved links, facts, and
logs.

Documentarian can also expose its own workbench MCP tools through Home
Assistant's public webhook mechanism. This is disabled by default. When
`workbench_mcp_proxy_enabled` is enabled and the add-on restarts, Documentarian
installs a small Home Assistant custom integration, registers a generated
`/api/webhook/mcp_...` URL, and forwards that webhook to the add-on's internal
`/mcp` endpoint using a private internal header. The generated webhook URL is a
bearer secret; anyone with the full URL can reach the workbench MCP tools.
Use `workbench_mcp_proxy_regenerate_webhook` for one restart to rotate the URL
and then turn the option back off.

## Credits

Add-on icon derived from the
[Document icon](https://www.flaticon.com/free-icon/document_8964905) by
Talha Dogar on Flaticon.
