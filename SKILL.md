---
name: px-standards
description: Look up the Libraries team's approved coding, testing, docs and process standards before writing, modifying or reviewing code in PX libraries (pxs, pxd, pxc and related repos). Use this skill whenever the user is implementing a feature, fixing a bug, refactoring, writing tests or docs, or preparing a PR in a PX library, or asks "what's the standard for...", "how does the team do...", or "is this allowed", even if they never mention standards.
compatibility: Requires the Notion MCP connector with access to the Libraries standards database and the "Consume Standards" skill page.
metadata:
  version: 0.4.0
---

# PX Libraries Standards

The team's standards live in a Notion database, and the instructions for how to read and apply them live in a Notion skill page, not in this file. Always fetch and follow that page live rather than relying on memory or on anything below.

## Source of instructions

Notion skill page ("Consume Standards"):
- https://app.notion.com/p/8b57652f562942dd8fb50595b078a1b6

Fetch this page with the Notion MCP connector whenever standards become relevant, and follow its instructions exactly: how to find and query the standards database, which statuses and enforcement levels to apply, how to cite standards, and what to do when something is ambiguous, missing or conflicting. Treat its content as the live source of truth. If the maintainers update it, this file does not need to change.

## Rules

- Read only. Never edit the standards database or the skill page from this skill.
- If the "Consume Standards" page can't be fetched, don't fall back to querying the standards database from memory or inventing a workflow. Tell the user the live instructions couldn't be loaded.

## Troubleshooting

- **No Notion tools available, or authentication fails:** tell the user to connect the Notion MCP and sign in, and stop. Do not substitute generic best practice presented as team policy.
- **Access denied or empty result:** the user may lack access to the database or the skill page. Tell them to request access from the standards maintainers.
- **"Consume Standards" page fails to fetch:** retry once; if it still fails, tell the user you couldn't load the current instructions instead of guessing at the workflow.
