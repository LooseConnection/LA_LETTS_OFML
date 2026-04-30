# ConnectCIC — MANDATORY PROCESS RULES

> **STOP. Read this entire section before doing ANY work in this repo.**
>
> These rules are MANDATORY for every Claude instance working on ConnectCIC provider JSON repos.
> The user (Rob) has established these through repeated corrections. They are not optional.
> They are not suggestions. They cannot be deferred, batched, or skipped.
>
> **If you are tempted to skip a step to save time — STOP. The step exists because
> skipping it caused real failures. Do it now or do not report progress.**

**Source of truth:** `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\PROCESS_RULES.md`
**Last synced:** 2026-04-30

## 1. SESSION START — Before Any Work

1. Read this CLAUDE.md completely
2. Read the KB master rules: `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md`
3. Run `git status` — confirm working tree is CLEAN and branch is synced with remote
4. Verify `docs/` contains: STATUS.txt, SQVR.txt, BUILD_NOTES.txt, `base/` with 5 report files
5. Verify `tests/` directory exists
6. **If ANYTHING from steps 3-5 is wrong: fix it FIRST before starting the requested task**

## 2. TRIGGER RULES — Automatic Chaining

When a trigger fires, ALL chained actions are part of the same unit of work.

**You edit or create any `.json` →** Run `build_report.ps1` → Verify 0 FAIL → Commit JSON + reports → `git push` → Update STATUS.txt / SQVR.txt if changed

**You complete a live test →** Fill test log → `git add/commit/push` → Update STATUS.txt test matrix → Update SQVR.txt ([PENDING] → [CONFIRMED])

**You update any KB file →** Push KB → Check cross-repo impact → Update affected repos

**You discover a new limitation/anti-pattern/import error →** Add to KB → Fire KB trigger

## 3. MANDATORY GATES — Blocking Requirements

**GATE 1 (post-build):** `build_report.ps1` + 0 FAIL + commit reports + push. BLOCKED until done.
**GATE 2 (pre-test):** `new_test_log.ps1` creates stub in `tests/`. BLOCKED until stub exists on disk.
**GATE 3 (post-test):** Fill log + commit + push. BLOCKED from next test until done.
**GATE 4 (post-session):** Update STATUS.txt + commit + push.
**GATE 5 (pre-DONE):** Verify: `ls tests/` (count matches), `docs/base/` (5 reports), STATUS.txt current, SQVR.txt current, `git status` clean. BLOCKED until all pass.
**GATE 6 (post-KB-update):** Push KB + check cross-repo impact + update affected repos.

## 4. END-OF-RESPONSE VERIFICATION

Before ending ANY response with file changes: (1) all committed+pushed? (2) reports generated? (3) logs saved? (4) STATUS current? (5) SQVR current? (6) KB pushed if updated? (7) anything deferred? Fix now or state why.

## 5. TOOLS

```powershell
# Build report (5 reports) — GATE 1
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path <json>
# Test log stub — GATE 2
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\new_test_log.ps1 -Provider <NAME> -Variant BASE -Version <ver> -Entity <entity> -Combo <combo> -Description "<desc>"
# Validator only
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\connectcic-validator\validate.ps1 -Path <json>
```

## 6. KB REFERENCE — Read Before Every Session

- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md` — Master build rules
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\README.txt` — KB index (13 docs)
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\BUILD_CHECKLIST.txt` — Full checklists

## 7. CANONICAL REPO STRUCTURE

```
<PROVIDER>/
├── CLAUDE.md, .gitignore, <PROVIDER>_BASE.json, <PROVIDER>_MC.json
├── docs/ (STATUS.txt, BUILD_NOTES.txt, SQVR.txt, JSON_INVENTORY.md, base/, mc/)
├── tests/ (one log file per live test)
├── phases/, release/, scripts/, source/
```

If this repo does not match, fix it before doing any other work.

---
<!-- END PROCESS RULES — Provider-specific content below -->

# LA_LETTS_OFML Provider JSON

Owner: rob.sgambellone@mark43.com

## ATTENTION

This build has **3 warnings** that should be resolved before import. See below.

## Status

| Variant | Version | Validator | Live Test | Date |
|---------|---------|-----------|-----------|------|
| BASE | v1.0 | 40 PASS / 0 FAIL / 3 WARN | NOT TESTED | 2026-04-24 |

BASE is **REVIEWED** but has known warnings. Build script is a stub (needs completion).

## Open Issues

1. **Build script is a stub** — `scripts/build_la_letts_ofml.ps1` has placeholder text and hardcoded path (`C:\Users\Gordon Hallof\LA_LETTS_OFML`). Needs to be completed following the NJ_NJCJIS build script pattern. Update path to use `$PSScriptRoot`.

2. **WARN: ENTITIES bundle not first** — Bundle order should be PROVIDER -> ENTITIES -> RMS. Reorder bundles.

3. **WARN: No entity order array** — ENTITIES bundle is missing the `order` array. Add display order (see NJ reference: `["Person","Vehicle","Firearm","Article","Boat"]`).

4. **WARN: GunTypeCode sourceField missing from Firearm QIF** — QIDM `LA_LEMS_GunQuery` references `GunTypeCode` but no matching fieldId in Firearm QIF. Either add the field or remove from QIDM.

5. **No MC variant** — Only BASE exists. Phase 2 (multi-card) not started.

## Build & Validate

```powershell
# BASE (stub — needs completion)
powershell -ExecutionPolicy Bypass -File scripts/build_la_letts_ofml.ps1

# Full report
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path LA_LETTS_OFML_BASE.json

# Release bundle
powershell -ExecutionPolicy Bypass -File C:\Users\RobSgambellone\.local\bin\build_report.ps1 -Path LA_LETTS_OFML_BASE.json -Release
```

## Knowledge Base

Read before every session:
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\CLAUDE.md` — Master build rules
- `C:\Users\RobSgambellone\.local\bin\ConnectCIC-KB\knowledge-base\README.txt` — KB index (11 docs)

## Source Materials

- `source/LA_LEMS .xml` — XML metadata (primary build authority)
- `source/LA_LEMS_OFML.pdf` — CommSys devdoc
- `source/HIDLE.json` — RMS/auth base template
