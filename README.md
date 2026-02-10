# CSA — GAS Source Backups
02-10-2026 10:30 AM MST
DEV046

This repository contains **manual, point-in-time snapshots** of the Google Apps Script (GAS)
source code for the CSA (Church Safety Alerts) project.

## What this is
- Immutable snapshots of the live GAS project
- Created intentionally by an operator
- Stored for audit, recovery, and historical reference

## What this is NOT
- Not CI/CD
- Not automated syncing
- Not a deployment mechanism
- Not “latest only”

## How snapshots are created
1. An on-demand GAS script pulls the live project source via the Apps Script REST API
2. A timestamped snapshot folder is written to Google Drive
3. That snapshot is manually pushed to this repo as a single commit

## Snapshot structure
/backups/gas/YYYY/MM/DD/
CSA_GAS_BACKUP_YYYYMMDD_HHMMSS/


Each folder represents a complete, immutable snapshot at that moment in time.

## Commit semantics
- One snapshot = one commit
- Commits are append-only
- Older snapshots are never overwritten

## Why this exists
To preserve a clean, human-auditable history of CSA’s Apps Script source
independent of the Google editor, CLASP, or deployment state.

When in doubt: snapshots > cleverness.

