# Generate

## Overview

Generate all public-facing assets from an approved internal doc. Creates blog.md, all platform files, XHS card images, blog.json, and updates all indexes. Run only after Shengfu has reviewed and approved the internal draft.

## Parameters

- **slug** (required): The base slug of the internal doc to generate from (e.g. `2026-03-29-ai-velocity`). For series, this is the base slug — part suffixes are added automatically.

**Constraints for parameter acquisition:**
- If all required parameters are already provided, You MUST proceed to the Steps.
- If any required parameters are missing, You MUST ask for them before proceeding.
- When asking for parameters, You MUST request all parameters in a single prompt.
- When asking for parameters, You MUST use the exact parameter names as defined.

## Steps

### 1. Read Source and Detect Series

Read all files in `internal/YYYY-MM-DD-slug/` and determine whether this should become a single post or a series.

**Constraints:**
- You MUST read `draft.md` as the primary source.
- You MUST read `notes.md` and any other `*.md` files as supplementary context.
- You MUST identify the sharpest insight(s) before writing anything.
- You MUST preserve the main thesis if the draft is a full-picture doc with one clear central argument.

**If single arc:**
- You MUST identify the one sentence a reader would quote. Everything distills toward that.
- You MUST proceed to Step 2.

**If multiple arcs (series detection):**
- You MUST detect when the draft contains 2+ distinct publishable arcs — each with its own problem → insight → takeaway loop.
- You MUST NOT split a draft into a series only because it contains multiple mechanisms, implementation details, or open questions. If the draft still has one central argument and the extras are clearly follow-up depth, keep it as one post and use the draft's **Series candidates** as future-post inputs instead.
- You MUST propose the split before writing anything:
  > "This draft has 2 natural parts: **Part 1** — [spine]. **Part 2** — [spine]. I'll generate both as `slug-part-1` and `slug-part-2`. Each stands alone but Part 1 calls out Part 2 at the end. Proceed, or adjust the split?"
- You MUST wait for Shengfu's confirmation before writing anything.
- You MUST name parts: `YYYY-MM-DD-base-slug-part-1`, `YYYY-MM-DD-base-slug-part-2`, etc.
- You SHOULD generate all parts in one run unless Shengfu says otherwise.

**Series logical order — You MUST verify:**
- Parts follow a natural progression the reader would expect (Problem → Solution, Why/What → How, Concept/Framework → Implementation → Measurement).
- Part 1 is the most foundational — a reader who only reads Part 1 gets a complete mental model.
- If the parts could run in either order, the split point is wrong — reconsider before proceeding.

### 2. Generate blog.md

Write the long-form post for each slug (one per part if series).

**Constraints:**
- You MUST target 700–1,100 words per part.
- You MUST write in personal voice, first person, 2/3 technical.
- You MUST write for a public engineering audience — remove confidential or internal-only framing, but keep concrete lived observations and practitioner specificity when they are safe to share.
- You MUST NOT include a byline ("By Shengfu Zhang"), since post.html renders the author automatically and a duplicate byline would appear twice.
- You MUST distill — a long internal doc produces a focused blog, not a transcription.
- Each post MUST feel self-contained: complete insight loop (problem → insight → takeaway).
- You MUST prefer one memorable, quotable idea over exhaustive coverage because that is what compounds into a recognizable personal brand over time.
- If the topic genuinely needs more than the target word count to complete the insight loop, You MAY go longer, but You MUST keep the post tight and readable.

**Rich content — You MUST follow this priority order:**
1. **Mermaid (default for all diagrams):** use ` ```mermaid ``` ` blocks for flowcharts, sequence diagrams, timelines, graphs. Check draft ASCII art — it maps directly to mermaid syntax.
2. **Markdown tables:** include when they make a comparison or data set clearer.
3. **Inline PNG images:** use `![alt](images/filename.png)` only for content mermaid cannot handle (stat cards, styled charts, complex illustrations).

**You MUST NOT use a PNG where mermaid can do the job, because mermaid scales to any screen size, stays crisp, and needs no image file.**

**Hyperlinks in blog.md — You MUST follow:**
- You MUST NOT use relative paths (e.g. `../other-post/`) for any link in `blog.md`, because `post.html` renders from the site root and relative paths resolve relative to `post.html`, not the post folder.
- For links to other posts on this site: use `post.html?slug=YYYY-MM-DD-slug` format.
- For external links: use the full `https://` URL.
- For inline images: use `images/filename.png` (relative to the post folder — this is the one exception, as it is resolved by the renderer's base path logic).

**For series posts — blog.md structure:**

*Part 1:*
- You MUST open with a series map block (before the first paragraph):
  ```
  *This is a [N]-part series on [topic]. Part 1 covers [scope].
  Part 2 covers [scope]. [Part 3 covers ... if applicable.]*
  ```
- You MUST close with a series callout (after the final paragraph):
  ```
  ---
  *Part 2 of this series covers [specific topic]: [one sentence naming
  the concrete thing the reader will learn]. Coming next.*
  ```

*Middle parts:*
- You MUST open with a back-reference block:
  ```
  *Part [N] of [M]. [Part N-1](post.html?slug=YYYY-MM-DD-slug-part-N-1) covered [10-word recap]. This part covers [scope].*
  ```
- You MUST close with a series callout to Part N+1.

*Last part:*
- You MUST open with a back-reference block covering all previous parts in one sentence.
- You MUST NOT add a closing callout since this is the final part — there is no next part to tease.

**Rules for callouts — You MUST follow:**
- Be specific: "Part 2 covers the metrics layer" beats "Part 2 goes deeper."
- The tease names a concrete thing the reader will learn, not a theme.
- The back-reference is one sentence — enough to orient a new reader, not a full recap.
- The series map in Part 1 is the only place all parts are listed together.

### 3. Generate Platform Files

Write `linkedin.md`, `twitter.md`, and `xiaohongshu.md` for each slug.

**Constraints for linkedin.md:**
- You MUST write 150–250 words, plain text, professional narrative tone.
- You MUST structure as: hook (1–2 lines) → context → insights → takeaway.
- You MUST end with a question to drive comments.
- You MUST add `[Blog link in comments]` placeholder at the end — no link in the body.
- You MUST use spaced paragraphs with no markdown formatting.
- You MUST prefer useful professional insight, first-hand learning, and practical takeaways over motivational filler or broad self-promotion.
- You MUST NOT use hashtags unless Shengfu explicitly asks for them, because they are not part of this repo's standard LinkedIn format.
- For series: You MUST include "Part N of M:" in the hook. Part N (not last) MUST end with: "Part N+1 covers [topic] — [tease]."

**Constraints for twitter.md:**
- You MUST write 5–7 tweets, each ≤280 characters.
- You MUST number every tweet: `1/`, `2/`, etc. at the start.
- You MUST include a character count comment after each tweet: `<!-- 124 chars -->`.
- You MUST NOT include a link in tweet 1 because it hurts algorithmic reach.
- Tweet 1 MUST stand alone as a sharp insight — the hook.
- Tweets 1–3 MUST already carry the core value of the thread, because long threads are truncated in-feed and many readers will never reach the end.
- Tweets 2–N MUST each cover one key idea, short lines for mobile.
- Last tweet SHOULD include blog link placeholder `[link]`.
- You SHOULD avoid hashtags entirely. If used, You MUST keep them relevant and sparse, no more than 2 total in any tweet.
- For series: You MUST include "(Part N of M)" naturally in tweet 1. Last tweet of non-last parts MUST tease Part N+1.

**Constraints for xiaohongshu.md (caption only — not card content):**
- You MUST structure as:
  ```
  [2-3 line hook, casual Chinese, first person]

  [3-5 key takeaways as plain text lines]

  完整英文版见 GitHub Pages 👨‍💻
  shengfu-zhang.github.io

  #程序员 #AI #技术分享 [2-3 topic-specific tags, total 5–8 hashtags]
  ```
- You MUST use plain-text domain only — no `https://` — because 小红书 prohibits clickable external links.
- You MUST include 5–8 hashtags total.
- You MUST write natural, first-person Chinese rather than translated corporate prose.
- You MUST rewrite for native Chinese rhythm instead of translating line by line from the English post.
- You MUST NOT force awkward Chinese replacements for technical terms that are already natural in Chinese dev writing (`agent`, `prompt`, `lint`, `SOP`, and sometimes `harness`).
- You SHOULD translate technical terms into Chinese when the Chinese version is already more natural (`评审`, `返工`, `上线`, `老系统`, `构建校验`, etc.) and meaning is unchanged.
- When an English technical term is kept, You MUST make the surrounding sentence read like natural Chinese instead of mixed-language notes or direct translation.
- You MUST prefer natural Chinese phrasing such as `评审`, `返工`, `上线`, `隐性约束`, `构建校验` when meaning is unchanged.
- After drafting, You MUST do a fluency pass and rewrite any sentence that feels blunt, literal, or like translationese.
- You SHOULD place the most searchable topic words naturally near the top of the caption, because discovery on 小红书 is strongly keyword-driven.
- For series: You MUST note "第N篇，共M篇" in the hook.

### 4. Generate Blog Images

For each item in the Image Generation Queue in `draft.md`, decide how to render it.

**Constraints:**
- You MUST default to Mermaid for flowcharts, timelines, and sequence diagrams — write the block directly in `blog.md`, no image file needed.
- For stat cards, data charts, complex visuals that mermaid cannot handle:
  - You MUST build an HTML file in `tmp/` using the blog design system (bg `#f4f2ee`, accent `#1c6454`, DM Mono labels).
  - You MUST screenshot via MCP Chrome DevTools, using `emulate` to set exact pixel dimensions first.
  - You MUST save to `internal/YYYY-MM-DD-slug/images/blog/`.
  - You MUST copy to `posts/YYYY-MM-DD-slug/images/` only if `blog.md` references the file via `![...]`.
- For complex illustrations or visual metaphors only:
  - You MAY navigate to `https://chatgpt.com/images` via Chrome DevTools MCP.
  - You MUST prompt with exact palette (`#f4f2ee` bg, `#1c6454` teal), style (minimal, clean, flat), and dimensions.
  - You MUST save to `internal/YYYY-MM-DD-slug/images/blog/`, copy to `posts/` only if used in `blog.md`.
- You MUST NOT copy images to `posts/` that are not referenced in `blog.md`, because unused images bloat the public repo without adding value.

### 5. Generate 小红书 Cards

Build and screenshot the XHS image carousel for each slug.

**Constraints:**
- You MUST write `posts/YYYY-MM-DD-slug/xhs-cards.html` with 6–7 portrait cards at 1080×1440 ratio by default.
- You MAY use 8 cards only when the story genuinely needs the extra step and every card still carries one clean idea.
- You MUST NOT add a filler card just to reach a round number.
- You MUST use the warm palette (bg `#f4f2ee`, accent `#1c6454`), Noto Serif SC for Chinese headings, DM Sans for body, DM Mono for labels.
- Card structure MUST follow: Cover → Problem/Hook → Key Concept(s) → Insight/Quote → Actions → CTA.
- Each card MUST contain ONE idea only — title + 1 key point + optional short example. No dense paragraphs.
- The full carousel MUST tell a complete story arc — a reader who only sees the cards (no caption) still gets the full insight loop.
- All Chinese card copy MUST follow the same fluency rules as `xiaohongshu.md`: native-sounding Chinese first, English only when it is the clearest term, and short punchy wording over translated prose.
- `xhs-cards.html` is a publish artifact and the source of truth for XHS card copy. You MUST keep it in the post folder for review and reproducibility.
- For series: XHS cards for non-last parts MUST include a final card teasing Part N+1.
- Every card footer MUST show plain-text `shengfu-zhang.github.io` — no `/` suffix, not a link.
- You MUST open `posts/YYYY-MM-DD-slug/xhs-cards.html` via Chrome DevTools MCP, use `emulate` to set viewport to `1080x1440x1`, and screenshot each card by scrolling 1440px per card.
- You MUST save as `internal/YYYY-MM-DD-slug/images/xhs/xhs-card-01.png` through `xhs-card-N.png`.
- You MUST copy all XHS cards to `posts/YYYY-MM-DD-slug/images/` — always copy all, every card is used.

### 6. Create blog.json

Write the metadata file for each slug.

**Constraints:**
- You MUST create `posts/YYYY-MM-DD-slug/blog.json` with this shape:
  ```json
  {
    "title": "Post Title",
    "date": "YYYY-MM-DDT00:00:00Z",
    "tags": ["Tag1", "Tag2"],
    "summary": "One or two sentence summary for the index card."
  }
  ```
- You MUST use the exact publish date for `date`. Default time is `00:00:00Z`.
- For series parts published on the same day: You MUST use distinct times (Part 1 = `T00:00:00Z`, Part 2 = `T01:00:00Z`, etc.) so the date sort produces the correct reading order.

### 7. Update All Indexes

Update `posts/index.json` and `README.md` to include the new post(s).

**Constraints for posts/index.json:**
- You MUST add new slug(s) to the array. Array order does not matter — `index.html` sorts by `date` from `blog.json` descending at runtime.
- For ordering same-day posts (e.g. series parts), set distinct ISO timestamps in `blog.json` (e.g. Part 1 = `T00:00:00Z`, Part 2 = `T01:00:00Z`). The sort handles display order automatically.
- You MUST NOT edit `post.html` or `index.html`, since they are fully data-driven and changes there affect every post on the site.

**Constraints for README.md:**
- You MUST read the current Posts table before editing.
- You MUST prepend new row(s), newest first:
  `| [Date] | [Title](posts/YYYY-MM-DD-slug/blog.md) | [topics] |`
- For series: You MUST add one row per part, with titles like "Title — Part 1" and "— Part 2".
- You MUST NOT touch anything else in README.md, because other sections are manually maintained by Shengfu.

### 8. Commit

**Constraints:**
- You MUST run `git add` on all new/modified files and `git commit`.
- Commit message format: `post: YYYY-MM-DD-slug`.
- For series: `post: YYYY-MM-DD-base-slug (parts 1–N)`.
- You MUST NOT push because pushing publishes to the live site and requires Shengfu's explicit approval.

### 9. Print Summary and Hand Off

**Constraints:**
- You MUST print a generation summary in this format:
  ```
  ✓ posts/YYYY-MM-DD-slug/
    blog.md        — N words, M min read
    linkedin.md    — N words
    twitter.md     — N tweets (all ≤280 chars ✓)
    xiaohongshu.md — N hashtags, XHS cards: N images ✓
    blog.json      — "Title", YYYY-MM-DD, [tags]

  indexes updated:
    posts/index.json  ✓
    README.md         ✓
  ```
- For series: You MUST print one block per part.
 - You MUST end with: "Preview at `http://localhost:8000/post.html?slug=YYYY-MM-DD-slug` — run `/review` for feedback or `/publish` when ready."

## Examples

### Example 1: Single post
**Input:** `slug: 2026-04-15-agent-design`

**Expected behavior:** Reads `internal/2026-04-15-agent-design/draft.md`, detects single arc, generates all assets for one slug, prints summary.

### Example 2: Series
**Input:** `slug: 2026-05-01-example-series`

**Expected behavior:** Reads draft, detects 2 independent publishable arcs with separate problem → insight → takeaway loops, proposes split into `-part-1` and `-part-2`, waits for confirmation, generates all assets for both parts with series map/callouts, commits as one, prints two summary blocks.

## Troubleshooting

### Series parts feel interchangeable
If either part could logically come first, the split point is wrong. Reconsider the boundary — look for the natural "Part 1 must establish X before Part 2 can address Y" dependency.

### Image not rendering in post
Verify the `![alt](images/filename.png)` path in `blog.md` matches the actual filename in `posts/YYYY-MM-DD-slug/images/`. Only images referenced in `blog.md` are copied from `internal/images/blog/`.

### XHS card cut off in screenshot
Ensure `emulate` sets viewport to exactly `1080x1440x1` before screenshotting. Scroll exactly 1440px per card. If a card is taller than 1440px, reduce font size or split the card content.

### Twitter tweet over 280 chars
Rewrite the tweet — do not abbreviate with ellipsis. Break it into two tweets if the idea requires more space.
