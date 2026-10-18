# px-standards (agent skill)

A Claude Code skill that lets the agent read the Libraries team's approved standards from Notion while you write or review code in PX libraries, so you don't have to switch context. It contains no standards itself. They are read live from Notion.

## Install

You need access to the standards database in Notion.

1. Clone this repo straight into your personal skills folder. The folder must be named `px-standards`:
   ```bash
   git clone <repo-url> ~/.claude/skills/px-standards
   ```
2. Connect the Notion MCP (skip if already connected), then sign in via `/mcp` inside Claude Code:
   ```bash
   claude mcp add --transport http notion https://mcp.notion.com/mcp
   ```
3. Start a new Claude Code session.

That's it. The skill is now available in every repo you open.

## Use

Nothing to do in the normal case. It activates when you implement, fix, refactor, test, document or prepare a PR in a PX library. You can also ask directly:

- "What's the standard for error handling in pxd?"
- "Check the team standards before you write this loader."
- "Review this diff against the standards."

The agent reads only **Active** standards, applies them by enforcement level (Requirement, Guideline, Nitpick, Off-limits), and cites them by UID.

## Update

```bash
cd ~/.claude/skills/px-standards && git pull
```

Standards content is always current because it is read from Notion. Pulling only updates how the agent reads and applies it. To pin a version instead of tracking the latest: `git checkout v0.2.0`.

## Alternative: commit it into a library repo

If you want every contributor to get the skill automatically on clone, copy the folder into the repo instead (without the `.git` directory):

```
<library-repo>/.claude/skills/px-standards/SKILL.md
```

Commit it. Contributors still need the Notion MCP connected. The trade-off is that each repo holds its own copy, so updates need a PR per repo.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Skill never triggers | Check the folder is exactly `~/.claude/skills/px-standards/SKILL.md`, start a new session, and try asking explicitly. |
| "No Notion tools" or auth errors | Reconnect the Notion MCP with `/mcp`. |
| Empty results or access denied | You probably lack access to the standards database. Ask the standards maintainers. |

## Maintainers

Before first use:
- Check the "Consume Standards" Notion skill page URL in `SKILL.md` still resolves, and that the page's instructions (how to query the database, enforcement levels, citation format) are current. Update the page in Notion to change agent behaviour; `SKILL.md` itself rarely needs to change.
- Standards become visible to agents when their Status is set to Active.
- Restrict edit rights on Active standards and on the "Consume Standards" skill page in Notion. Agents treat that content as authoritative.

To release a change:
1. Edit `SKILL.md`.
2. Bump `metadata.version` and add a `CHANGELOG.md` entry.
3. Merge, then tag, e.g. `git tag v0.3.0 && git push --tags`.
