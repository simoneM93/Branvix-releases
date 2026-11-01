# Branvix

[![Release](https://img.shields.io/github/v/release/simoneM93/Branvix-releases)](https://github.com/simoneM93/Branvix-releases/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-%E2%9D%A4-ea4aaa?logo=github-sponsors)](https://github.com/sponsors/simoneM93)
[![Changelog](https://img.shields.io/badge/Changelog-view-blue)](CHANGELOG.md)

A Git client for Windows with visual branch management, commit history and graphs, staging, diffs, stash, tags, and an integrated PowerShell terminal.

## Download

Current release: **1.2.0**. See the [release notes](https://github.com/simoneM93/Branvix-releases/releases/tag/v1.2.0) or [all releases](https://github.com/simoneM93/Branvix-releases/releases).

| Distribution | Download | Installation |
| --- | --- | --- |
| Installer | [Branvix-Setup-1.2.0.exe](https://github.com/simoneM93/Branvix-releases/releases/download/v1.2.0/Branvix-Setup-1.2.0.exe) | Run the setup wizard; includes shortcuts, uninstall support and integrated updates. |
| Portable | [Branvix-1.2.0-portable.exe](https://github.com/simoneM93/Branvix-releases/releases/download/v1.2.0/Branvix-1.2.0-portable.exe) | Run without installation. Extraction on each launch can make startup slower. |

## Requirements

- Windows with Git for Windows available to Branvix. The app checks the supported Git version at startup.
- If Git is missing, choose **Install Git** to start the interactive winget installation, or install an approved version through your IT team.
- Node.js is included with the app and does not need to be installed separately.
- Configure your usual Git authentication for remote operations, such as Git Credential Manager or SSH keys.

Branvix builds are currently unsigned. Windows may show a SmartScreen warning on first launch; verify the download source before proceeding.

## Getting started

1. Launch Branvix and choose **Open repository…**.
2. Use **Changes** to review diffs, stage files and create commits.
3. Manage branches, tags and stashes from the sidebar and context menus.
4. Use **Sync**, **Push**, or the integrated PowerShell terminal for remote work.

The interface supports Italian and English, light and dark themes, and saved workspace preferences. **Ctrl+K** opens the command palette; **Ctrl+Backquote** opens the terminal.

## Updates

Both distributions check for stable updates after startup and every four hours. You can also use **Settings → Updates → Check for updates**.

- The title-bar bell shows an unread dot. Opening its list marks the notification as read while keeping its action available.
- A dismissible banner appears once per version and closes after ten seconds, pausing while hovered or focused.
- The status-bar action remains available after closing the banner.

**Installer:** updates download in the background. Choose **Restart and update** when ready, or install the downloaded update when closing Branvix.

**Portable:** choose **Download portable** to open the official release page, download the new portable executable, close Branvix and replace the old executable manually.

Network errors do not interrupt Git work. Check Settings for errors and retry when connected.

## Changelog and license

See [CHANGELOG.md](CHANGELOG.md) for released changes and [LICENSE](LICENSE) for the MIT license.

This repository distributes application builds and user documentation. The source repository is maintained separately; GitHub's source archives here contain only this repository's own files.
