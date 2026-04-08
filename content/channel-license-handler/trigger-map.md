---
title: Trigger Map
weight: 7
---

# Who Calls What — Trigger Map

## Handler Routing

| Trigger | Handler | Method |
|---|---|---|
| Plan change (upgrade/downgrade/enable/disable) | `IMChannelLicenseHandler` | `handleFeatureEnable` / `handleFeatureDisable` / `handleFeatureLimitUpdate` |
| Department disabled | `DepartmentDisableIMChannelHandler` | `disableHandler(MicrozEvent)` via Kafka async |
| Department enabled | `DepartmentEnableIMChannelHandler` | `enableChannels(deptId)` via sync call |
| User clicks Activate/Deactivate in UI | `DeskIMChannelAuthorizedAPIImpl` | `activateChannel` / `deActivateChannel` via REST |
| License usage query | `IMChannelLicenseHandler` | `getUsedCount()` → read-only |

## Execution Order

{{< callout type="info" >}}
**Department handler (`order=1`) runs FIRST. License handler (`order=5`) runs AFTER.**

_Why?_ License handler assumes departments are already enabled/disabled by the time it runs. It uses `getAllActiveDepartments()` to split channels into "active dept" vs "disabled dept" buckets.

If license ran first, it wouldn't have accurate department state.
{{< /callout >}}

## Kafka vs Sync

| Handler | Invocation | Why |
|---|---|---|
| `DepartmentDisableIMChannelHandler` | **Kafka async** (`departmentOffload.json`) | Dept disable can trigger many channels — offloaded to avoid blocking the API call |
| `DepartmentEnableIMChannelHandler` | **Sync** (called directly from `DepartmentAPIImpl.enable()`) | Enable needs to be synchronous so channels are live before the response returns |
| `IMChannelLicenseHandler` | **License framework** | Triggered by plan change events from the license service |
