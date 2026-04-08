---
title: Manual Channel Operations
weight: 6
---

# Manual Channel Operations

## Manual Activate

REST API → `activateChannel`. Triggered when user clicks "Activate" on a channel in the UI.

{{< steps >}}

### Lookup channel by externalId

### Check license

- Already ACTIVE? → **Skip check**
- UNKNOWN + active in IM SDK? → **Skip check** _(just resolving status)_
- DISABLED / LICENSE_DISABLED → Check license limit → Throws `DeskLicenseException` if over limit

### Activate in IM SDK

### Desk DB → ACTIVE

{{< /steps >}}

**Result:** IM SDK: active, Desk DB: ACTIVE

---

## Manual Deactivate

REST API → `deActivateChannel`. Triggered when user clicks "Deactivate" on a channel in the UI.

{{< steps >}}

### Deactivate in IM SDK

### Redis cleanup

Delete `channelId + ONLINE_AGENTS`

### Desk DB → DISABLED

Always DISABLED, never LICENSE_DISABLED.

{{< /steps >}}

**Result:** IM SDK: inactive, Desk DB: DISABLED, Redis: cache cleared

---

## Lean Activate/Deactivate (Internal)

Used internally by license and dept handlers. Handlers call these, then update Desk DB themselves via `channelQueryAPI.updateChannelStatus()`.

| Operation | What it does | What it does NOT do |
|---|---|---|
| `activate(channelId)` | IM SDK only | No license check. No Desk DB update. |
| `deActivate(channelId)` | IM SDK only | No Desk DB. No Redis cleanup. |

{{< callout type="info" >}}
**Why separate lean operations?** Each caller sets a _different_ Desk DB status after the same SDK call:

| Caller | IM SDK call | Desk DB status | Redis cleanup? |
|---|---|---|---|
| License handler | deActivate | LICENSE_DISABLED (2) | No |
| Dept disable handler | deActivate | DISABLED (0) | No |
| Manual deactivate | deActivate | DISABLED (0) | Yes |
| License handler | activate | ACTIVE (1) | No |
| Dept enable handler | activate | ACTIVE (1) | No |
| Manual activate | activate | ACTIVE (1) | No |
{{< /callout >}}
