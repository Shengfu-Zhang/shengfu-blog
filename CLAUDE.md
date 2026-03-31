# CLAUDE.md
> Agent instructions for Shengfu Zhang's personal blog.
> Read this before doing anything. Use the slash commands below for each phase.

---

## Quick Reference

| Phase | Command | When |
|-------|---------|------|
| 1. Draft | `/draft` | Idea / notes / transcript → scaffold internal doc |
| 2. Review | `/review` | Need structured feedback on an internal draft or generated artifact |
| 3. Generate | `/generate` | Doc approved → create all publish-ready assets |
| 4. Publish | `/publish` | Reviewed → upload via browser/MCP |

**Git rule:** Always `git commit` locally. Never `git push` unless Shengfu explicitly says "push".

---

## Repo

- **Live site:** `https://shengfu-zhang.github.io`
- **Repo:** `https://github.com/Shengfu-Zhang/shengfu-zhang.github.io`
- **Local dev:** `python3 -m http.server 8000` (fetch() requires HTTP, not file://)

---

## Structure

```
shengfu-zhang.github.io/
├── CLAUDE.md
├── README.md
├── index.html                         ← Homepage (two-column card layout)
├── post.html                          ← Shared post renderer (?slug= param)
├── agent-sops/                        ← Agent SOPs (draft.sop.md, review.sop.md, generate.sop.md, publish.sop.md)
├── .claude/commands/                  ← Thin wrappers that load agent-sops/ (one line each)
├── internal/                          ← GITIGNORED. Raw source docs, local only.
│   └── YYYY-MM-DD-slug/               ← Full date for precision (e.g. 2026-04-15-agent-design)
│       ├── draft.md                      Main internal doc: ASCII art, 🖼️ placeholders, image queue
│       ├── notes.md                      Raw research, links, supplementary data, follow-up angles
│       ├── *.md                          Any additional supplement files
│       └── images/                       ALL generated images live here first, split by destination
│           ├── blog/                       Diagrams/charts for blog.md — copy only what blog.md references
│           └── xhs/                        XHS card screenshots — copy all to posts/images/
└── posts/
    ├── index.json                     ← Ordered slug list (newest first); index.html fetches each blog.json
    └── YYYY-MM-DD-slug/               ← Single post  (or YYYY-MM-DD-slug-part-1, -part-2 for series)
        ├── blog.json                  ← Post metadata: title, date (ISO), tags, summary
        ├── blog.md                    ← Long-form (blog). Part N ends with series callout to Part N+1.
        ├── linkedin.md
        ├── twitter.md
        ├── xiaohongshu.md             ← Chinese
        ├── xhs-cards.html             ← Source HTML for XHS carousel; reviewable + reproducible artifact
        └── images/
            ├── cover.png
            └── *.png                  ← Generated images replacing placeholders
```

---

## Phase 1 — Draft (`/draft`)

Read `agent-sops/draft.sop.md` and follow the instructions.

---

## Phase 2 — Review (`/review`)

Read `agent-sops/review.sop.md` and follow the instructions.

Use this any time you want structured feedback on:
- `internal/YYYY-MM-DD-slug/draft.md`
- `posts/YYYY-MM-DD-slug/blog.md`
- `posts/YYYY-MM-DD-slug/linkedin.md`
- `posts/YYYY-MM-DD-slug/twitter.md`
- `posts/YYYY-MM-DD-slug/xiaohongshu.md`

---

## Phase 3 — Generate (`/generate`)

Read `agent-sops/generate.sop.md` and follow the instructions.

---

## Phase 4 — Publish (`/publish`)

Read `agent-sops/publish.sop.md` and follow the instructions.

---

## Design Rules

- **Theme:** Warm parchment (`#f4f2ee`), dot-grid background, white cards, teal accent (`#1c6454`)
- **Fonts:** Syne 800 (headings), DM Sans (body), DM Mono (labels/meta/tags)
- **Layout:** Two-column sticky sidebar + scrollable post cards
- **Read time:** Always shown as filled teal pill at the top of every post, format: `N min read · X,XXX words`
- **Tags:** Filled pill, `rgba(28,100,84,.1)` bg, teal text
- **No frameworks, no build step.** CDNs: Google Fonts, marked.js, highlight.js, mermaid.js
- Never change design without being asked

---

## Author

- **Name:** Shengfu Zhang
- **Bio:** Engineer exploring how AI is reshaping the way we build software. Sharing what I'm learning along the way.
- **Topics:** AI agents, engineering systems, developer productivity, software quality
- **Platforms:** Blog · LinkedIn · X/Twitter · 小红书
- **Language:** English everywhere except 小红书 (Chinese)
- **Do not mention** current employer or job title explicitly

---

## Ad Hoc Tool Usage

These MCP plugins are available outside the main workflow — use them on request:

| Tool | When to use |
|------|-------------|
| `context7` | Look up current docs for any library, framework, or API (React, Next.js, GitHub API, etc.) |
| `chrome-devtools` | Browser automation — screenshot pages, fill forms, navigate, inspect console/network |

Examples:
- "Check the GitHub Pages API docs" → use `context7`
- "Take a screenshot of the live site" → use `chrome-devtools`
- "Screenshot each XHS card" → use `chrome-devtools` (already part of `/generate`)

---

## What NOT to Do

- Never `git push` without explicit approval
- Never publish to any platform without Shengfu doing final review
- Never create per-folder `blog.html` — use `post.html?slug=` instead
- Never put internal docs in `posts/` — `internal/` is gitignored for a reason
- Never invent data or metrics — use `[placeholder]` if real numbers aren't available
- Never change `index.html` or `post.html` design without being asked
- Never use relative paths (`../other-post/`) in `blog.md` links — `post.html` renders from the root, so inter-post links must use `post.html?slug=YYYY-MM-DD-slug`
