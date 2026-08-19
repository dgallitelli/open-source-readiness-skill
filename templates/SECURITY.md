# Security Policy

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.**

Report them privately using either of the following:

- **Email:** [CONTACT_EMAIL] — include `SECURITY` in the subject line.
- **GitHub private vulnerability reporting:** open the repository's **Security** tab and choose **Report a vulnerability** (if enabled).

To help triage quickly, please include as much of the following as you can:

- The type of issue (e.g. injection, path traversal, credential exposure, privilege escalation).
- The affected version, commit SHA, or release tag.
- Full paths of the source files related to the issue.
- Step-by-step instructions to reproduce, including any special configuration.
- Proof-of-concept or exploit code, if you have it.
- The impact — what an attacker can achieve, and any preconditions required.

Please report in English where possible.

## What to Expect

| Stage | Target |
|---|---|
| Acknowledgement of your report | Within 3 business days |
| Initial assessment and severity triage | Within 7 business days |
| Status updates while work is in progress | At least every 14 days |
| Fix released, or a documented decision not to fix | Within 90 days of triage for confirmed issues |

This is a personal, spare-time project rather than a commercially supported product, so these are good-faith targets and not a contractual SLA. If you have not heard back within the acknowledgement window, please send a follow-up — it means the first message was missed, not ignored.

## Disclosure Policy

- Please give a reasonable opportunity to release a fix before disclosing publicly. **90 days** from acknowledgement is the default coordinated-disclosure window, and shorter timelines can be agreed for issues already being exploited.
- Once a fix is released, a security advisory will be published and you will be credited by name or handle unless you ask to remain anonymous.
- If a report is declined as not-a-vulnerability, you are free to disclose it publicly, and an explanation of the reasoning will be provided.

## Supported Versions

Security fixes are applied to the versions below. Older versions receive no patches — upgrade to a supported release.

| Version | Supported |
|---|---|
| [LATEST_MINOR].x | Yes |
| < [LATEST_MINOR].0 | No |

## Scope

**In scope:** vulnerabilities in this repository's own source code, its build and release configuration, and its default runtime behavior.

**Out of scope:**

- Vulnerabilities in third-party dependencies — report those upstream, though a heads-up here is welcome so the dependency can be pinned or replaced.
- Findings that require an already-compromised machine, a malicious local user, or physical access.
- Missing hardening or best-practice recommendations with no demonstrated impact.
- Results from automated scanners submitted without a working reproduction.
- Social engineering, and denial of service through resource exhaustion in a local CLI context.

## Safe Harbor

Good-faith security research on this project is welcome. If you make a genuine effort to avoid privacy violations, data destruction, and service disruption, and you report promptly and do not exploit the issue beyond what is needed to demonstrate it, no legal action will be pursued over your research.

Thanks for helping keep this project and its users safe.
