# Branvix

[![Release](https://img.shields.io/github/v/release/simoneM93/Branvix-releases)](https://github.com/simoneM93/Branvix-releases/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-%E2%9D%A4-ea4aaa?logo=github-sponsors)](https://github.com/sponsors/simoneM93)
[![Changelog](https://img.shields.io/badge/Changelog-view-blue)](CHANGELOG.md)

A Git client for Windows with visual branch management, commit history and graphs, staging, diffs, stash, tags, and an integrated PowerShell terminal.

## Download

Current release: **1.5.1**. See the [release notes](https://github.com/simoneM93/Branvix-releases/releases/tag/v1.5.1) or [all releases](https://github.com/simoneM93/Branvix-releases/releases).

Includes HTTPS/Bitbucket Data Center authentication fixes, group creation from the welcome screen, clickable group filters, side-by-side Groups/Repositories panes and color-coded local upstream states. Line/hunk staging and an editable Base/Ours/Theirs conflict view remain included; saving, staging and continuing an operation stay explicit, separate actions.

| Distribution | Download | Installation |
| --- | --- | --- |
| Installer | [Branvix-Setup-1.5.1.exe](https://github.com/simoneM93/Branvix-releases/releases/download/v1.5.1/Branvix-Setup-1.5.1.exe) | Run the setup wizard; includes shortcuts, uninstall support and integrated updates. |
| Portable | [Branvix-1.5.1-portable.exe](https://github.com/simoneM93/Branvix-releases/releases/download/v1.5.1/Branvix-1.5.1-portable.exe) | Run without installation. Extraction on each launch can make startup slower. |

## Repository overview and folder groups

Open **Repositories** from the folder button in the top bar or the repository selector. The overview combines opened/cloned repositories and configured folder groups; adding or removing a group never changes folders on disk. Expand a group in the selector to open a nested repository. One repository remains active, and switching keeps the terminal confirmation.

Use **Add group**, available on the welcome screen too, to select an ordinary folder and immediately discover its repositories without opening one. The folder itself need not be a Git repository. **Groups** and **Repositories** occupy side-by-side panes with independent scrolling. Click a group (or focus it and press Enter/Space) to filter its repositories; the Group dropdown stays synchronized, and **All** / **Registered** restore the broader lists.

Use **Scan repositories** to repeat discovery, including linked worktrees. Discovery does not follow symbolic links/junctions or enter `.git`. A scan is limited to 8 levels, 2,000 directories, 20,000 entries, 200 repositories and a cooperative 5-second budget; filesystem calls may take longer. A partial result is labeled: retry with a more specific folder. Missing folders/repositories remain recoverable through refresh.

**Local upstream** uses text and counts alongside color: green when aligned, amber for pending pull/push, red when diverged, and neutral without an upstream. These states compare local upstream references, so green does not imply the server has no newer commits; use **Refresh remote** to fetch first.

Search by name/path/branch, filter changed or unavailable repositories, and sort by name, latest commit time or last opened. The list shows 100 repositories per page to keep rendering bounded. Migrated recent entries have no historical opening timestamp. Local snapshots refresh every 15 seconds while the overview is visible, with at most four concurrent snapshots, 5-second Git timeouts and a 10-second cache. Ahead/behind describes local upstream references; **Refresh remote** explicitly fetches for the selected repository. There is no periodic network access.

**Pull all** takes the currently scanned group as its target list and runs one `git pull --ff-only` at a time through the existing authentication flow. Dirty repositories are attempted; Git refuses incompatible changes or divergent history. Repositories without an upstream, with detached HEAD or an operation in progress are skipped with a reason. Errors do not stop the rest; **Cancel** stops the current owned process and remaining queue. Results are reported per repository. There is no automatic stash, reset, merge or push. A partial scan only pulls the repositories it found; scan a narrower folder to include omitted repositories.

### Visual Git Blame

Choose **View Blame** in a changed file or commit file's diff. The view attributes the full file snapshot: **Working tree content**, **Staged content**, or **Commit content**. Staged and working changes are kept separate; uncommitted lines have no commit hash, copy action or graph target. Untracked files show an explanation; staging a new file makes its uncommitted lines viewable. Git rename history is followed when identifiable.

Select a row's attribution to inspect author, date, full hash and commit subject; the same metadata is available in its keyboard-accessible label and tooltip. **Copy full hash** and **Open in graph** use that selection. When the commit is outside the loaded history page, Branvix opens a focused ancestry graph of up to 300 commits; **Full history** restores the normal view. **Back to diff** preserves the original file/commit selection.

Blame is read-only and local. It supports regular UTF-8 text files up to 2 MiB and 200,000 lines; binary files, symlinks, paths outside the repository and conflicted index entries are rejected. The viewport loads small ranges (normally 120 rows, at most 400 for very tall windows), with cancellable 10-second Git commands and a content-keyed cache bounded to 8 MiB/8 ranges for 30 seconds. **Cancel** stops the current request; **Refresh** rechecks the snapshot. Repository refreshes also reload an open blame view, and late results cannot replace a newer selection.

### Repository files and read-only preview

Select **Files** in the sidebar, or its icon on the collapsed rail, to browse the active repository. Expand folders on demand; Branches, tags, stash and Changes keep their existing flows. The tree includes tracked/untracked files, dotfiles and empty directories; `.git` metadata and Git-ignored entries are hidden. Force-tracked files remain visible under ignored folders. Nested repositories/submodules use their own Git ignore rules while filesystem access stays inside the active root. Links are shown but cannot be expanded or previewed.

Folder badges summarize modified, untracked or conflicted descendants. Use arrow keys to move/expand/collapse, Home/End to reach the first/last visible row, and Enter/Space to open a file or load more entries. The Branches/Files tabs support Left/Right. **Refresh** reloads expanded folders; repository refreshes and returning to the app pick up external file changes. A repository switch cancels obsolete work and resets tree memory.

Selecting a file opens a **Read-only preview** inside Branvix. Text is never interpreted as HTML or executed. **Back to diff** restores the preceding diff/blame selection (or Close when there was none); IDE and File Explorer actions remain explicit alternatives. The preview supports regular UTF-8 text up to 1 MiB and 20,000 lines; binary/invalid text, oversized files, ignored paths, metadata and external links receive clear messages. Refresh rechecks the file and Cancel stops the owned request. Browsing/preview never stages or writes repository files.

Lists load 200 entries per page, with a 2,000-entry scan cap per directory and a cooperative 5-second filesystem budget. Git commands have a 5-second timeout and 8 MiB output bound; status is briefly cached per Git root (2 seconds). Partial lists and unavailable Git status are labeled. The current tree retains at most 50 expansions/4,000 cached entries and renders at most 2,000 rows; collapse folders to free capacity. The preview renders a small window (normally 120 rows, at most 400 on tall screens). Filesystem calls already in progress can exceed the cooperative budget.

### Stage individual lines or hunks

Open a changed file's diff and choose **Select lines**. Check individual added/deleted lines or the checkbox on a hunk header, then **Stage selection**. On the staged side, **Unstage selection** reverses only the checked changes. Additions and deletions are independent: select both sides of a replacement, or its whole hunk, to move the complete replacement. If an isolated EOF change would alter an unselected line boundary, select the complete replacement/hunk. Native checkboxes support keyboard focus and Space; unified and side-by-side views share the selection.

Only the index changes; working files and other staged changes stay intact. **Clear selection** clears the checkboxes; **Finish selection** restores the whole-file actions. **Refresh** reads a new snapshot. An index, HEAD or file change invalidates the old snapshot and requires selecting again. Failures never reset the entire index. App mutations are serialized per canonical working tree; a real Git index lock protects the temporary-index transaction. Finish an external Git operation before retrying a locked index.

Regular UTF-8 text, CRLF/LF, missing final newlines, new/unborn files and deletions are supported within limits: 1 MiB per working file/diff, 5,000 diff rows, 32 MiB index, and 5 seconds per Git command. Empty files without textual hunks, binary/invalid text, symlinks/submodules, mode changes, configured clean filters/working-tree encodings and staged rename reversal require the whole-file action or an editor. Worktree content edits after a staged rename can still be selected. Truncated display diffs cannot be applied partially. Repository text stays inert, and historical/stash/preview views do not expose partial-write controls.

### Visual merge conflict editor

Open a conflicted file in **Changes** and choose **Resolve conflict**. Base, Ours and Theirs come from actual index stages 1/2/3; missing versions are labeled. Provenance shows the current checkout and incoming commit. During rebase, Ours is the rebased checkout and Theirs is the commit being replayed. Source tabs show 500 lines per page with changes against Base highlighted; oversized comparisons are labeled without preventing manual editing.

Use **Previous/Next** to select a conflict and **Accept Ours/Theirs/both**, or edit the result directly. **Use entire version in result** also supports binary content and mixed line endings by preserving the chosen raw bytes. Choosing a missing Ours/Theirs explicitly deletes the file when saved, with confirmation; an absent Base cannot be chosen. Rename conflicts are handled per remaining index path. Text and script-like content stay inert.

**Save result** (Ctrl+S) changes only the working file. **Mark resolved** stages the saved result only after conflict markers are removed. Continue merge/rebase/cherry-pick separately through the existing operation controls. Other staged files remain intact. Leaving a dirty editor, refreshing it, reloading, quitting or installing an update requires discarding confirmation. External file/index/reference changes reject stale writes and retain the draft; copy needed edits before explicitly refreshing and discarding it.

Manual editing supports regular UTF-8 text with optional BOM and LF or CRLF, preserving the working file's format. Limits are 1 MiB per version/result, 20,000 lines for text editing/display, 32 MiB index and 5 seconds per Git command; filesystem calls can take longer. Source highlighting has bounded Git output and a 5,000-row cap; configured conflict-marker widths 1–64 are supported. Binary/invalid text, text exceeding the line cap and mixed-EOL results require a whole-version choice. Symlinks/junction paths, submodules, differing/nonregular modes, clean filters and working-tree encodings are unsupported; use an external editor/Git for those cases.

## Requirements

- Windows with Git for Windows available to Branvix. The app checks the supported Git version at startup.
- If Git is missing, choose **Install Git** to open the guided download and installation flow, or select an approved existing git.exe through Settings.
- Node.js is included with the app and does not need to be installed separately.
- Configure your usual Git authentication for remote operations, such as Git Credential Manager or SSH keys.

Branvix builds are currently unsigned. Windows may show a SmartScreen warning on first launch; verify the download source before proceeding.

## Git setup

**Settings → Git for Windows** shows Git's version and executable path. **Select git.exe…** saves a validated existing executable for Branvix and new terminal sessions without changing the Windows PATH. If the saved executable is moved, Branvix tries an available installation and lets you choose again.

**Set up Git…** downloads the stable installer from the official Git for Windows release with percentage and downloaded MB. Before installation, Branvix verifies the published SHA-256 and Windows signature against an approved certificate. Choose **Install Git** only after the package is verified. Downloads can be cancelled and retried; closing the dialog keeps the job available in Settings. Unverified direct-download packages are not run.

The Git installer has its own window and may request Windows administrator approval. Keep Branvix open and cancel from the installer's window if needed. After installation Branvix checks Git again; use **Select git.exe…** for a custom location if necessary. **Install with winget** is an explicit fallback using the package manager's own verification. Git already available is not upgraded automatically. These controls also work from Branvix portable, but installing Git still installs it on the PC.

## Getting started

1. Launch Branvix and choose **Add group** to discover repositories inside an ordinary folder, or **Open repository…** to open one directly.
2. Use **Changes** to review diffs, stage files and create commits.
3. Manage branches, tags and stashes from the sidebar and context menus.
4. Use **Sync**, **Push**, or the integrated PowerShell terminal for remote work.

Double-click a remote branch to switch to its existing local branch when available; otherwise Branvix creates a local tracking branch. Checkout does not pull or reset local commits.

## Clone a repository

Choose **Clone repository…** on the welcome screen, in the repository menu or in the command palette. Paste an HTTPS or SSH address (including `git@host:group/repo.git` and `ssh://`). Branvix suggests the local folder name. The HTTPS/SSH switch converts recognized GitHub, GitLab and Bitbucket addresses; enter an alternate URL manually for custom hosts or ports.

Use Branvix's custom folder picker to select or create a parent folder. It accepts folders without a repository, remembers the last parent and shows the final destination before cloning. An existing destination is never overwritten. Advanced options include branch, shallow history, single branch and submodule initialization.

The dialog shows Git's available progress and lets you cancel. Public repositories work without credentials; private HTTPS/SSH repositories use your configured Git Credential Manager or OpenSSH setup. After cloning, the repository appears in recents; open it in Branvix or your preferred IDE. A failed or cancelled clone may leave partial files at the displayed destination when safe cleanup is uncertain. Inspect or remove that folder before retrying. Installer and portable builds use the same flow.

The interface supports Italian and English, light and dark themes, and saved workspace preferences. **Ctrl+K** opens the command palette; **Ctrl+Backquote** opens the terminal.

Each workspace column scrolls independently, keeping its headers and controls visible. The detail panel's close button stays available while you scroll a diff. Changes keeps the commit controls reachable with the terminal open; the description editor adapts to the available height.

Files inside new folders appear individually in Changes with their relative paths. You can inspect, stage, unstage or discard one file without affecting its siblings. Ignored files remain hidden, and nested Git repositories are not expanded into the parent repository's file list.

## Editors and IDEs

Use the toolbar's **Open with…**, a file/recent-repository context menu, or **Settings → Editors and IDEs → Configure IDEs…**. Branvix detects Visual Studio, VS Code, Rider, WebStorm, IntelliJ IDEA and PyCharm in standard Windows locations, modern Toolbox locations, registered App Paths and executable launchers. Custom locations can be selected manually.

Automatic choice follows this order: repository preference, compatible detected IDE, global default, manual choice. Branvix recognizes solution/project and manifest filenames without running project code. Detection is limited to two subdirectory levels, 64 folders and 2000 entries; hidden/dependency/build folders and links are skipped. For multiple projects, choose a detected solution/project from the selector. IDE discovery is cached for 30 seconds.

Choose an IDE and **Save preference** for the repository or global scope. Choose **Automatic** and save to remove that preference. Missing IDEs trigger an alternative choice; remove and re-add an unavailable custom IDE to update its path. **Add custom IDE** accepts a trusted existing `.exe` and `{target}`, `{repo}`, or `{repo} {target}` argument templates. These refer to the file/folder and repository root and are passed separately, without shell commands. Preferences stay in Branvix's application profile and do not change Windows file associations or PATH.

Repository opens use the appropriate solution/project or folder, while file opens target the selected file. VS Code remains supported. If no IDE is available, choose one manually or use the toolbar menu's File Explorer action to reveal the target. This does not run repository executables or scripts through file associations. Installer and portable use the same IDE workflow.

## SSH authentication

**Settings → SSH authentication → Configure SSH…** shows the OpenSSH client, agent status and public fingerprints for existing Ed25519, RSA and ECDSA keys. A readable paired `.pub` file is required; **Select existing key…** supports other locations. Save a repository identity or a global fallback, or select **Existing configuration and agent** and save to remove the preference. Branvix stores path references only, never private key contents or passphrases. The selected identity is added to configured identities; existing agents stay active. Inventory is bounded to 200 entries, with up to 20 manually selected paths.

Managed OpenSSH requests encrypted-key passphrases in a protected dialog with show/hide, retry and cancel. The passphrase is transient and is not placed in command arguments, settings, logs or Credential Manager. Cancel stops Git and its child processes; reload, crash, quit and update restart also clean up authentication. The operation timeout is 120 seconds, including prompt time. Installer and portable use the same flow.

OpenSSH continues to handle config, aliases, Include, Match, ports and proxies. Branvix does not edit SSH configuration or start agents. The selected Git's client is preferred, then Windows OpenSSH; a plain absolute configured ssh.exe is supported with its matching agent. Git for Windows and Windows OpenSSH agents have different transports. Custom shell commands/options, Plink/Pageant prompts, SSH account passwords/2FA and hardware-key confirmations stay with their own agent or terminal rather than the graphical passphrase dialog.

Host verification stays mandatory. Verify an unknown fingerprint independently before adding it through a terminal; investigate changed fingerprints with the server administrator before modifying known_hosts. **Test connection** reads references from a configured SSH remote without pushing, committing or accepting host keys. HTTPS continues to use existing Git credential helpers.

## HTTPS authentication

Open **Settings → HTTPS authentication → Manage HTTPS** to see the detected Git Credential Manager version, configured helper types/scopes and HTTPS remotes. Custom helper commands and URL paths stay hidden. Git determines the effective helper chain, including scope-specific entries and resets; Branvix never rewrites it. If GCM is not detected, public access and other configured helpers remain available.

Select a remote and choose **Check access / retry**. This read-only check allows the configured helper to show its login UI when needed. Valid credentials are reused; Git/GCM handle rejected or expired credentials. Branvix does not clear credential stores, force a new login or switch accounts. Browser/token choices depend on the provider and helper version. GitHub, GitLab, Azure DevOps, Bitbucket and compatible servers use their configured helper flows; self-hosted servers may need provider-specific configuration. Branvix stores no tokens and offers no token form. A read check does not verify push permission.

Separate messages explain authentication, denied access, unavailable repositories, rejected pushes, TLS and network errors. HTTPS URLs may include a username, such as `https://alice@bitbucket.company/scm/TEAM/repo.git`; passwords/tokens, query and fragment remain forbidden. Certificate verification is mandatory; existing CA and proxy configuration remains in effect. **Cancel operation** stops the owned Git/helper job; reload, crash, quit and update restart also clean it up. Unrelated browser processes are not closed; a provider tab may need closing manually. Network operations time out after 120 seconds including authentication.

The remote selector lists **Fetch** and **Push** destinations separately, including multiple configured push URLs. **Check access / retry** reads the exact selected endpoint; changed configuration invalidates the old selection and requires refreshing the list. URL paths stay hidden and the renderer cannot supply an arbitrary test URL. Diverged branches, local changes that would be overwritten and missing upstream tracking have specific guidance; only safe fixed summaries cross the bridge, never arbitrary helper diagnostics. Recursive HTTPS clones also prepare the managed SSH identity/passphrase bridge for SSH submodules, while keeping the HTTPS helper active.

## Updates

Verification limit for 1.5.1: the installed A→B upgrade, automatic reopening and preservation of preferences still await isolated Windows acceptance. Local downloader and smoke checks do not verify that transition. Clean Windows/NSIS and real provider/OS acceptance remain separate from local preparation.

Both distributions check for stable updates after startup and every four hours. You can also use **Settings → Updates → Check for updates**.

- The title-bar bell shows an unread dot. Opening its list marks the notification as read while keeping its action available.
- A dismissible banner appears once per version and closes after ten seconds, pausing while hovered or focused.
- The status-bar action remains available after closing the banner.

**Installer:** updates download in the background, showing the real percentage when available in notifications, the banner, status bar and Settings. Choose **Restart** once the download is verified: Branvix closes, the installer shows its progress, and the updated app reopens automatically when installation finishes. Branvix asks before interrupting an active terminal, including a hidden one; cancelling keeps it running. Windows may request installation permission. Downloaded updates also install on an actual normal exit, without forcing Branvix to reopen. With background mode enabled, **X** only hides the window and defers installation; use **Exit** from the tray or Settings to quit. **Restart** exits and requests installation/reopening even when background mode is enabled.

**Portable:** choose **Download portable** to open the official release page, download the new portable executable, close Branvix and replace the old executable manually.

Network errors do not interrupt Git work. Check Settings for errors and retry when connected.

## Open in Claude Code

Choose **Open with → Open in Claude Code** from the repository toolbar, or right-click a recent repository and choose the same action. Branvix asks the installed native Claude Code CLI to open the **Code tab of Claude Desktop** on that repository root. It sends no prompt and does not start a terminal session or change the default IDE.

This handoff requires **Windows x64**, the **native Claude Code CLI 2.1.285 or later**, Claude Desktop, and sign-in with a Claude subscription. Branvix supports the official native launcher in `~/.local/bin/claude.exe` and standard WinGet links; npm `.cmd` shims, arbitrary PATH executables and repository-local copies are not used. It validates the Windows signature/publisher before checking the CLI version. Install or update Claude separately using the [official instructions](https://code.claude.com/docs/en/setup).

The status bar shows the request in progress. Missing/unverified/outdated CLI or a failed Desktop handoff displays a message with **View requirements**, which opens the [official Desktop guide](https://code.claude.com/docs/en/desktop). **Request sent** confirms the CLI handoff, not that Desktop's window or subscription is ready. If no window appears, check Desktop installation and sign-in; Branvix never installs Claude or reads account credentials. See the [CLI reference](https://code.claude.com/docs/en/cli-reference) for current `--desktop` requirements.

## System tray and background mode

Branvix has a Windows system tray icon with **Open Branvix** and **Exit**. In **Settings → System tray**, enable **Keep Branvix running in the background when closing the window** to hide the window instead of quitting. This is off by default and saved across launches. Click the tray icon or choose **Open Branvix** to return. An active terminal keeps running while the window is hidden.

Choose **Exit** from the tray or Settings to quit. Branvix asks before stopping an active terminal or managed operation. Installer update **Restart** still closes Branvix and installs the update; portable updates remain manual.

## Windows notifications

In **Settings → Windows notifications**, choose updates, completed operations and failed operations independently. All three categories are enabled by default and saved in the application profile. Toasts are silent and only sent while Branvix is in the background, including when hidden in the tray. Supported managed operations are clone, fetch, pull, push, sync and tag push; commands typed in the terminal are not monitored. Cancelling an operation does not produce an error notification.

Clicking a toast restores Branvix; it never opens a repository URL or starts an update. Notifications contain only the operation name or release version, without repository paths, URLs, credentials or Git diagnostics. Update availability, download and ready states share one notification per version, with receipts preserved across restarts. Events suppressed in the foreground or with a category disabled are not replayed. The in-app bell, update status and operation messages remain available.

Windows **Settings → System → Notifications**, **Do not disturb** (Focus Assist), organizational policies and app identity can suppress toast delivery. The installer creates a Start Menu shortcut for Branvix. Portable builds use the same app identity but Windows may not recognize them as a registered app, especially after moving the executable or on a clean PC; native delivery is not guaranteed. No shortcut, registration workaround or permission change is applied by Branvix for the portable. Use the in-app messages if Windows hides notifications.

## Changelog and license

See [CHANGELOG.md](CHANGELOG.md) for released changes and [LICENSE](LICENSE) for the MIT license.

This repository distributes application builds and user documentation. The source repository is maintained separately; GitHub's source archives here contain only this repository's own files.
