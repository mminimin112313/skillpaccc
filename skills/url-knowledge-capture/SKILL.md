---
name: url-knowledge-capture
displayName: URL Knowledge Capture
version: 2.0.0
description: Compile a user-provided URL into a persistent, source-grounded personal wiki. Preserve source evidence, triage against existing knowledge, update or create wiki pages, preserve outbound resources, maintain Zettelkasten relations with explicit reasons, generate Recall Cards, and lint the knowledge graph.
when_to_use:
  - The user provides a URL and asks to process, archive, research, summarize, save, or put it in Notion.
  - The user says “이거 정리”, “이거 프로세스”, “노션에 넣어”, “큐도”, or equivalent while referring to a web source.
  - The user asks what resources, links, or projects they previously supplied and expects relation-aware retrieval.
  - The user wants a Karpathy-style LLM-maintained wiki rather than one-off summaries.
argument-hint: "<url> [optional pasted body/context]"
allowed-tools:
  - web
  - Notion
  - GitHub
---

# URL Knowledge Capture 2.0

## Design principle

This skill follows the LLM Wiki pattern described by Andrej Karpathy:

- **Source layer** — immutable evidence and provenance.
- **Wiki layer** — mutable, LLM-maintained synthesis.
- **Schema layer** — the rules in this skill.

Primary design reference:
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

The goal is not to create one summary per URL. The goal is to make every accepted source improve the existing knowledge base.

## Notion mapping

Use the existing database `학습 큐 · Recall Cards` as the global index.

### Record types

- `유형 = 원문` — source-specific record; preserves provenance and a readable source article.
- `유형 = 리소스` — atomic tool/project/model/paper/site/dataset/entity.
- `유형 = 위키` — compiled topic/concept/comparison/overview page that can be rewritten as evidence accumulates.
- `유형 = 안내` — operating instructions.
- child page `큐` — recall cards owned by the source or wiki page.

### Ingest disposition

Use `수집 판정`:

- `New` — introduces a genuinely new concept/resource/page.
- `Update` — materially improves one or more existing pages.
- `Disputed` — conflicts with existing knowledge and requires both claims to remain visible.
- `No material` — source is preserved but adds no meaningful knowledge.

A source may be both `New` and `Update`.

## Non-negotiable architecture

### 1. Source layer is evidence, not synthesis

Always preserve:

- exact user-provided URL
- author/publisher when verifiable
- publication date when verifiable
- user-pasted original text when supplied
- directly contained outbound URLs
- source claim wording when it matters historically
- verification state

Never silently rewrite history to match current facts.

If the source says “300+ prompts” and the current site says “104 templates,” preserve both as separate statements.

### 2. Wiki layer is compiled knowledge

Before writing a new conceptual page, search the existing wiki.

Then decide:

- same thesis → update existing page
- new concept → create a new `위키` page
- contradiction → update affected page with a disputed/conflict section
- no new information → preserve source and stop compilation

One ingest may update many existing pages. Do not limit the cascade to the source-specific page.

### 3. Schema layer is this skill

The human should not need to remember filing rules. The skill owns:

- source preservation
- triage
- page routing
- metadata
- relation semantics
- cue generation
- lint rules

## Ingest workflow

### STEP 1 — Capture

Fetch the source.

If inaccessible:

- preserve the exact URL
- store any user-pasted body verbatim
- mark `검증 상태 = 미확인`
- do not invent body, links, claims, or cues
- create a pending Source Note only

If the source becomes available later, update the same Source Note instead of creating a duplicate.

### STEP 2 — Extract source graph

Extract directly meaningful URLs from the supplied source.

Default traversal depth is one hop.

For each outbound URL:

- preserve original URL
- identify canonical/current URL if different
- classify platform separately from function
- determine whether it deserves an atomic Resource Note
- preserve redirects, rename history, archival state, or current unavailability

Do not recursively explode a large corpus. If a linked repository contains hundreds of children, capture the repository/index/data path first and fan out only when useful.

### STEP 3 — Triage against existing wiki

Before compilation, search existing Notion pages using:

- exact project/entity names
- aliases
- synonyms
- functional category
- related technology
- existing source URL

Assign disposition:

`New | Update | Disputed | No material`

State the disposition internally before editing.

### STEP 4 — Additional research

Research current, quantitative, comparative, or operational claims.

Source preference:

1. official project/product docs
2. primary repositories/papers
3. first-party release notes
4. high-quality secondary sources
5. community reports only for community experience

Keep four evidence classes separate:

- **Source fact** — stated in the supplied source
- **Current verified fact** — confirmed from current authoritative evidence
- **Inference** — synthesis
- **Unverified** — unresolved

### STEP 5 — Compile, do not append blindly

For each durable idea, decide its home.

- Update existing concept/overview pages when the new source changes their understanding.
- Create a new `위키` page only when no existing page has the same core thesis.
- Create/update atomic `리소스` notes for independently reusable objects.
- Keep the source-specific article readable, but do not force every durable insight to live only there.

The test:

> If this source disappeared tomorrow, would the durable idea still exist in the right wiki page?

If no, compilation is incomplete.

### STEP 6 — Write the source article

The source record must still contain a natural article because the user wants a readable posting for each URL.

Use this flow:

`direct thesis → problem/cause → mechanism/structure → trade-off/result → application`

Requirements:

- substantial articles have a table of contents
- first paragraph answers what matters
- stable terminology
- facts, measurements, inference, and uncertainty remain distinct
- no generic meta sentences such as “this connection is important”
- no mechanical listicle structure when prose explains better
- natural Korean editorial rhythm when writing in Korean

### STEP 7 — Create atomic Resource Notes

Create one Resource Note per reusable:

- tool
- project
- library
- model
- paper
- dataset
- site
- product
- protocol

Recommended fields:

- `유형 = 리소스`
- `질문 / 제목`
- `원문 URL`
- `세트`
- `태그`
- `카테고리`
- `검증 상태`
- `수집 판정`
- `출처 포스팅`
- `관련 노트`
- `연결 이유`

### STEP 8 — Build Zettelkasten edges

Do not connect notes because they share a tag.

Create a relation only when it captures reasoning.

Allowed relation semantics include:

- Source
- Uses
- Feeds into
- Alternative to
- Complements
- Implements
- Reference for
- Built on
- Same problem, different layer
- Inspired by / Derived from
- Updates / Supersedes
- Disputes

Every relation needs a sentence explaining why.

Weak:
`둘 다 UI 도구이기 때문`

Strong:
`Amicro는 Motion 기반 component source를 제공하고 transitions.dev는 motion selection/audit/refine 규칙을 agent skill로 제공하므로 asset layer → operating-knowledge layer 관계다.`

### STEP 9 — Cascade-update existing wiki pages

This is the main 2.0 change.

After creating the source record, search for existing pages that should become better because of this source.

Update:

- concept pages
- comparison pages
- overview/resource maps
- related atomic notes
- relation reasons
- historical/current fact distinctions

Do not rewrite old source history. Add a clearly labeled follow-up update when a newer source changes a compiled overview.

### STEP 10 — Create Recall Cards

Create exactly one child page named `큐`.

Format:

```text
### Qn. <focused question>

**힌트:** <minimal retrieval cue>

<details>
<summary>정답보기</summary>
    <concise complete answer>
</details>
```

Prioritize:

- structure
- causality
- mechanism
- comparison
- application
- source-vs-current discrepancy
- why relations exist

Do not create trivia merely to increase card count.

For `No material`, do not invent new cards. Point the cue page to existing relevant cards or explicitly state that no new recall item was created.

## Query behavior

When the user asks:

- “내가 H3 관련해서 줬던 거 뭐였지?”
- “최근 GitHub 프로젝트만”
- “가속 관련만”
- “서로 대체재인 것끼리 비교”

query the **compiled wiki and graph first**, not chat memory and not raw sources first.

Use:

1. topic/set
2. aliases
3. category/function
4. platform if requested
5. relations + `연결 이유`
6. verification state
7. source/updated dates

Raw/source records are evidence fallbacks, not the primary reading surface.

## Lint

Run a lightweight lint after every ingest and a full lint on demand.

Flag:

- duplicate source URLs
- same resource under multiple names without aliasing
- Resource Note with no source/provenance
- relation with empty `연결 이유`
- orphan resource or wiki page
- `미확인` claim presented as fact
- current value overwriting historical source claim
- conflicting claims not marked `Disputed`
- new source that should have updated an existing wiki page but did not
- wiki page with no recent supporting source after material upstream changes
- source and wiki roles being conflated
- outbound URLs lost during rewriting
- stale redirect/canonical URLs

## Grounding invariant

Every load-bearing number, date, quote, version, or comparative claim in a wiki page must be traceable to a preserved Source Note or authoritative Resource Note.

If the evidence cannot support the precision of a claim, lower the claim's precision.

Do not upgrade confidence because multiple secondary pages repeat the same unsupported claim.

## Persistence verification

Before reporting success, verify:

- exact source URL preserved
- source article exists
- metadata populated
- `수집 판정` set
- outbound URLs preserved
- reusable outbound objects have Resource Notes
- affected existing wiki pages were searched
- cascade updates applied where material
- relations have explicit reasons
- child `큐` exists
- verification states match evidence
- no historical claim was silently overwritten

## Definition of done

PASS only when the source has improved the knowledge base, not merely added another page.

FAIL if:

- only a prose summary exists
- only bookmarks exist
- a new page was created without checking existing wiki pages
- a material source did not trigger relevant cascade updates
- a relation lacks an explanation
- a source claim was overwritten by a current value
- inaccessible content was invented
- raw/source evidence and compiled wiki knowledge are treated as the same layer
