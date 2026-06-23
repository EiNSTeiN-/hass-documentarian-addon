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

## Workbench MCP webhook proxy

Documentarian includes an optional webhook proxy for its own workbench MCP
server. This is separate from the Home Assistant MCP integration described
above. It makes Documentarian's workbench MCP tools reachable through Home
Assistant at a URL shaped like:

```text
https://<home-assistant-origin>/api/webhook/mcp_<secret>
```

The proxy is disabled by default. To enable it:

1. Set `workbench_mcp_proxy_enabled` to `true`.
2. Set `workbench_mcp_proxy_remote_url` to your Home Assistant public origin,
   such as `https://home.example.com`, or leave it blank to let Documentarian
   try the enabled Nabu Casa remote URL.
3. Restart the add-on.
4. Copy the generated URL from the add-on logs, or combine your Home Assistant
   public origin with the logged `/api/webhook/mcp_...` path.

Treat the generated webhook URL like a password. The random webhook id protects
the public endpoint, and Documentarian also protects the internal add-on hop
with a private token that is generated under
`/data/documentarian/workbench_mcp_proxy/` and mirrored into Home Assistant's
generated proxy config. Neither token should be shared in screenshots,
diagnostics, or issue reports.

To disable the proxy, set `workbench_mcp_proxy_enabled` to `false` and restart
the add-on. Documentarian stops exporting the internal backend token so the
proxy fails closed even if Home Assistant cannot reload the integration. To
rotate the URL, set `workbench_mcp_proxy_regenerate_webhook` to `true` for one
restart, copy the new URL from the logs, then set that option back to `false`.

## Credits

Add-on icon derived from the
[Document icon](https://www.flaticon.com/free-icon/document_8964905) by
Talha Dogar on Flaticon.
