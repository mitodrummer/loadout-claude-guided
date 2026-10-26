---
name: report-issue
description: Use when the user wants to report a bug, suggest a feature, or note a small tweak for later instead of changing it now — "file an issue", "report this", "log a bug", "add this to the backlog", "can someone look at". Turns their description into a GitHub issue on the current repository, after they approve the draft.
---

# Report an issue

The user describes something in their own words; you turn it into an issue a
developer can act on, without making them learn GitHub.

## 1. Understand it

Work out which kind it is:

- **Bug**: something is broken or looks wrong.
- **Feature**: something new the site or app should do.
- **Tweak**: a small change to wording, spacing, colour or layout.

Ask at most three short questions, only for what you can't find out
yourself: which page, what they expected, what they saw. Find the rest
yourself: open the page on the dev server, read the code behind it, take a
screenshot to confirm what they describe. Use their words for the problem;
add the technical detail (file paths, the component) in a separate section.

## 2. Check it isn't already filed

```bash
gh issue list --state open --search "<a few key words>" --limit 10
```

If one matches, show it to them and ask whether to add a comment there
(`gh issue comment <n>`) instead of opening a new one.

## 3. Draft it

Follow the repository's issue templates when it has them
(`.github/ISSUE_TEMPLATE/`). Otherwise:

- **Title**: what's wrong or wanted, in under 70 characters
  ("Pricing page: plan cards overlap on narrow screens").
- **Body**:
  - *What happens* (bug) or *What I'd like* (feature, tweak), in the user's words
  - *Where*: the page path, and the screen size or browser if it matters
  - *Expected* (bugs only)
  - *Details for developers*: the files and components involved, and what
    you saw when you checked
- **Labels**: only ones the repo already has (`gh label list`); `bug` or
  `enhancement` when present. Never create labels.

Show the user the title and body in plain words and ask "Shall I file this?"
File nothing until they say yes.

## 4. File it

```bash
gh issue create --title "<title>" --body-file <file> [--label <label>]
```

Give them the link, and say in one line what happens next: a developer will
look at it, and they'll see replies on GitHub.

## Don'ts

- Don't start fixing it unless they ask; reporting is the point.
- Don't assign people or set milestones.
- Don't paste secrets, tokens or personal data into an issue.
