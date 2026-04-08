---
title: Getting Started
weight: 1
---

# Getting Started

This is a personal knowledge base for Zoho Desk internals — documenting system behaviour, lifecycle flows, and implementation details for future reference.

## How to use this site

Browse docs using the left sidebar. Each section covers a specific feature or system component in depth.

## Available docs

{{< cards >}}
  {{< card link="/channel-license-handler" title="IM Channel License Handler" icon="cog" subtitle="Complete lifecycle guide for channel statuses, plan tier limits, upgrade/downgrade flows, and department events." >}}
{{< /cards >}}

## Adding a new doc

1. Create `content/{your-topic}/_index.md` with a `title` and `weight` in the front matter
2. Add a card in `content/_index.md` and here pointing to the new page
3. `git push` — GitHub Actions builds and deploys automatically
