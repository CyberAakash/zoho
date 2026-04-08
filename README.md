# Zoho Desk Docs

Personal knowledge base documenting internal system behaviour, lifecycle flows, and implementation details for Zoho Desk — for future reference.

**Live site:** https://CyberAakash.github.io/zoho/

Built with [Hugo](https://gohugo.io/) + [Hextra](https://imfing.github.io/hextra/) theme.

## Docs

| Document | Description |
|---|---|
| [IM Channel License Handler](content/channel-license-handler/_index.md) | Complete lifecycle guide — channel statuses, plan tier limits, upgrade/downgrade flows, department enable/disable, and edge cases |

## Local Development

```bash
# Prerequisites: brew install hugo go
hugo server --buildDrafts --disableFastRender
# Preview at http://localhost:1313/zoho/
```

## Adding a New Doc

1. Create `content/{your-topic}/_index.md` with front matter (`title`, `description`)
2. Add a `{{< card >}}` entry in `content/_index.md`
3. `git add . && git commit -m "Add {topic} doc" && git push` — GitHub Actions builds and deploys automatically
