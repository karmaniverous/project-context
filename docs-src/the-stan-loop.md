---
title: The STAN Loop
---

# The STAN Loop

![STAN Loop](https://github.com/karmaniverous/stan/raw/main/assets/stan-loop.png)

STAN establishes a simple, reproducible loop for AI‑assisted development.

## 1) Build & Snapshot

- Edit code locally.
- Run `stan run` to:
  - Execute your configured scripts (build/test/lint/typecheck, etc.).
  - Capture deterministic text outputs under `.stan/output/*.txt`.
  - Create `archive.tar` (full snapshot of text sources) and `archive.diff.tar` (files changed since the last snapshot).
- Archive warnings are written to the console (binaries excluded; large text call‑outs).

Tips:
- Use `stan run -p` to print the plan without side effects.
- Use `-q` for sequential execution (preserves `-s` order).
- Use `-c` to include outputs inside archives and remove them from disk (combine mode).

## 2) Share & Baseline

- Attach `.stan/output/archive.tar` (and `archive.diff.tar` if present) in your chat.
- Optionally run `stan snap` to update the diff baseline (and use `undo`/`redo` to navigate history).
- In chat, STAN reads the system prompt from the archive and verifies integrity before proceeding.

Notes:
- The bootloader system prompt ensures the correct `stan.system.md` is loaded from the archive (see Getting Started).
- If the system prompt appears to differ from the packaged baseline or docs were updated, CLI preflight prints a concise nudge.

## 3) Discuss & Patch

- Iterate in chat to refine requirements and approach.
- STAN generates plain unified diffs with adequate context (no base64).
- Apply patches:
  - `stan patch` (clipboard by default),
  - `stan patch -f <file>` (from a file),
  - `stan patch --check` (validate only; writes to sandbox).
- On failure, STAN writes a compact FEEDBACK packet and (when possible) copies it to your clipboard—paste it back into chat to get a corrected diff.

## Rinse and repeat

- Return to step 1 and continue until the feature is complete, CI is green, and the plan (`.stan/system/stan.todo.md`) is up‑to‑date.

## Why this loop?

- Reproducible context: all inputs to the discussion are deterministic (source snapshot + text outputs).
- Auditable diffs: patches are plain unified diffs with adequate context; failures come with actionable FEEDBACK.
- Minimal ceremony: one command to capture, one to apply.

## Pro tips

- Keep patches tight and anchored. Include ≥3 lines of context and use `a/` and `b/` path prefixes.
- Use `--check` before overwriting files; let FEEDBACK drive corrections instead of manual fixes.
- Update `.stan/system/stan.todo.md` as part of each change set; include a commit message (fenced) in chat.
- Prefer smaller loops (fewer files at a time) when exploring or refactoring to keep reviews fast and reliable.
