---
title: Complete Transitions Visual
weight: 11
---

# Complete Visual: Every Plan Transition

## Plan Tier Diagram

```mermaid
graph TD
    Free["FREE (max=0)"]
    Standard["STANDARD (max=1)"]
    Prof["PROFESSIONAL (max=10)"]
    Enterprise["ENTERPRISE (max=50)"]

    Free -->|"handleFeatureEnable(max=1)"| Standard
    Standard -->|"handleFeatureDisable()"| Free
    Standard -->|"handleFeatureLimitUpdate(max=10)"| Prof
    Prof -->|"handleFeatureLimitUpdate(max=1)"| Standard
    Prof -->|"handleFeatureLimitUpdate(max=50)"| Enterprise
    Enterprise -->|"handleFeatureLimitUpdate(max=10)"| Prof
    Enterprise -->|"handleFeatureDisable()"| Free
    Free -->|"handleFeatureEnable(max=50)"| Enterprise
```

## What Each Direction Does

### ⬆ On every UPGRADE

Triggered by `handleFeatureLimitUpdate(newMax)` or `handleFeatureEnable(max)` for Free→Paid:

1. Resolve UNKNOWN in disabled depts → LICENSE_DISABLED or DISABLED
2. Resolve UNKNOWN in active depts → ACTIVE / LICENSE_DISABLED / DISABLED
3. Trim excess ACTIVE if new limit < current count
4. **Re-enable LICENSE_DISABLED in active depts** (up to new limit)

### ⬇ On every DOWNGRADE

Triggered by `handleFeatureLimitUpdate(newMax)` or `handleFeatureDisable()` for Paid→Free:

1. LICENSE_DISABLE ACTIVE channels in disabled depts
2. Resolve UNKNOWN in disabled depts
3. Count current ACTIVE in active depts
4. Resolve UNKNOWN in active depts
5. **Trim excess ACTIVE** → LICENSE_DISABLED (TAIL first)
6. Re-enable LICENSE_DISABLED in active depts up to new limit (usually 0 room left)

## Channel Status Flow

```mermaid
graph LR
    UNKNOWN -->|"SDK active + room"| ACTIVE
    UNKNOWN -->|"SDK active + no room"| LICENSE_DISABLED
    UNKNOWN -->|"not in SDK"| DISABLED
    ACTIVE -->|"limit exceeded / feature off"| LICENSE_DISABLED
    ACTIVE -->|"manual deactivate / dept disable"| DISABLED
    LICENSE_DISABLED -->|"limit increased / feature on / manual activate"| ACTIVE
    DISABLED -->|"manual activate / dept enable"| ACTIVE
```
