# CSA_v1 — Church Safety Alerts (CSA_v1.0)

**Governed Proof-of-Concept Safety Alert Platform**
Google Apps Script (GAS) + Google Sheets + Leaflet WebApp

---

## 1. What This Is

CSA_v1 is a structured, governance-driven proof-of-concept safety alert system that:

* Ingests structured incident data
* Applies sheet-driven keyword + risk scoring
* Writes append-only outputs
* Preserves deterministic audit posture
* Maintains strict stable checkpoint discipline

The runtime baseline is **CSA_v1.0**.
Governed evolution artifacts are versioned under **CSA_v1.1**.

This distinction is intentional and must remain consistent.

---

## 2. Current System Posture (As of DEV055 Stable Checkpoint)

* Environment: `DEV`
* Triggers: `0`
* Function Count: `180`
* Namespace Canonicalized (DEV052)
* Execution Surfaces Consolidated (DEV050)
* Ingest Engine Anchored at DEV048
* Audit Log Schema Canonical (7-column)

No uncontrolled mutation paths exist.

Runtime impact since DEV050: **Additive only. No contract mutation.**

---

## 3. Core Architecture

### Ingest Engine (DEV048)

Inputs:

* `<prefix>_poc_ingest_queue`
* `<prefix>_keyword_defs`

Outputs:

* `<prefix>_incident_data`
* `<prefix>_discarded_events`

Invariants:

* Append-only writes
* No deletes
* No updates
* Included + Discarded = Candidates
* Refuse in PROD
* Refuse prefix mismatch
* RawPubDate stored verbatim (context-only)

Risk model remains PoC baseline (SIGNAL count, capped).

---

### Namespace Canonical Surface

Each prefix namespace must include:

```
<prefix>_poc_ingest_queue
<prefix>_incident_data
<prefix>_discarded_events
<prefix>_keyword_defs
<prefix>_audit_log
<prefix>_error_log
<prefix>_safety_zones
<prefix>_cop_calls_raw
```

Provisioner refuses if namespace already exists.

---

### Execution Surface Discipline

* Single canonical ToolRunner
* No active lowercase `csa_*` mutators
* Duplicate wrappers removed
* Installer isolated from history controls
* Trigger count governed at 0

---

## 4. Audit & Observability

Canonical `audit_log` schema:

```
id
timestamp
action
source
details
user
run_id
```

* Append-only
* No hash_id column
* No mutation of historical rows

`error_log` remains capture-only and append-only.

Logging must not alter execution flow.

---

## 5. Governance Model

CSA operates under formal governance discipline:

* Stable Checkpoints required
* Baseline recapture after structural drift
* Verifier PASS required before freeze
* No silent contract mutation
* No undocumented namespace changes

Authority order:

Stable Checkpoint > Active Contract > Decision Log > DEV Insert > Working Draft > Historical

---

## 6. DEV Workflow Rules

Every DEV thread must:

1. Declare starting state
2. Document structural mutations
3. Recapture baseline if inventory changes
4. Verify no drift
5. Produce binder-ready closeout
6. Freeze before new DEV thread begins

No overlapping mutation scopes.

---

## 7. Hygiene Discipline

Structural hygiene includes:

* Function inventory tracking
* Property inventory tracking
* Namespace completeness
* Trigger discipline
* Header SHA validation

Inventory drift must be:

* Intentional
* Documented
* Baseline-recapped

No unexplained deltas permitted at checkpoint.

---

## 8. What This Repo Contains

* All GAS source files
* ToolRunner surfaces
* Verifier utilities
* Governance documentation artifacts
* Stable checkpoint lineage

Backups are timestamped and preserved under `/backups/gas/YYYY/MM/DD/`.

---

## 9. What This Repo Does NOT Contain

* Production triggers
* Automated external schedulers
* Direct PROD mutation surfaces
* Silent schema changes
* UI mutation logic

CSA_v1 remains a controlled PoC engine.

---

## 10. Development Status

Latest stable checkpoint: **DEV055**

* Audit schema corrected
* Function inventory reconciled (180)
* Baseline recaptured
* Verifier PASS confirmed
* DEV055 frozen (historical/reporting only)

Next development must occur under a new DEV thread (e.g., DEV056) with a single primary charter.

---

## 11. Guiding Principle

Move slowly enough to never lose track.
Move clearly enough that future-you can trust past-you.

Deterministic. Governed. Append-only.

---

**CSA_v1.0 — Protected Baseline**
**CSA_v1.1 — Governed Evolution Layer**
