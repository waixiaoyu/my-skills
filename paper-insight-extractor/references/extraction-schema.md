# Extraction Schema

This schema describes the contract between deterministic code and the paper-extraction agent.

## Input: Paper Evidence Package

Use this shape when code or a user can prepare structured input.

```json
{
  "task": {
    "mode": "single_paper | weekly_report | prompt_design | schema_check",
    "audience": "researcher | engineering_lead | general_reader",
    "language": "zh-CN"
  },
  "paper": {
    "id": "arXiv id, DOI, URL, or stable local id",
    "title": "",
    "authors": [],
    "published": "",
    "categories": [],
    "absLink": "",
    "pdfLink": "",
    "summary": ""
  },
  "evidence": {
    "level": "metadata-only | abstract-only | html-excerpt | full-text | external-context",
    "chunks": [],
    "missing": []
  },
  "codeSignals": {
    "emailDomains": [],
    "affiliationCandidates": [],
    "topicSignals": [],
    "industryTags": [],
    "scores": {}
  },
  "outputPolicy": {
    "readerFacing": true,
    "forbiddenReaderTerms": ["复评", "复评分", "候选下限", "内部阈值"],
    "scoreLabel": "阅读价值评分",
    "affiliationLanguage": "zh-CN",
    "allowUncertainAffiliation": true
  }
}
```

## Evidence Chunks

Each item in `evidence.chunks` should use:

```json
{
  "type": "author_block | affiliation_block | abstract | introduction | method | experiment | result | limitation | acknowledgement | project_page | repository | other",
  "source": "arxiv-metadata | arxiv-html | pdf-text | tex-source | project-page | repo | doi-page | user-provided",
  "text": "",
  "confidence": "high | medium | low"
}
```

Interpretation rules:

- `author_block` and `affiliation_block` are strongest for affiliations.
- Email domains are supporting evidence, not final affiliation proof by themselves.
- Acknowledgements can support institution or lab context, but may identify funders rather than author affiliations.
- `abstract` is enough for problem and contribution, but weak for implementation details, evidence quality, and affiliations.
- `external-context` must be clearly separated from paper text.

## Output: Paper Extraction Result

Use this shape for structured output.

```json
{
  "evidenceBoundary": {
    "level": "html-excerpt",
    "usableEvidence": [],
    "missingEvidence": [],
    "boundaryStatement": ""
  },
  "affiliations": {
    "namesZh": [],
    "evidence": "",
    "confidence": "high | medium | low",
    "needsVerification": false
  },
  "paperAbstraction": {
    "researchProblem": "",
    "coreContribution": "",
    "methodFramework": "",
    "experimentsAndResults": "",
    "limitations": [],
    "transferableValue": "",
    "evidenceNotes": []
  },
  "domainFit": {
    "fit": "target_network_autonomy | general_ai_system | out_of_scope_domain | unclear",
    "reason": "",
    "notQualitySignal": true
  },
  "weeklyReportFields": {
    "affiliationLine": "",
    "scoreLine": "",
    "dimensionLine": "",
    "problemSection": "",
    "contributionSection": "",
    "methodSection": "",
    "experimentSection": "",
    "limitationsSection": [],
    "adnValueSection": []
  }
}
```

## Affiliation Output Rules

`affiliations.namesZh` must contain Chinese institution names.

Examples:

- `Stanford University` -> `斯坦福大学`
- `Massachusetts Institute of Technology` or `MIT` -> `麻省理工学院`
- `University of Cambridge` -> `剑桥大学`
- Unclear standard translation -> `English Name（中文意译）`
- Insufficient evidence -> `单位线索不足`

`affiliations.evidence` should name the evidence type:

- `作者区显示 ...`
- `机构脚注显示 ...`
- `邮箱域名 ... 与机构候选一致`
- `摘要和元数据没有机构脚注或邮箱线索`

Never infer affiliations from author names or nationality.

## Domain Fit Rules

Use `target_network_autonomy` when the main problem concerns telecom/network infrastructure, autonomous networks, ADN, O-RAN, 5G/6G, network digital twins, intent-driven networking, closed-loop operations, QoS, routing, slicing, spectrum, handover, fault diagnosis, or service assurance.

Use `general_ai_system` when the contribution is a transferable AI, LLM, Agent, RAG, tool-use, benchmark, safety, evaluation, orchestration, or system-architecture method.

Use `out_of_scope_domain` when the main problem, target object, and evaluation are primarily a vertical domain such as medicine, life science, geospatial, game, education, finance, law, social science, or recommender systems, and the paper does not present a transferable general method.

Use `unclear` when the evidence does not show target-domain relevance or transferable method value.

## Reader-Facing Weekly Notes

For publishable weekly-report text:

- Use `阅读价值评分：{score}` if a score line is required.
- Keep `发表单位：...（依据：...）` in the per-paper body when affiliations are requested.
- Do not write `复评`, `复评分`, `候选下限`, `内部阈值`, or implementation details.
- Keep the final paper index table compact unless the user asks for more columns.
