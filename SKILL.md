---
name: tau-whoop-refresh
description: Refresh Tau WHOOP data, build and validate the dashboard, and save a dated report without masking failures or running authentication autonomously.
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Tau WHOOP refresh

## When to use

Use for the scheduled Tau refresh or a requested Tau health-dashboard update in the Tau project folder. Do not use to authenticate WHOOP, edit report files directly, or interpret current health data beyond the requested report.

## Inputs / context to gather

1. Confirm the working directory and requested scope: refresh only, build/validate, or dated report save.
2. Read current scheduler constraints: stop on unexpected errors, do not run `whoop-auth` autonomously, and use only `tau report-save` for writes.

## Procedure

1. Run `./bin/tau pull` directly; capture full output and its real exit status. Do not pipe it through `tail`.
2. On success, confirm `wrote 6 files to .../tau/data/raw`, then run `./bin/tau build` and `./bin/tau build --validate`.
3. Treat validation as passing only if `coach.strainBins`, `coach.counter`, and `anomalies.metrics` are the only documented differences and there are `0 skipped` checks. Do not require an old fixed total: totals can evolve.
4. If asked for analysis, inspect `tau today`, reminders, experiments, and `store/brief.md`. For a large brief, search and read chunks.
5. Save only through `tau report-save`. Use `better`, `worse`, `same`, or `new` for every `changes[].direction`; preserve the dated saved-report path.

## Efficiency plan

- Run pull/build/validate once in sequence and retain their concise status lines.
- Read large briefs selectively; avoid a whole-file read that exceeds limits.
- Stop on unexpected errors. A timeout may be retried only when requested.

## Pitfalls and fixes

- `socket.timeout: The read operation timed out` -> it is not necessarily auth failure. Do not run `whoop-auth`; suggest it only for login/401/403/Cloudflare errors.
- Pipeline reports success after Tau crashes -> do not pipe the primary command through `tail`; capture the direct command status.
- `changes[n].direction must be one of better/worse/same/new` -> correct that field exactly and rerun `tau report-save`.

## Verification checklist

- Pull, build, and validation all have direct success output.
- Only documented replacement blocks differ and validation has no skipped checks.
- Any write used `tau report-save`, with valid direction enums.
- Current health measurements/reminders are labelled date-specific, not durable facts.
