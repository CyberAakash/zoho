---
title: Plan Upgrade/Downgrade Lifecycles
weight: 4
---

# Plan Upgrade/Downgrade Lifecycles

## Lifecycle 1: Free → Enterprise → Free

**Initial state:** All channels have UNKNOWN status (from dd-changes migration). License handler NOT invoked — the IM feature doesn't exist on Free.

---

### ⬆ UPGRADE to Enterprise (maxPerIST=50)
Trigger: `handleFeatureEnable(max=50)`

{{< steps >}}

### Resolve UNKNOWN in disabled depts

UNKNOWN → check SDK → active?
- Yes → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED
- No → **Desk DB**: DISABLED

### Resolve UNKNOWN in active depts (limit=50)

UNKNOWN → check SDK → active?
- Yes + room → **Desk DB**: ACTIVE _(SDK already active, no SDK call)_
- Yes + no room → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED
- No → **Desk DB**: DISABLED
- _(with 50 limit, almost all fit)_

### Re-enable LICENSE_DISABLED in active depts (up to 50)

LICENSE_DISABLED → activate in **IM SDK** → **Desk DB**: ACTIVE

{{< /steps >}}

{{< callout type="info" >}}**Result:** Channels come alive up to 50 per IST. All UNKNOWN resolved.{{< /callout >}}

---

### ⬇ DOWNGRADE to Free
Trigger: `handleFeatureDisable()`

{{< steps >}}

### LICENSE_DISABLE all active channels

ACTIVE → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED

### Resolve ALL UNKNOWN

→ 0 found _(all resolved during upgrade)_

{{< /steps >}}

{{< callout type="info" >}}**Result:** ZERO active. All LICENSE_DISABLED (or DISABLED from manual deactivation).{{< /callout >}}

---

## Lifecycle 2: Enterprise → Free → Enterprise

**Initial state:** Many channels ACTIVE across departments. May have UNKNOWN channels if customer was Enterprise pre-migration.

---

### ⬇ DOWNGRADE to Free
Trigger: `handleFeatureDisable()`

{{< steps >}}

### LICENSE_DISABLE all active channels

ACTIVE → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED

### Resolve ALL UNKNOWN

UNKNOWN → check SDK → active? deActivate → LICENSE_DISABLED. Not active? → DISABLED
_(0 found if a prior license/dept event already resolved them)_

{{< /steps >}}

{{< callout type="warning" >}}
**Why LICENSE_DISABLED (not DISABLED)?** The channel itself is fine — only the LICENSE prevents it. If they upgrade again, we restore them.
{{< /callout >}}

{{< callout type="info" >}}**Result:** 0 active. All LICENSE_DISABLED.{{< /callout >}}

---

### ⬆ UPGRADE back to Enterprise (max=50)
Trigger: `handleFeatureEnable(max=50)`

{{< steps >}}

### Resolve UNKNOWN in disabled depts → 0 found

_(downgrade resolved all)_

### Resolve UNKNOWN in active depts → 0 found

_(downgrade resolved all)_

### Re-enable LICENSE_DISABLED in active depts

LICENSE_DISABLED → activate in **IM SDK** → **Desk DB**: ACTIVE. All channels per IST come back!

{{< /steps >}}

{{< callout type="info" >}}
**Result:** Channels restored exactly as before.

**Key insight:** LICENSE_DISABLED preserves the channel for re-activation. DISABLED means "user chose to turn it off" — license handler doesn't touch it.
{{< /callout >}}

---

## Lifecycle 3: Standard → Enterprise → Standard

**Initial state:** Standard (maxPerIST=1) — 1 WhatsApp channel ACTIVE per IST. May have UNKNOWN channels from pre-migration.

---

### ⬆ UPGRADE to Enterprise (max=50)
Trigger: `handleFeatureLimitUpdate(max=50)`

{{< steps >}}

### Disable ACTIVE in disabled depts → LICENSE_DISABLED

### Resolve UNKNOWN in disabled depts

_(0 found if prior event resolved them, else resolves to LICENSE_DISABLED/DISABLED)_

### Build active count

### Resolve UNKNOWN in active depts

_(0 found if prior event resolved them, else resolves to ACTIVE/LICENSE_DISABLED/DISABLED)_

### Trim excess?

1 active, limit 50 → **no excess**

### Re-enable LICENSE_DISABLED in active depts

LICENSE_DISABLED → activate in **IM SDK** → **Desk DB**: ACTIVE

{{< /steps >}}

{{< callout type="info" >}}**Result:** User can now activate up to 50 channels per IST.{{< /callout >}}

---

### ⬇ DOWNGRADE to Standard (max=1)
Trigger: `handleFeatureLimitUpdate(max=1)`

_(User created 10 WA channels while on Enterprise — all ACTIVE)_

{{< steps >}}

### Disable ACTIVE in disabled depts

ACTIVE → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED

### Resolve UNKNOWN in disabled depts

_(0 found if prior event resolved them)_

### Build active count: 10 WA active in active depts

### Resolve UNKNOWN in active depts

_(0 found if prior event resolved them)_

### TRIM EXCESS

10 active, limit 1 → excess = 9. Disable from TAIL: ch10, ch9, ch8… ch2 → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED. **ch1 survives** (first created).

### Re-enable LICENSE_DISABLED?

0 room left → nothing restored

{{< /steps >}}

{{< callout type="info" >}}**Result:** 1 ACTIVE, 9 LICENSE_DISABLED (restorable on upgrade).{{< /callout >}}

---

## Lifecycle 4: Enterprise → Standard → Enterprise

**Initial state:** Enterprise (max=50) — 10 WhatsApp channels ACTIVE. May have UNKNOWN channels from pre-migration.

---

### ⬇ DOWNGRADE to Standard (max=1)
Trigger: `handleFeatureLimitUpdate(max=1)`

{{< steps >}}

### Disable ACTIVE in disabled depts

_(0 found if no disabled depts, else ACTIVE → LICENSE_DISABLED)_

### Resolve UNKNOWN in disabled depts

_(0 found if prior event resolved them)_

### Build active count: 10 WA active in active depts

### Resolve UNKNOWN in active depts

_(0 found if prior event resolved them)_

### TRIM EXCESS

Excess = 10 − 1 = 9. ch10→ch2 → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED. **ch1 stays ACTIVE**.

### Re-enable LICENSE_DISABLED?

0 room left → nothing restored

{{< /steps >}}

{{< callout type="info" >}}**Result:** 1 ACTIVE, 9 LICENSE_DISABLED.{{< /callout >}}

---

### ⬆ UPGRADE back to Enterprise (max=50)
Trigger: `handleFeatureLimitUpdate(max=50)`

{{< steps >}}

### Disable ACTIVE in disabled depts → 0 found

_(downgrade resolved all)_

### Resolve UNKNOWN in disabled depts → 0 found

_(downgrade resolved all)_

### Build active count: 1 WA active

### Resolve UNKNOWN in active depts → 0 found

_(downgrade resolved all)_

### Trim? 1 active, limit 50 → no excess

### Re-enable LICENSE_DISABLED in active depts

Room = 50 − 1 = 49. 9 LICENSE_DISABLED → activate in **IM SDK** → **Desk DB**: ACTIVE.

{{< /steps >}}

{{< callout type="info" >}}**Result:** All 10 channels active again. Nothing lost.{{< /callout >}}
