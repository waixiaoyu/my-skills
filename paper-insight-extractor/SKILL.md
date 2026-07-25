---
name: paper-insight-extractor
description: Define evidence-bounded paper abstraction and extraction for Paper Insight-style workflows. Use when Codex needs to turn paper metadata, abstracts, arXiv HTML/PDF text, project pages, or prepared evidence packages into structured research fields; identify Chinese affiliation names and evidence; preserve uncertainty; design or review extraction prompts and schemas; or prepare reader-facing weekly-report material without exposing internal scoring or review mechanics.
---

# Paper Insight Extractor

Use this skill as a paper extraction protocol, not as a runtime implementation. Deterministic work stays in code: fetching, caching, HTML/PDF parsing, text chunking, regex extraction, scoring formulas, sorting, schema validation, retries, and deployment.

The skill defines how an agent should interpret evidence, decide what can be claimed, and shape output fields for Paper Insight-style single-paper analysis or weekly-report material.

## Boundary

This skill does:

- define the input evidence package expected from code or a user;
- define the output fields an agent should produce;
- define affiliation, evidence-boundary, domain-fit, and reader-facing wording rules;
- help design or review prompts that perform paper abstraction.

This skill does not:

- fetch arXiv, DOI, PDF, HTML, repository, or project-page content;
- clean HTML/PDF text or extract chunks deterministically;
- compute scores, thresholds, rankings, or recommendation decisions;
- edit application source code or deploy services;
- guarantee factual completeness when the input evidence is incomplete.

## Workflow

1. Inspect the requested task mode: single-paper analysis, weekly-report field extraction, prompt design, or schema check.
2. Identify the evidence level: metadata only, abstract only, HTML excerpt, full text, or external context.
3. Read the provided evidence chunks and keep claims tied to the strongest available source.
4. Extract affiliations from explicit evidence only. Translate institution names into Chinese.
5. Produce the structured output. Mark unsupported fields as `unknown`, `线索不足`, or `需要核验`.
6. For reader-facing output, remove internal process wording such as `复评`, `复评分`, `候选下限`, and `内部阈值`.

When field-level detail is needed, read [references/extraction-schema.md](references/extraction-schema.md).

## Core Rules

- Treat topic fit and paper quality separately. Network autonomy, telecom, ICT, or ADN relevance is not itself evidence of research quality.
- Do not infer telecom relevance from the word `network` alone; distinguish telecom/network infrastructure from social, biological, neural, graph, or regulatory networks.
- Do not mark a vertical-domain paper as out of scope when its main contribution is a transferable AI, Agent, evaluation, architecture, or system method.
- Do not invent affiliations from author names, countries, or familiar-looking email fragments.
- Use common Chinese institution names when known. If no standard Chinese name is obvious, use `English Name（中文意译）`.
- Preserve uncertainty in plain language: `基于摘要看`, `原文摘录显示`, `单位线索不足`, `需要打开 PDF 核验`.

## Reader-Facing Wording

Use reader-facing labels only when preparing publishable text:

- Use `阅读价值评分`, not `周报复评分` or `复评分`.
- Use `发表单位`, not `affiliation`.
- Do not mention internal candidate floors, thresholds, fallback selection, or review batches.
- Keep full paper-list tables compact. If a score, dimensions, or affiliations are needed, place them in the per-paper body, not in the final index table unless the user explicitly asks.

## Expected Minimal Output

For a basic extraction, return:

- evidence boundary;
- Chinese affiliations and affiliation evidence;
- research problem;
- core contribution;
- method framework;
- experiments and results;
- limitations and verification needs;
- domain fit and reason;
- optional weekly-report lines when requested.
