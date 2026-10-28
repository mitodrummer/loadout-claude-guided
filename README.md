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
4. a pull request in plain words, for review
5. the checks watched and fixed until they pass (the repo's `/shipit` when it has one)

Claude never merges, pushes to `main`, or asks you to run a command.

To report a bug or idea without changing anything, say "file an issue":
Claude asks a couple of questions, checks it isn't already filed, shows you
the draft, and opens it once you agree.

## Install

Needs min 0.6.0 or later (`min --version`).

```sh
git clone https://github.com/mitodrummer/loadout-claude-guided \
  ~/.config/minimal/loadouts/claude-guided
~/.config/minimal/loadouts/claude-guided/setup
```

`setup` writes your settings and makes this your default loadout. Then, in
the project:

```sh
min session activate --network own_ip --ingress 4321:4321 --allow-subnets 0.0.0.0/0 --attach
```

`--ingress` makes the project's dev server (here on port 4321; use your
project's port) open at <http://localhost:4321> on your computer, and
`--allow-subnets` gives the session internet access. Without them Claude can
still make changes, but you can't preview them.

The first time, you sign in to GitHub and then to Claude, each by opening a
link and entering a code; Claude then welcomes you with a short tour. Your
changes are committed under your GitHub account's name and email unless you
set `GIT_USER_*`.

## For the reviewer

The prompts are guidance, not enforcement. Before handing this out, protect
the branch: require a pull request and your review to merge into `main`, and
have the repository assign reviewers (CODEOWNERS or a ruleset), or set
`GUIDED_REVIEWER`. Give the contributor write access to that repository
only.

## Settings

`local/config` (from [`config.example`](config.example)):

| Setting | Default | |
| --- | --- | --- |
| `GUIDED_REVIEWER` | none | Reviewer(s) to request, comma-separated. Unset: the repository assigns them |
| `GUIDED_AUTOSHIP` | `true` | Keep going after the pull request opens until the checks pass |
| `GUIDED_PERMISSION_MODE` | `auto` | Claude Code's `--permission-mode` |
| `CLAUDE_ARGS` | none | Extra arguments for claude |
| `GIT_USER_NAME`, `GIT_USER_EMAIL` | your GitHub account | Your git identity for commits |

## Files

| File | Purpose |
| --- | --- |
| `loadout.toml` | Packages, patches, hook and variables |
| `setup` | Writes `local/config` and adds the loadout to your defaults |
| `bin/guided-session` | At attach: GitHub sign-in and git identity, `CLAUDE.md`, then Claude |
| `bin/claude-launch` | `claude update`, then the newest claude |
| `share/CLAUDE.md.in` | How Claude works with you |
| `skills/report-issue/` | Files a bug or idea as a GitHub issue |
