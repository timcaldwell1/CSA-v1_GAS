# 2️⃣ New `README.md` for GitHub

**Replace the existing README entirely.**

Below is a clean, production-grade, governance-aware README.

---

```markdown
# CSA_v1 — Church Safety Alerts

Church Safety Alerts (CSA_v1) is a Google Apps Script–based Proof of Concept (PoC) system for monitoring and visualizing local incident data for campus safety awareness.

This repository contains the canonical GAS source snapshots exported from the container-bound script project.

---

# Project Status

**Current Checkpoint:** DEV042  
**Stable Verification:** PASS  
**Date (MST):** 2026-02-13 04:53:30 MST  
**Governance Model:** ToolRunner-only execution surface  
**Triggers Installed:** 0  
**WebApp Entry Point:** `doGet` in `CSA_Router_v1_0`

DEV042 focused on governance tightening and QD (Quick & Dirty) surface reduction.  
Runtime behavior was not altered.

---

# Architecture Overview

Runtime pipeline:

```

Router → WebApp_Facade → DataBridge → AppShell

```

Key principles:

- Deterministic structural verification
- No implicit execution (no onOpen, no menus)
- No triggers
- Single operator surface via ToolRunner
- Redacted binder-safe reporting tools

---

# Canonical Operator Surface

All editor-run operations must go through:

```

CSA_ToolRunner_DEV041.gs

```

Approved run targets include:

- Stable checkpoint verification
- Baseline capture
- DataBridge validation
- Container binding proof
- Read-only reporting (binder-safe)

No other entrypoints are permitted.

---

# Structural Integrity Model

CSA_v1 uses a structural signature model that fingerprints:

- Script ID + Timezone
- Script Properties (secret-safe)
- Function inventory
- Spreadsheet schema hashes
- Trigger inventory

Verification loop:

```

Change → Verify (FAIL expected) → Capture → Verify PASS

```

Secrets such as PAT values are excluded from structural drift detection.

---

# DEV042 Governance Outcomes

The following were removed or isolated:

- QD scratch tooling
- Abandoned backup experiments
- One-time setup initializers
- Sheets menu surfaces
- Property mutators not required for PoC
- Uncontrolled execution paths

Remaining system is lean and deterministic.

---

# Security Notes

- Secrets are never committed to this repository.
- Verifier redacts secret values.
- GitHub PAT is operational and not part of structural fingerprinting.
- Binder-safe reporting tools generate redacted output.

---

# Repository Structure

This repository contains exported GAS source snapshots under:

```

backups/gas/YYYY/MM/DD/

```

Each snapshot represents a stable export at a specific checkpoint.

---

# Installation / Deployment

This repository does not auto-deploy.

The live system is a container-bound Google Apps Script project.

Deployment steps are controlled manually through ToolRunner and Apps Script Editor.

---

# Scope

CSA_v1 is currently a Proof of Concept focused on:

- Incident ingestion from Sheets
- Time normalization
- Map + card rendering
- Observability logging
- Governance integrity

It is not yet a production multi-campus deployment system.

---

# Maintainer

Tim Caldwell  
CSA_v1 — Church Safety Alerts

---

# License

Private project — Not for redistribution.
```

---

## 3️⃣ Optional (Recommended)

After pushing:

* Create a Git tag: CSA GAS Source Backup complete: CSA_GAS_BACKUP_20260213_055447

```
DEV042_STABLE_2026-02-13
```

This prevents ambiguity later.

