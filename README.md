# Maxxwell downloads

Latest release: [v0.1.223](https://github.com/rindler-ai/ide-releases/releases/tag/v0.1.223)

## Release identity

- Version: v0.1.223
- Build: global-519
- Source commit: 0db0e4ccdafe0564024dcd8f9b2ec5e129d2edc4
- Staged: November 6, 2026 at 11:39:45 AM PST

| Platform | Download | Install |
| --- | --- | --- |
| macOS arm64 | [`Maxxwell-darwin-arm64.dmg`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/Maxxwell-darwin-arm64.dmg) | Open it and drag Maxxwell to Applications. |
| Linux x64 | [`maxxwell_0.1.223-global-519_amd64.deb`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell_0.1.223-global-519_amd64.deb) | Double-click it to install. Recommended on Ubuntu 23.10 and later: the .deb installs the Chromium sandbox helper. |
Advanced: [Linux tarball (`Maxxwell-linux-x64.tar.gz`)](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/Maxxwell-linux-x64.tar.gz). On Ubuntu 23.10 and later, it needs a Chromium sandbox helper; the .deb installs it for you.
| Windows x64 (unsigned) | [`Maxxwell-windows-x64.zip`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/Maxxwell-windows-x64.zip) | Right-click the ZIP, choose Extract All, then open Maxxwell. SmartScreen will warn. |
| Phone (Android) | [`Maxxwell-Android.apk`](https://github.com/rindler-ai/maxxwell-releases/releases/download/v0.1.223/Maxxwell-Android.apk) | Download it on your phone, open the APK, and allow installation when Android asks. Release build signed with the stable Maxxwell Android key, so a newer APK installs over this one. Attached after the desktop build is published. |

[SHA256SUMS.txt](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/SHA256SUMS.txt) verifies every published artifact.

## The `maxxwell` CLI on its own

A coding agent that runs in your terminal, for a headless or SSH box with no desktop app.
The desktop app above already bundles it.

Give it a provider API key and it works on its own. Or point it at an agent CLI
you already have installed and signed in — `claude`, `codex`, `cursor-agent` — and it
drives that instead, without ever holding the credential.

```
maxxwell                               open an interactive session (signed-in agent CLI works without a provider key)
maxxwell run "fix the failing test"    run one task and exit (uses a configured provider key)
maxxwell help                          every command
```

| Platform | Download | Install |
| --- | --- | --- |
| macOS arm64 | [`maxxwell-darwin-cli-arm64-global-519.tar.gz`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-darwin-cli-arm64-global-519.tar.gz) | `tar -xzf maxxwell-darwin-cli-arm64-global-519.tar.gz` |
| macOS x64 | [`maxxwell-darwin-cli-x64-global-519.tar.gz`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-darwin-cli-x64-global-519.tar.gz) | `tar -xzf maxxwell-darwin-cli-x64-global-519.tar.gz` |
| Linux x64 | [`maxxwell-linux-cli-x64-global-519.tar.gz`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-linux-cli-x64-global-519.tar.gz) | `tar -xzf maxxwell-linux-cli-x64-global-519.tar.gz` |
| Linux arm64 | [`maxxwell-linux-cli-arm64-global-519.tar.gz`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-linux-cli-arm64-global-519.tar.gz) | `tar -xzf maxxwell-linux-cli-arm64-global-519.tar.gz` |
| Windows x64 (unsigned) | [`maxxwell-windows-cli-x64-global-519.zip`](https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-windows-cli-x64-global-519.zip) | `Expand-Archive .\maxxwell-windows-cli-x64-global-519.zip` (SmartScreen will warn) |

Each archive contains a single `maxxwell` executable. Put it on your `PATH`.
Sessions and the fleet commands drive `tmux`, so install that too on Linux and macOS.
The same SHA256SUMS.txt covers these archives (`sha256sum -c SHA256SUMS.txt --ignore-missing`),
and `maxxwell self-update` checks it before installing anything.

If an existing Linux arm64 CLI says no build is published, install the Linux arm64 archive above once; that older binary cannot self-update to this fix. The current build can update normally.

It needs one of these before it can do anything, and tells you so if it has neither:

- **a provider API key** in your environment — `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`,
  `OPENROUTER_API_KEY`, `DEEPSEEK_API_KEY`, `ZAI_API_KEY` or `DASHSCOPE_API_KEY`; or
- **an agent CLI installed and already signed in** — `claude`, `codex` or `cursor-agent`.

Full provider API-key list: [internal/byokruntime/providers.go](https://github.com/rindler-ai/maxxwell-ide/blob/main/internal/byokruntime/providers.go) (`ProviderConfigurations`).

## If `maxxwell self-update` fails with HTTP 404

The standalone `maxxwell` CLI at v0.1.61 or earlier can't update itself. Its `self-update`
downloads from a repository that is no longer public, so it stops with
`read the release checksums: GET https://github.com/rindler-ai/maxxwell-cli/...: HTTP 404`
and leaves the old version in place.

**Check what `maxxwell` on your `PATH` actually is before running anything below:**

```sh
command ls -l "$(command -v maxxwell)"
```

(`command` keeps an `ls` alias out of the way.)

- **A link (`->`) into the desktop app** — `Maxxwell.app` on macOS, `/opt/maxxwell` or
  wherever the app was unpacked on Linux. **Update the app instead of running anything
  below.** On macOS or a Linux tarball install the app updates itself; on Linux `.deb`
  installs, update through the system software installer.
- **A plain file (no `->`)** is a standalone CLI that the app will not replace, even if the
  app is also installed: it never overwrites a real file with a link. Reinstall it with the
  commands below.
- **A link to anything else:** run `command ls -l` on the path after the `->` (a relative
  path there is relative to the link's own directory), repeat until it is no longer a link,
  and apply the two rules above to where it ends.
- **`No such file` naming `maxxwell` or an alias:** a shell alias or function called
  `maxxwell` is in the way. `type -a maxxwell` lists the file it really runs; check that one,
  and use its path in place of `"$(command -v maxxwell)"` below.
- **`No such file` for an empty name** (`ls: : No such file…` or `cannot access ''`):
  nothing called `maxxwell` is on your `PATH`, so this does not apply to you.

To reinstall, run the block for your platform:

macOS arm64:

```sh
cd "$(mktemp -d)"
curl -fLO https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-darwin-cli-arm64-global-519.tar.gz
tar -xzf maxxwell-darwin-cli-arm64-global-519.tar.gz
install -m 0755 maxxwell "$(command -v maxxwell)"
```

macOS x64:

```sh
cd "$(mktemp -d)"
curl -fLO https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-darwin-cli-x64-global-519.tar.gz
tar -xzf maxxwell-darwin-cli-x64-global-519.tar.gz
install -m 0755 maxxwell "$(command -v maxxwell)"
```

Linux x64:

```sh
cd "$(mktemp -d)"
curl -fLO https://github.com/rindler-ai/ide-releases/releases/download/v0.1.223/maxxwell-linux-cli-x64-global-519.tar.gz
tar -xzf maxxwell-linux-cli-x64-global-519.tar.gz
install -m 0755 maxxwell "$(command -v maxxwell)"
```

If the `install` line fails because that location is not writable, nothing has changed;
re-run just that line with `sudo`.

From v0.1.62 on, `maxxwell self-update` downloads from this repository.

## Platform build status

| Platform | Status | Evidence |
| --- | --- | --- |
| macOS | BUILT | arm64 packaged, signed, notarized and stapled on a real Mac, and the .dmg verified against Gatekeeper; target Mach-O modules validated; no walk of this build was recorded when it was staged; last recorded walk v0.1.42 (2026-09-05): packaged GUI launch and the native workspace-folder picker, end to end, by the founder |
| Linux | BUILT | self-hosted native .tar.gz, .deb, and standalone x64 CLI tarball validated; no walk of this build was recorded when it was staged; last recorded walk v0.1.137 (2026-09-30): update in-app from v0.1.123 with the workspace trust kept and the first send answered |
| macOS Intel (x64) | CLI ONLY | no Intel app is published: Intel Macs are not a supported app platform (Apple Silicon is). The standalone x64 CLI tarball is cross-built by the Linux job, unsigned, and carries the `maxxwell` executable alone with NO bundled tmux, so the host supplies tmux |
| Linux arm64 | CLI ONLY | the standalone arm64 CLI tarball is cross-built by the Linux job (pure Go) and included in the signed release manifest; there is no arm64 app or .deb, because the Electron bundle and node-pty need host-native packaging. Like the x64 CLI tarball it carries the `maxxwell` executable alone and NO bundled tmux -- the tmux pin manifest declares one asset, darwin-arm64 -- so the host supplies tmux, which is what the .deb's `Depends: tmux` already assumes |
| Windows | BUILT | Linux-cross-built unsigned x64 app/CLI ZIPs; target PE modules validated; SmartScreen will warn; no walk of this build was recorded when it was staged; last recorded walk v0.1.137 (2026-09-30): first run to a working seat and two sends (J1) 3/3; update in-app and by ZIP from v0.1.134, v0.1.135 and v0.1.136 6/6; the seat resumed after its tmux server was killed (J3) |
| Android phone | NOT IN THIS RUN | the R8 release build passed its emulator save smoke here; the APK is built and signed locally by scripts/release/publish-android.sh and attached to this release as Maxxwell-Android.apk, never by CI |

No declared platform artifacts are pending.

Release bar for this build's candidate (its release-bar/<platform> commit statuses on 0db0e4ccdafe0564024dcd8f9b2ec5e129d2edc4, read before publish):
PASS = walked and passed; FAIL = walked and failed; NOT MEASURED = no walk result for this release run.

- linux NOT MEASURED -- not measured on linux (no release-bar/linux status on 0db0e4ccdafe0564024dcd8f9b2ec5e129d2edc4)
- windows NOT MEASURED -- not measured on windows (no release-bar/windows status on 0db0e4ccdafe0564024dcd8f9b2ec5e129d2edc4)
- darwin NOT MEASURED (no release-bar/darwin status) -- not measured on darwin (no release-bar/darwin status on 0db0e4ccdafe0564024dcd8f9b2ec5e129d2edc4); no step waits on the Mac
