# generate-changelog

> **One bash script. Full `CHANGELOG.md`. Zero dependencies.**

> **A self-contained bash script that turns your git history into a beautiful,
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/)–formatted `CHANGELOG.md` — automatically.**

> ☕ **If this saved you time, [buy me a coffee](https://ko-fi.com/trigger68510) or [sponsor me on GitHub](https://github.com/sponsors/TriggerXZ)!** Every contribution helps me keep building free tools.

- ✅ **Zero deps** — just `bash`, `git`, and `awk`. No npm, no Python, no jq.
- ✅ **Conventional commit aware** with heuristic fallback for non-conventional repos
- ✅ **BREAKING CHANGE detection** with a dedicated warning section
- ✅ **Auto-bumps semver** from your last git tag (`v1.2.3` → `1.2.4`)
- ✅ **Idempotent** — re-running refuses to duplicate
- ✅ **Safety first** — refuses to run on a dirty working tree
- ✅ **MIT-licensed** — use it, modify it, ship it in commercial products
- ✅ **Claude Code integration** — drop `SKILL.md` in `~/.claude/skills/` for the `/generate-changelog` slash command

## Quick start

```bash
# 1. Download changelog.sh into your repo
curl -O https://raw.githubusercontent.com/TriggerXZ/generate-changelog/main/changelog.sh
chmod +x changelog.sh

# 2. Run it
bash changelog.sh

# Output:
# changelog.sh: wrote CHANGELOG.md (version 1.2.4, 17 commits)
#   Added:    8
#   Fixed:    4
#   Changed:  5
#   Removed:  0
#   Breaking: 1
```

That's it. Your `CHANGELOG.md` is now ready.

## Example output

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.4] - 2026-08-01

### ⚠️ BREAKING CHANGES
- feat(api)!: rename /v1/users to /v2/users (a7ecb8e)

### Added
- feat(api)!: rename /v1/users to /v2/users (a7ecb8e)
- feat(auth): add password reset flow (f61790c)
- added dark mode support (4eb95c9)

### Fixed
- fix: handle null pointer in login (7b1f900)

### Changed
- refactor: extract auth middleware (878063c)
- perf: cache user lookups (4080117)

### Removed
- removed deprecated v1 endpoints (88bc0dd)
```

## Features

### Conventional commit aware

Recognizes `feat`, `fix`, `refactor`, `perf`, `docs`, `chore`, `remove`, and more. Falls back to a first-verb heuristic (`added` → Added, `removed` → Removed) for repos that don't follow the convention.

### BREAKING CHANGE detection

Commits with `!` after the type (`feat(api)!: ...`) or `BREAKING CHANGE` text in the body get a dedicated `### ⚠️ BREAKING CHANGES` section at the top of the release block.

### Smart version inference

Reads your last git tag (`v1.2.3`) and bumps the patch version (`1.2.4`). Override with `--version 2.0.0` when you need to.

### Idempotent

Running the script twice for the same version refuses to duplicate. CI-friendly.

### Safety first

Refuses to run on a dirty working tree (so it can't mix uncommitted work with your changelog). Bypass with `--allow-dirty` if you know what you're doing.

## Usage

```bash
# Default: since the most recent git tag
bash changelog.sh

# Explicit range
bash changelog.sh --since v1.0.0

# Explicit version
bash changelog.sh --version 2.0.0

# Don't consume an existing [Unreleased] block
bash changelog.sh --keep-unreleased

# Bypass the clean-tree check
bash changelog.sh --allow-dirty

# Custom output file
bash changelog.sh --output HISTORY.md

# See all options
bash changelog.sh --help
```

## Claude Code integration

Drop `SKILL.md` into `~/.claude/skills/generate-changelog/`. Then in any project, type `/generate-changelog` and Claude will run the script for you.

```bash
mkdir -p ~/.claude/skills/generate-changelog
curl -o ~/.claude/skills/generate-changelog/SKILL.md \
  https://raw.githubusercontent.com/TriggerXZ/generate-changelog/main/SKILL.md
```

## Testing

Tested against a synthetic 10-commit repo with mixed conventional and non-conventional messages. All 7 scenarios pass with real, verified output:

| # | Scenario | Result |
|---|----------|--------|
| 1 | Default (since last tag) | ✅ |
| 2 | Full history with breaking change | ✅ |
| 3 | Idempotency (re-run) | ✅ |
| 4 | Dirty tree refused | ✅ |
| 5 | Empty range | ✅ |
| 6 | `--help` flag | ✅ |
| 7 | Non-conventional heuristic | ✅ |

See [`SAMPLE_OUTPUT.md`](./SAMPLE_OUTPUT.md) for the full test runs.

## Compatibility

- ✅ macOS, Linux, WSL
- ✅ Git Bash on Windows (tested)
- ✅ Any repo with `git` installed
- ✅ Conventional commits, semi-conventional, and non-conventional histories

## Why this exists

I got tired of writing `CHANGELOG.md` by hand every release. Existing tools (`standard-version`, `release-please`, `auto-changelog`) are great but all depend on Node.js. I wanted something that works on any Unix system with just `bash` and `git` — no `npm install`, no `package.json`, no `node_modules` bloat.

If you've ever procrastinated on updating your changelog, this is for you.

## License

MIT — see [`LICENSE`](./LICENSE).

## Author

Dante ([@TriggerXZ](https://github.com/TriggerXZ)) — built with the help of an AI agent.

☕ **Support this project**: [buymeacoffee.com/trigger68510](https://ko-fi.com/trigger68510) | [GitHub Sponsors](https://github.com/sponsors/TriggerXZ)

## Related

- [Keep a Changelog](https://keepachangelog.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
