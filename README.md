# after

Terminal countdown timer: set a fixed duration or count down to a specific time of day, and get notified when complete.

[![Latest Release](https://img.shields.io/github/v/release/mtn-man/after)](https://github.com/mtn-man/after/releases/latest)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Go](https://img.shields.io/badge/go-1.23+-00ADD8?logo=go&logoColor=white)](https://go.dev/dl/)

<img src="assets/demo.gif" width="600" alt="after demo" />

## Why after?

Most terminal timers leave you wondering if they're still running. `after` shows a live countdown to a duration or time of day, plays an audio alert when done, and integrates cleanly with scripts and pipelines.

## Quick Start

Install and run in under a minute (requires [Homebrew](https://brew.sh)):

```bash
brew install mtn-man/tools/after
after 10m
```

If `after` is not found once installed, see [Troubleshooting](#troubleshooting).

## Installation

> **Note:** Windows is not currently supported. If you want it, let us know!

### Install With Go

Requires Go 1.23+ on macOS, Linux, or BSD. Get Go: https://go.dev/dl/

Install the latest version:
```bash
go install github.com/mtn-man/after@latest
```

Or install a specific release:
```bash
go install github.com/mtn-man/after@<version>
```

Or clone and build locally:
```bash
git clone https://github.com/mtn-man/after.git
cd after
go build -o after .
./after --version
```

### Install Prebuilt Release Binary

<details>
<summary>Binary installation steps (macOS and Linux)</summary>

1. Download your platform archive and `checksums.txt` from the
   [latest release](https://github.com/mtn-man/after/releases/latest).
   Replace `<version>` in the examples below with the release tag
   (for example, `vX.Y.Z`).
   Archive naming pattern:
   - `after_<version>_darwin_amd64.tar.gz`
   - `after_<version>_darwin_arm64.tar.gz`
   - `after_<version>_linux_amd64.tar.gz`
   - `after_<version>_linux_arm64.tar.gz`

   Note: release filenames use `darwin` to refer to macOS.
   The examples below use macOS Apple Silicon filenames; swap in the
   archive and binary names for your OS/architecture.
2. Verify checksum (optional but recommended):
   Example for macOS Apple Silicon:
   ```bash
   grep "after_<version>_darwin_arm64.tar.gz$" checksums.txt | shasum -a 256 -c -
   ```
   Example for Linux:
   ```bash
   grep "after_<version>_linux_amd64.tar.gz$" checksums.txt | sha256sum -c -
   ```
3. Extract your archive (example shown for macOS Apple Silicon):
   ```bash
   tar -xzf after_<version>_darwin_arm64.tar.gz
   ```
4. Install the extracted binary into `/usr/local/bin` (default):
   ```bash
   sudo install -m 0755 after_darwin_arm64 /usr/local/bin/after
   ```
   If you are on a different platform, replace the archive and binary
   filenames above with the matching release files for your
   OS/architecture.
5. Alternative (no `sudo`): install to `~/.local/bin`:
   ```bash
   mkdir -p ~/.local/bin
   install -m 0755 after_darwin_arm64 ~/.local/bin/after
   ```
   If `~/.local/bin` is not in your `PATH`, add it to your shell
   startup file (for example, `~/.zshrc` or `~/.bashrc`), then reload
   your shell:
   ```bash
   # zsh
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc

   # bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
   source ~/.bashrc
   ```
6. Verify:
   ```bash
   after --version
   ```

</details>

## Usage

```bash
after [options] <duration|time>
```

Durations are relative. Times refer to the next occurrence —
wrapping to tomorrow if already passed.

### Examples

```bash
# durations
after 30        # bare number = seconds
after 5m        # minutes
after 1h30m     # hours and minutes
after 1.5h      # decimal hours

# times of day
after 9am       # next 9:00 AM
after 9p        # next 9:00 PM
after 14:30     # 24-hour format
after 2:30 PM   # 12-hour with AM/PM
after noon      # 12:00 PM
after midnight  # 12:00 AM

# flags
after -q 5m                    # suppress alarm and status output
after -qs 5m                   # quiet but keep alarm
after -qt 5m                   # quiet and no title bar updates
after -f ~/sounds/bell.mp3 5m  # custom alert sound

# scripting
after 10m 2> /tmp/after.log   # capture lifecycle output
after -s 10m 2> /dev/null &   # background with alarm
```

Options may be placed before or after the time value. Short flags can
be combined: `-qt`, `-qs`, `-qts`. Run `after --help` for all flags.

## How It Works

Status output goes to `stderr`, leaving `stdout` clean for pipelines.
The countdown shows only significant fields (`1:23` for 83 seconds,
`1:02:03` for just over an hour).

When output is redirected (e.g. `2> /tmp/after.log`), the countdown is
suppressed and only lifecycle lines are emitted: `after: started (...)`,
`after: complete`, and `after: cancelled`. The alarm does not play in
this mode unless `--sound` is specified.

On macOS, `after` prevents the system from sleeping for its duration.
Use `--caffeinate` to force this when output is redirected.

## Troubleshooting

- `after` not found after install (`after: command not found`): Ensure
  your install location is in `PATH` (`/opt/homebrew/bin` or
  `/usr/local/bin` for Homebrew, `$(go env GOPATH)/bin` or `GOBIN` for
  `go install`, `/usr/local/bin` or `~/.local/bin` for manual install),
  then restart or reload your shell.
- `Permission denied` while installing to `/usr/local/bin`: Use
  `sudo install ...` or install to `~/.local/bin` instead.
- Homebrew command ambiguity with an existing `after` formula: use
  `brew install mtn-man/tools/after` and
  `brew info mtn-man/tools/after`.

## Contributing

Bug reports, suggestions, and pull requests are welcome. Open an issue to start a conversation.

## License

MIT License. See [LICENSE](LICENSE) file for details.
