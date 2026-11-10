# Contributing

- Folder name equals the frontmatter `name` (lowercase, kebab-case). Quote the `description` value.
- Each `SKILL.md` follows the same sections: Use when, Inputs, Procedure, Output.
- A skill must work alone: the Skills CLI may install only that folder. Reference private settings by the names in the README contract, never by path, and always give a default.
- Never commit account data, private project names, local paths, quota snapshots, model performance notes or secrets. Those belong in a private policy repository.
- Do not hardcode a third-party CLI's flags or output schema; point to `tool-discovery` instead.
- Before publishing, check frontmatter parses as YAML, links resolve and no private references slipped in.
