# dsh-mcma

A self-evolving math tutoring and grading agent, packaged as a DeepSeek Harness plugin. MCMA maps
the PyTorch training loop onto agent work: a forward pass teaches one problem or grades one
solution attempt, the Evaluator is the loss function, and the Evolver is the gradient.

## Install

```text
npm install --no-audit --no-fund
npm run build
```

Install the directory as a DSH bundle. The bundle patch `cordis.patch.yml` inserts one row,
`dsh-mcma`, and the browser half loads from `client/index.js`.

## Commands

| Command | Effect |
| --- | --- |
| `/mcma_help [--topics <topics>]` | Print the help page and the live state. |
| `/mcma_forward [--tutor\|--grader] <problems>` | Training pass. The default mode is `--tutor`. |
| `/mcma_backward <session-id> ...` | Apply one pending evolution plan per session. |
| `/mcma_feedback --session <id> <text> ...` | Add human feedback to pending sessions. |
| `/mcma_validate [--tutor\|--grader] <problems>` | Validation pass. It rolls back after three worse passes. |
| `/mcma_test [--tutor\|--grader] <problems>` | Test pass. It changes no parameters version. |
| `/mcma_clear_appdata [--yes]` | Restore the packaged seed app data. |

A problem argument is a file path, a directory path, an image path, or the problem text itself.

## Behavior

- One forward pass fits 10 minutes, and it runs the tutoring stage or the grading stage, never
  both. It writes `report.html` and `code/` into the session workspace, and the evaluation record
  into the app data.
- The Tutor and the Grader create a Python environment with `uv`, and they verify each deduction
  with numpy, scipy, and matplotlib. A computed result outranks an inferred one.
- Every subagent follows the two style guides in `<app_data>/styles/`.
- App data defaults to `~/.dsh-mcma/`. The plugin settings page sets the app data root, the five
  stage time limits, and the rollback threshold.

## Development

```text
npm run typecheck
npm test
npm run smoke
npm run docs
npm run seed
```

`AGENTS.md` holds the style rules and the repository conventions.

## Versioning

This repository has one special rule: its version tag matches the version tag of the `agents` submodule
exactly. The two repositories release as one pair, so `agents` `v0.39.2` publishes `agents-docs`
`v0.39.2`.

- Read the plugin tag before you release this repository: `git -C ../agents describe --tags --abbrev=0 main`.
  The `main` argument matters, because the working branch `rel` holds no tag.
- Tag with that exact string. Do not choose an independent number, and do not add a suffix.
- Tag this repository after the plugin tag reaches the remote, so a reader never sees a docs tag without
  the plugin release behind it.
- A documentation change without a plugin change still takes the current plugin tag. Two tags then name
  the same plugin version, and the later commit carries the newer documentation.

## Copyright

```text
Copyright (C) 2026 Yucheng Liu. Released under the GNU AGPL 3.0 and CC BY-NC-SA 4.0 Licenses.

Textual contents are released under the GNU AGPL 3.0 License.
See [LICENSE](./LICENSE) and https://www.gnu.org/licenses/agpl-3.0.txt.

Non-textual contents are released under the CC BY-NC-SA 4.0 License.
See [license-2](./license-2) and https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode.txt.
```
