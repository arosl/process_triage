# This fork

arosl/process_triage is a fork of [Dicklesworthstone/process_triage](https://github.com/Dicklesworthstone/process_triage), made for [arosl/agentic_coding_flywheel_setup](https://github.com/arosl/agentic_coding_flywheel_setup). That ACFS fork runs its coding agents in [herdr](https://herdr.dev) instead of tmux, and upstream's pt classes the herdr server as an abandoned process that is safe to kill. The fork follows upstream closely and carries only the changes below. Upstream is merged regularly (`git fetch upstream && git merge upstream/main`).

## Changes from upstream

| Change | Why | Commit |
|---|---|---|
| herdr is treated as a terminal multiplexer, like tmux and screen: its server is protected, and a herdr pane counts as a session signal | pt must never kill the server that runs every agent | 38c08b8 |
| `install.sh` defaults `OWNER` to arosl, and the `pt` wrapper's `pt update` reads the fork's releases | an install or an update would otherwise fetch upstream's build, which lacks the herdr change | d5d7065 |
| The version carries a `-herdr.N` suffix (`2.3.0-herdr.1`), and the installer's installed-version check keeps the suffix | it tells the fork's builds apart from upstream's; without the suffix the check never matches, and every run reinstalls | d5d7065 |
| CI's version-format check accepts that suffix | the check accepted only `X.Y.Z` | 8e94674 |

## Releases

Push events don't start this fork's workflows. To release, tag `v<upstream version>-herdr.<N>` and start the release workflow on the tag: `gh workflow run release.yml --ref <tag> -f dry_run=false`.

Releases are unsigned until the fork has its own signing key, so `pt update` (which verifies by default) refuses them. Use `pt update --no-verify`, or update through ACFS.
