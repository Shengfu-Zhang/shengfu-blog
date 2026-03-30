# CLAUDE.md
> Standing instructions for AI agents (Claude Code, etc.) working on this repo.
> Read this first before doing anything.

---

## What This Repo Is

Personal blog and content artifact store for **Shengfu Zhang**.
Live site: `https://shengfu-zhang.github.io/shengfu-blog/`
Hosted via: GitHub Pages (static HTML, no build step)

---

## Repo Structure

```
shengfu-blog/
├── CLAUDE.md                          ← You are here
├── README.md                          ← Human-facing repo description
├── index.html                         ← Personal site homepage
├── post.html                          ← Shared post template (reads ?slug= param)
├── theme-preview.html                 ← Dev only, not linked publicly
├── internal/                          ← Private drafts, not linked publicly
│   └── YYYY-MM-slug.md
└── posts/
    └── YYYY-MM-slug/
        ├── blog.md                    ← Long-form post (Substack version) ← source of truth
        ├── linkedin.md                ← LinkedIn post
        ├── twitter.md                 ← X/Twitter thread
        ├── xiaohongshu.md             ← 小红书 post (Chinese)
        └── images/
            ├── cover.png              ← Cover image (1200×630px)
            └── *.png                  ← Diagrams referenced in blog
```

**Note:** There is NO per-folder `blog.html`. The single `post.html` at root renders any post dynamically via `?slug=YYYY-MM-slug`.

---

## How to Create a New Post

### Step 1 — Get source material
Source is a Claude chat transcript, rough markdown, or bullet list of ideas.

### Step 2 — Create the post folder
```
posts/YYYY-MM-slug/
```
Kebab-case slug, 2–4 words. Example: `2026-04-agent-design-patterns`

### Step 3 — Draft all platform variants

| File | Format | Length | Tone |
|------|--------|--------|------|
| `blog.md` | Markdown | 600–900 words | Personal, 2/3 technical, first person |
| `linkedin.md` | Plain text | 150–250 words | Thought leadership, ends with question |
| `twitter.md` | Plain text | 5–7 tweets | Punchy, opinionated, hook in tweet 1 |
| `xiaohongshu.md` | Markdown | 300–450 words | Chinese, casual, relatable, emoji ok |

**Voice:** Practitioner-thinker. Not academic. Not hype. Grounded in real experience, systems-thinking lens. First person. Honest about uncertainty.

### Step 4 — Generate images
Save to `posts/YYYY-MM-slug/images/`:

| File | Size | Notes |
|------|------|-------|
| `cover.png` | 1200×630px | Warm parchment bg (#f6f3ec), teal (#1c6454), clean |
| Diagrams | 1000×600px | Match site aesthetic |

**Visual style:** bg `#f6f3ec`, accent `#1c6454` (teal), DM Mono for labels. Clean, minimal, warm.

### Step 5 — Register post in post.html
Add an entry to the `POSTS` object in `post.html`:
```js
'YYYY-MM-slug': {
  title: 'Post Title Here',
  date: 'Month YYYY',
  tags: ['Tag1', 'Tag2'],
},
```

### Step 6 — Add post row to index.html
Add above the `<!-- Add new posts above this line -->` comment:
```html
<a class="post-row" href="post.html?slug=YYYY-MM-slug">
  <span class="post-year">YYYY</span>
  <span class="post-title">Post Title Here</span>
  <div class="post-tags">
    <span class="tag">Tag1</span>
  </div>
  <span class="post-arrow">→</span>
</a>
```

### Step 7 — Commit and push
```bash
git add .
git commit -m "post: YYYY-MM-slug"
git push origin main
```

---

## How to Update an Existing Post

1. Edit `posts/YYYY-MM-slug/blog.md`
2. If title/date/tags changed, update the `POSTS` entry in `post.html`
3. Commit: `update: YYYY-MM-slug — [what changed]`

---

## Site Design

- **Theme:** Warm parchment, single teal accent, Igor Mahr-inspired minimal layout
- **Colors:** bg `#f6f3ec`, accent `#1c6454`, borders `rgba(28,100,84,.15)`
- **Fonts:** Syne 800 (outlined hero), DM Sans (body), DM Mono (labels/tags/meta)
- **Layout:** 3-column — thin sidebars with rotated text, center content max 720–780px
- **Nav style:** Pill-bordered links (border-radius: 100px)
- **Section labels:** `= Writing` style with DM Mono
- **No frameworks.** No build tools. Pure HTML + CSS + minimal JS.
- **CDNs allowed:** Google Fonts, marked.js, highlight.js, mermaid.js

---

## Author Context

**Name:** Shengfu Zhang
**Bio:** Engineer exploring how AI is reshaping the way we build software. Sharing what I'm learning along the way.
**Topics:** AI coding agents, engineering systems, developer productivity, multi-agent design, software quality
**Platforms:** Substack, LinkedIn, X/Twitter, 小红书
**Language:** English for all platforms except 小红书 (Chinese)
**Do not mention:** Current employer or job title explicitly

---

## What NOT to Do

- Don't create per-folder `blog.html` files — use `post.html?slug=` instead
- Don't change `index.html` or `post.html` design without being asked
- Don't publish to Substack/LinkedIn/X automatically — drafts only, human approves
- Don't put internal docs in `posts/` — use `internal/` instead
- Don't invent data or metrics — use `[X%]` placeholders if real numbers aren't provided
