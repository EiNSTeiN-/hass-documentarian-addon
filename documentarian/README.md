# Documentarian

Documentarian is a documentation assistant for appliance and machine manuals.
It discovers, ingests, indexes, and searches durable documentation sets, then
serves the existing Documentarian web UI through Home Assistant ingress.

The add-on stores all durable state under `/data/documentarian/` so Home
Assistant backups can include documentation sets, indexes, conversations,
attachments, package templates, generated packages, saved links, facts, and
logs.

