---
title: ytm-rs
one_liner: YouTube Music in a fast, native desktop app — built in Rust with egui and rodio, no Electron, ~130 MB while playing.
status: active
updated: 2026-11-27
discord_channel: https://discord.com/channels/1486966847810043924/1575787700148113408
---

## Where it stands

- Fork of upstream grapedjuice/ytm-rs (first project in the new `~/Local-Projects-2026/Forks/` folder); origin at https://github.com/Caffa/ytm-rs.
- Focus-steal fix (ab6cb54, 482bb64) is upstream PR #441, still awaiting review.
- Branch personal/quiet-challenge (pushed as side-screen-challenge-window) carries the side-screen version — f0a36d9 places the challenge window on the first non-primary screen, only floats it if it stays on the primary/only screen, and honours YTM_HIDE_CHALLENGE=1; 3ca0ce0 creates it at the final size and centers it on the measured outer frame. Open upstream as stacked PR #442.
- Installed app /Applications/ytm-rs.app now runs this personal build (portable.txt marker + ad-hoc re-sign, so settings live in /Applications/data, not ~/Library/Application Support); the backed-up upstream app is in personal-build/upstream-backup/.
- Research memo: `/Users/caffae/Local-Projects-2026/Content/research/ytm-rs-focus-steal-on-song-change.md`.

## Log

### 2026-11-27 — Challenge window on the side screen; installed app swapped to the personal build

Branch personal/quiet-challenge gained f0a36d9 (challenge window placed on the first non-primary screen as soon as it is created, always-on-top only if it is left on the primary or only screen, and YTM_HIDE_CHALLENGE=1 in the environment to keep the window hidden) and 3ca0ce0 (created at the final 360x160 size and centered on the runtime-measured outer-inner frame delta instead of the content box). The work is open upstream as PR #442 "Lyrics: the verification window moves to a side screen" (head branch side-screen-challenge-window on fork Caffa/ytm-rs), stacked on the still-open PR #441. The installed app at /Applications/ytm-rs.app was swapped to this personal build with a portable.txt marker and an ad-hoc re-sign — settings live in /Applications/data because portable_dir() resolves there, not in ~/Library/Application Support — and the upstream app is backed up in personal-build/upstream-backup/.

Next: watch PRs #441 and #442; merge or rebase as upstream review lands.

https://discord.com/channels/1486966847810043924/1575787700148113408

### 2026-11-27 — Forked ytm-rs and fixed the lyrics-window focus steal (PR #441)

Cloned grapedjuice/ytm-rs into `~/Local-Projects-2026/Forks/ytm-rs` and fixed the bug where the Cloudflare Turnstile lyrics-verification child window stole macOS focus on every song change (tao's `set_focus()` becomes `activateIgnoringOtherApps`, and a failed verify-turnstile exchange bypassed the challenge_failed backoff so the window could respawn per song). The fix makes `show()` display and float the window without taking focus and stamps the 15-minute backoff after a failed exchange — two commits (ab6cb54, 482bb64) on branch `lyrics-verification-window-focus`, with upstream PR https://github.com/grapedjuice/ytm-rs/pull/441 open. Learnings: `request_user_attention` is a no-op for Accessory apps with no Dock icon, so there is no non-stealing "notify" path on macOS.

Next: watch PR #441 for upstream review; merge or rebase once a decision lands.
