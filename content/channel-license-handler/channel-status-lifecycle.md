---
title: Channel Status Lifecycle
weight: 2
---

# Channel Status Lifecycle

## The 4 Statuses

{{< cards >}}
  {{< card title="ACTIVE (ID: 1)" subtitle="Channel is live and accepting messages." >}}
  {{< card title="DISABLED (ID: 0)" subtitle="Manually turned off, or department was disabled." >}}
  {{< card title="LICENSE_DISABLED (ID: 2)" subtitle="Turned off because license limit was hit or feature was disabled." >}}
  {{< card title="UNKNOWN (ID: 3)" subtitle="Status not yet determined — needs SDK reconciliation." >}}
{{< /cards >}}

## Two Types of Records

### Existing Customers — `UNKNOWN`

Before the STATUS column existed, channels had no status. `dd-changes` added the column with `DEFAULT = 3`, so all existing records got **UNKNOWN**. These channels _might_ be active in SDK, _might_ not. We don't know until we check (reconciliation).

### New Customers — `ACTIVE`

`CustomChannel.java` sets the field default: `channelStatus = ChannelStatus.ACTIVE`. New channels are created with status ACTIVE. No reconciliation needed — status is accurate from the start.

{{< callout type="warning" >}}
**Why UNKNOWN exists:** When the STATUS column was added, existing channels were already running in production. We couldn't blindly mark them ACTIVE or DISABLED — we don't know their real state. So `DEFAULT=3` (UNKNOWN) means: _"check with SDK before deciding."_

Every handler resolves UNKNOWN by:
1. Looking up the channel in SDK → is it active there?
2. If active in SDK → decide based on context (enable/disable/limit)
3. If not in SDK → mark DISABLED (it's dead)
{{< /callout >}}

{{< callout type="info" >}}
**Important:** UNKNOWN channels can exist in any department _before_ the first license or department event processes it. After that first event, all UNKNOWN in that department are resolved — and they never come back (no Java code creates UNKNOWN).

**UNKNOWN and LICENSE_DISABLED cannot coexist in the same department.** Every handler that creates LICENSE_DISABLED also resolves ALL UNKNOWN in the same invocation. So in any scenario, a department has either:
- UNKNOWN channels (not yet processed), OR
- LICENSE_DISABLED channels (already processed)
- Never both.
{{< /callout >}}

## Valid State Transitions

| From | To | Triggered By |
|---|---|---|
| UNKNOWN (3) | ACTIVE (1) | Reconcile: SDK active + room available |
| UNKNOWN (3) | DISABLED (0) | Reconcile: not in SDK (dead channel) |
| UNKNOWN (3) | LICENSE_DISABLED (2) | Reconcile: SDK active + no room, or feature off |
| ACTIVE (1) | DISABLED (0) | Manual deactivate, department disabled |
| ACTIVE (1) | LICENSE_DISABLED (2) | License limit exceeded, feature disabled |
| DISABLED (0) | ACTIVE (1) | Manual activate, department enabled |
| LICENSE_DISABLED (2) | ACTIVE (1) | License limit increased, feature enabled, manual activate |

{{< callout type="info" >}}
**Note:** DISABLED and LICENSE_DISABLED never transition to each other directly.
{{< /callout >}}
