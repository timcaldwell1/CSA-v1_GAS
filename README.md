# CSA — GAS Source Backups
02-10-2026 10:30 AM MST
DEV046

```md
# CSA_v1 — Church Safety Alerts (PoC)

CSA_v1 is a **Google Apps Script–based Proof of Concept** for ingesting, processing, and visualizing incident data for church campus safety awareness.

This repository represents an **active PoC**, not a production system.  
Stability, auditability, and guardrails are prioritized over feature velocity.

---

## Project Status

- **Phase:** Proof of Concept (PoC)
- **Governance:** Decision-logged, binder-driven
- **Current Focus:** Hygiene, consolidation, and integrity verification
- **Production:** Not yet applicable

Stable checkpoints are declared periodically and treated as immutable reference states.

---

## Architecture (High Level)

CSA_v1 consists of:

- **Google Apps Script (GAS)** backend
- **Google Sheets** as the primary data store
- **WebApp UI** for incident visualization
- **Installer / Bootstrap tooling (OBI)** for PoC setup
- **DEV / QA tooling** for observability and validation

Key architectural principles:
- Append-only logging
- No silent PROD mutation
- Deterministic QA runs
- Explicit guards over implicit assumptions

---

## Environments & Data Isolation

### DEV / QA
DEV and QA share a spreadsheet but use **prefix-based isolation**.

| Environment | Prefix |
|------------|--------|
| DEV | `test3` |
| QA  | `test4` |

Example sheet names:
```

test3_incident_data
test4_error_log

```

### PROD
PROD **does not use an environment prefix**.

Production data is **campus-namespaced**, not environment-namespaced.

Example:
```

Laveen_incident_data
Mesa_incident_data

```

This asymmetry is intentional and required for multi-campus scaling.

---

## Canonical Tool Runner (Execution Airlock)

CSA_v1 uses a **single, canonical execution surface** for all editor-run tools.

This prevents tool sprawl, preserves safety boundaries, and ensures long-term maintainability.

### Canonical Runner

**File**
```

CSA_ToolRunner_DEV041.gs

````

This is the **only file** where tools may be exposed for execution via the Apps Script editor.

All other tools remain internal unless explicitly surfaced here.

---

### Governance Rules (Binding)

1. **All runnable tools MUST be surfaced via the canonical runner**
2. **One wrapper → one underlying tool**
   - No logic
   - No branching
   - No environment switching
3. **Safety intent MUST be encoded in the function name**
   - `RUN_PROD_` → PROD-safe, read-only
   - `RUN_DEV_`  → DEV-only
   - `RUN_QA_`   → QA-only
4. **Each wrapper MUST include a one-line comment**
   - What the tool does
   - Its safety boundary
5. **Installer logic (OBI) is explicitly excluded**
   - Installers are not operator tools

---

### Example

```js
// PROD-safe, read-only. Verifies runtime state against the stable checkpoint.
function RUN_PROD_VerifyStableCheckpoint() {
  CSA_DEV041_VerifyStableCheckpoint_();
}
````

```js
// DEV/QA only. Clears test logs below header. No PROD writes.
function RUN_DEV_ClearTestLogs() {
  csa_clear_test_logs_TEMP();
}
```

---

### Adding New Tools (Forward Process)

New tools follow a strict **airlock process**:

1. Tool logic may live anywhere and include its own guards.
2. A tool is only surfaced if it is expected to be used again.
3. Surfacing requires a compliant wrapper in the canonical runner.
4. No other execution surfaces are permitted.

This rule permanently closes execution sprawl (G-03).

---

## Installer (OBI)

The **One-Button Installer (OBI)** is:

* **PoC-critical**
* Responsible for initial setup required before `incident_data` processing
* **Not runtime logic**
* **Not subject to hygiene pruning**

OBI may be incomplete, but it is protected and retained until explicitly superseded.

---

## Error Logging

* Single canonical writer
* Append-only
* Never throws
* DEV / QA verified under controlled failure conditions
* PROD guarded

Error logging behavior is governed by decision logs and must not be altered casually.

---

## Hygiene & Consolidation

Hygiene focuses on:

* Eliminating duplicate tooling
* Preserving behavior
* Improving discoverability
* Retaining evidence and auditability

Hygiene **does not**:

* Refactor core logic
* Change runtime behavior
* Modify PROD semantics

---

## What This Repo Is Not

* ❌ A finished production system
* ❌ A generic template
* ❌ A playground for unguarded experimentation

This repository is intentionally conservative.

---

## Contribution Expectations

If you add or modify anything:

* Respect stable checkpoints
* Do not bypass the canonical runner
* Do not weaken guards
* Document intent clearly
* Prefer explicit decisions over silent changes

Future maintainers should be able to answer:

> “Why does this exist?”
> without guessing.

---

## License & Use

Internal PoC use.
No warranty.
No implied fitness for production use.

---

**CSA_v1 — Proof of Concept**
Governed. Guarded. Auditable.

```
```
