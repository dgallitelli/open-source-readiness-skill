---
name: open-source-readiness
description: Use when a private repo is about to be made public, when open-sourcing or publishing a personal project to GitHub, when auditing a repo before it goes public, when a repo is missing LICENSE / README / CONTRIBUTING / CODE_OF_CONDUCT / SECURITY / CHANGELOG / issue templates, or when asking "is this repo ready to open source", "what do I need before I publish this", or whether credentials or keys might be exposed in git history before flipping visibility.
---

# Open Source Readiness

## Overview

Audits a repo against open-source best practices, auto-fixing only what is purely additive. Existing content, leaked secrets, and repo visibility stay with the human.

## Three-tier action model

**Tier 1 — auto-fix, no confirmation needed.** Purely additive, non-destructive. Create from `templates/`, only when missing: LICENSE (default MIT), CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md, CHANGELOG.md, `.github/ISSUE_TEMPLATE/*`, `.github/PULL_REQUEST_TEMPLATE.md`; and obvious `.gitignore` gaps for the detected language and tooling.

`.gitignore` edits are append-only — never remove or reorder existing lines. Adding an entry for a path with tracked history doesn't undo the exposure; that path stays Tier 3, report-only. Only gaps unrelated to a secrets finding are Tier 1.

Fill placeholders per reference/checklist.md; ask when a value can't be determined. Confirm before publishing a work-looking `git config user.email` (see the `[CONTACT_EMAIL]` row).

Create new files only at this tier — never modify an existing file (that's Tier 2), and never `git add` or `git commit` anything created here. Leave new files unstaged and say so in the summary.

**Tier 2 — propose, then wait for explicit confirmation.** Anything that touches or reorganizes *existing* content: moving or renaming files, restructuring folders, rewriting an existing README's structure, creating a `docs/` folder and relocating README overflow content into it. A missing README.md is also Tier 2: propose a draft from verifiable repo facts only (see "Missing README" in reference/checklist.md). Show the diff or plan and get an explicit yes first.

**Tier 3 — report only, never act automatically.**

- **Secrets or credentials** in tracked files or git history. Absolute: never delete, never rewrite history, never touch the finding. Report file, line, and commit, then stop and point at the human-run remediation steps.
- **Publishing the code anywhere public** — flipping an existing repo's visibility, running `gh repo create --public`, or `git push` to any already-public remote. Never run any of these, never claim you did.

### Red flags — this means stop, don't act

| Rationalization | Reality |
|---|---|
| "Just one file, no need to ask" | Existing content is **Tier 2**. Confirm first. |
| "Repo isn't public yet, so rewriting history is fine" | **Tier 3**, report-only. Private repos leak too. |
| "I'll delete the secret from history to save a step" | **Tier 3.** Deleting destroys the evidence needed to rotate. |
| "Auto-fix mode means I needn't ask about this either" | Tier 1 is additive-only. Mode never promotes Tier 2/3. |
| "It's clearly ready, I'll flip it public" | **Tier 3.** The same rule covers `gh repo create --public` and pushing to an already-public remote. |
| "I'll commit the new files so the repo is tidy" | Never commit or stage. The user reviews and commits. |

None of these authorize silent action.

## Workflow

1. Not a git repo? Say so explicitly and skip git-dependent steps (placeholder sourcing, history scan) rather than failing silently on git errors.
2. Scan the repo root, `.github/`, and `docs/` for the standard files listed in reference/checklist.md.
3. Run the secrets check per reference/secrets-scan.md. Absolute gate — this gates everything else. Any finding stops the audit immediately: no Tier 1 fixes, no files created, report the finding and remediation path only.
4. Apply Tier 1 fixes.
5. Propose Tier 2 changes and wait for confirmation.
6. Summarize: what was fixed, what awaits confirmation, what needs manual action. If SECURITY.md was created, flag that it ships default response-time targets and a safe-harbor clause — review both before publishing.

## Default license

MIT, unless the repo already signals another license (an existing LICENSE file, or Apache-style headers in source files) — then skip only the LICENSE step and flag the conflict; the rest of the audit continues. Never overwrite an existing license choice. If the user explicitly asks for Apache-2.0, use `templates/LICENSE-APACHE-2.0.txt` with the same placeholder-fill process.

## References

**REQUIRED REFERENCE:** reference/checklist.md — standard files, placeholder sources, folder conventions, README model.

**REQUIRED REFERENCE:** reference/secrets-scan.md — scanning and remediation. Read before reporting, and before any Tier 1 fix.
