# Downstream maintenance

This fork keeps Forgejo `origin` authoritative and GitHub `upstream` read-only.
The GitHub `github` remote is only for changes intentionally offered upstream.
Apply the same remote roles in LibreEcho, LibreEcho-UI,
LibreEcho-Platform, and LibreEcho-Linux-6.1.

Keep the downstream delta small. Each behavioral fix gets its own commit,
regression test, branch, and Forgejo pull request. Do not put device identifiers,
credentials, firmware dumps, recovery artifacts, or private runbooks in these
repositories.

## Patch ledger

| Repository | Patch | Branch | Status |
| --- | --- | --- | --- |
| LibreEcho-Platform | Accept the compatible MT8163 owner-firmware revision | `fix/owner-firmware-variants` | Pending Forgejo PR |
| LibreEcho-UI | Emit Wyoming 1.10 artifact metadata | `fix/wyoming-info-schema` | Forgejo PR 1, pending merge |
| LibreEcho-UI | Keep Home Assistant and voice-pipeline modes synchronized | `fix/home-assistant-mode-sync` | Forgejo PR 2, pending merge |

Update this table whenever a patch is added, replaced, merged, or retired.

## Upstream update procedure

1. Fetch `upstream` in all four repositories and record the exact upstream
   commits or release being considered. Do not change the device yet.
2. Compare each ledger entry with upstream source and tests. A similar commit,
   clean merge, or release note is not enough to retire a patch.
3. Prove the patch's regression test against the new upstream baseline. Retire
   it only when upstream passes the same behavior without downstream code.
4. Create a focused update branch from downstream `main`, merge the selected
   upstream baseline without rewriting published history, resolve only relevant
   conflicts, and open a Forgejo PR.
5. Run each component's required checks on the exact PR head. Keep host tests,
   image tests, and hardware acceptance as separate evidence.
6. After explicit merge approval, build a candidate from the exact merged
   component commits. Deploy through the documented A/B update procedure while
   preserving a known-good slot.
7. Verify a cold boot, Wi-Fi, web UI, voice mode, Wyoming/Home Assistant
   connectivity when enabled, and the top-board hardware before declaring the
   update good.

If upstream absorbs a patch, remove it in the update PR and mark it retired in
the ledger. If behavior is uncertain, keep the patch and investigate separately.
