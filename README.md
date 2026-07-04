# Runtipi Custom app store: repo2tipi

App store and automated converter for [Runtipi](https://runtipi.io/) — turning GitHub repositories into deployable Runtipi apps.

### ⚠️ UNDER CONSTRUCTION ⚠️ This repository is ongoing work in progress. But what is in here, works flawless!

---
A community-maintained custom app store for Runtipi. Add it to your Runtipi instance to install apps not available in the official store.

## Add to Runtipi

1. Open your Runtipi dashboard
2. Go to **Settings** → **App Stores**
3. Add this URL:

```
https://github.com/1080000000kmph/repo2tipi
```

4. Save — the apps will appear in your store

## Repository Structure

```
repo2tipi/
├── apps/                        # App definitions
│   ├── azuracast/
│   │   ├── config.json           # App metadata & form fields
│   │   ├── docker-compose.yml    # Service definitions (x-runtipi)
│   │   ├── description.md        # Long-form description
│   │   ├── logo.jpg              # App icon
│   │   └── metadata/             # Additional assets
│   ├── project-nomad/
│   └── web-check/
├── scripts/
│   └── update-config.ts         # Auto-update config on image bumps
├── __tests__/                   # App validation tests
├── .github/workflows/           # CI/CD pipelines
├── renovate.json                # Renovate bot for auto-updates
├── config.js                    # Allowed CI commands
└── package.json                 # Dependencies & test runner
```

### Per-App Files

| File | Required | Purpose |
|------|----------|---------|
| `config.json` | Yes | App metadata, categories, form fields, version |
| `docker-compose.yml` | Yes | Service definitions with `x-runtipi` metadata |
| `description.md` | No | Long-form description shown in the UI |
| `logo.jpg` / `logo.png` | No | App icon in the store |

## Auto-Updates

[Renovate Bot](https://docs.renovatebot.com/) watches Docker image tags and creates PRs when updates are available:

- Detects new image versions in `docker-compose.yml` files
- Bumps `tipi_version` in `config.json` automatically
- Runs validation tests before merging
- Database images (PostgreSQL, MariaDB, MySQL, MongoDB, Redis) are excluded for stability

## Development

```bash
git clone https://github.com/1080000000kmph/repo2tipi.git
cd repo2tipi
bun install
bun test          # Validate all apps
```

### Add a New App

1. Create a folder under `apps/` with a lowercase hyphenated id
2. Add `config.json` — [reference](https://runtipi.io/docs/reference/config-json)
3. Add `docker-compose.yml` with `x-runtipi` — [reference](https://runtipi.io/docs/reference/dynamic-compose)
4. Add `description.md` and a logo (recommended)
5. Run `bun test` to validate
6. Open a Pull Request

---

# Part 2 — **[Repo 2 Tipi Automator](https://repo-2-tipi.lovable.app/)**

A zero-touch Docker → Runtipi converter built with [Lovable](https://lovable.dev/). Paste any GitHub URL and get a complete, ready-to-deploy Runtipi app package.

## How It Works

1. **Paste** a GitHub repository URL
2. **Fetch** — The tool scans the repo for Docker Compose files or Dockerfiles
3. **Analyze** — Parses services, ports, volumes, environment variables, and dependencies
4. **Generate** — Produces a full Runtipi app package:
   - `config.json` — App metadata with form fields extracted from env vars
   - `docker-compose.yml` — Service definitions with `x-runtipi` metadata
   - `description.md` — App description
   - `logo.jpg` — App icon (when available)
5. **Validate** — Output is validated against the Runtipi schema before download

## Smart Detection

The converter searches for Docker Compose files in standard locations:

- `docker-compose.yml` / `docker-compose.yaml`
- `compose.yml` / `compose.yaml`
- `docker/docker-compose.yml`
- `.docker/compose.yml`
- `deploy/docker-compose.yaml`

If no compose file is found, it falls back to parsing the `Dockerfile` directly — extracting `EXPOSE` ports, `ENV` variables, and the base image.


## Contributing

- **Add apps** to the store → Fork, add app folder, validate, PR
- **Improve the converter** → Use the [web tool](https://repo-2-tipi.lovable.app/) and report issues
- **Request an app** → Open an issue with the GitHub repo URL

## License

[WTFPL v2](http://www.wtfpl.net/)

## Links

- **App Store Repo:** [github.com/1080000000kmph/repo2tipi](https://github.com/1080000000kmph/repo2tipi)
- **Web Converter:** [repo-2-tipi.lovable.app](https://repo-2-tipi.lovable.app/)
- **Runtipi Docs:** [runtipi.io/docs](https://runtipi.io/docs)
- **Dynamic Compose Reference:** [runtipi.io/docs/reference/dynamic-compose](https://runtipi.io/docs/reference/dynamic-compose)
- **config.json Reference:** [runtipi.io/docs/reference/config-json](https://runtipi.io/docs/reference/config-json)
- **Create Your Own App Store:** [runtipi.io/docs/guides/create-your-own-app-store](https://runtipi.io/docs/guides/create-your-own-app-store)
