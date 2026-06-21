# Changelog

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
