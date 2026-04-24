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
