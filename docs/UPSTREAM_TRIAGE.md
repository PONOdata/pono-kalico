# Upstream triage: pono-kalico against OpenCentauri/kalico

Measured 2026-09-28: branch `rpmsg-with-new-hx71x` carries 62 commits that
upstream's `rpmsg-with-new-hx71x` does not, and is 0 behind it. That branch
is OpenCentauri/kalico's default (`upstream/HEAD`); their `main` carries none
of the hifi4 port. None of the 62 is patch-equivalent to an upstream commit
(`git cherry`). The divergence cap in the Pono Print release manifest is 10,
and pono-print's `scripts/release_state.py` counts against this branch.
Absorbing upstream is milestone M3; this file is the first step, one row per
commit.

Classes:

- **UPSTREAM** (12): general interest, could be offered to OpenCentauri or KalicoCrew.
- **LOCAL** (29): specific to this machine, the Pono image, or our CI.
- **DROP** (14): reverted, superseded by a later commit in this list, or noise.
- **UNSURE** (7): the subject and file count do not settle it.

How it was made: each row was classified from the commit subject and its
file-change count only, on the Foundry tenant in three parts
(kalico-upstream-triage-20260927-c1..c3, claude-opus-5-5, hard, not
downshifted), then checked here so that every row names a real commit in
the range and every commit has exactly one row. The class is a starting
judgement, not a review of the diff: read the diff before offering any
UPSTREAM commit, and before squashing or dropping anything.

Corrected 2026-09-28. The first version counted against `upstream/main` and
had 153 rows. The 92 rows removed are upstream's own commits on its default
branch, the hifi4 port among them, so none of them was ours to offer. The
row for this file's own commit (#42) was added.

## First upstream candidates

The M112 latched-output fix, `afe7178d`, is upstream's commit. It is the head
of their default branch, so there is nothing to offer there. The first
version of this section named it, and said a `docs/M112_VERIFY.md` had to be
written before offering it; that step no longer applies. The E-STOP check on
this machine belongs to pono-print-os's flash checklist.

The clearest candidates of our own are two fixes to `src/hifi4/rpmsg.c`, a
file upstream wrote and last changed at `f66de876`, which is an ancestor of
both:

- `62d00eab` (#29): validate vring descriptors before dereferencing them.
- `9c3d3545` (#30): compute the rpmsg payload address as `hdr + 1`.

Both are correctness fixes in the host-to-DSP transport that every user of
the port runs. They wait on a bench check on the DSP hardware before they
are offered, per the public-PR rule that a hardware fix is verified on the
hardware first.

## Commits

| sha | date | subject | class | reason |
|---|---|---|---|---|
| fb924646 | 2026-05-22 | load_cell_fusion: clear sensor update flags after report | UPSTREAM | Load-cell fusion group; bugfix |
| f7147f4e | 2026-05-22 | load_cell_probe: int to float helpers to match docs | UPSTREAM | Small generic fix to upstream load_cell_probe |
| 4218e722 | 2026-05-22 | Reduce log rotate threshold | LOCAL | Pono image storage tuning |
| 06f701c0 | 2026-05-22 | docs: add DIVERGENCE.md tracking Brofalo-only commits | LOCAL | Fork bookkeeping docs |
| 299dedef | 2026-05-22 | heaters: make sensor sample loss tolerance configurable (#869) | UNSURE | Generic; unclear if #869 already merged upstream |
| 58a0dc25 | 2026-05-22 | docs: add CHANGELOG.md tracking releases | LOCAL | Fork release docs |
| 5d0a9ed8 | 2026-05-22 | configfile: remove save-config subfile duplicate check (OC PR #174 migration) | UNSURE | Mirrors OC PR #174; check OC status |
| 38ed32c6 | 2026-05-22 | docs: log 5d0a9ed8 + 299dedef divergence + Patch 1 migration | LOCAL | Fork bookkeeping docs |
| 08c51cba | 2026-05-23 | docs: clarify divergence count semantics + add FQ1/FQ2/OC-drift deferred rows | LOCAL | Fork bookkeeping docs |
| a30ddf63 | 2026-05-23 | hifi4: bound rpmsg cold-boot wait with ~10s timeout + warm-restart fallback | UPSTREAM | HiFi4 rpmsg group end; boot robustness |
| 5ae76b83 | 2026-05-23 | docs: FQ1 row moved from Deferred-patches to MIGRATED (a30ddf63) | LOCAL | Internal fork-tracking doc bookkeeping |
| cb8e1130 | 2026-05-23 | docs: FQ2 row updated with anchored Xtensa-strip blocker | DROP | Superseded by 58d475b0 FQ2 update |
| 58d475b0 | 2026-05-23 | docs: FQ2 row -> MIGRATED via pono-print-os@9c1e207 | LOCAL | Internal tracking doc, references Pono image repo |
| 32e92f89 | 2026-04-08 | Load Cell Tap Analysis (#836) | DROP | Already upstream KalicoCrew commit, back-merged |
| 714764d1 | 2026-05-08 | probe: add alternating probe direction support (#882) | DROP | Already upstream KalicoCrew commit, back-merged |
| 836dcb27 | 2026-04-19 | Default to extracting all moves in the trapq when the end time is not specified. This is the most common use-case. (#872) | DROP | Already upstream KalicoCrew commit, back-merged |
| 1f3aee10 | 2026-05-23 | docs: AQ2 KalicoCrew back-merge batch - 3 cherry-picks landed + 4 no-ops + 10 N/A | LOCAL | Fork back-merge log |
| 0efbde3f | 2026-05-23 | docs: D1 fix - correct Brofalo-only count 11 -> 17 (Class 185 + Class 222) | DROP | Count superseded by bdefaf15 recount |
| 24873c02 | 2026-05-23 | docs: add SECURITY.md and pivot README with Pono Print fork banner (#1) | LOCAL | Pono fork branding and policy |
| e2ed2377 | 2026-05-24 | docs(security): resolve TBD JACK-INPUT items (disclosure email + key custody) (#2) | LOCAL | Pono security contacts |
| bdefaf15 | 2026-05-24 | docs(divergence): recount to 19 Brofalo-only + a30ddf63 upstream-defer note + cross-refs (#3) | LOCAL | Fork divergence tracking |
| a2d06fe2 | 2026-05-24 | ci: coderabbit v2 - enable Pro-tier features | DROP | CodeRabbit config removed in 8ead7e8e |
| d295deb0 | 2026-05-24 | ci: coderabbit v3 - max-strict block-on-defect posture | DROP | CodeRabbit config removed in 8ead7e8e |
| 12ff84ad | 2026-05-24 | ci: coderabbit v3 - max-strict block-on-defect posture | DROP | Duplicate subject; config later removed |
| 47c4266a | 2026-06-07 | load_cell_probe: cast trigger_force to int for the uint32_t MCU field | UPSTREAM | General type-correctness bugfix for load_cell_probe |
| b421f2d4 | 2026-06-12 | Add the shared CI gate | LOCAL | CI gate feature start (with 141859aa, 70e18ca1) |
| 141859aa | 2026-06-12 | Vendor the shared CI gate locally | LOCAL | CI gate feature: vendored copy |
| 70e18ca1 | 2026-06-12 | Sync the vendored gate: semgrep baseline on PR events | LOCAL | CI gate feature: semgrep sync |
| 8ead7e8e | 2026-06-16 | chore: remove CodeRabbit config (#2) | LOCAL | Removes our own CI config |
| 968d345b | 2026-06-19 | ci: enable auto-merge on green PRs (#3) | LOCAL | Auto-merge feature start (with bd54b834) |
| bd54b834 | 2026-06-20 | ci: auto-merge enabler, signed re-land of #4 (#5) | LOCAL | Auto-merge feature: signed re-land |
| 85807ad3 | 2026-06-21 | fix(ci): add workflow permissions and pin actions to SHAs (#6) | UNSURE | Hardening may apply to shared upstream workflows |
| dbfdaa70 | 2026-07-14 | build(deps): bump the pip group across 2 directories with 3 updates (#7) | LOCAL | Dependabot bump, our lockfiles |
| e1d11fb7 | 2026-07-14 | build(deps): bump the uv group across 3 directories with 5 updates (#8) | LOCAL | Dependabot bump, our lockfiles |
| aaf3858c | 2026-07-24 | build(deps): bump the pip group across 2 directories with 1 update (#10) | LOCAL | Dependabot bump, our lockfiles |
| 3d630f2d | 2026-07-25 | build(deps): cap ruff below 0.16 to hold lint rules steady (#11) | DROP | Superseded by 868300ea rule-set pin |
| 7d771726 | 2026-07-27 | Run real commands in the required gate checks (#13) | LOCAL | Our CI gate |
| f1b4d172 | 2026-07-27 | Stop sensor_hx711s overflowing the small MCU targets (#14) | UPSTREAM | hx711s fix start (with #17 to #26) |
| dbb25380 | 2026-07-27 | Drop CONFIG_WANT_LOAD_CELL_PROBE from the test configs (#17) | UNSURE | hx711s series; may be CI-specific workaround |
| f1b30849 | 2026-07-27 | Average the fused load cell reading without a run-time divide (#18) | UPSTREAM | hx711s series: removes MCU divide |
| 7b4d0e78 | 2026-07-27 | Send the hx711s high-pass coefficient from the host (#19) | UPSTREAM | hx711s series: host-computed coefficient |
| 36a0c4d9 | 2026-07-27 | Send the hx711s high-pass coefficient from the host (#21) | DROP | Empty duplicate of #19 |
| 34d821c4 | 2026-07-27 | Remove the run-time integer divides from the hx711s probe path (#20) | UPSTREAM | hx711s series: divide-free probe path |
| 003c413d | 2026-07-27 | Arm the build gate (#22) | LOCAL | Our CI gate |
| 6dcdf508 | 2026-07-27 | Fail the divide check on the EABI aliases too (#23) | UNSURE | Divide check useful upstream if check itself offered |
| 8d1d33af | 2026-07-27 | Write down the hx711s bench check (#25) | UNSURE | Bench procedure may be machine-specific |
| aa1153b4 | 2026-07-28 | Enforce the small_divide bound instead of documenting it (#26) | UPSTREAM | hx711s series: enforces divide bound |
| 2a8b563c | 2026-07-28 | Make the required tell_check gate run something (#27) | LOCAL | Our CI gate |
| 868300ea | 2026-07-29 | ci(ruff): pin the rule set so the gate stops depending on ruff version (#28) | LOCAL | Our lint config |
| 62d00eab | 2026-07-31 | fix(hifi4): validate vring descriptors before dereferencing them (#29) | UPSTREAM | Safety fix; OpenCentauri shares hifi4 code |
| 9c3d3545 | 2026-07-31 | fix(hifi4): compute the rpmsg payload address as hdr + 1 (#30) | UPSTREAM | Pointer bugfix for OpenCentauri hifi4 |
| 806a3e5f | 2026-08-08 | Port the auto-merge enabler fix from pono-print-os (#31) | LOCAL | Our auto-merge workflow |
| aab34595 | 2026-08-08 | Adopt OpenCentauri hx711s-new2: per-channel tare and LOAD_CELL_CALIBRATE TARE (#32) | DROP | Already OpenCentauri's code; not re-offerable |
| 2f0eee3e | 2026-08-19 | Bump the reviewer pin to pick up escalation (#34) | DROP | pono-review caller removed in 1d621440 |
| 710ad270 | 2026-08-19 | Restore the trailing newline on the reviewer caller (#35) | DROP | pono-review caller removed in 1d621440 |
| 54e5a0f3 | 2026-08-23 | pono-review: bump the pin to pick up the zero-call guard (#36) | DROP | pono-review caller removed in 1d621440 |
| 1d621440 | 2026-08-23 | Remove the pono-review caller; public repos cannot call it (#37) | LOCAL | Our CI cleanup |
| 69b027bf | 2026-08-23 | Raise the docs Python floor to 3.10 and clear three dependency advisories (#38) | UNSURE | Docs deps may matter upstream; policy choice |
| a2c8c5e1 | 2026-08-23 | Save the drift filter cutoff only after the failure check (#39) | UPSTREAM | Load cell drift filter ordering bugfix |
| 3cb57ad1 | 2026-08-24 | automerge: merge under the Pono automerge GitHub App (#40) | LOCAL | Pono GitHub App |
| 49d02ec5 | 2026-09-17 | automerge: the direct merge inside the catch must not throw (#41) | LOCAL | Our auto-merge workflow |
| d9e09635 | 2026-09-26 | docs: triage every commit past upstream, 153 rows (#42) | LOCAL | This file |
