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


## Backup & Source Control — Google Apps Script Snapshot Push

### Overview

CSA_v1 includes an operator-controlled backup mechanism that:

1. Creates a Drive snapshot folder (`CSA_GAS_BACKUP_YYYYMMDD_HHMMSS`)
2. Pushes the contents to GitHub under:

```
backups/gas/YYYY/MM/DD/CSA_GAS_BACKUP_YYYYMMDD_HHMMSS/
```

This preserves full source history outside Apps Script versioning.

This tool does **not** modify ingest logic or runtime behavior.

---

### Required Script Properties

| Property                   | Description                              |
| -------------------------- | ---------------------------------------- |
| `GITHUB_PAT`               | Fine-grained Personal Access Token       |
| `GITHUB_REPO`              | e.g., `timcaldwell1/CSA-v1_GAS`          |
| `GITHUB_BRANCH`            | e.g., `main`                             |
| `GITHUB_BASE_PATH`         | e.g., `backups/gas`                      |
| `CSA_GAS_BACKUP_FOLDER_ID` | Drive folder containing snapshot folders |

---

### Required GitHub Token Permissions (Fine-Grained PAT)

Repository access:

* Select repository: `CSA-v1_GAS`

Repository permissions:

* **Contents → Read and write**
* Metadata → Read

Other permissions (Actions, Admin, etc.) are not required for contents writes.

If the token lacks `Contents: write`, GitHub will return:

```
403 Resource not accessible by personal access token
```

---

### ToolRunner Entry Points

| Function                                      | Purpose                     |
| --------------------------------------------- | --------------------------- |
| `RUN_GITHUB_AuthTest_DEV049()`                | Verify token authentication |
| `RUN_GITHUB_DiagnosePermissions_DEV049()`     | Print permission headers    |
| `RUN_GITHUB_PushLatestDriveSnapshot_DEV049()` | Push latest snapshot        |

---

### Successful Push Log Example

```
[GITHUB][PUSH] OK backups/gas/2026/02/16/CSA_GAS_BACKUP_20260216_142858/appsscript.json (CREATE)
[GITHUB][PUSH] DONE pushed=53.0 folder=CSA_GAS_BACKUP_20260216_142858
[ToolRunner][GITHUB][PUSH] Completed: CSA_GitHub_Push_Latest_GAS_Snapshot_To_GitHub_DEV049_
```

Interpretation:

* All files written successfully
* No 403
* No SHA mismatch
* No partial failure

---

### Governance Notes

* Snapshots are append-only in GitHub.
* Files are created or updated using SHA reconciliation.
* No runtime CSA ingest logic is affected.
* This tool exists strictly for operational backup control.

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

