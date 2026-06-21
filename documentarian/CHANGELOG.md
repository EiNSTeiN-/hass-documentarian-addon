# Changelog

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
