# Changelog

## 0.2.9

- Remove deferred loading from the native Home Assistant MCP Responses tool so
  OpenAI can import the remote tool list without requiring tool search.
- Add a Home Assistant MCP Test action that makes a real Responses API request
  and shows the raw diagnostic transcript, including the MCP URL and API error
  details.

## 0.2.8

- Add Home Assistant MCP settings so saved runtime MCP URLs can be refreshed,
  forgotten, or re-discovered without editing add-on files.
- Let imports continue when an archive contains duplicate data members by
  using the first validated copy and reporting a warning.
- Avoid writing duplicate child-conversation members into new export archives.

## 0.2.7

- Require Home Assistant MCP auto-configuration to use the Nabu Casa / Webhook
  Proxy for HA MCP `/api/webhook/mcp_...` endpoint instead of falling back to a
  direct MCP add-on secret path.
- Retry conversations without the optional Home Assistant MCP tool when OpenAI
  cannot retrieve the remote MCP tool list, so the agent can explain the
  connector issue instead of ending with an error-only turn.

## 0.2.6

- Upload import archives in browser-sized chunks so large exports can be
  previewed and restored through Home Assistant ingress and proxied add-on
  access.

## 0.2.5

- Fix Home Assistant MCP Confirm when Supervisor rejects direct reads of the
  Webhook Proxy add-on options.
- Use the documented add-on info endpoint for Home Assistant MCP option
  discovery and avoid returning ingress-transformable 502 responses for
  auto-configuration failures.

## 0.2.4

- Prefer the `ha-mcp` Webhook Proxy add-on when auto-configuring Home
  Assistant MCP from the main UI Confirm action.
- Show concise API error messages when Home Assistant ingress returns an HTML
  error page.

## 0.2.3

- Add a Camera attachment action to all message composers so mobile users can
  capture appliance tags and other photos directly.
- Reduce export archive size by omitting transient candidates, viewer caches,
  generated indexes, lock files, and model upload caches, and use staged
  browser-native export downloads for large archives.

## 0.2.2

- Auto-configure Home Assistant MCP from the detected `ha-mcp` add-on after
  the user confirms the main UI prompt.

## 0.2.1

- Fix live Home Assistant MCP tool bubbles so completed calls render as
  completed instead of remaining in the working state.

## 0.2.0

- Add optional Home Assistant MCP connection settings and autodetection support.
- Expose Home Assistant MCP to the main Documentarian agent as a native
  Responses API MCP tool when an OpenAI-reachable URL is configured.

## 0.1.3

- Allow Socket.IO to complete connections through Home Assistant ingress origins.

## 0.1.2

- Suppress disabled OpenTelemetry export warnings during Home Assistant add-on startup.

## 0.1.1

- Remove the custom AppArmor profile so Home Assistant uses its default profile.

## 0.1.0

- Initial private development add-on packaging for Documentarian.
