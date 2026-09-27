# Upstream triage: pono-kalico against OpenCentauri/kalico

Measured 2026-09-26: branch `rpmsg-with-new-hx71x` carries 153 commits that
`upstream/main` (OpenCentauri/kalico, itself a fork of KalicoCrew/kalico) does
not. The divergence cap in the Pono Print release manifest is 10. Absorbing
upstream is milestone M3; this file is the first step, one row per commit.

Classes:

- **UPSTREAM** (67): general interest, could be offered to OpenCentauri or KalicoCrew.
- **LOCAL** (32): specific to this machine, the Pono image, or our CI.
- **DROP** (44): reverted, superseded by a later commit in this list, or noise.
- **UNSURE** (10): the subject and file count do not settle it.

How it was made: each row was classified from the commit subject and its
file-change count only, on the Foundry tenant in three parts
(kalico-upstream-triage-20260927-c1..c3, claude-opus-5-5, hard, not
downshifted), then checked here so that every row names a real commit in
the range and every commit has exactly one row. The class is a starting
judgement, not a review of the diff: read the diff before offering any
UPSTREAM commit, and before squashing or dropping anything.

## First upstream candidate

The M112 latched-output fix, `afe7178d` ("hifi4: fix M112 shutdown leaving outputs latched"), is the
clearest general-interest change: an emergency stop that left heater and
motor outputs latched on the HiFi4 path is a safety bug for anyone running
this board. It waits on a bench check on the DSP hardware before it is
offered, per the public-PR rule that a hardware fix is verified on the
hardware first. That procedure is not in this repository yet: a
`docs/M112_VERIFY.md` was planned in May and appears in no branch here, so
writing it is the first step of this candidate.

## Commits

| sha | date | subject | class | reason |
|---|---|---|---|---|
| 2c4af27f | 2025-12-28 | hifi4: Initial implementation of hifi4 | UPSTREAM | [hifi4 port group] Start of the DSP MCU port for OpenCentauri |
| 0a1a1b3f | 2026-01-04 | hifi4: Now boots, interrupts not working yet | UPSTREAM | [hifi4 port group] WIP; squash into the port |
| f5ae92c2 | 2026-01-05 | hifi4: Interrupt working, kind of | UPSTREAM | [hifi4 port group] WIP interrupt work; squash |
| 95240a35 | 2026-01-05 | hifi4: Interrupt working a little more | UPSTREAM | [hifi4 port group] WIP interrupt work; squash |
| c6dde7d5 | 2026-01-06 | hifi4: Fix bug in timer driver | UPSTREAM | [hifi4 port group] Timer fix; the timer is later replaced by hstimer |
| 41da5853 | 2026-01-06 | hifi4: Better low level interrupt handler | UPSTREAM | [hifi4 port group] Interrupt handler rework |
| 7351200d | 2026-01-06 | hifi4: Fixup hstimer driver | UPSTREAM | [hifi4 port group] hstimer driver fix |
| db056f01 | 2026-01-06 | hifi4: Use hstimer rather than timer | UPSTREAM | [hifi4 port group] Switch to hstimer |
| d9057427 | 2026-01-06 | hifi4: Clean up code a little | UPSTREAM | [hifi4 port group] Cleanup; squash |
| 5c578ffc | 2026-01-06 | hifi4: Squash some bugs | UPSTREAM | [hifi4 port group] Bugfixes; squash |
| 82e3f4ae | 2026-01-08 | hifi4: Refine interrupt handling code | UPSTREAM | [hifi4 port group] Interrupt refactor |
| e00f331c | 2026-01-12 | hifi4: Added sharespace driver | UPSTREAM | [hifi4 port group] ARM-to-DSP shared memory transport |
| 16fb3293 | 2026-01-12 | hifi4: Add basic logging via sharespace | UPSTREAM | [hifi4 port group] Debug logging over sharespace |
| 619ca682 | 2026-01-13 | hifi4: Remove elegoo usb code, make dedicated driver for sharespace communication | UPSTREAM | [hifi4 port group] Replaces vendor USB code with a clean driver |
| 6de6a483 | 2026-01-13 | hifi4: Add GPIO DECL_ENUMERATION_RANGE and fix silly bug | UPSTREAM | [hifi4 port group] GPIO enumeration plus a fix |
| c9e60a6e | 2026-01-14 | hifi4: Opimise and simplify communication handler | UPSTREAM | [hifi4 port group] Communication handler simplification |
| 5367ff8d | 2026-01-14 | hifi4: Remove more sections that stock klipper doesn't understand | UPSTREAM | [hifi4 port group] Build/linker compatibility cleanup |
| 992e2908 | 2026-01-15 | hifi4: Fix bug in sharespace init | UPSTREAM | [hifi4 port group] Sharespace init fix |
| ac77ad06 | 2026-01-16 | hifi4: Implement watchdog driver | UPSTREAM | [hifi4 port group] Hardware watchdog; later reworked by 1d4dfb0e |
| ebc2f73d | 2026-01-16 | hifi4: Fix some more bugs | UPSTREAM | [hifi4 port group] Bugfixes; squash |
| 3efcff5d | 2026-01-19 | hifi4: Enable DDR caching for better runtimer performance | UPSTREAM | [hifi4 port group] DDR caching for performance |
| 92caa483 | 2026-01-19 | hifi4: Implement better lprintf support | UPSTREAM | [hifi4 port group] Improved debug printf |
| b59a6997 | 2026-01-19 | hifi4: Only invalidate sharespace dsp read region if we actually have data to read | UPSTREAM | [hifi4 port group] Cache invalidation optimisation |
| 906ec4d8 | 2026-01-19 | hifi4: Init timers after sharespace and disable watchdog at boot | UPSTREAM | [hifi4 port group] Init ordering fix |
| 35c42337 | 2026-01-19 | hifi4: Add timer_reset to prevent timer wraparound/overflow | UPSTREAM | [hifi4 port group] Timer overflow fix |
| 9d64198a | 2026-01-19 | hifi4: Clean up lprintf calls | UPSTREAM | [hifi4 port group] Cleanup; squash |
| 47b3f5f4 | 2026-01-20 | Update Command_Templates.md (#804) | DROP | Already in KalicoCrew (#804); arrives via merge |
| 746631ed | 2026-01-20 | hifi4: Don't invalidate cache of non-cached region | UPSTREAM | [hifi4 port group] Cache handling fix |
| e9a779d0 | 2026-01-20 | hifi4: Support UART4 and UART5 | UPSTREAM | [hifi4 port group] Adds UART4/5; completed by 2d97f78c |
| 1d4dfb0e | 2026-01-20 | hifi4: Use soft watchdog and add restart command | UPSTREAM | [hifi4 port group] Soft watchdog supersedes hardware approach |
| 8599e1f7 | 2026-01-20 | hifi4: Add ADC_MAX constant | UPSTREAM | [hifi4 port group] Constant needed by host code |
| 9c6bafa7 | 2026-01-20 | config: Update elegoo CC1 config | UNSURE | CC1 config; check for Pono-only settings |
| 0bcb31d9 | 2026-01-21 | Fix 'discard first result' in probe (#808) | DROP | Already in KalicoCrew (#808) |
| b42dd203 | 2026-01-21 | Fix path/filename for secondary heattest file (#809) | DROP | Already in KalicoCrew (#809) |
| a438bb02 | 2026-01-22 | stm32: Add STM32H723 520MHz support (#800) | DROP | Already in KalicoCrew (#800) |
| 8d9fea82 | 2026-01-22 | Sync PWM implementation with upstream Klipper (#799) | DROP | Already in KalicoCrew (#799) |
| 08c4fa63 | 2026-01-22 | ci: Add automatic monthly release tagging (#810) | DROP | Already in KalicoCrew (#810); check whether it runs in our CI |
| 20827723 | 2026-01-22 | adds support to python3.14 (#801) | DROP | Already in KalicoCrew (#801) |
| 9b3a56d7 | 2026-01-22 | build(deps): bump requests from 2.32.3 to 2.32.4 in /docs/_kalico (#813) | DROP | KalicoCrew dependabot bump |
| 967a81f1 | 2026-01-22 | build(deps): bump jinja2 from 3.1.5 to 3.1.6 in /docs/_kalico (#812) | DROP | KalicoCrew dependabot bump |
| da29c67c | 2026-01-22 | build(deps): bump urllib3 from 2.3.0 to 2.6.3 in /docs/_kalico (#811) | DROP | KalicoCrew dependabot bump |
| 3c929534 | 2026-01-22 | Increase stm32g431 speed to 170Mhz and add to benchmark (#802) | DROP | Already in KalicoCrew (#802) |
| c6601513 | 2026-01-22 | build(deps): bump pymdown-extensions in /docs/_kalico (#814) | DROP | KalicoCrew dependabot bump |
| 9bd9dbd9 | 2026-01-22 | ci: add concurrency to cancel duplicate workflow runs (#815) | DROP | Already in KalicoCrew (#815) |
| fb7b58f7 | 2026-01-22 | i2c upstream sync (#805) | DROP | Already in KalicoCrew (#805) |
| 4a173af8 | 2026-01-22 | deps: security updates (#816) | DROP | Already in KalicoCrew (#816) |
| 2d97f78c | 2026-01-29 | hifi4: Support UART4 and UART5 | UPSTREAM | [hifi4 port group] Follow-up to e9a779d0; squash together |
| 92b051d8 | 2026-01-29 | hifi4: Fix ADC reading, now use polling | UPSTREAM | [hifi4 port group] ADC switched to polling |
| ea05e172 | 2026-01-29 | hifi4: Hopefully fix dram caching issues | UPSTREAM | [hifi4 port group] DRAM caching fix; needs verification |
| f77757d7 | 2026-01-29 | hifi4: Update ECC1 config | UNSURE | [ECC1 config group] Large config rewrite; check for Pono-specific values |
| 89961441 | 2026-01-29 | hifi4: Update ECC1 config | UNSURE | [ECC1 config group] Stats missing from input; likely follow-up |
| 10f1d9da | 2026-01-30 | fix recursive status call in filament_width_sensor (#823) | DROP | Already in KalicoCrew upstream via merge |
| c70d21ab | 2026-01-31 | Fixed last_fan_value when kickstarting (#819) | DROP | Already in KalicoCrew upstream via merge |
| 156cb613 | 2026-01-31 | hifi4: Use internal timer instead of HSTimer | UPSTREAM | HiFi4 DSP port feature group; offer to OpenCentauri |
| 8749a930 | 2026-01-31 | hifi4: Small tweaks | UPSTREAM | HiFi4 group; squash into DSP port |
| 1c97e844 | 2026-02-02 | Add HiFi4 firmware build CI workflow and configs | LOCAL | Our CI workflow |
| 1bd49971 | 2026-02-02 | Rename to CC1 (Centauri Carbon 1) build | LOCAL | CI build naming |
| b4ea61cc | 2026-02-03 | hifi4: Allow restarting of dsp | UPSTREAM | HiFi4 group; DSP restart support |
| 769341a6 | 2026-02-03 | Merge remote-tracking branch 'upstream/main' into hifi4 | DROP | Merge commit, noise |
| 96a789f6 | 2026-02-03 | hifi4: Run at 600MHz now klipper supports it | UPSTREAM | HiFi4 group; clock increase |
| 2f06e137 | 2026-02-03 | Updating naming conventions and simplifying zip file to match Sims' naming… | LOCAL | Packaging for cc-fw-tools/Pono image |
| b49b2363 | 2026-02-04 | hifi4: Update cpu clock freq in config | UPSTREAM | HiFi4 group; config follows 600MHz change |
| faeaa17e | 2026-02-02 | [ElegooCC] Initial support for HX711s | UPSTREAM | HX711S group start; squash with port commits |
| 1450a250 | 2026-02-04 | hifi4: Update configs | UPSTREAM | HiFi4 group; config updates |
| d52e234d | 2026-02-06 | hifi4: Reinitialze data region from original copy on restart | UPSTREAM | HiFi4 group; restart correctness |
| 1f80a62a | 2026-02-06 | hifi4: Fix bug when calling longjmp from interrupt | UPSTREAM | HiFi4 group; interrupt bugfix |
| 733350dd | 2026-02-06 | hifi4: Fix up more quirks with sharespace init after restarting | DROP | Sharespace superseded by rpmsg (0f372d24) |
| 22a47603 | 2026-02-06 | [HX711S] Port to kalico and improve precision | UPSTREAM | HX711S group; Kalico port |
| b3c8eb87 | 2026-02-07 | [HX711S] Improve scheduling | UPSTREAM | HX711S group |
| bd1fe3a3 | 2026-02-08 | [HX711S] Use integer arithmetic | UPSTREAM | HX711S group |
| b4ee82af | 2026-02-08 | Remove libmath | UPSTREAM | HX711S group; follows integer arithmetic |
| 4be2799e | 2026-02-09 | [HX711S] Add small pauses | UPSTREAM | HX711S group |
| 7b448d5a | 2026-02-09 | [HX711S] Stop sampling after homing | UPSTREAM | HX711S group |
| fc6b0a96 | 2026-02-10 | [HX711S] Cleanup & fixes | UPSTREAM | HX711S group end |
| 415633af | 2026-02-10 | Update build script for new branch | LOCAL | Our build script branch name |
| b0f6a949 | 2026-02-17 | formatting: new ruff version (#831) | DROP | Already in KalicoCrew upstream via merge |
| 97ce44af | 2026-02-17 | PR: Load Cell Probe (#760) | DROP | Already in KalicoCrew upstream via merge |
| c0849a53 | 2026-02-18 | PR: Add NOZZLE_CLEANUP module (#833) | DROP | Already in KalicoCrew upstream via merge |
| 7beefc3d | 2026-02-18 | PR: Add `horizontal_z_clearance` option to `[bed_mesh]` (#832) | DROP | Already in KalicoCrew upstream via merge |
| 0f372d24 | 2026-02-21 | hifi4: Use rpmsg instead of sharespace | UPSTREAM | HiFi4 rpmsg group start |
| 6f2f8987 | 2026-02-24 | fix: AHT10 - Initialise the `mcu` property… (#845) | DROP | Already in KalicoCrew upstream via merge |
| 9500bd61 | 2026-02-24 | safe_z_home: auto detach dockable_probe on z home if required (#827) | DROP | Already in KalicoCrew upstream via merge |
| 79f7f7b5 | 2026-02-26 | hifi4: Don't user ddr1 for program memory | UPSTREAM | HiFi4 rpmsg group; memory layout |
| 27902226 | 2026-02-26 | PR: allow overriding `pressure_advance_smooth_time`… (#840) | DROP | Already in KalicoCrew upstream via merge |
| 0d0f76e2 | 2026-02-27 | hifi4: Fix bug in com.c which doen't process all packets… | UPSTREAM | HiFi4 rpmsg group; packet handling fix |
| 6bf75834 | 2026-03-04 | ldc1612: configurable crystal frequency and frequency divider (#852) | DROP | Already in KalicoCrew upstream via merge |
| 043f87c5 | 2026-03-04 | bed_mesh: Fix startup crash when using bed_mesh_default (#855) | DROP | Already in KalicoCrew upstream via merge |
| f26c79c7 | 2026-03-04 | stepper: ensure minimum time between step and dir pin changes (#853) | DROP | Already in KalicoCrew upstream via merge |
| 7049d04c | 2026-03-19 | Merge remote-tracking branch 'kalico-upstream/main' into rpmsg-with-new-hx71x | DROP | Merge commit, noise |
| 5f9fabbd | 2026-03-17 | [load_cell] Implement a generic fusion mechanism | UPSTREAM | Load-cell fusion group; generic, offer KalicoCrew |
| f66de876 | 2026-03-22 | hifi4: Allow rpmsg driver to reconnect if channel is already open | UPSTREAM | HiFi4 rpmsg group; reconnect robustness |
| afe7178d | 2026-04-04 | hifi4: fix M112 shutdown leaving outputs latched | UPSTREAM | HiFi4 group; safety fix, prioritize |
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
| f1b4d172 | 2026-07-27 | Stop sensor_hx711s overflowing the small MCU targets (#14) | UPSTREAM | hx711s fix start (with #17–#26) |
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
