Below is a **stronger GitHub `README.md`** designed for long-term maintainability. It explains the system clearly for future engineers while preserving your governance model and DEV discipline.

You can paste this directly into the repository.

---

# CSA_v1 — Church Safety Alerts

**Google Apps Script Runtime + Ingestion Pipeline**

CSA_v1 is a structured incident-intelligence pipeline designed to help churches and nonprofit organizations **collect, normalize, and analyze safety-related events**.

The runtime operates inside **Google Apps Script**, while this repository preserves **source snapshots and checkpoint history** for recovery, auditing, and development continuity.

---

# Repository Purpose

This repository serves three primary roles:

1. **Source preservation**
   GAS runtime snapshots captured from the production Apps Script project.

2. **Operational audit trail**
   Every stable checkpoint produces a repository snapshot.

3. **Disaster recovery**
   Full restoration of the runtime project is possible from repository snapshots.

The **authoritative runtime environment** remains Google Apps Script.

---

# High-Level Architecture

CSA_v1 intentionally separates the system into three layers:

```
Operator Surface
   │
   ▼
ToolRunner (DEV041)
   │
   ▼
Validation / Observation Layer
   DEV067
   │
   ▼
Deterministic Ingest Engine
   DEV048
```

This separation ensures:

* operational safety
* deterministic ingest behavior
* verifiable runtime state

---

# Core Components

## ToolRunner (DEV041)

The operator control surface.

ToolRunner exposes safe entrypoints for running system functions.

Example commands:

```
RUN_DEV057_Backup_Then_Push_To_GitHub
RUN_DEV067_Validate_Lane
RUN_DEV067_Show_Queue_Freeze_State
```

ToolRunner intentionally changes **very rarely** to maintain runtime stability.

---

## DEV048 — Deterministic Ingest Engine

The ingest harness processes queued incidents and writes normalized records.

Key characteristics:

* deterministic execution
* append-only data writes
* raw evidence preservation
* reproducible processing

The ingest engine **never modifies raw source records**.

---

## DEV067 — Validation & Observation Layer

DEV067 provides operator-side validation tools.

It verifies that the runtime environment and data pipeline remain structurally correct before ingestion occurs.

Capabilities include:

* environment validation
* prefix discipline checks
* queue schema verification
* queue freeze state inspection
* lane integrity verification

DEV067 performs **inspection only** and does not modify ingest behavior.

---

# Data Pipeline

The system processes incident records through three stages.

```
Raw Incident Feed
        │
        ▼
Raw Evidence Sheet
(test5_cop_calls_raw)
        │
        ▼
Normalized Queue
(test5_poc_ingest_queue)
        │
        ▼
Ingest Engine
(DEV048)
        │
        ▼
Append-Only Incident Storage
```

---

# Prefix-Based Environment Model

CSA_v1 isolates environments using sheet prefixes.

Example DEV configuration:

```
CSA_ENV           = DEV
CSA_SHEET_PREFIX  = test5_
CSA_INGEST_PREFIX = test5_
CSA_WEBAPP_PREFIX = test3_
```

Example sheet names:

```
test5_cop_calls_raw
test5_poc_ingest_queue
test5_process_log
```

This allows multiple runtime lanes without cross-contamination.

---

# Queue Schema (Canonical Contract)

Normalized ingest queue fields:

```
incident_id
incident_type
description
location
RawPubDate
```

Queue behavior:

* initially formula-backed
* frozen before ingestion
* append-only ingestion

---

# Raw Source Structure (Example)

Typical police call data fields:

```
INCIDENT_NUM
CALL_RECEIVED
FINAL_CALL_TYPE
HUNDREDBLOCKADDR
GRID
```

Raw sheets preserve the original data without transformation.

---

# Process Log

The system maintains an operator audit log.

Example columns:

```
timestamp_mst
run_id
prefix
sheet_role
stage
action_family
action_name
status
validation_result
freeze_state
source_name
raw_row_ref
queue_row_ref
incident_id
summary
details
operator_note
```

This provides traceability for every operator action.

---

# Execution Discipline

CSA_v1 enforces strict runtime discipline.

Current system posture:

```
Triggers: 0
Script Properties: ~28
Functions: ~239
```

Execution occurs **only through explicit ToolRunner commands**.

No scheduled triggers are used.

This prevents uncontrolled ingestion activity.

---

# Backup & Snapshot System

GAS source backups are captured via:

```
RUN_DEV057_Backup_Then_Push_To_GitHub
```

Backup process:

```
Apps Script Runtime
      │
      ▼
Local Drive Snapshot
      │
      ▼
GitHub Repository
```

Example snapshot path:

```
backups/gas/YYYY/MM/DD/CSA_GAS_BACKUP_TIMESTAMP
```

Each snapshot includes:

* full GAS source
* verifier baseline
* runtime configuration

---

# Stable Checkpoints

Development threads periodically declare **Stable Checkpoints**.

Checkpoint procedure:

1. Capture runtime baseline via DEV041 verifier
2. Create GAS source backup
3. Push snapshot to GitHub
4. Freeze development thread

Example evidence:

```
[DEV041][BASELINE][CAPTURED]
Stored in Script Properties:
CSA_DEV041_STABLE_CHECKPOINT_BASELINE_JSON
```

---

# Security

GitHub access uses a **fine-grained personal access token**.

Required permissions:

```
Repository Contents: Read + Write
Repository Metadata: Read
```

Token stored in Apps Script Script Properties:

```
CSA_GITHUB_TOKEN
```

Tokens must be rotated periodically.

---

# Current System Status

Latest checkpoint state:

```
DEV067 validation layer operational
Queue schema verified
Prefix discipline verified
Queue state: FORMULA
Lane validation: PASS
```

Runtime metrics:

```
Functions: 239
Triggers: 0
Properties: 28
```

System operating normally.

---

# Design Philosophy

CSA_v1 follows several engineering principles:

* deterministic ingest
* append-only data preservation
* operator-visible validation
* minimal runtime mutation
* disciplined checkpoint governance

These constraints help maintain long-term system reliability.

---

# Repository Structure

```
/backups
   /gas
      /YYYY
         /MM
            /DD
               CSA_GAS_BACKUP_TIMESTAMP
```

Each directory contains a complete GAS source snapshot.

---

# License

Internal project repository.

All rights reserved.

---

If you'd like, I can also produce **one more thing that dramatically improves this repo**:

A **visual architecture README with diagrams** showing:

* pipeline data flow
* runtime layers
* ingest lifecycle
* lane validation flow


2026-03-14 16:02 MST | ~64% remaining
