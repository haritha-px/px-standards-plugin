# Changelog

Versions track the skill protocol (retrieval logic, enforcement semantics, view URL), not the standards themselves, which are read live from Notion.

Keep `metadata.version` in `SKILL.md` in sync with the git tag.

## 0.3.0
- Query the data source with SQL (Status = Active, six columns) instead of a saved view. Saved views returned unapproved standards and all columns.
- Include the data source URL and the human-browsable view link.

## 0.2.0
- Treat `Enforcement Strategy` as a pointer, never as a command to run blindly.
- Explicit view-then-query read instructions, with `Status = Active` filter in the query.
- Loose matching for free-text `Scope`.
- Troubleshooting for missing Notion MCP or access.

## 0.1.0
- Initial draft.
