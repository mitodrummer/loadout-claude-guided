# loadout-claude-guided

A [Minimal](https://minimal.dev) loadout for people who want to change a
project without writing code or using git: designers, writers, PMs. Attach to
a session and Claude Code opens full screen. Describe a change in your own
words; Claude makes it, gives you a link to check it in your browser, and
opens a pull request for your reviewer.

Every change follows the same steps, set in the session's
`~/.claude/CLAUDE.md` ([`share/CLAUDE.md.in`](share/CLAUDE.md.in)):

1. a fresh branch from `main`, one per request
2. the change, previewed on the project's dev server, until you say it looks right
3. linters and nearby tests
4. a pull request in plain words, with your reviewer requested
5. the checks watched and fixed until they pass (the repo's `/shipit` when it has one)

Claude never merges, pushes to `main`, or asks you to run a command.

## Install

Needs min 0.6.0 or later (`min --version`).

```sh
git clone https://github.com/mitodrummer/loadout-claude-guided \
  ~/.config/minimal/loadouts-untrusted/claude-guided
~/.config/minimal/loadouts-untrusted/claude-guided/setup
```

`setup` asks for your name, email and reviewer, and makes this your default
loadout. Then, in the project:

```sh
min session activate --attach
```

The first time, GitHub asks you to sign in through your browser.

## For the reviewer

The prompts are guidance, not enforcement. Before handing this out, protect
the branch: require a pull request and your review to merge into `main`
(a CODEOWNERS entry does the requesting automatically). Give the contributor
a GitHub account with write access to that repo only, or a fine-grained token
(`GH_TOKEN`) limited to its contents and pull requests.

## Settings

`local/config` (from [`config.example`](config.example)):

| Setting | Default | |
| --- | --- | --- |
| `GUIDED_REVIEWER` | none (Claude asks) | GitHub username(s), comma-separated |
| `GUIDED_AUTOSHIP` | `true` | Keep going after the pull request opens until the checks pass |
| `GUIDED_PERMISSION_MODE` | `auto` | Claude Code's `--permission-mode` |
| `CLAUDE_ARGS` | none | Extra arguments for claude |

From your host shell, each optional: `GH_TOKEN`, and
`CLAUDE_CODE_OAUTH_TOKEN` or `ANTHROPIC_API_KEY` (otherwise Claude asks you
to log in).

## Files

| File | Purpose |
| --- | --- |
| `loadout.toml` | Packages, patches, hook and variables |
| `setup` | Asks for your details and writes `local/` |
| `bin/guided-session` | At attach: GitHub sign-in, `CLAUDE.md`, then Claude |
| `bin/claude-launch` | `claude update`, then the newest claude |
| `share/CLAUDE.md.in` | How Claude works with you |
