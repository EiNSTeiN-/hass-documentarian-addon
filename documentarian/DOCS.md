# Documentarian Home Assistant Add-on

## Configuration

Set `openai_api_key` in the add-on configuration before starting the add-on.
Model, reasoning, search, discovery, attachment, and logging options are also
configurable from the Home Assistant add-on UI.

The optional Home Assistant MCP integration connects from the Documentarian
backend to the Home Assistant MCP server and exposes selected MCP tools to the
main agent as ordinary Documentarian function tools. Set
`home_assistant_mcp_url` to a backend-reachable Home Assistant MCP URL when you
want to provide an explicit override. Internal add-on URLs and LAN URLs are
valid. Treat this URL as a secret because it can contain a private random path
segment.

When `home_assistant_mcp_url` is empty and `home_assistant_mcp_auto_detect` is
enabled, Documentarian probes Supervisor only to detect whether the `ha-mcp`
add-on appears installed. If it is present, the UI shows a confirmation prompt.
When you confirm, Documentarian reads the `ha-mcp` add-on configuration,
resolves the internal MCP URL, lists the available tools through the backend MCP
client, and saves the internal URL for future agent turns. OpenAI does not
receive the URL and does not connect to Home Assistant directly.

## Data

The add-on persists runtime data under `/data/documentarian/`. The source image
does not contain downloaded manuals, parsed documents, indexes, conversations,
attachments, generated packages, logs, or credentials.

## Access

The add-on exposes the Documentarian UI through Home Assistant ingress. It does
not expose a host port by default.

## Credits

Add-on icon derived from the
[Document icon](https://www.flaticon.com/free-icon/document_8964905) by
Talha Dogar on Flaticon.
