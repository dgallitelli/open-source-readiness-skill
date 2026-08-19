# open-source-readiness

A [Claude Code](https://docs.claude.com/en/docs/claude-code) skill that audits a repo before it's made public and acts on it using a three-tier model, so you don't have to re-decide folder structure and best practices every time:

- **Auto-fix, no confirmation** — purely additive files: `LICENSE`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `CHANGELOG.md`, `.github/ISSUE_TEMPLATE/*`, `.github/PULL_REQUEST_TEMPLATE.md`, obvious `.gitignore` gaps.
- **Propose and confirm** — anything touching existing content: a missing README (drafted from verified repo facts only, never invented commands), moving/renaming files, restructuring folders.
- **Report only, never act** — secrets or credentials found in tracked files or git history, and actually changing a repo's visibility to public. Both are always the human's call.

See [`SKILL.md`](SKILL.md) for the full tier model, [`reference/checklist.md`](reference/checklist.md) for the folder-structure and README-content-model reference, and [`reference/secrets-scan.md`](reference/secrets-scan.md) for the pre-publish secrets-scanning procedure. License and Code of Conduct templates are byte-accurate copies of the canonical MIT License, Apache License 2.0, and Contributor Covenant v2.1 texts.

## Install

Copy this directory into your Claude Code skills folder:

```bash
git clone https://github.com/dgallitelli/open-source-readiness-skill.git ~/.claude/skills/open-source-readiness
```

Claude Code picks up new skills automatically — no restart required in most cases. The skill's description is written to auto-trigger on phrases like "prep this repo for open source" or "is this ready to publish," or invoke it explicitly by name.

## License

MIT — see [LICENSE](LICENSE). This applies to the skill's own instructional content; the templates it generates (LICENSE, CODE_OF_CONDUCT.md, etc.) carry their own licensing per their respective upstream specs.
