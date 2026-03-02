# Rebrand: cove → clove

## Motivation

"clove" is a natural typo when reaching for "cove" (muscle memory from typing "claude"). Leaning into it gives the tool a distinct identity and a mascot — the pixel-art clove character.

## Mascot

Source image: `assets/clove-mascot.png` (to be added)
Pixel-art style clove character with a rectangular head, two black square eyes, a long stem body, and two curly clove-bud arms.

## Stack Structure (2 diffs)

### Diff 1: Rename cove → clove (mechanical)

Find-replace across ~135 occurrences in the codebase.

#### Cargo.toml

- Package name: `cove-cli` → `clove-cli`
- Binary name: `cove` → `clove`
- Repository URL: `rasha-hantash/cove` → `rasha-hantash/clove`
- Homepage URL: same
- Keywords: `"cove"` → `"clove"`

#### Rust source files

| File                     | What changes                                                                                                                                                          |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `src/cli.rs`             | `#[command(name = "cove"` → `"clove"`                                                                                                                                 |
| `src/tmux.rs`            | `pub const SESSION: &str = "cove"` → `"clove"`                                                                                                                        |
| `src/commands/init.rs`   | All `cove hook` string literals → `clove hook`, function `cove_bin_path()` → `clove_bin_path()`, path `~/.local/bin/cove` → `~/.local/bin/clove`, all test assertions |
| `src/commands/hook.rs`   | `~/.cove/events/` → `~/.clove/events/`                                                                                                                                |
| `src/commands/start.rs`  | `~/.local/bin/cove` → `~/.local/bin/clove`, user-facing messages                                                                                                      |
| `src/commands/kill.rs`   | `"No active cove session"` → `"No active clove session"`                                                                                                              |
| `src/commands/list.rs`   | Same                                                                                                                                                                  |
| `src/commands/resume.rs` | Same, plus `"Run clove to create one"`                                                                                                                                |
| `src/sidebar/state.rs`   | `~/.cove/events/` → `~/.clove/events/`                                                                                                                                |

#### Config & metadata files

| File                            | What changes                                                                  |
| ------------------------------- | ----------------------------------------------------------------------------- |
| `CLAUDE.md`                     | All `cove` references in docs, install commands, architecture section         |
| `README.md`                     | All `cove` references: title, install commands, usage examples, command table |
| `dist-workspace.toml`           | Homebrew tap: `rasha-hantash/homebrew-cove` → `rasha-hantash/homebrew-clove`  |
| `.github/workflows/release.yml` | Homebrew repository reference                                                 |
| `command-r-pr-review-plan.md`   | All `cove` references in plan doc                                             |
| `plan-context-viewer.md`        | "Cove" → "Clove"                                                              |

#### Data directory migration

- `~/.cove/` → `~/.clove/`
- On first run after upgrade, check if `~/.cove/` exists and `~/.clove/` doesn't — print a migration hint telling the user to `mv ~/.cove ~/.clove` (or auto-migrate with confirmation).

#### Manual steps (not in code)

- Rename GitHub repo: `rasha-hantash/cove` → `rasha-hantash/clove`
- Rename Homebrew tap repo: `rasha-hantash/homebrew-cove` → `rasha-hantash/homebrew-clove`
- Update any external references (brain-os docs, dotfiles tmux config, etc.)

---

### Diff 2: Add mascot branding

#### CLI startup banner

Hand-crafted ~6-8 line Unicode block-art (`█ ▀ ▄ ▐ ▌`) version of the mascot, displayed when running `clove` or `clove start`. Uses the Catppuccin Mocha palette (existing `colors.rs` ANSI codes). Example placement: after session creation message in `commands/start.rs`.

#### Sidebar header

Keep the sidebar functional — no ASCII art in the narrow 30% pane. Add a subtle colored "clove" text label to the header line, replacing the plain session count or prepending it. Something like:

```
 clove · 3 sessions · ↑↓ navigate
```

#### README

Embed the actual PNG mascot image at the top of README.md. Store the source file at `assets/clove-mascot.png`.

#### What NOT to do

- Don't put ASCII art in the sidebar (too narrow, wastes vertical space)
- Don't replace the Claude Code logo (that's Claude Code's own UI, not controllable)
- Don't add the banner to every command (only startup/session creation)
