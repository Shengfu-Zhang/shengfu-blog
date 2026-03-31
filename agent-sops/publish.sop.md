# Publish

## Overview

Upload post content to platforms using browser MCP tools. Always stops at draft/preview — never hits the final publish button. Shengfu does the final review and publishes manually.

## Parameters

- **slug** (required): The post slug to publish (e.g. `2026-03-29-ai-velocity-part-1`).
- **platforms** (optional, default: all): Comma-separated platforms to publish to: `linkedin`, `twitter`, `xiaohongshu`. Defaults to all three.

**Constraints for parameter acquisition:**
- If all required parameters are already provided, You MUST proceed to the Steps.
- If any required parameters are missing, You MUST ask for them before proceeding.
- When asking for parameters, You MUST request all parameters in a single prompt.
- When asking for parameters, You MUST use the exact parameter names as defined.

## Steps

### 1. Read Post Files

Load the content that will be pasted into each platform.

**Constraints:**
- You MUST read `posts/YYYY-MM-DD-slug/linkedin.md`, `twitter.md`, and `xiaohongshu.md` before opening any browser.
- You MUST confirm all files exist before proceeding.
- You MUST NOT proceed if any required file is missing, since pasting incomplete content into a platform composer wastes Shengfu's review time — report which file is missing instead.

### 2. Publish to LinkedIn

**Constraints (only if `linkedin` is in platforms):**
- You MUST navigate to `https://www.linkedin.com/feed/`.
- You MUST click "Start a post" to open the composer.
- You MUST paste the full content from `linkedin.md` into the composer.
- You MUST stop before clicking Post.
- You MUST tell Shengfu: "LinkedIn post ready in composer — review and post when ready."
- You MUST NOT click the Post button because Shengfu does the final publish.

### 3. Publish to X / Twitter

**Constraints (only if `twitter` is in platforms):**
- You MUST navigate to `https://x.com/compose/tweet`.
- You MUST paste the first tweet (tweet `1/`) from `twitter.md` into the composer.
- You MUST stop before posting or adding thread tweets.
- You MUST tell Shengfu: "First tweet in composer — add remaining tweets manually and post when ready."
- You MUST NOT post the tweet or build the thread because Shengfu does the final publish.

### 4. Publish to 小红书

**Constraints (only if `xiaohongshu` is in platforms):**
- You MUST navigate to `https://creator.xiaohongshu.com/publish/publish`.
- You MUST upload card images in order: `posts/YYYY-MM-DD-slug/images/xhs-card-01.png` through `xhs-card-N.png`.
- You MUST paste the full caption from `xiaohongshu.md` into the text field.
- You MUST stop before clicking 发布.
- You MUST tell Shengfu: "小红书 draft ready — images uploaded, caption filled — review and 发布 when ready."
- You MUST NOT click 发布 because Shengfu does the final publish.

### 5. Handoff

**Constraints:**
- You MUST summarize which platforms have drafts ready.
- You MUST NOT run `git push` unless Shengfu explicitly says to push, because pushing publishes to the live site.

## Examples

### Example 1: All platforms
**Input:** `slug: 2026-03-29-ai-velocity-part-1`

**Expected behavior:** Opens LinkedIn composer and pastes content, opens Twitter composer with tweet 1, uploads XHS cards and pastes caption. Stops at each before publishing. Reports all three ready.

### Example 2: Single platform
**Input:** `slug: 2026-03-29-ai-velocity-part-1, platforms: linkedin`

**Expected behavior:** Opens LinkedIn composer only, pastes content, stops before posting. Reports LinkedIn ready.

## Troubleshooting

### Platform composer doesn't open
Verify Shengfu is logged in to the platform in the browser. Use Chrome DevTools MCP `list_pages` to check current session state.

### XHS image upload fails
Confirm image files exist at `posts/YYYY-MM-DD-slug/images/xhs-card-01.png`. If missing, run `/generate` for this slug first.

### Content file missing
If `linkedin.md`, `twitter.md`, or `xiaohongshu.md` is missing, run `/generate` for this slug before publishing.
