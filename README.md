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
5. the checks watched and fixed, and automated review comments answered, until both are clean

Claude never merges, pushes to `main`, or asks you to run a command.

To report a bug or idea without changing anything, say "file an issue":
Claude asks a couple of questions, checks it isn't already filed, shows you
the draft, and opens it once you agree.

## New to the terminal? Start here

These steps set up a Mac (Apple Silicon, M1 or later) from nothing. The
terminal is an app where you type commands: paste each command below, press
Return, and wait for it to finish before the next one. The examples use
[gominimal/webapp](https://github.com/gominimal/webapp); swap in the
project you were invited to.

1. **Install Ghostty**, the terminal app. Download it from
   <https://ghostty.org/download>, drag it into Applications, and open it.
   Everything below happens in its window.

2. **Install Homebrew**, which installs the rest. It asks for your Mac
   password (nothing shows as you type; that's normal) and may install
   Apple's command line tools, which takes a few minutes.

   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

   When it finishes it prints **Next steps** with a couple of commands. Paste
   and run those too, then close Ghostty and open it again.

3. **Install the GitHub command line tool:**

   ```sh
   brew install gh
   ```

4. **Sign in to GitHub.** Choose GitHub.com, HTTPS, and "Login with a web
   browser", then enter the code it shows in the browser page that opens.
   `--scopes user:email` lets your commits carry your real email.

   ```sh
   gh auth login --scopes user:email
   ```

5. **Install Minimal:**

   ```sh
   curl --proto "=https" --tlsv1.2 -fsSL https://go.minimal.dev/stable | sh
   ```

   Close Ghostty and open it again, then check it worked: `min --version`
   prints a version number.

6. **Get the project** and go into its folder:

   ```sh
   gh repo clone gominimal/webapp ~/webapp
   cd ~/webapp
   ```

Then continue with [Install](#install) below. Next time, open Ghostty, run
`cd ~/webapp`, and run the `min session activate` command from Install.

## Install

Needs min 0.6.0 or later (`min --version`).

```sh
git clone https://github.com/mitodrummer/loadout-claude-guided \
  ~/.config/minimal/loadouts/claude-guided
~/.config/minimal/loadouts/claude-guided/setup
```

`setup` writes your settings and makes this your default loadout. Then, in
the project's folder (`cd ~/webapp` in the example above):

```sh
GH_TOKEN="$(gh auth token)" min session activate --network own_ip --ingress 4321:4321 \
  --allow-subnets 0.0.0.0/0 --attach
```

`GH_TOKEN="$(gh auth token)"` hands your GitHub sign-in from `gh auth login`
on your Mac to the session, so you don't sign in again there. Without it, the
session asks you to sign in to GitHub. `--ingress` makes the project's dev
server (port 4321 for webapp; use your project's port) open at
<http://localhost:4321> on your computer. `--allow-subnets` gives the session
internet access, which Claude needs.

The first time, you sign in to Claude by opening a link and entering a code,
and Claude then welcomes you with a short tour. Your changes are committed
under your GitHub account's name and email unless you set `GIT_USER_*`.

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
| `bin/guided-session` | At attach: GitHub and Claude sign-in, git identity, `CLAUDE.md`, then Claude |
| `share/CLAUDE.md.in` | How Claude works with you |
| `skills/report-issue/` | Files a bug or idea as a GitHub issue |
