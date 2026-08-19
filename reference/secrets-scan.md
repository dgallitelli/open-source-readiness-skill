# Pre-publish secret scanning

## Why this matters

Once a private repo goes public, **anything ever committed to git history is exposed permanently**. Removing a secret from the current HEAD before flipping visibility is not enough — the blob still sits in history, reachable by commit SHA, and GitHub keeps unreachable objects around long enough for anyone watching to fetch them. Search bots index newly public repos within minutes.

Corollary: scan **before every visibility change**, not just the first one.

## Countering "it's still private, so it's fine"

Private repos leak too:

- Collaborators and org members with read access (past and present).
- CI/CD logs, build artifacts, and cached workflow output.
- Third-party GitHub App installs and OAuth integrations with repo scope.
- Forks made while access was granted, which survive access being revoked.
- Local clones on machines you don't control.

A credential committed to a private repo should be treated as compromised from the moment of the commit. "Private" changes the size of the blast radius, not the fact of exposure.

## What to check

Both **tracked files in the working tree** and **the full git history** — HEAD-only scanning is the single most common mistake here. Also check **untracked files sitting in the working tree**, which git-history-based scanners never see.

| Pattern | What it looks like |
|---|---|
| AWS access key IDs | `AKIA…` (long-term), `ASIA…` (temporary/STS) |
| Private keys | `-----BEGIN RSA PRIVATE KEY-----`, `-----BEGIN OPENSSH PRIVATE KEY-----`, `-----BEGIN PRIVATE KEY-----`, `-----BEGIN EC PRIVATE KEY-----` |
| Env files | `.env`, `.env.local`, `.env.production` committed rather than ignored |
| Credential files | `credentials.json`, `service-account*.json`, `*.pem`, `*.p12`, `*.keystore`, `id_rsa`, `.npmrc`, `.pypirc`, `.netrc` |
| Provider tokens | `ghp_`, `github_pat_`, `gho_`, `xoxb-`/`xoxp-` (Slack), `sk-`/`sk-ant-` (LLM API keys), `AIza` (Google) |
| Generic high-entropy assignments | long random-looking string assigned to a var named `token`, `secret`, `key`, `password`, `passwd`, `apikey`, `api_key`, `auth`, `credential` |
| Connection strings | `postgres://user:pass@…`, `mongodb+srv://…`, any URL with inline `user:password@` |
| Cloud config | `~/.aws/credentials` copied into the repo, kubeconfig with embedded tokens, `terraform.tfstate` (state files hold secrets in plaintext) |

## How to scan

Prefer a real scanner. These understand entropy and provider-specific formats, which grep does not.

```bash
# gitleaks — full git history (preferred invocation)
gitleaks git -v .

# gitleaks — working tree, including untracked files (git history is NOT scanned here)
gitleaks dir .

# trufflehog — full history, with credential verification
trufflehog git file://"$(pwd)"
```

`gitleaks git -v .` is the currently-preferred form. The older `gitleaks detect --source . -v` still works but is being superseded and emits a deprecation warning in newer versions.

Run **both** gitleaks modes. `gitleaks git` walks commits, so it cannot see an untracked, un-ignored file that exists on disk but has never been committed; `gitleaks dir .` (equivalently `--no-git`) scans the working tree independent of git history and catches exactly that case. Then cross-check with `git status --porcelain` for anything untracked and unexpected before publishing — an untracked secret file is one `git add .` away from being permanent.

Check availability with `command -v gitleaks` / `command -v trufflehog`.

**Fallback only if neither tool is installed** — and say so explicitly in the report, because grep-only is *not* a reliable substitute (no entropy analysis, no provider-format awareness, high false-negative rate, and it misses anything base64-encoded or line-wrapped).

Use the **same** pattern for the working tree and for history. Asymmetric patterns are how secrets get missed: a token that was committed and later deleted from HEAD is invisible to a history scan whose pattern doesn't cover it, even though the working-tree pattern would have caught it had the file still been present.

```bash
# One pattern, used verbatim in both commands below
PAT='AKIA[0-9A-Z]{16}|ASIA[0-9A-Z]{16}|-----BEGIN [A-Z ]*PRIVATE KEY-----|ghp_[A-Za-z0-9]{20,}|github_pat_[A-Za-z0-9_]{22,}|gho_[A-Za-z0-9]{20,}|xox[baprs]-|sk-[A-Za-z0-9]{20,}|AIza[0-9A-Za-z_-]{30,}|://[^/[:space:]:@]+:[^/[:space:]:@]+@'

# tracked files in the working tree
git grep -nIE "$PAT"

# full history — prints SHA:path:line:match
git grep -nIE "$PAT" $(git rev-list --all)

# files that were ever committed but should never have been
git log --all --pretty=format: --name-only --diff-filter=A | sort -u | grep -E '(^|/)\.env|credentials\.json|\.pem$|\.p12$|id_rsa$|\.tfstate$'
```

The history command searches every commit reachable from every ref, so it can be **slow on large histories** (and the `$(git rev-list --all)` argument list can get long on repos with tens of thousands of commits — narrow it with `git rev-list --all -- <path>` or use a real scanner instead). Unlike `git log -p --all | grep`, it gives you the commit SHA, the path, and the line number, which is exactly what the report below requires.

Recommend installing a real scanner rather than trusting a clean grep result.

## If anything is found: STOP

This is **Tier 3 — report only**. Report the file, the line, and the commit SHA if the finding is in history. Then stop.

**Do not** delete the file. **Do not** rewrite history. **Do not** `git filter-repo`, `git rebase`, `git commit --amend`, or force-push. **Do not** redact the value in place and call it fixed — that adds a commit and leaves the original blob intact. **Do not** print the secret value back into the conversation; report its location, not its contents.

### Remediation the human must perform

1. **Rotate or revoke the credential immediately** — before anything else, and regardless of whether the repo ever goes public. Treat it as compromised the moment it was committed, even to a private repo. Rotation is the only step that actually closes the exposure; history rewriting is cleanup.
2. **Purge the history** if the history has value: `git filter-repo --invert-paths --path <file>` (recommended; `git filter-branch` is deprecated) or BFG Repo-Cleaner (`bfg --delete-files <file>` / `--replace-text`). If the history has no particular value, the simpler and safer option is to start a fresh repo with no history and publish that.
3. **Force-push and coordinate** after any rewrite: every collaborator must re-clone or hard-reset, since stale clones and old forks will otherwise reintroduce the purged objects. Ask GitHub Support to garbage-collect unreachable objects, and delete forks made before the purge.
4. **Re-scan** after remediation, and confirm the rotated credential is sourced from the environment or a secret manager rather than the repo.
5. **Understand what `.gitignore` does and does not do.** `.gitignore` only prevents *future* commits of a path — it has zero effect on content already in git history. Treat this as a forward-looking safeguard only, never as having remediated the actual exposure.
6. Add the offending path to `.gitignore` so it cannot come back.
