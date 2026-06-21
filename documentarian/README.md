# Documentarian

Documentarian is a documentation assistant for appliance and machine manuals.
It discovers, ingests, indexes, and searches durable documentation sets, then
serves the existing Documentarian web UI through Home Assistant ingress.

Documentarian can optionally connect to the Home Assistant MCP server exposed
by `ha-mcp` through the OpenAI Responses API native MCP tool. When
autodetection finds `ha-mcp`, Documentarian shows a confirmation prompt in the
main UI. Confirming lets Documentarian read the `ha-mcp` add-on configuration,
combine its private path with the current Home Assistant external URL, and save
the resulting OpenAI-reachable HTTPS MCP URL. The `home_assistant_mcp_url`
option remains available as an explicit override.

The add-on stores all durable state under `/data/documentarian/` so Home
Assistant backups can include documentation sets, indexes, conversations,
attachments, package templates, generated packages, saved links, facts, and
logs.
