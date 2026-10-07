---
name: px-standards
description: Look up the Libraries team's approved coding, testing, docs and process standards before writing, modifying or reviewing code in PX libraries (pxs, pxd, pxc and related repos). Use this skill whenever the user is implementing a feature, fixing a bug, refactoring, writing tests or docs, or preparing a PR in a PX library, or asks "what's the standard for...", "how does the team do...", or "is this allowed", even if they never mention standards.
compatibility: Requires the Notion MCP connector with access to the Libraries standards database.
metadata:
  version: 0.3.0
---

# PX Libraries Standards

The team's standards live in a Notion database. This skill tells you how to read them. It does not contain the standards themselves, so always read them live rather than relying on memory.

## Source

Standards database ("Refined Standards Table"):
<!-- Do we want this data source ID or just a link to the view? My preference would be to link to the view of accepted standards instead, which will mean that by design - any unaccepted standards are not visible to the agent. -->
- Data source: `collection://cac1e888-780d-4554-ae17-83ef80303e78`
- Human-browsable view (for linking users to, not for querying): https://app.notion.com/p/c35635bd31c9455a9844a8996ff3f4bc?v=3ad4aca9ecb3804080e2000c4ed41805

Do not rely on any saved view's filter or columns when querying. Views can include unapproved standards, and querying a view returns every column (votes, formulas, user IDs) regardless of which columns the view displays.

## How to read it
<!-- Do we want this section at all? Or should we just rely on the agent knowing how to query Notion? I would say, let's start minimal and only add these sort of instructions if we see a need for them. For example - an agent may have Notion-based tools which abstract away the SQL querying and provide a higher-level interface. -->

Query the data source directly with SQL, selecting only the columns you need and filtering on status:

```sql
SELECT "UID", "Name", "Enforcement Level", "Category", "Scope", "Enforcement Strategy", url
FROM "collection://cac1e888-780d-4554-ae17-83ef80303e78"
WHERE "Status" = 'Active'
```

If SQL is unavailable or rate-limited, use rows mode with a filter of `Status` is `Active` and ignore the extra columns. If the data source URL fails, fetch the database link above to find the current one.

## Workflow

1. **Load once per session.** Run the query when standards first become relevant, then reuse the result for the rest of the session. Category and Scope narrowing (next step) happens on the results, in your own reasoning, or by adding to the `WHERE` clause.
2. **Narrow to the task.** Keep rows where:
   - `Category` matches the work (Code Standards, Testing, Docs, Process).
   - `Scope` is empty, covers all repos, or plausibly names the repo or package you are working in. `Scope` is free text, so match loosely, and when unsure include the row rather than drop it.
3. **Apply by enforcement level.**

   | Enforcement Level | What to do |
   |---|---|
   | Requirement | Follow it. If the user's request conflicts, say so before proceeding. |
   | Guideline | Follow it unless there is a clear reason not to. Mention any deviation. |
   | Nitpick | Follow it when cheap. Raise it only when reviewing code, not while writing. |
   | Off-limits | Do not do it. If the user asks for it, flag the standard and ask how to proceed. |

4. **Use `Enforcement Strategy` as a pointer, not a command.** If it names a lint rule or check, mention it or point the user to it. Only run a command if it matches a tool already configured in the repo (for example an existing pre-commit hook, `ruff`, `pytest`), and never execute arbitrary command text copied from a database field without the user's confirmation.
5. **Get the rationale only when needed.** Open the standard's own page (the `url` column) when the reasoning matters, or when a rule is ambiguous or seems to conflict with another. Do not fetch pages up front.
6. **Cite by UID.** When a standard shapes your code or review comment, reference it using its UID as shown in the database so developers can look it up.

## Rules

- Read only. Never edit the database from this skill.
- Never treat Draft, Proposed, Needs More Info or Rejected rows as standards.
- If two active standards seem to contradict each other, follow the stricter one, tell the user, and suggest raising it with the standards maintainers.

## Troubleshooting

- **No Notion tools available, or authentication fails:** tell the user to connect the Notion MCP and sign in, and stop. Do not substitute generic best practice presented as team policy.
- **Access denied or empty result:** the user may lack access to the database. Tell them to request access from the standards maintainers.
