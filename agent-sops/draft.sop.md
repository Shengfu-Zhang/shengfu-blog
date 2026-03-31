# Draft

## Overview

Scaffold a new internal doc or update an existing internal draft from any stage of source material — a vague idea, rough notes, a chat transcript, or a mostly-written piece. Nothing goes into `posts/` during this phase. The output is an internal `draft.md` for Shengfu to review before generation.

## Parameters

- **slug** (optional): Post slug in `YYYY-MM-DD-short-title` format. If not provided, ask for it during Scaffold.
- **target_draft** (optional): Path to an existing internal `draft.md` to update in place instead of creating a new folder.
- **source_material** (optional): Pasted notes, transcript, seed idea, or file path to source content.

**Constraints for parameter acquisition:**
- If all required parameters are already provided, You MUST proceed to the Steps.
- If any required parameters are missing, You MUST ask for them before proceeding.
- When asking for parameters, You MUST request all parameters in a single prompt.
- When asking for parameters, You MUST use the exact parameter names as defined.

## Steps

### 1. Detect Input Mode

Read what Shengfu provided and select the appropriate mode.

**Constraints:**
- You MUST select exactly one mode based on this table:

  | Input | Mode |
  |-------|------|
  | A topic or seed idea, little else | Interview |
  | Bullet points, rough notes, fragments | Cleanup |
  | Chat transcript or long brain dump | Extract |
  | Mostly written but unstructured | Writeup Review |

- If the input is ambiguous, You MUST ask: "Is this a seed idea, rough notes, or a fuller writeup?" before proceeding.
- You MUST NOT begin writing the draft until the mode is determined, because the wrong mode produces the wrong output structure.

### 2. Gather Content (mode-specific)

Collect and clarify the material based on the selected mode.

**Constraints (Interview mode):**
- You MUST ask questions in rounds — one group at a time, never all at once:
  - Round 1: "What's the thing you want to say? What made you want to write about this now?"
  - Round 2: "What do most people get wrong or miss about this? Is there a before/after, tradeoff, or surprising result?"
  - Round 3: "Do you have a concrete example, metric, or story that shows this? Who is this for?"
- You MUST synthesize back what you heard after each round and confirm before continuing.
- You MUST NOT proceed to Scaffold until all three rounds are complete, since all three together produce enough signal for a complete draft.

**Constraints (Cleanup mode):**
- You MUST read all provided material and identify the central argument.
- You SHOULD ask one clarifying question if the core insight is ambiguous — no more than one.
- You MUST expand bullets into full prose, preserving original voice and examples.
- You MUST NOT sanitize or genericize the content, because the original voice and concrete examples are the value of the draft.

**Constraints (Extract mode):**
- You MUST extract signal: core argument, supporting points, concrete examples, data or metrics.
- You MUST discard scaffolding: back-and-forth, restated questions, filler.
- You SHOULD NOT ask clarifying questions unless a core argument is genuinely missing.

**Constraints (Writeup Review mode):**
- You MUST read the full piece before commenting.
- You MUST identify: core argument, what is missing (evidence, concrete example, clearer takeaway), what is redundant.
- You MUST identify whether the piece has a clear narrative spine or is drifting across multiple competing frameworks, genres, or abstraction levels.
- You MUST identify and restate the intended genre before scaffolding or rewriting: personal blog thesis doc, leadership memo, implementation note, or another specific target.
- If the current piece reads like one genre but the requested target is another, You MUST say so explicitly and use the requested target as the rewrite anchor.
- You MUST flag gaps explicitly before scaffolding: "Missing: a concrete example in section 2."
- If the piece is intended as a thesis or full-picture doc, You MUST restate the proposed spine before scaffolding:
  `observation → where we are → what is wrong → why it matters → what to do → open questions`
- If the piece is intended as a thesis or full-picture doc, You MUST ensure the opening contains at least one concrete anchor: an observation, example, metric, or lived experience. If missing, You MUST flag it explicitly.
- If a section introduces a second major framework that is not required to explain the core argument, You MUST flag it as a candidate for `notes.md` or **Series candidates** rather than folding it into the main body.
- If Shengfu explicitly asks to rewrite or update the existing draft after review feedback, You MUST proceed after restating the new spine and major changes. You MUST NOT wait for another approval because the rewrite instruction is already explicit.
- You MUST NOT begin scaffolding until gaps are flagged and Shengfu has responded, since missing content cannot be recovered from the draft alone.

### 3. Scope Check

Assess whether the material contains one publishable arc or multiple.

**Constraints:**
- If the material contains one clear central argument, You MUST proceed to Scaffold.
- If the material contains 2+ distinct publishable arcs — each with its own problem → insight → takeaway loop — You MUST flag it:
  > "This draft has N natural arcs: [A] and [B]. I'd suggest splitting into separate drafts: `slug-a` and `slug-b`. Each will generate a tighter post. Want to split, or pick one to start?"
- You MUST wait for Shengfu's decision before proceeding.
- You MUST NOT force multiple arcs into a single draft, because each arc generates a separate post and splitting later loses the natural focus of each part.
- If the material is still one central argument but contains 2+ deep-dive mechanisms or secondary frameworks, You SHOULD keep one main thesis doc and move the extras into **Series candidates** instead of splitting immediately.

### 4. Scaffold Internal Doc

Create the internal folder and files.

**Constraints:**
- If `target_draft` is provided, You MUST update that draft in place and You MUST NOT create a new folder unless Shengfu explicitly asks for a split or new draft.
- If `target_draft` is not provided, You MUST ask for the slug if not provided. Format: `YYYY-MM-DD-short-title`. Use today's date.
- If `target_draft` is not provided, You MUST create `internal/YYYY-MM-DD-slug/` with two files: `draft.md` and `notes.md`.
- You MUST NOT create anything in `posts/` during this phase, because `posts/` is public and only holds approved, generated content.

**For `draft.md`, You MUST include:**
- Front matter: title, type, audience, date, `status: Draft`
- All content — expanded, structured, well-argued. No artificial length limit. Capture the idea fully.
- A clear narrative spine. For thesis / full-picture docs, default to:
  `observation → where we are → what is wrong → why it matters → what to do → open questions`
- A stable genre. The draft MUST read consistently as the intended format rather than shifting between personal blog, leadership memo, and implementation spec.
- At least one concrete anchor near the top for thesis / full-picture docs: a lived observation, example, metric, or explicit `[placeholder]` marking what is still needed.
- ASCII art for every diagram, flowchart, or chart — draw it out, do not describe it. Each preceded by: `<!-- 🖼️ IMAGE: filename.png — description -->`
- Tables for any comparison, structured option, or data set
- `[placeholder]` anywhere a real number or example is needed but missing
- An **Image Generation Queue** table at the bottom — one row per 🖼️ comment, target path: `internal/YYYY-MM-DD-slug/images/blog/filename.png`
- A **Series candidates** section at the bottom listing any distinct arcs that could stand alone as follow-up posts

**For `draft.md`, You SHOULD:**
- Use headers to separate major sections
- Include voice guidance inline where tone matters
- Keep one primary framework in the main body. If a second framework is useful but not essential, move it to `notes.md` or **Series candidates**
- Distinguish clearly between:
  - what the draft is asserting confidently now
  - what the draft is proposing as an open question, future mechanism, or follow-up post
- Prefer a thesis doc that maps the territory cleanly over a draft that tries to fully solve every sub-problem at once
- When revising an existing draft, preserve what is already working and rewrite around the strongest central argument instead of rebuilding from scratch by default

**For `notes.md`, You MUST include:**
- Raw research, links, supplementary data
- Potential follow-up angles not included in the draft

**You MUST NOT artificially compress content to hit a word count**, because a thorough internal doc is more useful than a compressed one.
**You MUST NOT omit ASCII art in favor of describing a diagram in prose**, since prose descriptions are harder to convert to mermaid/images during generation.

### 5. Handoff

Tell Shengfu the draft is ready.

**Constraints:**
- You MUST state: "Draft at `<draft-path>` (~N words) — review and say `/generate` when ready."
- You SHOULD include a one-sentence summary of the central argument.
- You MUST NOT proceed to generation without explicit approval, because generation creates public-facing assets that represent Shengfu's voice.

## Examples

### Example 1: Seed idea
**Input:** `source_material: "I want to write about how AI code review is broken"`

**Expected behavior:** Agent enters Interview mode, asks Round 1 questions, synthesizes, continues through Round 2 and 3, then scaffolds a draft.

### Example 2: Rough notes
**Input:** `source_material: "- AI writes code fast\n- review still slow\n- nobody talks about quality harness"`

**Expected behavior:** Agent enters Cleanup mode, identifies central argument, asks at most one clarifying question, expands into full prose draft.

### Example 3: Pre-written doc
**Input:** `source_material: /path/to/internal-doc.md`

**Expected behavior:** Agent enters Writeup Review mode, reads fully, flags gaps, waits for response, then scaffolds.

### Example 4: Update existing draft
**Input:** `target_draft: internal/2026-03-29-ai-velocity/draft.md`

**Expected behavior:** Agent updates the existing draft in place, preserves the strongest material, rewrites around one clear narrative spine, and does not create a new slug or folder.

## Troubleshooting

### Mode is unclear
If the input could fit multiple modes, default to asking: "Is this a seed idea, rough notes, or a fuller writeup?" Do not guess.

### Draft is getting very long
Length has no hard limit — capture the idea fully. If the draft naturally splits into 2+ arcs mid-writing, stop and flag it per the Scope Check step rather than continuing.

### Slug not provided
Ask for it during Scaffold, step 4. Do not invent a slug without asking.
