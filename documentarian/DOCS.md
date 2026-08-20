# Documentarian Home Assistant Add-on

## Configuration

Set `openai_api_key` in the add-on configuration before starting the add-on.
Model, reasoning, search, discovery, attachment, and logging options are also
configurable from the Home Assistant add-on UI.

Resource and archive options control local admission and failure boundaries.
`durable_json_max_bytes` may be left unset to derive the reviewed whole-record
JSON boundary from available memory. `durable_record_decode_concurrency` may be
left unset to derive concurrency, or set lower to reserve more memory for other
workloads.
The defaults account for a 1 GiB heavy-work memory pool, a 512 MiB source
parse reservation, a fixed 5 MiB attachment-response stream, a 150 MB
attachment, and a 10 MiB default multipart parser reservation consisting of
the configured spool and framing limits plus a fixed native allowance. The
general source-file cap is 150 MiB, while materialized plain text is capped at
16 MiB and HTML at 1 MiB before allocation. The
archive defaults admit 2 GiB compressed and 8 GiB extracted. Staging and
publication can coexist with two target-side copies while the controller
retains 1 GiB free. Because the add-on places those paths together under
`/data/documentarian/`, a host intended to exercise every maximum should keep
about 30 GiB free there, plus space for existing durable data, and about 2 GiB
of memory headroom for the add-on alongside Home Assistant and other add-ons.
Lower the input ceilings and matching reservations for smaller hosts.

The backend validates the resource settings as one group. Increasing worker,
input, manifest, or spool limits can require a corresponding increase in
record, memory, or disk capacity. Invalid combinations prevent startup rather
than being clamped. Reservations are local reliability bookkeeping: they do
not preallocate every configured byte, and raising them permits more local
resource use, not more privileges.

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

The add-on persists runtime data under `/data/documentarian/`. Its bounded
rotating application log lives under `/tmp/documentarian/logs/`, outside Home
Assistant backups, while stdout remains available through Supervisor logs. The
source image does not contain downloaded manuals, parsed documents, indexes,
conversations, attachments, generated packages, logs, or credentials.
Private upload, archive, and controller state lives under
`/data/documentarian/resource_staging/` and participates in Home Assistant
backup and restart recovery.

## Conversation validation

Documentarian trusts conversations after it accepts them and does not rescan
all settled conversation content when the add-on starts. Portable conversation
imports are normalized, structurally audited, and checked as one proposed
hierarchy before their first conversation is published.

To request a read-only full-store diagnostic, open the application menu beside
**Import data** and **Home Assistant MCP**, select **Audit conversations**, and
confirm **Perform full audit**. The modal shows structural and relationship
progress in realtime. You may close it while the server continues; reopening
shows the active or latest report. The audit reports findings and operational
failures separately and never repairs, deletes, or rewrites a conversation.

<!-- runtime-store-contract: documentarian-runtime-stores-v1 -->

The generated
[runtime-store inventory](https://github.com/EiNSTeiN-/chat-documentarian/blob/main/docs/runtime-store-inventory.md)
is the authoritative root, member, backup, and exclusion policy.

## Access

The add-on exposes the Documentarian UI through Home Assistant ingress. It does
not expose a host port. A normal browser request arrives over the user's
cloudflared HTTPS tunnel, is authenticated by Home Assistant, and then reaches
the add-on through authenticated ingress. The backend listens on loopback
inside the add-on container. Publishing the add-on port directly is
unsupported.

## Health and automatic recovery

The add-on starts the nginx ingress front door before configuration preflight
and backend recovery. `/addon-health` checks nginx/container-front-door liveness.
`/addon-ready` confirms that backend startup completed without rerunning deep
storage and index diagnostics. Until it succeeds, backend-dependent traffic is
withheld without restarting the add-on. `/startup-status` exposes a redacted
active recovery phase and elapsed diagnostic duration directly through nginx.
Cold-start data recovery has no elapsed-time deadline and continues until the
backend becomes live or reports a real error. The open tab enters the
workbench automatically when startup admission succeeds. Detailed operational
health remains available from `/api/health` without controlling UI admission.

Enable **Watchdog** on this installed add-on's Home Assistant page to have
Supervisor restart it after a fatal backend or nginx failure. The add-on
manifest supplies the proxy-owned `/addon-health` monitoring URL, while the
internal runner detects backend exit and probes the backend's cheap `/health`
endpoint after startup. Any successful probe resets the failure count; three
consecutive completed failures terminate the add-on generation so Watchdog can
replace it. Detailed readiness does not contribute to this count. Each probe
gives curl a connection-phase limit and aborts a transfer only when it remains
below one byte per second for the configured no-progress interval. These
observational transport boundaries are separate from startup and detailed
readiness. The per-install Watchdog setting controls whether Home Assistant
performs automatic recovery.
The separate start-on-boot setting does not provide runtime failure recovery.

An operator stop or update allows up to 20 seconds for graceful backend,
nginx, and helper shutdown, escalates remaining processes, and completes the
internal shutdown sequence within 24 seconds. Home Assistant's outer stop
deadline is 30 seconds.

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
4. Copy the generated URL printed after Home Assistant successfully registers
   or reloads the integration.

Treat the generated webhook URL like a password. The random webhook id protects
the public endpoint. Home Assistant user authentication is not applied to this
webhook: possession of its unguessable URL grants access. A remote MCP client
reaches that URL over cloudflared HTTPS. Home Assistant Core then forwards the
request over private HTTP and Documentarian protects that internal add-on hop
with a separate token generated under
`/data/documentarian/workbench_mcp_proxy/` and mirrored into Home Assistant's
generated proxy config. Neither token should be shared in screenshots,
diagnostics, or issue reports.

Documentarian records new and rotated credentials as a pending generation and
reuses it across failed reloads. It prints the full capability URL only after
backend health and Home Assistant registration succeed. Routine starts and
failed attempts keep the URL masked.

To disable the proxy, set `workbench_mcp_proxy_enabled` to `false` and restart
the add-on. Documentarian stops exporting the internal backend token so the
proxy fails closed even if Home Assistant cannot reload the integration. To
rotate the URL, set `workbench_mcp_proxy_regenerate_webhook` to `true` for one
successful restart, copy the new URL from the logs, then set that option back
to `false` so a later false-to-true transition can rotate it again.

## Credits

Add-on icon derived from the
[Document icon](https://www.flaticon.com/free-icon/document_8964905) by
Talha Dogar on Flaticon.
