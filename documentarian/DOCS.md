# Documentarian Home Assistant Add-on

## Configuration

Set `openai_api_key` in the add-on configuration before starting the add-on.
Model, reasoning, search, discovery, attachment, and logging options are also
configurable from the Home Assistant add-on UI.

## Data

The add-on persists runtime data under `/data/documentarian/`. The source image
does not contain downloaded manuals, parsed documents, indexes, conversations,
attachments, generated packages, logs, or credentials.

## Access

The add-on exposes the Documentarian UI through Home Assistant ingress. It does
not expose a host port by default.

