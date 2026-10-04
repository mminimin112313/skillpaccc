---
name: opus55-creative-video-reference
displayName: Opus 5.5 Creative Video Reference
version: 1.0.0
description: Use the yihui-dev/awesome-opus5-5-videos + Skillry corpus as a reference system for designing code-generated motion graphics, explainers, 3D scenes, and interactive games without blindly copying prompts.
when_to_use:
  - The user wants an Opus 5.5 / Claude Code motion graphic, explainer, 3D scene, launch video, or interactive game.
  - The user asks for prompt inspiration or implementation references from the 475-case Opus 5.5 corpus.
  - The user wants to improve an AI-generated video by comparing it with successful code-generated examples.
argument-hint: "<goal> [duration] [aspect ratio] [assets/codebase]"
allowed-tools:
  - GitHub
  - web
---

# Opus 5.5 Creative Video Reference

## Purpose

Use the public Opus 5.5 creative-coding corpus as an evidence-backed reference library, not as a prompt-copying dump.

Primary sources:

- GitHub corpus: https://github.com/yihui-dev/awesome-opus5-5-videos
- Structured index: https://github.com/yihui-dev/awesome-opus5-5-videos/blob/main/data/videos.json
- Prompt files: https://github.com/yihui-dev/awesome-opus5-5-videos/tree/main/prompts
- Visual browse / live remakes: https://skillry.dev/ai-videos/opus-5-5
- Productized execution reference: https://kitcut.ai/

The GitHub repository records 475 cases. The README shows only 100 highlights. Skillry exposes the full category counts: Motion graphics 288, Explainers 62, 3D scenes 55, Games 70.

## Mental model

These are primarily **code-generated videos**.

Typical flow:

`idea / prompt → Claude writes rendering code → browser renders → frames or screen are captured → video`

Common rendering layers include HTML, Canvas, SVG, CSS, Three.js, GLSL, GSAP, Remotion, and related tools.

Do not describe this as ordinary text-to-video unless the specific case actually uses a video model.

## Workflow

### 1. Classify the target

Choose the dominant route before searching:

- **Motion graphics** — kinetic type, product launch, UI sequences, brand motion
- **Explainer** — diagrams, concepts, timelines, educational visualizations
- **3D scene** — camera movement, spatial models, product/architecture scenes
- **Game / interactive** — playable Canvas/Three.js scenes, physics, input-driven demos

If a target spans categories, pick one primary route and one secondary route.

### 2. Find 3–7 analogs

Start with Skillry for visual browsing. Prefer examples that match at least two of:

- visual grammar
- duration / pacing
- aspect ratio
- content type
- rendering stack
- asset constraints
- interaction model

Then open the matching GitHub prompt files and original source posts.

Do not select examples merely because they are popular.

### 3. Extract transferable variables

For each selected example, record only the reusable structure:

- purpose and audience
- duration and aspect ratio
- scene count / cut cadence
- typography behavior
- camera or spatial behavior
- color / contrast rules
- animation rhythm
- rendering technology
- whether real product assets are used
- whether audio is present and how it is synchronized
- whether the source used HyperFrames, Remotion, Three.js, external image/video/audio models, or other helpers
- validation method: still frames, contact sheets, deterministic capture, browser tests, etc.

Separate **creator-stated facts** from your inference.

### 4. Do not copy a prompt wholesale

Use public prompts to infer structure, then compile a new prompt for the user's project.

A good output prompt should replace generic praise such as “make it amazing” with concrete constraints:

- visual system
- temporal structure
- assets
- scene transitions
- camera rules
- typography rules
- output format
- technical stack
- validation criteria

Do not reproduce long third-party prompts verbatim unless the user explicitly asks and doing so is permitted. Prefer summarizing the pattern and writing a new prompt.

### 5. Pick the rendering stack deliberately

Default routing:

- **Canvas 2D** — pixel art, particle fields, custom 2D drawing, single-file portability
- **SVG** — diagrams, typography, vector motion, crisp UI graphics
- **CSS / DOM** — UI-centric motion with real HTML structure
- **Three.js / WebGL** — cameras, 3D spaces, depth, lighting, shader-driven effects
- **GLSL** — procedural visual effects and fragment-level control
- **GSAP / Motion** — timeline control and complex tween orchestration
- **Remotion** — frame-accurate React video composition, audio, captions, deterministic rendering

Use the lowest-complexity stack that can express the intended result.

### 6. Make the result editable

Prefer outputs whose important parameters remain explicit:

- timeline constants
- text copy
- palette tokens
- camera paths
- scene durations
- easing curves
- asset paths
- audio timing

For prototypes, prefer a single self-contained HTML when feasible. For production, allow modular source files when that materially improves maintainability.

### 7. Validate visually before final render

Do not judge a long animation only by watching one full render.

Use some combination of:

- sampled stills
- contact sheet across the whole timeline
- keyframe snapshots
- browser tests
- deterministic repeated frame capture when reproducibility matters
- performance checks for dropped frames or GPU overload

Check for:

- unreadable text
- awkward camera motion
- dead time
- repetitive AI-looking transitions
- layout collisions
- off-brand colors or typography
- unnecessary effects that obscure the message

### 8. Report provenance and uncertainty

When presenting a derived concept, name the reference cases or sources used and explain why they were selected.

Never claim:

- “Opus alone did this” when the source used HyperFrames, Remotion, external models, TTS, or manual edits
- “zero-shot” unless the source explicitly supports that claim
- that the 475-case corpus is a controlled benchmark

It is a heterogeneous reference corpus, not an apples-to-apples model evaluation.

## Corpus structure to remember

GitHub repository:

- 475 total entries in `prompts/` and `data/videos.json`
- README highlights 100 examples only
- highlighted README distribution: 58 Motion graphics / 16 Explainers / 14 3D scenes / 12 Games & interactive
- creator original posts are linked from entries
- some prompt files contain only partial prompt material when that is all the creator shared

Skillry full collection:

- 288 Motion graphics
- 62 Explainers
- 55 3D scenes
- 70 Games
- total 475

## Rights and reuse

The repository code is MIT-licensed, but the README credits videos and prompts to their original creators.

Treat third-party creative work as reference material. Keep source links and attribution. Build a new prompt and implementation rather than republishing someone else's full work as your own.

## Definition of done

A run is complete when it provides:

1. target category and technical route
2. 3–7 relevant reference cases or a justified smaller set
3. extracted design / motion / implementation variables
4. a newly compiled prompt or implementation plan
5. an editable rendering stack recommendation
6. visual validation criteria
7. source/provenance notes and uncertainty where needed

A raw list of links is FAIL.
A copied prompt with no adaptation is FAIL.
A claim that all 475 examples are controlled Opus-only tests is FAIL.