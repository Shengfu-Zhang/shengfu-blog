# Review

## Overview

Review an internal draft or generated platform artifact and return structured feedback before Shengfu decides whether to revise, generate, or publish. This SOP is feedback-only by default: it evaluates the work against repo rules, platform-native best practices, and Shengfu's long-term personal brand goals.

The review lens is not "does this sound okay?" It is:
- does this build a recognizable body of thought over time
- does this sound like Shengfu rather than generic AI content
- does this fit the platform it will live on
- does this respect the repo's explicit guardrails and restrictions

## Parameters

- **slug** (required): Base slug or concrete post slug to review, for example `2026-03-29-ai-velocity` or `2026-03-29-ai-velocity-part-1`.
- **artifact_type** (optional, default: `auto`): What to review. Allowed values: `auto`, `draft`, `blog`, `linkedin`, `twitter`, `x`, `xiaohongshu`, `all`.

**Constraints for parameter acquisition:**
- If all required parameters are already provided, You MUST proceed to the Steps.
- If any required parameters are missing, You MUST ask for them before proceeding.
- When asking for parameters, You MUST request all parameters in a single prompt.
- When asking for parameters, You MUST use the exact parameter names as defined.

## Steps

### 1. Resolve the Review Target

Map `slug` and `artifact_type` to the exact file(s) that should be reviewed.

**Constraints:**
- If `artifact_type` is `x`, You MUST normalize it to `twitter`.
- If `artifact_type` is `draft`, You MUST review `internal/YYYY-MM-DD-slug/draft.md`.
- If `artifact_type` is one generated artifact (`blog`, `linkedin`, `twitter`, `xiaohongshu`), You MUST review the matching file in `posts/YYYY-MM-DD-slug/`.
- If `artifact_type` is `auto`, You MUST use this resolution order:
  - If `internal/YYYY-MM-DD-slug/draft.md` exists and no exact post folder exists, review the internal draft.
  - If `posts/YYYY-MM-DD-slug/` exists, review all generated artifacts for that concrete slug.
  - If no exact post folder exists but `posts/YYYY-MM-DD-slug-part-*` folders exist, review all matching generated artifacts across the series.
- If `artifact_type` is `all`, You MUST review all matching generated artifacts that exist for the resolved slug or series.
- If `artifact_type` targets generated files and only `slug-part-*` folders exist, You MUST review all matching parts unless Shengfu explicitly named a single part slug.
- You MUST surface exactly which files were reviewed.
- You MUST NOT edit files in review mode because this phase is for judgment and feedback, not silent rewriting.
- If the target file does not exist, You MUST stop and report which path is missing because partial review feedback is misleading.

### 2. Load Context and Choose the Right Lens

Read the relevant repo rules and establish the evaluation criteria before writing feedback.

**Constraints:**
- You MUST use `CLAUDE.md` as the source of truth for repo guardrails.
- If reviewing generated artifacts, You MUST also use `agent-sops/generate.sop.md` as the source of truth for artifact-specific format requirements.
- You MUST evaluate from the perspective of gradual personal brand building:
  - clear topic territory
  - authentic first-person voice
  - useful insight over vague inspiration
  - consistency over growth hacks
  - trust-building over short-term engagement bait
- You SHOULD use current platform best practices when the judgment depends on platform behavior or discovery mechanics, because platform norms change over time.
- You MUST distinguish repo rules from platform norms in the feedback so Shengfu can tell whether a suggestion is a hard workflow constraint or a softer optimization.

### 3. Review by Artifact Type

Apply the appropriate review criteria for each artifact.

**Constraints (draft):**
- You MUST review the draft primarily for story architecture and judgment, not sentence-level polish.
- You MUST check for:
  - a concrete opening observation, example, metric, or explicit `[placeholder]`
  - a clear spine: `observation → where we are → what is wrong → why it matters → what to do → open questions`
  - one main framework rather than multiple competing frameworks
  - a stable genre, especially for thesis docs: personal blog thesis doc vs leadership memo vs implementation note
  - a clear distinction between confident assertions, recommendations, and open questions
  - whether the piece should remain one full-picture doc or split into follow-up posts
- You SHOULD identify:
  - what is already working
  - what deserves more space
  - what should be trimmed
  - what belongs in **Series candidates** instead of the main body
- You MUST NOT optimize sentence polish before the argument is sound because structure is the leverage point at draft stage.

**Constraints (blog):**
- You MUST check that the post has one sharp insight a reader could quote back after reading.
- You MUST check that the post feels self-contained even when it is part of a series.
- You MUST verify public-audience fit:
  - no internal org framing
  - no confidential details
  - no mention of current employer or job title
  - no invented metrics or claims presented as facts
- You MUST verify repo format guardrails:
  - long-form markdown file exists at the correct path
  - first-person voice
  - no byline
  - inter-post links use `post.html?slug=...`
  - image paths use `images/...` when applicable
- You SHOULD judge whether the post sounds like a practitioner with lived experience rather than a generic explainer, because original observation is the main brand-building advantage of a personal blog.
- You SHOULD prefer clarity, memorable framing, and one complete insight loop over exhaustive coverage.

**Constraints (linkedin):**
- You MUST review for helpful professional value and constructive discussion because LinkedIn says it prioritizes content that adds value and contributes constructively to discussions with the broader professional community.
- You MUST reject:
  - promotional copy without added insight
  - off-topic filler
  - unoriginal repost-style writing
  - obvious engagement bait
- You MUST verify repo format guardrails:
  - 150–250 words
  - plain text only
  - spaced paragraphs
  - structure: hook → context → insight(s) → takeaway
  - meaningful question at the end
  - `[Blog link in comments]` placeholder at the end, with no body link
- You SHOULD prefer behind-the-scenes learning, real observations, and practical takeaways over generic motivation, because those are more aligned with long-term personal brand credibility on LinkedIn.
- You SHOULD flag questions that read like cheap bait ("Agree?") instead of real prompts for discussion.

**Constraints (twitter / x):**
- You MUST review for fast mobile readability and standalone value in tweet `1/`.
- You MUST verify repo format guardrails:
  - 5–7 tweets
  - each tweet starts with `N/`
  - each tweet is `<=280` characters
  - each tweet has a character-count comment
  - no link in tweet `1/`
- You MUST ensure tweets `1/` through `3/` already carry the core value, because X officially truncates threads with 4 or more posts in timeline view.
- You MUST keep hashtags relevant and sparse. If hashtags are used, You SHOULD enforce no more than 2 per tweet because X recommends no more than 2 hashtags as best practice.
- You MUST reject:
  - duplicate or near-duplicate tweets
  - link-only posting
  - repetitive self-promotion
  - excessive or unrelated hashtags
  - obvious engagement spam
- You SHOULD ensure each tweet adds one clear idea and the thread progresses rather than restating the same claim.

**Constraints (xiaohongshu):**
- You MUST review `xiaohongshu.md`.
- If `posts/YYYY-MM-DD-slug/xhs-cards.html` exists, You MUST also review it as the source of truth for XHS card copy and story flow.
- If `posts/YYYY-MM-DD-slug/images/xhs-card-*.png` exist, You SHOULD also verify that the caption and card story agree, because the cards carry most of the user attention on the platform.
- You MUST review for:
  - natural Chinese that sounds native and personal
  - genuine sharing rather than translated corporate copy
  - practical value, specific takeaways, and concrete scenarios
  - a hook that states the topic and value clearly
  - searchable topic words or phrases used naturally near the top
- If `xhs-cards.html` exists, You MUST also review:
  - card-to-card story progression
  - whether each card carries one idea cleanly
  - whether the cover promise matches the caption and final CTA
- You MUST verify repo format guardrails:
  - plain-text domain only, no `https://`
  - 5–8 hashtags total
  - hook + takeaways + plain-text blog mention + hashtags structure
- You SHOULD flag caption or card cover promises that are sensational, vague, or mismatched with the actual content because Xiaohongshu rewards saving and trust more than empty curiosity.
- You SHOULD prefer first-person, experience-based framing because credible Xiaohongshu content performs better when it feels like genuine sharing rather than top-down teaching.

### 4. Print Structured Feedback

Deliver the review in a format that Shengfu can act on quickly.

**Constraints:**
- You MUST lead with the highest-signal issues first, not minor copy edits.
- You MUST separate hard constraints from softer optimization suggestions.
- For each reviewed artifact, You MUST include:
  - `Decision:` `ready`, `revise`, or `blocked`
  - `What works:`
  - `Main gaps:`
  - `Suggested edits:`
- For `draft` and `blog`, You SHOULD also include:
  - `Worth expanding:`
  - `Worth trimming:`
  - `Spinout candidates:`
- If multiple parts or artifacts were reviewed, You MUST group feedback by file or part so it is easy to act on.
- You MUST NOT rewrite the artifact unless Shengfu explicitly asks for edits because the purpose of `/review` is evaluation and direction first.

## Examples

### Example 1: Review an internal thesis draft
**Input:** `slug: 2026-03-29-ai-velocity, artifact_type: draft`

**Expected behavior:** Review `internal/2026-03-29-ai-velocity/draft.md`, evaluate narrative spine, genre stability, strongest ideas, weak sections, and whether any material should spin out into future posts.

### Example 2: Review one LinkedIn post
**Input:** `slug: 2026-03-29-ai-velocity-part-1, artifact_type: linkedin`

**Expected behavior:** Review `posts/2026-03-29-ai-velocity-part-1/linkedin.md`, check hook, professional value, question quality, repo-specific formatting, and whether the post builds Shengfu's credibility without sounding promotional.

### Example 3: Review all generated assets for a part
**Input:** `slug: 2026-03-29-ai-velocity-part-2, artifact_type: all`

**Expected behavior:** Review `blog.md`, `linkedin.md`, `twitter.md`, and `xiaohongshu.md` for that part, then print grouped feedback by artifact.

### Example 4: Review a generated series from the base slug
**Input:** `slug: 2026-03-29-ai-velocity, artifact_type: blog`

**Expected behavior:** Detect `posts/2026-03-29-ai-velocity-part-1/` and `-part-2/`, review both `blog.md` files, and return grouped feedback by part.

## Troubleshooting

### Base slug resolves to both internal draft and generated posts
If `artifact_type` is `auto`, prefer the internal draft only when there is no exact generated post folder. If generated post folders exist, review the generated artifacts because that is the closer publishing surface.

### User wants feedback plus edits
Run `/review` first and give the structured judgment. If Shengfu then asks for changes, switch to editing mode and update the files directly.

### Target post is a series
If the user passed the base slug and asked for a generated artifact, review all matching parts unless Shengfu explicitly narrowed to one part.

### Xiaohongshu cards are missing
Review the caption anyway, but explicitly note that card-story alignment could not be checked because the card images are missing.
