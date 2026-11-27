---
title: ytm-rs
one_liner: YouTube Music in a fast, native desktop app — built in Rust with egui and rodio, no Electron, ~130 MB while playing.
status: active
updated: 2026-11-27
discord_channel: https://discord.com/channels/1486966847810043924/1575787700148113408
---

## Where it stands

- Fork of upstream grapedjuice/ytm-rs (origin https://github.com/Caffa/ytm-rs); both upstream PRs are merged — #441 stops the lyrics-verification window stealing keyboard focus on song change, #442 opens the challenge window centered on its screen. The maintainer kept this fork's centering/frame-delta math in the #442 merge but dropped the side-screen placement and YTM_HIDE_CHALLENGE, adding macOS points-based positioning and a Linux saturating_sub instead.
- Fork main is synced to upstream main (a69c30a); the merged PR branches are deleted on the fork, and personal/quiet-challenge is kept for history.
- Installed app /Applications/ytm-rs.app runs the personal-only branch personal/side-screen (e8f7bfa = upstream main + side-screen challenge-window placement, no hide env) — a fork of official 0.4.0, built with a portable.txt marker and an ad-hoc re-sign so settings live in /Applications/data, not ~/Library/Application Support.
- To update the installed app later: rebase personal/side-screen onto upstream main and rebuild.
- Research memo: `/Users/caffae/Local-Projects-2026/Content/research/ytm-rs-focus-steal-on-song-change.md`.

## Log

### 2026-11-27 — Both PRs merged upstream; personal side-screen build installed

Both upstream PRs landed: #441 (verification window no longer steals keyboard focus on song change) and #442 (challenge window centered on its screen). In the #442 merge the maintainer kept the centering and frame-delta math from this fork but dropped the side-screen placement and YTM_HIDE_CHALLENGE, adding macOS points-based positioning and a Linux saturating_sub instead. Fork hygiene followed: fork main was synced to upstream main (a69c30a) and the merged PR branches were deleted on the fork. A new personal-only branch personal/side-screen (e8f7bfa — upstream main plus side-screen placement only, no hide env) was built, bundled with portable.txt, ad-hoc signed, and installed to /Applications/ytm-rs.app, where the app is now running with its data in the /Applications/data portable dir; the installed build is a fork of official 0.4.0 with side-screen challenge-window placement. Old variant branch personal/quiet-challenge is kept for history.

Next: when upstream moves past 0.4.0, rebase personal/side-screen onto upstream main and rebuild the installed app.

https://discord.com/channels/1486966847810043924/1575787700148113408

Branch personal/quiet-challenge gained f0a36d9 (challenge window placed on the first non-primary screen as soon as it is created, always-on-top only if it is left on the primary or only screen, and YTM_HIDE_CHALLENGE=1 in the environment to keep the window hidden) and 3ca0ce0 (created at the final 360x160 size and centered on the runtime-measured outer-inner frame delta instead of the content box). The work is open upstream as PR #442 "Lyrics: the verification window moves to a side screen" (head branch side-screen-challenge-window on fork Caffa/ytm-rs), stacked on the still-open PR #441. The installed app at /Applications/ytm-rs.app was swapped to this personal build with a portable.txt marker and an ad-hoc re-sign — settings live in /Applications/data because portable_dir() resolves there, not in ~/Library/Application Support — and the upstream app is backed up in personal-build/upstream-backup/.

Next: watch PRs #441 and #442; merge or rebase as upstream review lands.

https://discord.com/channels/1486966847810043924/1575787700148113408

### 2026-11-27 — Forked ytm-rs and fixed the lyrics-window focus steal (PR #441)

Cloned grapedjuice/ytm-rs into `~/Local-Projects-2026/Forks/ytm-rs` and fixed the bug where the Cloudflare Turnstile lyrics-verification child window stole macOS focus on every song change (tao's `set_focus()` becomes `activateIgnoringOtherApps`, and a failed verify-turnstile exchange bypassed the challenge_failed backoff so the window could respawn per song). The fix makes `show()` display and float the window without taking focus and stamps the 15-minute backoff after a failed exchange — two commits (ab6cb54, 482bb64) on branch `lyrics-verification-window-focus`, with upstream PR https://github.com/grapedjuice/ytm-rs/pull/441 open. Learnings: `request_user_attention` is a no-op for Accessory apps with no Dock icon, so there is no non-stealing "notify" path on macOS.

Next: watch PR #441 for upstream review; merge or rebase once a decision lands.
