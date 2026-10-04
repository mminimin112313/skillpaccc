---
name: url-knowledge-capture
displayName: URL Knowledge Capture
version: 1.0.0
description: Convert a user-provided URL into a verified, reusable knowledge package: natural article, metadata, preserved outbound links, additional research, atomic resource notes, Zettelkasten relations with explicit reasons, and a Recall Cards child page.
when_to_use:
  - The user provides a URL and asks to process, archive, summarize, research, save, or put it in Notion.
  - The user says “이거 정리”, “이거 프로세스”, “노션에 넣어”, “큐도”, or equivalent while referring to a web source.
  - The user asks what resources/links/projects they previously supplied and expects category- or relation-aware retrieval.
argument-hint: "<url> [optional pasted body/context]"
allowed-tools:
  - web
  - Notion
  - GitHub
---

# URL Knowledge Capture

## Purpose

Turn one web source into a durable knowledge object rather than a disposable summary.

A successful run preserves the original source, separates source claims from current verification, produces readable prose, captures every meaningful outbound URL, connects related notes by reason, and creates review cues in the user's existing Recall Cards structure.

## Non-negotiable output contract

For each URL, produce and persist this package:

1. **Article** — a natural explanatory article written for a human reader.
2. **Metadata** — source URL, source type, author/publisher when verified, captured date, source date when verified, topic, functions, verification state, and set name.
3. **Cue page** — a child page named `큐` using `질문 → 힌트 → 정답보기` toggles.
4. **Outbound-link preservation** — every meaningful URL contained in the source becomes a preserved link; when it represents a reusable tool/project/resource, create an atomic Resource Note.
5. **Additional research** — verify current facts against official or primary sources when possible. Preserve both the historical/source claim and the current verified value when they differ.
6. **Zettelkasten relations** — connect notes only when a meaningful relationship exists, and record *why* the connection exists.

Do not declare the run complete if one of these parts is silently omitted.

## Destination: Recall Cards

Use the existing Notion database `학습 큐 · Recall Cards`.

### Source/article record

Create or update one database record with:

- `유형 = 원문`
- `질문 / 제목 = article title`
- `원문 URL = exact user-provided URL`
- `세트 = stable set name`
- `태그 = retrieval-oriented tags`
- `카테고리 = Source Note / <topic>`
- `검증 상태 = 확인 | 부분 확인 | 미확인`
- `관련 노트 = atomic Resource Notes directly derived from or materially related to this source`
- `연결 이유 = why the Source Note is connected to those notes`

The article page itself owns the child page `큐`.

### Cue child page

Create exactly one child page named `큐` below the article page.

Repeat this structure:

```text
### Qn. <one focused recall question>

**힌트:** <minimum cue that helps retrieval without giving away the answer>

<details>
<summary>정답보기</summary>
    <concise but complete answer>
</details>
```

Cue design priorities:

- structure
- causality
- comparison
- application
- evidence/verification when it changes interpretation

Do not create trivia cards merely to increase card count.

## Workflow

### 1. Preserve the source before interpreting it

Record the exact URL exactly as supplied.

Identify, when verifiable:

- author or publisher
- publication time/date
- source type
- title or first-line claim
- quoted or embedded parent source

Never replace the original URL with a redirect or canonical URL. If a canonical/current URL differs, store both and label them.

### 2. Acquire source content

Prefer the original page. If the source is inaccessible, use trustworthy mirrors, caches, quoted copies, official reposts, or other direct evidence.

Keep an evidence ledger internally:

- **source fact** — explicitly stated in the supplied source
- **current verified fact** — confirmed from a current authoritative source
- **inference** — synthesis derived from evidence
- **unverified** — not established

If the actual body cannot be recovered, do **not** invent the article, outbound links, or cues. Create/update a pending Source Note with `검증 상태 = 미확인`, record the retrieval failure, preserve the exact URL, and stop content synthesis until the body becomes available.

### 3. Extract outbound URLs

Collect all meaningful URLs in the **supplied source itself**, including URLs mentioned as plain domains when the intended destination is clear. Default traversal depth is one hop: preserve and research links directly present in the supplied source, but do not recursively explode every link contained inside a large linked corpus.

When a directly linked resource is itself a large index or dataset (for example, hundreds of GitHub prompt files or source-post links), preserve that collection as one atomic Resource Note and capture its machine-readable index/data path. Only fan out its children when the user asks, when a small bounded subset is materially useful, or when individual children are independently central to the article.

For each link:

- preserve the original URL
- identify the resource name
- classify its role/function
- verify whether it is currently active when material
- note redirects, renamed products, archival status, or changed domains

Do not collapse GitHub/Civitai/Hugging Face/etc. into functional categories. Platform is provenance; category/function is what the resource does.

### 4. Additional research

Research claims that are current, quantitative, comparative, or operationally important.

Prefer:

1. official project/product documentation
2. primary repositories or papers
3. first-party release notes
4. high-quality secondary sources
5. community reports only for community experience/reaction

When source and current facts differ, write both:

```text
Source claim: 300+ prompts
Current official value: 104 templates, 14 UI screens
```

Never silently overwrite history with the current value.

### 5. Write the article

The article must read like edited human prose, not database output.

Use this causal flow unless the subject requires a stronger domain-specific ordering:

`direct thesis → problem/cause → mechanism/structure → trade-off/result → application`

Requirements:

- first paragraph answers what matters
- include a table of contents for substantial articles
- use stable terminology
- distinguish fact, measurement, inference, and uncertainty
- explain relations through mechanism, not generic transitions
- remove meta prose such as “this connection is important,” “the interesting point is,” or “the key takeaway is” when the following sentence can state the substance directly
- avoid title spam, one-sentence paragraphs, and mechanical advantage/disadvantage lists
- preserve natural Korean editorial rhythm when writing in Korean

### 6. Create atomic Resource Notes

Create one Resource Note per reusable tool, project, model, paper, library, dataset, site, or other independently retrievable object.

Recommended fields:

- `유형 = 리소스`
- `질문 / 제목`
- `원문 URL`
- `세트`
- `태그`
- `카테고리`
- `검증 상태`
- `출처 포스팅 = Source Note relation`
- `관련 노트`
- `연결 이유`

A Resource Note body should answer:

1. 무엇인가
2. 무엇을 해결하는가 / 어디에 쓰는가
3. 현재 확인된 사실
4. 이 Source Note에서 왜 저장했는가
5. 다른 노트와 왜 연결되는가

### 7. Build Zettelkasten relations

Do not create relations because two notes share a tag.

Create a relation only when it adds recoverable reasoning. Useful relation types include:

- **Source** — discovered/derived from
- **Uses** — A directly uses B
- **Feeds into** — A's output becomes B's input
- **Alternative to** — same problem, materially different approach
- **Complements** — combined use covers different needs
- **Implements** — implementation of a concept/reference
- **Reference for** — used as a design/evaluation reference
- **Built on** — dependency/foundation
- **Same problem, different layer** — same goal at different abstraction levels
- **Inspired by / Derived from** — lineage

For every relation, record a sentence in this form:

```text
B — <A의 어떤 속성과 B의 어떤 속성이 어떤 메커니즘으로 연결되는지>.
```

Reject weak explanations such as “둘 다 AI 도구이기 때문”.

### 8. Build cues after the article and research stabilize

Cues test the final understanding, not the raw source text.

Include updated research when it materially changes interpretation. Typical cue targets:

- causal bottleneck
- mechanism
- category structure
- important comparison
- application rule
- historical claim vs current verified fact
- why two notes are connected

### 9. Verify persistence

Before reporting success, verify:

- exact source URL exists
- article record is in Recall Cards with `유형=원문`
- metadata is populated
- child `큐` page exists
- all meaningful outbound URLs are preserved
- reusable outbound resources have atomic notes
- source relations are present
- Zettelkasten relation reasons are present
- verification states match evidence
- no unverified claim is written as fact

Report incomplete parts explicitly.

## Retrieval behavior

When the user later asks questions such as:

- “내가 H3 관련해서 줬던 거 뭐였지?”
- “최근 GitHub 프로젝트만 카테고리별로”
- “가속 관련만”
- “서로 대체재인 것끼리 비교”

retrieve using both metadata and relation semantics:

1. Topic/set first
2. Platform only when requested
3. Function/category for grouping
4. Relations and `연결 이유` for comparison or lineage
5. Source date / added date for recency

Do not rely on chat memory when the Notion knowledge base contains the durable record.

## Failure rules

### Source inaccessible

- preserve URL
- mark `미확인`
- record retrieval failure
- do not invent source body, links, or cues

### Outbound link inaccessible

- preserve URL
- mark resource `미확인` or `부분 확인`
- describe only what the source itself establishes

### Conflicting evidence

- preserve both claims
- identify source/date of each
- do not force a false reconciliation

### Existing note found

Update the existing note rather than create a duplicate. Preserve prior valid metadata and relations.

## Definition of done

PASS only when the run leaves behind a source-preserving, searchable and reviewable knowledge package.

A prose summary alone is FAIL.
A bookmark list alone is FAIL.
A Relation without an explicit reason is FAIL.
A cue page outside its parent posting is FAIL.
Current facts that overwrite the historical source claim are FAIL.
Invented content from an inaccessible source is FAIL.