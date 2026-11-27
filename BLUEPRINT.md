---
title: ytm-rs
one_liner: YouTube Music in a fast, native desktop app — built in Rust with egui and rodio, no Electron, ~130 MB while playing.
status: active
updated: 2026-11-27
discord_channel: https://discord.com/channels/1486966847810043924/1575787700148113408
---

## Where it stands

- Fork of upstream grapedjuice/ytm-rs (first project in the new `~/Local-Projects-2026/Forks/` folder for app forks); origin at https://github.com/Caffa/ytm-rs.
- Branch `lyrics-verification-window-focus` carries the focus-steal fix (commits ab6cb54, 482bb64); clippy, 739 tests, and release build all pass.
- Upstream PR open and awaiting review: https://github.com/grapedjuice/ytm-rs/pull/441
- Research memo: `/Users/caffae/Local-Projects-2026/Content/research/ytm-rs-focus-steal-on-song-change.md`.

## Log

### 2026-11-27 — Forked ytm-rs and fixed the lyrics-window focus steal (PR #441)

Cloned grapedjuice/ytm-rs into `~/Local-Projects-2026/Forks/ytm-rs` and fixed the bug where the Cloudflare Turnstile lyrics-verification child window stole macOS focus on every song change (tao's `set_focus()` becomes `activateIgnoringOtherApps`, and a failed verify-turnstile exchange bypassed the challenge_failed backoff so the window could respawn per song). The fix makes `show()` display and float the window without taking focus and stamps the 15-minute backoff after a failed exchange — two commits (ab6cb54, 482bb64) on branch `lyrics-verification-window-focus`, with upstream PR https://github.com/grapedjuice/ytm-rs/pull/441 open. Learnings: `request_user_attention` is a no-op for Accessory apps with no Dock icon, so there is no non-stealing "notify" path on macOS.

Next: watch PR #441 for upstream review; merge or rebase once a decision lands.
