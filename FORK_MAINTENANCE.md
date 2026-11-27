# Fork maintenance notes

Addresses upstream [issue #1](https://github.com/QianYushi/turnintoserver/issues/1) (heavy disk writes) and [issue #2](https://github.com/QianYushi/turnintoserver/issues/2) (silent sleep-prevention reset).

## In-memory history only

- The rolling 24-hour CPU and memory history remains available while turnintoserver runs.
- No history plist is read or written. All previous persistence code is removed, not merely disabled.
- Live system monitoring and in-session history remain available. History resets when the app restarts.
- To reclaim disk space from old versions, quit the old app and delete the old history file at ~/Library/Application Support/turnintoserver/memory-history-v1.plist.

## Optional closed-lid monitor

- Separate menu switch: Monitor and Restore Closed-Lid Mode (disabled by default).
- While Server Mode is requested and permitted by power policy, poll kernel SleepDisabled every 10 seconds.
- If another process clears the flag, reapply it via the existing non-interactive sudo -n pmset disablesleep 1 allowance.
- Verify restoration, show a warning if verification or recovery fails, and do not ask for administrator credentials from the background poller.
- Timed expiry, deliberate system sleep, paused Server Mode, battery policy, and manual disable are respected.
- Best-effort repair only; another process could clear the global setting between checks.

## GitHub Actions

Build on macOS-15 with Xcode and publish an unsigned CI ZIP as a short-lived artifact. The unsigned artifact is not notarized or appropriate as a trusted public release. Public distribution needs independently trusted Apple Developer ID signing and notarization.

## Licensing

The upstream repository currently has no explicit LICENSE. GitHub fork permissions are not a grant of rights to redistribute derivative binaries. Seek upstream authorization before publicly distributing the derived app.
