# Open source readiness checklist

Grounded in: [Open Source Guides](https://opensource.guide/starting-a-project/), [GitHub — setting up your project for healthy contributions](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions), [Contributor Covenant](https://www.contributor-covenant.org/), [Keep a Changelog](https://keepachangelog.com/), [standard-readme](https://github.com/RichardLitt/standard-readme), [Semantic Versioning](https://semver.org/).

## Standard top-level files

| File | Purpose | Status |
|---|---|---|
| `README.md` | Entry point: what the project is, why it exists, how to install and use it | **Required** |
| `LICENSE` | Grants others the legal right to use, modify, and redistribute. Without it the work is "all rights reserved" by default and legally unusable by others, however public the repo is | **Required** |
| `CONTRIBUTING.md` | How to set up, test, and submit changes. GitHub surfaces a link to it when a contributor opens an issue or PR | Recommended |
| `CODE_OF_CONDUCT.md` | Expected community behavior and enforcement path. GitHub detects and displays it in the community profile | Recommended |
| `SECURITY.md` | How to report a vulnerability privately. GitHub surfaces it under the repo's Security tab and on new-issue pages | Recommended |
| `CHANGELOG.md` | Human-readable record of notable changes per release | Recommended |
| `.gitignore` | Keeps build output, virtualenvs, editor cruft, and — critically — local secret/env files out of history | Recommended |
| `.github/ISSUE_TEMPLATE/*` | Structures incoming bug reports and feature requests | Recommended |
| `.github/PULL_REQUEST_TEMPLATE.md` | Prompts contributors for context and a self-check | Recommended |
| `CITATION.cff` | Only if the project is likely to be cited academically | Optional |
| `CODEOWNERS` | Only meaningful with multiple maintainers or review routing | Optional |

GitHub's "community profile" checks for README, LICENSE, CONTRIBUTING, CODE_OF_CONDUCT, and issue/PR templates — matching all five is a good proxy for "looks like a maintained project."

Any of these files is recognized in the repo root, in `.github/`, or in `docs/`. Root is the conventional home for README, LICENSE, and CHANGELOG; `.github/` is a reasonable home for CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, and the templates if you prefer a tidy root. If placed in `.github/` instead of root, fix the relative links inside them — e.g. a link to `LICENSE` becomes `../LICENSE` — since LICENSE itself always stays at repo root.

## Where content belongs

| Destination | Belongs there | Does **not** belong there |
|---|---|---|
| `README.md` | User-facing only: what it is and why, install, quickstart/usage, a short config or example block, then links out | Full license text, the full contribution process, exhaustive API reference, architecture deep-dives, changelog history |
| `CONTRIBUTING.md` | Dev environment setup, branch/commit conventions, how to run tests and linters, PR process and expectations, code style, review/release process | Community behavior rules (that's CODE_OF_CONDUCT), vulnerability reporting (that's SECURITY) |
| `CODE_OF_CONDUCT.md` | The community behavior standard. Use Contributor Covenant v2.1 verbatim — the only field normally customized is the enforcement contact method | Technical contribution mechanics, project-specific policy |
| `SECURITY.md` | Vulnerability reporting process and private contact, supported-versions table if multiple versions are maintained, expected response time and disclosure policy | Bug reporting for non-security issues (that's the issue template) |
| `CHANGELOG.md` | One section per release, newest first, dated `YYYY-MM-DD`, grouped by change type; links to SemVer | A dump of `git log`; commit-message noise; unreleased work presented as released |
| `docs/` | Deep reference: architecture, API docs, tutorials, design notes, operational runbooks — anything that would bloat the README | Content a first-time user needs within ten seconds (that stays in the README) |

Rule of thumb: the README answers "should I use this, and how do I start?" Everything that answers "how does this work internally?" goes to `docs/`.

## README structure model

standard-readme-inspired, in order. Optional sections may be dropped, but keep the relative order — readers scan for these in this sequence.

1. **Title** — the project name as an `h1`.
2. **One-line description** — a single sentence directly under the title, no marketing padding. What it does, for whom.
3. **Badges** *(optional)* — build status, license, package version, on one line. Skip rather than pad; a wall of badges on a small CLI is noise.
4. **Table of contents** *(include once the README is long enough to scroll past a screen or two)*.
5. **Background / motivation** *(optional)* — why this exists, what problem it solves, prior art. Useful when the project's existence needs justifying; skippable for an obvious utility.
6. **Install** — prerequisites and the copy-pasteable install command. Must actually work on a clean machine.
7. **Usage / quickstart** — the shortest path to a working result, with real command output where it helps.
8. **Configuration / examples** — a short representative block. Once it exceeds roughly a screen, move it to `docs/` and link.
9. **Contributing** — one line plus a link: `See [CONTRIBUTING.md](CONTRIBUTING.md).` Never inline the process.
10. **License** — one line plus a link: `MIT © [AUTHOR_NAME]. See [LICENSE](LICENSE).` Never paste the license text.
11. **Maintainers / acknowledgments** *(optional)* — who maintains it, credit for prior art or dependencies.

Note: standard-readme itself places Maintainers and Thanks *before* Contributing and License, and treats a TOC as required for long READMEs. Either ordering is defensible; be internally consistent.

## `.github/` layout

```
.github/
  ISSUE_TEMPLATE/
    bug_report.md         # created by this skill
    feature_request.md    # created by this skill
    config.yml            # optional: disable blank issues, add contact links
  PULL_REQUEST_TEMPLATE.md  # created by this skill
  workflows/                # conventional home for GitHub Actions CI
  FUNDING.yml               # optional: sponsor button
  dependabot.yml            # optional: dependency updates
```

`workflows/` is noted here only so the convention is documented — **this skill does not create CI workflows.** CI is project-specific (language, test runner, matrix, secrets) and writing a plausible-looking workflow that has never been executed is worse than having none.

Markdown templates use YAML front matter (`name`, `about`, `title`, `labels`, `assignees`). GitHub also supports YAML *issue forms* (`.github/ISSUE_TEMPLATE/*.yml`) with structured, validatable fields — a reasonable upgrade for a project receiving real traffic, but markdown templates are simpler and fully sufficient for a small project.

## Template placeholders

Every placeholder used across `templates/`, and where its value comes from:

| Placeholder | Source |
|---|---|
| `[PROJECT_NAME]` | Repo name from `git remote -v`, else the directory name |
| `[AUTHOR_NAME]` | `git config user.name` — a human display name. Use it **only** for copyright and attribution lines (LICENSE, the README license line). Never in a URL |
| `[GITHUB_OWNER]` | Parsed from `git remote get-url origin` (the path segment before the repo name), or `gh api user --jq .login` if no remote is configured yet; ask if neither is available. This is the login used in clone/compare/release URLs |
| `[CONTACT_EMAIL]` | `git config user.email`. If it looks like a work/employer address rather than a personal one, confirm with the user before publishing it in CODE_OF_CONDUCT.md / SECURITY.md rather than applying it silently — publishing a work email in a personal open-source project is a decision, not a default |
| `[YEAR]` | Current year |
| `[LATEST_MINOR]` | From `git tag --sort=-v:refname`: take the newest tag, strip a leading `v` if present, drop the patch component (`v2.3.1` → `2.3`). If there are no tags yet there is no supported-versions table to fill — ask, or note in SECURITY.md that no release has shipped yet |

`[AUTHOR_NAME]` and `[GITHUB_OWNER]` are deliberately distinct: a display name with a space in it dropped into a clone URL produces a broken link.

There is **no first-release placeholder** by design. `templates/CHANGELOG.md` ships only an `## [Unreleased]` section — the first dated release section gets added manually when a release is actually cut, never fabricated by this skill. Same reasoning as the CI-workflow exclusion: a plausible-looking but false version and date is worse than nothing.

Not everything that looks like `[WORD]` is a fill-in placeholder — `[FAQ]`, `[LICENSE]`, `[VERSION]`, `[Unreleased]` and similar in the templates are markdown link references, not substitution targets. Only replace the placeholders listed in the table above.

Also replace the `(replace with this project's actual command)` markers in `CONTRIBUTING.md` with the real install/test/lint commands for the detected tooling. Leaving them in place ships a template that tells contributors nothing.

## Missing README

README.md is Required, but generating one is **Tier 2** — propose it and wait for confirmation, never auto-create it.

Build the draft only from verifiable facts about the repo: the actual detected entry point, actual filenames, the language and tooling actually present. Never invent install commands for a package that isn't published anywhere — don't write `pip install <name>` unless it is confirmed published to PyPI. This mirrors why this skill never generates CI workflows: a plausible-looking but wrong README is worse than an honest `usage: see main.py` placeholder.

If a README exists but is thin, rewriting its structure is the ordinary Tier 2 "touches existing content" case.

## Pre-publish sanity checks

- Secrets scanned in tracked files **and full history** — see secrets-scan.md. This gates everything else.
- README install and usage commands actually run on a clean checkout.
- No internal-only content: employer-internal URLs, ticket IDs, hostnames, wiki links, internal package registries, colleague names.
- No hardcoded personal paths (`/Users/<name>/…`) or machine-specific config.
- `.gitignore` covers the detected language and tooling, and ignores local env/secret files.
- LICENSE year and author are filled in, and the copyright holder is correct.
- Repo description and topics set on GitHub; these drive discoverability.
- Version tagged per [SemVer](https://semver.org/) (`MAJOR.MINOR.PATCH`) if anything is published to a package registry.
- Employer IP/open-source policy satisfied before publishing anything written on work time or with work resources.
