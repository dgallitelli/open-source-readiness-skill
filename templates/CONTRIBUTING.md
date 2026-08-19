# Contributing to [PROJECT_NAME]

Thanks for your interest in improving [PROJECT_NAME]. This document covers how to get set up, what conventions the project follows, and what to expect when you open a pull request.

By participating in this project you agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md).

## Before you start

- **Bugs and small fixes** — open an issue first if the behavior is ambiguous, otherwise a direct PR is fine.
- **New features or larger changes** — open an issue to discuss the idea before writing code. This avoids work being rejected on direction rather than quality.
- **Security vulnerabilities** — do **not** open a public issue. Follow [SECURITY.md](SECURITY.md).
- Check existing issues and open PRs first to avoid duplicate effort.

## Development setup

```bash
# 1. Fork the repository on GitHub, then clone your fork
git clone https://github.com/<your-username>/[PROJECT_NAME].git
cd [PROJECT_NAME]

# 2. Add the upstream remote so you can keep your fork current
git remote add upstream https://github.com/[GITHUB_OWNER]/[PROJECT_NAME].git

# 3. Install dependencies
#    (replace with this project's actual command, e.g. npm install / uv sync / make setup)

# 4. Verify the setup by running the test suite
#    (replace with this project's actual command)
```

If any step above fails on a clean checkout, that is itself a bug worth reporting.

## Branching

Work on a branch off the default branch — never commit directly to it.

```bash
git switch -c <type>/<short-description>
```

Use a `<type>` prefix matching the change: `feat/`, `fix/`, `docs/`, `refactor/`, `test/`, `chore/`. Keep the description short and hyphenated, e.g. `fix/handle-empty-config`.

Rebase onto the latest upstream default branch before opening a PR:

```bash
git fetch upstream
git rebase upstream/<default-branch>
```

`<default-branch>` is usually `main`, but check the repo before assuming.

## Commit messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short imperative summary>

<optional body explaining what changed and, more importantly, why>

<optional footer, e.g. "Closes #123" or "BREAKING CHANGE: ...">
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`.

Guidelines:

- Imperative mood in the summary: "add retry handling", not "added retry handling".
- Keep the summary under ~72 characters and don't end it with a period.
- Explain **why** in the body; the diff already shows what.
- One logical change per commit. Split unrelated changes into separate commits.
- Mark breaking changes with a `!` after the type (`feat!:`) or a `BREAKING CHANGE:` footer.

## Running tests

Run the full suite before pushing:

```bash
# Run all tests
#    (replace with this project's actual command)

# Run linters and formatters
#    (replace with this project's actual command)
```

- All tests must pass locally before you open a PR.
- New features need tests. Bug fixes need a regression test that fails before the fix and passes after.
- Don't disable, skip, or loosen an existing test to make your change pass — if a test is genuinely wrong, say so explicitly in the PR description and explain why.

## Code style

- Match the surrounding code. Consistency with the existing codebase beats personal preference.
- Run the project's formatter and linter before committing; CI enforces them.
- Prefer clear names over comments explaining unclear ones. Comment the *why*, not the *what*.
- Keep changes focused — no drive-by reformatting or unrelated refactors in a feature PR, as it makes review substantially harder.
- Update documentation in the same PR as the behavior change, not in a follow-up.

## Pull request process

1. Push your branch to your fork and open a PR against the default branch.
2. Fill in the pull request template completely, and link the related issue.
3. Keep the PR as small as it can be while remaining coherent. Large PRs get slow, shallow reviews.
4. CI must be green. If it fails, fix it — don't wait to be asked.
5. Respond to review feedback with follow-up commits rather than force-pushing over the reviewed history, so reviewers can see what changed. Squashing happens at merge.
6. A maintainer merges once the PR is approved and CI passes.

### PR checklist

- [ ] Tests pass locally
- [ ] Linter and formatter pass
- [ ] Tests added or updated for the change
- [ ] Documentation updated (README, `docs/`, docstrings) where behavior changed
- [ ] `CHANGELOG.md` updated under `## [Unreleased]` if the change is user-visible
- [ ] Commit messages follow the convention above
- [ ] No secrets, credentials, tokens, or personal paths in the diff
- [ ] Change is focused, with no unrelated edits

## Review expectations

This is a personal side project maintained in spare time, so review may take days rather than hours. A PR that sits unreviewed isn't a rejection — feel free to ping it after a week. Maintainers may decline changes that expand the project's scope, and that's a judgment about direction rather than about the quality of your work.

## License

By contributing, you agree that your contributions will be licensed under the same license as this project — see [LICENSE](LICENSE).
