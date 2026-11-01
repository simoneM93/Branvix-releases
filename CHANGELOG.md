# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and version numbers follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.2.0] - 2026-11-01

### Added

- Update notifications for installed and portable Windows builds: title-bar bell with unread dot, a dismissible ten-second banner shown once per version, and a persistent status-bar action. Read/banner receipts survive restarts; the banner pauses on hover or focus. Settings shows the current version and a manual update check in Italian and English.
- Portable builds check stable GitHub Releases after initialization and every four hours, offering the official release page for manual portable replacement. Installed builds retain background download and restart/install behavior, with availability shown before the download completes.
- Offline update-service, notification persistence and preload tests, real Electron notification smoke, and an isolated real NSIS download/checksum verification script.

- Early desktop loading screen with the saved theme, Italian/English messages, reduced-motion support and a retry action for initialization failures. Portable builds also show a native splash during extraction.

- Offer **Install Git** at startup when Git is missing. An explicit click starts the latest `Git.Git` version available from winget using the interactive installer and normal Windows/UAC prompts. Detect standard install locations afterward, refresh the app's process PATH/cache, and report unavailable winget, cancelled/failed setup or a required restart in Italian and English. Existing Git installations are not automatically upgraded.

- Single-commit cherry-pick from the History and Graph context menus, with confirmation of the selected commit and destination branch. Merge commits requiring a mainline parent are not supported.
- Standard rebase of the current branch onto a selected local or remote branch, with confirmation of the direction and a warning about rewriting published commits. Interactive rebase is not supported.
- Continue, Skip commit, and Abort operation controls for cherry-pick and rebase, including conflict resolution and skipping empty cherry-picks. Conflicts are resolved in an external editor and marked as resolved before continuing.
- Collapsible sidebar with a saved preference and vertical Local, Remote, All, and Tags icons. Hovering over an icon opens its filter; leaving the sidebar closes the temporary panel.
- Sidebar icon navigation through focus, click, Up/Down arrows, and Home/End, plus a control to keep the sidebar permanently expanded.
- Git and route tests covering successful operations, conflicts, continuation, skipping, aborting, empty commits, invalid input, linked worktrees, and protection of local changes.
- IPC transport tests requiring the Electron preload bridge and verifying request forwarding and coded errors.
- Electron smoke coverage for Git menus, conflict resolution, rebase abort, sidebar hover, keyboard navigation, menus/dialogs, saved widths, and stable central content positioning; automatic screenshots for visual review.

### Changed

- Show the desktop window before full UI initialization, load the native terminal on first use, and defer update checks for installed and portable builds until the workspace is ready.
- Publish Windows executables and updater metadata to the separate public `Branvix-releases` repository while keeping the source repository private. The release workflow uses a scoped `RELEASES_TOKEN` secret; no credentials are included in the app.

- Branvix is now a Windows desktop-only application distributed as an installer or portable executable. The Electron preload bridge is required for application operations.
- Packaging includes an explicit list of application modules; development requires Node.js 22.12 or later.
- Repository status now detects merge, cherry-pick, and rebase operations from Git metadata, including in linked worktrees and after refreshing or reopening a repository.
- Cherry-pick and rebase require an attached branch with commits and a clean working tree. Neither operation uses autostash, and rebase does not implicitly update other branches even when `rebase.updateRefs` is enabled. No force push runs automatically.
- Incompatible operations are blocked while a Git sequence is paused, including branch switching, ordinary commits, amend, stash, pull, and push. Merges retain their existing completion flow through a commit.
- Expanded sidebar filters now use a single row with compact icons, labels, and counters. Temporary panels show the selected filter title without duplicate filters or expansion controls.
- Temporary sidebar expansion overlays the central content without moving it or losing the saved width; focus, context menus, and related dialogs keep the panel open.
- Updated Italian and English interface text, error messages, feature documentation, and limitations.
- Extended translation and packaging checks to cover the new interface text and smoke modules.

### Fixed

- Update the Windows installer and portable file description to "Branvix - Git Client for Windows".

- `npm test` now uses Node's native test discovery instead of a quoted glob that Node 20.11 does not expand.

### Removed

- Local web server, browser HTTP fallback, web startup command, and HTTP integration test. The folder picker remains available through desktop IPC.

### Security

- Packaged Windows builds disable Node-options environment handling and Node inspector flags during normal application startup. RunAsNode remains available for node-pty's Windows helper and retains its own execution/debugging capabilities.
- Enable embedded ASAR integrity validation and archive-only application loading. Tampered archive headers/content and loose application replacements are rejected; unpacked native files and publisher authentication remain outside this protection.
- Add executable-level security checks with a calibrated preload probe, packaged terminal/UI smoke tests, and tampered-copy rejection tests. Code signing remains deferred.
- Check Git for Windows against the deployment baseline reviewed on 2026-10-31 (`2.56.0.windows.4`), caching the result until an explicit installation refresh. Missing Git offers opt-in winget setup; older or unrecognized installations retain translated IT guidance. The installer IPC accepts no renderer command/package arguments and requires the trusted main frame. Existing installations are not automatically upgraded; version checks do not authenticate Git binaries.

---

## [1.1.0] - 2026-10-31

### Added

- Integrated PowerShell terminal in the desktop app, opened through its toolbar button, Ctrl+Backquote, or search. Sessions start at the repository root, remain active when hidden, and can be terminated after confirmation.
- Terminal resizing through a splitter and keyboard, with saved height; copy/paste, command interruption, and Git view updates after shell changes.
- Protected repository switching with an active terminal: confirmation before closing the session, while cancellation, selecting the same repository, or an invalid path preserves it.
- Lightweight and annotated tag creation from commits, a Tags sidebar filter, navigation to associated commits, tag push, and local deletion. Existing tags are never overwritten.
- Local branch renaming, including the current branch, preserving tracking configuration and preventing another branch from being overwritten. Remote branch names remain unchanged.
- Right-click context menus for branches, stashes, changed files, commits, and recent repositories, also available through the Menu key or Shift+F10. Actions include copying hashes, messages, names, or paths and creating branches/tags from commits.
- Settings for theme and language, with Italian, English, or system-language selection and immediate text updates.
- Sync action: fetch all remotes with pruning, then pull using fast-forward only when the current branch has an upstream. Fetch and Pull remain available separately.
- Commit and push from the Commit button menu or Ctrl+Shift+Enter. A failed push preserves the commit and reports this explicitly.
- Open the project folder in File Explorer from the title bar, repository menu, or command palette.
- Remove repositories from the recent list without changing their folders on disk.
- Automated WCAG AA contrast checks and tests for translations, branch names, Sync, and Commit and push.
- PTY service and real-terminal smoke tests covering IPC validation, session isolation, input/output, resizing, shell and child-process cleanup, reload, crash, and app shutdown.
- Terminal UI smoke tests for keyboard input, copy/paste, heavy output, the Git pager, command interruption, themes, languages, and protected repository switching.
- `CONTEXT.md` documenting project architecture, constraints, and known pitfalls.

### Changed

- Introduced the Soft Light theme with muted blue-gray surfaces and centralized colors.
- Destructive-action confirmations now match Branvix's theme and language and initially focus Cancel to prevent accidental confirmation with Enter.
- Branvix's folder picker is available in the desktop app alongside the native Windows dialog.
- New branch names are cleaned while typing: spaces become `_`, invalid characters are removed, and a preview appears when final normalization changes the name.
- The title bar shows the full repository path in the selector tooltip, keeping the toolbar compact.
- SSH and network errors provide translated guidance for unavailable keys, unknown hosts, and timeouts.
- Improved spacing, shortcut key styling, and notification icons.
- Desktop development now requires Node.js 22.12 or later.

### Fixed

- Aligned author, date, and hash columns in Graph; narrow panels show the author's avatar only.
- Corrected cursor behavior and explanations for disabled checkboxes.
- Prevented accidental menu reopening after a second click on the menu's button triggers automatic popover dismissal.
- Included the local xterm assets and licenses required by the terminal.
- Hardened Windows terminal IPC smoke tests, comparison of actual repository state after cancelling a repository switch, and handling of empty clipboard items in tests.

---

## [1.0.0] - 2026-10-30

### Added

- Initial release with a Windows Electron app and local web UI sharing the same interface and Git operations; NSIS installer and portable Windows build.
- Repository folder picker, recent repository list, and automatic reopening of the last used repository.
- Local and remote branch management: search, Local/Remote/All filters, favorites, collapsible folders for names containing `/`, ahead/behind indicators, creation, checkout, and deletion with protection for unmerged branches.
- Remote branch checkout with creation of a corresponding local tracking branch.
- Merge into the current branch with conflict listings, marking files as resolved, and merge abort.
- Staging checkboxes, partial-stage indicators, stage/unstage, and commit messages with a title and description.
- Amend the last commit, disabled when there are no commits, during a merge, or when the last commit is already published to the upstream.
- Discard file changes after confirmation, restoring the HEAD version or deleting newly added files.
- Stash with an optional description and untracked files; apply, pop, drop, and diff viewing. Conflicting pops preserve the stash.
- History and graph across all branches, grouped by day, with continuous lanes, locally generated avatars, refs, author, date, and hash; search loaded commits and load additional pages of 300 commits.
- Commit details and unified/side-by-side file diffs with added/removed line counts; merge commits are compared against their first parent.
- Details panel shown only after selecting a commit, file, or stash, closed with × or Esc; resizable panels with saved widths.
- Fetch, fast-forward-only pull, and push with upstream setup on the first push and guidance for common SSH errors.
- Open repositories and files in VS Code, or reveal them in File Explorer when VS Code is unavailable.
- Ctrl+K command palette for actions, branches, changed files, commits, and repository files; commit search extends beyond loaded history.
- Automatic, light, and dark themes; shortcuts for refresh, tabs, commits, and search.
- Automatic view refresh when returning to the window and notifications for operation results and errors.
- Automatic updates from GitHub Releases for the installed app, with notification and restart to install. Portable builds require manual updates.
- GitHub Actions workflows for tests and Electron smoke checks on Windows, builds and artifact publication on version tags, and `latest.yml`/`.blockmap` update metadata.
- Tests using temporary repositories and a local bare remote, plus checks that imported app modules are included in the package.
- MIT license and documentation for installation, development, Git authentication, and SSH configuration.

### Fixed

- Corrected Windows/CI path comparisons to handle abbreviated 8.3 names.

### Security

- Isolated Electron renderer with sandbox and context isolation, internal `app://local` protocol, and limited IPC APIs; no HTTP port in the desktop app.
- Web server bound to `127.0.0.1`, with Host/Origin checks, JSON request protection, and CSP.
- Git execution without a shell; validated repositories, branch names, and paths; disabled Git prompts and bounded network operations.
- File Explorer fallback reveals files instead of executing them through their default application.

---

[1.2.0]: https://github.com/simoneM93/Branvix-releases/releases/tag/v1.2.0
[1.1.0]: https://github.com/simoneM93/Branvix-releases/releases/tag/v1.1.0
[1.0.0]: https://github.com/simoneM93/Branvix-releases/releases/tag/v1.0.0
