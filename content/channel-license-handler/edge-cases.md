---
title: Edge Cases & Error Handling
weight: 10
---

# Edge Cases & Error Handling

## Complete Edge Case Table

| Scenario | Desk DB | IM SDK | Notes |
|---|---|---|---|
| SDK activate/deactivate throws | No change | No change | Skipped, continue with next channel |
| RuntimeException in license handler | No change | No change | Wrapped as `LicenseException(OPERATION_FAILURE)` |
| Empty department list | No change | No change | No-op (guard clause) |
| Empty channel list | No change | No change | No-op (guard clause) |
| UNKNOWN, not in SDK map | → DISABLED | No call | Dead channel, never existed in SDK |
| UNKNOWN, active in SDK, disabled dept | → LICENSE_DISABLED | deActivate | Deactivated in SDK, marked for license restoration |
| UNKNOWN, active in SDK, active dept, room | → ACTIVE | No call (already active) | Just resolves Desk DB status |
| UNKNOWN, active in SDK, active dept, no room | → LICENSE_DISABLED | deActivate | Over limit, shut down in SDK |
| maxPerIST = 0 (Free plan) | No change | No change | Dept enable exits early, no restoration |
| activateChannel on already ACTIVE | No change | activate (idempotent) | License check skipped |
| activateChannel on UNKNOWN + SDK active | → ACTIVE | activate (idempotent) | License check skipped (just resolving) |
| Manual deactivate | → DISABLED | deActivate + Redis cleanup | Always DISABLED, never LICENSE_DISABLED |
| LICENSE_DISABLED in disabled dept, then dept re-enabled | → ACTIVE (if room) | activate | **FIXED:** DeptEnableHandler now also queries LICENSE_DISABLED |
| LICENSE_DISABLED in disabled dept, then license upgrade | Stays LICENSE_DISABLED | No call | **Known gap:** license handler scopes to active depts only |
| LICENSE_DISABLED stranded, then plan changes while dept is ON | → ACTIVE | activate | Rescued by license handler (dept now active) |
| UNKNOWN + LICENSE_DISABLED in same dept | — | — | **Cannot happen:** every handler resolves all UNKNOWN before creating LICENSE_DISABLED |

## Error Handling Rules

{{< callout type="warning" >}}
**SDK call failures are soft:** If activating or deactivating a channel in the SDK throws, the handler logs the error and **continues to the next channel**. One failure does not abort the entire batch.
{{< /callout >}}

{{< callout type="error" >}}
**RuntimeException in license handler = hard failure:** Any unexpected exception during license handler execution is caught and re-thrown as `LicenseException(OPERATION_FAILURE)`, rolling back any DB changes made so far.
{{< /callout >}}

## The Known Gap

When a department is **disabled** and a **license upgrade** happens, LICENSE_DISABLED channels in that disabled department are **not restored**.

The license handler only processes active departments (`getAllActiveDepartments()`). Channels in disabled departments are invisible to it.

They are rescued when the department is later re-enabled — the dept enable handler (after the fix) fetches LICENSE_DISABLED channels and restores them if there is room.
