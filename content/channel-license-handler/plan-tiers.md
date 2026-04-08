---
title: Plan Tiers & Limits
weight: 3
---

# Plan Tiers & Limits

## Channel Limits per IST

| Tier | Max Channels per IST |
|---|---|
| Free | 0 |
| Standard | 1 |
| Professional | 10 |
| Enterprise | 50 |

{{< callout type="info" >}}
**Per-IST means:** WhatsApp limit is independent of Telegram limit. Enterprise with 50 = up to 50 WhatsApp channels _AND_ up to 50 Telegram channels.
{{< /callout >}}

## What "IST" Means

IST = **Integration Service Type**. Each messaging platform (WhatsApp, Telegram, Instagram, etc.) is its own IST. License limits are enforced independently per IST.

## Free Plan Behaviour

When `maxPerIST = 0`:
- The `DepartmentEnableIMChannelHandler` exits immediately without restoring any channels
- No channels can be activated
- The feature effectively doesn't exist

## Plan Change Handlers

| Change | Handler method |
|---|---|
| Free → any paid plan | `handleFeatureEnable(max)` |
| Any paid plan → Free | `handleFeatureDisable()` |
| Paid plan → different paid plan | `handleFeatureLimitUpdate(max)` |
