# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this repository, please report it responsibly.

**Do NOT open a public GitHub issue.**

Instead, use one of the following methods:

- **GitHub Security Advisories:** [Report a vulnerability](https://github.com/1080000000kmph/repo2tipi/security/advisories/new)
- **Email:** Send details to the repository maintainer

Please include:

- Description of the vulnerability
- Affected app(s) or component
- Steps to reproduce
- Potential impact
- Suggested fix (if available)

We will acknowledge your report within **48 hours** and provide a detailed response within **7 days**.

---

## Scope

### In Scope

- Vulnerabilities in `docker-compose.yml` configurations (exposed ports, privileged containers, missing security options)
- Hardcoded secrets, passwords, or API keys in app configurations
- Insecure default configurations in `config.json` (e.g., `privileged: true` without justification)
- Container escape risks or excessive capabilities (`cap_add`)
- Supply chain risks in Docker image references (unpinned tags, unverified images)
- Cross-app network exposure via misconfigured `add_to_main_network`
- Vulnerabilities in the web converter at [repo-2-tipi.lovable.app](https://repo-2-tipi.lovable.app/)

### Out of Scope

- Vulnerabilities in upstream Docker images (report to the image maintainer)
- Vulnerabilities in Runtipi itself (report to [runtipi/runtipi](https://github.com/runtipi/runtipi))
- Issues in third-party applications packaged in this store
- Denial of service attacks
- Social engineering

---

## Security Best Practices for Apps

All apps in this store follow these guidelines:

| Practice | Requirement |
|----------|-------------|
| **No hardcoded secrets** | Passwords and tokens use `${VARIABLE}` references with `form_fields` |
| **No privileged mode** | `privileged: true` only when absolutely necessary and documented |
| **Minimal capabilities** | `cap_add` only when required; prefer `cap_drop: ALL` |
| **Pinned image tags** | Docker images use specific version tags, not `latest` |
| **Persistent data** | Volumes use `${APP_DATA_DIR}`, not host paths |
| **No host network** | `network_mode: host` is avoided unless essential |
| **Read-only root** | `read_only: true` preferred where possible |
| **Non-root user** | `user: "1000:1000"` when the image supports it |

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest `main` branch | ✅ |
| Previous releases | ❌ |

We only maintain the latest version. Update your app store regularly.

---

## Disclosure Policy

- Vulnerabilities are disclosed after a fix is merged and released
- We follow **coordinated disclosure** — reporters are notified before public disclosure
- CVEs are requested for significant vulnerabilities when appropriate
- Credit is given to reporters unless they prefer to remain anonymous

---

## Automated Security

This repository uses:

- **Renovate Bot** — Automatically creates PRs for Docker image updates, keeping apps on patched versions
- **GitHub Actions** — CI pipeline validates app configurations on every PR
- **Schema validation** — All `config.json` and `docker-compose.yml` files are validated against the Runtipi schema

---

## Thank You

Responsible disclosure helps keep the Runtipi ecosystem safe. We appreciate all security reports and will work with you to resolve issues promptly.
