---
title: DISABLED vs LICENSE_DISABLED
weight: 8
---

# DISABLED vs LICENSE_DISABLED — The Key Distinction

## Comparison

| | DISABLED (0) | LICENSE_DISABLED (2) |
|---|---|---|
| Set by | Manual deactivate, Dept disable | License limit exceeded, Feature disabled |
| Meaning | User/admin chose to turn this off | Channel is fine, license prevents it |
| License handler | Does **NOT** touch these | **WILL** restore on limit increase |
| Dept enable handler | **WILL** try to restore | Does **NOT** touch these _(before fix)_ |
| Manual activate | WILL work (with license check) | WILL work (with license check) |

## Why This Distinction Exists

{{< callout type="info" >}}
**The restoration problem:**

Enterprise(50) → Standard(1): 49 channels become LICENSE_DISABLED.
Standard(1) → Enterprise(50): Those 49 _auto-restore_ to ACTIVE.

If we used DISABLED instead of LICENSE_DISABLED, the license handler couldn't tell the difference between:
- Channels the user explicitly turned off (should stay off on upgrade)
- Channels the license system turned off (should be restored on upgrade)

LICENSE_DISABLED = _"the system turned this off, not the user."_
{{< /callout >}}

## Who Sets Each Status

### DISABLED is set by:
- `deActivateChannel` (manual REST call — user clicked "Deactivate")
- `DepartmentDisableIMChannelHandler` (dept turned off)

### LICENSE_DISABLED is set by:
- `IMChannelLicenseHandler` — when limit is hit during `handleFeatureLimitUpdate`
- `IMChannelLicenseHandler` — during `handleFeatureDisable` (all active channels)
- Any handler resolving UNKNOWN when there's no room (`handleFeatureEnable`, `DepartmentEnableIMChannelHandler`)

## The Invariant

{{< callout type="warning" >}}
**UNKNOWN and LICENSE_DISABLED never coexist in the same department.**

Every handler that creates LICENSE_DISABLED also resolves ALL UNKNOWN in the same invocation. This invariant is enforced by the order of operations within each handler.
{{< /callout >}}
