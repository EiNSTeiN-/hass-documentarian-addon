# Documentarian Home Assistant Add-on

## Configuration

Set `openai_api_key` in the add-on configuration before starting the add-on.
Model, reasoning, search, discovery, attachment, and logging options are also
configurable from the Home Assistant add-on UI.

The optional Home Assistant MCP integration uses the OpenAI Responses API
native MCP tool. Set `home_assistant_mcp_url` to an OpenAI-reachable HTTPS MCP
URL, such as a Secure MCP Tunnel endpoint. Treat this URL as a secret because
it can contain a private random path segment.

When `home_assistant_mcp_url` is empty and `home_assistant_mcp_auto_detect` is
enabled, Documentarian probes Supervisor only to detect whether the `ha-mcp`
add-on appears installed. If it is present, the UI shows a configuration prompt.
Documentarian does not read `ha-mcp` secret-path options or add-on logs, and it
does not save Home Assistant's internal add-on DNS URL because OpenAI cannot
connect to private Home Assistant network addresses.

## Data

The add-on persists runtime data under `/data/documentarian/`. The source image
does not contain downloaded manuals, parsed documents, indexes, conversations,
attachments, generated packages, logs, or credentials.

## Access

The add-on exposes the Documentarian UI through Home Assistant ingress. It does
not expose a host port by default.
