# Tau WHOOP refresh (Codex)

Codex version of the Tau refresh: pull WHOOP data, build and validate the dashboard, and save a dated report without hiding failures.

**Type:** Codex skill · **Area:** Health dashboard

## What it does

- Runs `tau pull` directly and captures its real exit status (never piped through `tail`, which once hid a crash).
- Builds and validates the dashboard; passes only if the documented blocks are the only differences and no checks were skipped.
- Saves reports only through `tau report-save`, which rejects malformed fields.

## Guardrails

- Never runs the WHOOP login itself; a timeout is not treated as an auth failure.
- Health readings are labelled as date-specific, not durable facts.

## When it triggers

> Refresh Tau WHOOP data, build and validate the dashboard, and save a dated report without masking failures or running authentication autonomously.

## What's in this repo

- `SKILL.md`: the skill itself, the file Claude or Codex reads.
- Not included: Tau itself (a local, single-file health dashboard) and all health data are private and not published.
- Written to be generic: where the skill relies on the user's own files, it names them by role, not by content.

## Install

```bash
mkdir -p ~/.codex/skills/tau-whoop-refresh
curl -fsSL https://raw.githubusercontent.com/adiyt4soni/tau-whoop-refresh/main/SKILL.md -o ~/.codex/skills/tau-whoop-refresh/SKILL.md
```
