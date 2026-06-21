# Documentarian

Documentarian is a documentation assistant for appliance and machine manuals.
It discovers, ingests, indexes, and searches durable documentation sets, then
serves the existing Documentarian web UI through Home Assistant ingress.

Documentarian can optionally connect to the Home Assistant MCP server exposed
by `ha-mcp` through the OpenAI Responses API native MCP tool. Configure an
OpenAI-reachable HTTPS MCP URL, such as a Secure MCP Tunnel endpoint, in the
add-on options. Autodetection can notice that `ha-mcp` is installed and show a
configuration prompt, but OpenAI cannot use Home Assistant's internal add-on
DNS URL directly.

The add-on stores all durable state under `/data/documentarian/` so Home
Assistant backups can include documentation sets, indexes, conversations,
attachments, package templates, generated packages, saved links, facts, and
logs.
