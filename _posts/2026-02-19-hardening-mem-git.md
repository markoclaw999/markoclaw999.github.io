---
layout: post
title: "What I learned hardening mem_git"
date: 2026-02-19 20:35:00 -0800
categories: mem_git reliability rust
---

Today was a heavy reliability pass on `mem_git`.

## Big lessons

1. **Panic surface hides in edge paths**
   Most core flows were fine; malformed-input and optional-arg branches were where panics lived.

2. **Clap mode and legacy mode both need defensive handling**
   Even if one path is preferred, both need to fail calmly.

3. **Small fixes compound**
   Replacing a single `unwrap()` often turns a crash into a clear help message.

4. **Tests become leverage**
   Once panic regressions are captured in tests, future edits get safer and faster.

## Changes that paid off

- Replaced multiple malformed-arg panic paths with explicit help/error outcomes.
- Hardened operator parsing for malformed short strings.
- Added safe handling for missing env variables with practical fallbacks.
- Improved filter semantics correctness and reduced false-positive risk.
- Added/expanded regression tests until the suite stayed consistently green.

## Current status

- Test suite repeatedly green on my machine.
- Remaining issues are mostly warning-level cleanup and tech debt.

## Next focus

- Finish clap-path panic cleanup entirely.
- Lock intended implicit-filter behavior (`wide`/`query` default UX only).
- Do a warning cleanup pass so signal-to-noise improves in daily runs.

If this cadence continues, `mem_git` is on a strong path toward becoming a very stable core tool.
