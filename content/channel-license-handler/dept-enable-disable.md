---
title: Department Lifecycles
weight: 5
---

# Department Enable/Disable Lifecycles

## Lifecycle 5a: Dept ON → Disable → Enable (UNKNOWN channels)

This happens when the department event is the **first event** to process these channels after migration. No license event has run yet for this org.

**Initial state:** Dept 1 is ON. Has 3 WA channels, all UNKNOWN (from dd-changes migration).

---

### ⬇ DISABLE Department 1
Trigger: Kafka async → `DepartmentDisableIMChannelHandler`
Fetches: **ACTIVE + UNKNOWN**

{{< steps >}}

### ACTIVE channels: none found (all are UNKNOWN)

### Resolve UNKNOWN channels

- ch1 (UNKNOWN) → check SDK → active? deActivate in **IM SDK** → **Desk DB**: DISABLED
- ch2 (UNKNOWN) → same → **Desk DB**: DISABLED
- ch3 (UNKNOWN) → same → **Desk DB**: DISABLED

{{< /steps >}}

{{< callout type="warning" >}}
**Note:** Dept handler always sets DISABLED (not LICENSE_DISABLED). All UNKNOWN resolved.
{{< /callout >}}

{{< callout type="info" >}}**Result:** 0 ACTIVE, 3 DISABLED in dept 1.{{< /callout >}}

---

### ⬆ ENABLE Department 1
Trigger: Sync call → `DepartmentEnableIMChannelHandler`
Fetches: **DISABLED + UNKNOWN**

{{< steps >}}

### Check license limit

maxPerIST = 0? → Exit (Free plan). Otherwise proceed.

### Restore DISABLED channels

Group by IST. `room = maxPerIST − currentActiveCount` (across ALL depts)
- ch1 (DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE. Room decremented.
- ch2 (DISABLED) → room left? Yes → activate in **IM SDK** → **Desk DB**: ACTIVE
- ch3 (DISABLED) → room left? No → stays DISABLED

### No UNKNOWN to resolve

_(all resolved during disable)_

{{< /steps >}}

{{< callout type="info" >}}**Result:** Channels come back (within license limit).{{< /callout >}}

---

## Lifecycle 5b: Dept ON → Disable → Enable (LICENSE_DISABLED channels)

This happens when a license downgrade already ran, creating LICENSE_DISABLED channels in this department, then the dept gets disabled.

**Initial state:** Dept 1 is ON, Enterprise plan (max=50). Has 5 WA channels: 4 ACTIVE + 1 LICENSE_DISABLED (set by a previous license downgrade).

---

### ⬇ DISABLE Department 1
Trigger: Kafka async → `DepartmentDisableIMChannelHandler`
Fetches: **ACTIVE + UNKNOWN only** — LICENSE_DISABLED is invisible.

{{< steps >}}

### ACTIVE channels

ch1-ch4 (ACTIVE) → deActivate in **IM SDK** → **Desk DB**: DISABLED

### Resolve UNKNOWN → 0 found

_(all resolved by prior license event)_

{{< /steps >}}

{{< callout type="warning" >}}
**Note:** LICENSE_DISABLED channel (ch5) is NOT fetched. NOT processed. Stays LICENSE_DISABLED. Dept handler is blind to LICENSE_DISABLED.
{{< /callout >}}

{{< callout type="info" >}}**Result:** 4 DISABLED + 1 LICENSE_DISABLED in dept 1.{{< /callout >}}

---

### ⬆ ENABLE Department 1
Trigger: Sync call → `DepartmentEnableIMChannelHandler`
Fetches: **DISABLED + UNKNOWN + LICENSE_DISABLED** _(after the fix)_

{{< steps >}}

### Check license limit: maxPerIST = 50 → proceed

### Restore DISABLED channels (room = 50 − activeCount)

ch1-ch4 (DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE

### Resolve UNKNOWN → 0 found

### Restore LICENSE_DISABLED channels (FIXED)

**FIXED:** DeptEnableHandler now also fetches LICENSE_DISABLED and restores if room available.
ch5 (LICENSE_DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE

{{< /steps >}}

{{< callout type="info" >}}**Result:** All 5 channels ACTIVE.{{< /callout >}}

---

## Lifecycle 6a: Dept OFF → Enable → Disable (UNKNOWN channels)

Department was disabled BEFORE migration. dd-changes set all channels to UNKNOWN. Now the dept gets enabled for the first time.

**Initial state:** Dept 2 is OFF. Has 3 WA channels, all UNKNOWN. No handler has ever processed these.

---

### ⬆ ENABLE Department 2
Trigger: Sync call → `DepartmentEnableIMChannelHandler`
Fetches: **DISABLED + UNKNOWN**
maxPerIST=5, currently 3 WA active (in other depts). Room = 5 − 3 = 2.

{{< steps >}}

### Check license limit: maxPerIST = 5 → proceed

### No DISABLED channels to restore

### Resolve UNKNOWN channels (room=2)

- ch1 (UNKNOWN) → SDK active? Yes + room → **Desk DB**: ACTIVE. Room left: 1
- ch2 (UNKNOWN) → SDK active? Yes + room → **Desk DB**: ACTIVE. Room left: 0
- ch3 (UNKNOWN) → SDK active? Yes + **no room** → deActivate in **IM SDK** → **Desk DB**: LICENSE_DISABLED
  _(or if not in SDK → Desk DB: DISABLED)_

{{< /steps >}}

{{< callout type="info" >}}**Result:** 2 ACTIVE (ch1, ch2) + 1 LICENSE_DISABLED (ch3). All UNKNOWN resolved.{{< /callout >}}

---

### ⬇ DISABLE Department 2
Trigger: Kafka async → `DepartmentDisableIMChannelHandler`
Fetches: **ACTIVE + UNKNOWN only** — LICENSE_DISABLED is invisible.

{{< steps >}}

### ACTIVE channels

ch1, ch2 (ACTIVE) → deActivate in **IM SDK** → **Desk DB**: DISABLED

### Resolve UNKNOWN → 0 found

_(all resolved during enable)_

{{< /steps >}}

{{< callout type="warning" >}}
**Note:** LICENSE_DISABLED channel (ch3) is NOT fetched. NOT processed. Stays LICENSE_DISABLED.
{{< /callout >}}

{{< callout type="info" >}}**Result:** 2 DISABLED + 1 LICENSE_DISABLED in dept 2.{{< /callout >}}

---

## Lifecycle 6b: Dept OFF → Enable → Disable (LICENSE_DISABLED channels)

A license downgrade created LICENSE_DISABLED channels while the dept was ON, then the dept was later disabled.

**Initial state:** Dept 2 is OFF.
- ch1, ch2, ch3 → DISABLED (set by dept disable handler)
- ch4 → LICENSE_DISABLED (set by license downgrade before dept was disabled)

---

### ⬆ ENABLE Department 2
Trigger: Sync call → `DepartmentEnableIMChannelHandler`
Fetches: **DISABLED + UNKNOWN + LICENSE_DISABLED** _(after fix)_
maxPerIST=5, currently 2 WA active. Room = 3.

{{< steps >}}

### Check license limit: maxPerIST = 5 → proceed

### Restore DISABLED channels (room=3)

- ch1 (DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE. Room left: 2
- ch2 (DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE. Room left: 1
- ch3 (DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE. Room left: 0

### No UNKNOWN to resolve

### Restore LICENSE_DISABLED channels (FIXED)

ch4 (LICENSE_DISABLED) → activate in **IM SDK** → **Desk DB**: ACTIVE

{{< /steps >}}

{{< callout type="info" >}}**Result:** All 4 channels ACTIVE.{{< /callout >}}

---

### ⬇ DISABLE Department 2
Trigger: Kafka async → `DepartmentDisableIMChannelHandler`

{{< steps >}}

### ACTIVE channels

ch1, ch2, ch3 (ACTIVE) → deActivate in **IM SDK** → **Desk DB**: DISABLED

### Resolve UNKNOWN → 0 found

_(all resolved by prior license event)_

{{< /steps >}}

{{< callout type="warning" >}}
**Note:** LICENSE_DISABLED channel (ch4) is NOT fetched. NOT processed. Stays LICENSE_DISABLED (still stranded).
{{< /callout >}}

{{< callout type="info" >}}**Result:** 3 DISABLED + 1 LICENSE_DISABLED (ch4 still stranded) in dept 2.{{< /callout >}}
