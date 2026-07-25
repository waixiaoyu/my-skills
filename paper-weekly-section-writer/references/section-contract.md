# 单篇论文小节输入输出契约

这个文件定义代码框架调用 `paper-weekly-section-writer` 时建议传入和读取的字段。字段名可以按现有代码适配，但语义边界应保持一致。

## 输入：Weekly Paper Section Request

```json
{
  "report_context": {
    "audience": "科研读者和技术负责人",
    "topics": ["大模型", "智能体", "网络自治", "网络数字孪生", "系统架构", "华为 ADN"],
    "source_basis": "full_text_excerpt | abstract_fallback",
    "display_policy": {
      "show_affiliation": true,
      "show_reading_value": true,
      "show_dimensions": true,
      "avoid_internal_process_terms": true
    }
  },
  "section_context": {
    "level": "本周必读 | 值得跟进 | 快速扫读",
    "index": 1,
    "selection_reason": "threshold | fallback",
    "reading_level": "本周必读"
  },
  "paper": {
    "id": "paper id",
    "title": "English paper title",
    "authors": ["author names"],
    "categories": ["cs.NI"],
    "primaryCategory": "cs.NI",
    "published": "2026-07-25",
    "absLink": "https://arxiv.org/abs/...",
    "link": "https://arxiv.org/pdf/...",
    "summary": "abstract text",
    "originalText": {
      "status": "available | unavailable",
      "source": "arxiv-html",
      "chars": 12000,
      "excerpt": "paper excerpt"
    },
    "analysis": {
      "tldr": "已有一句话判断",
      "scores": {
        "scenarioProblemValue": 80,
        "methodNovelty": 75,
        "practicalValue": 78,
        "evidence": 72
      },
      "dimensionDetails": [
        {
          "key": "scenarioProblemValue",
          "label": "研究问题价值",
          "score": 80
        }
      ],
      "matchedDimensions": ["研究问题价值 80"],
      "industryTags": ["ICT", "网络自治"],
      "whyRecommend": "已有推荐理由"
    },
    "readingListReview": {
      "score": 82,
      "scores": {
        "scenarioProblemValue": 80,
        "methodNovelty": 75,
        "practicalValue": 78,
        "evidence": 72
      },
      "dimensionDetails": [
        {
          "key": "scenarioProblemValue",
          "label": "研究问题价值",
          "score": 80
        }
      ],
      "affiliations": ["斯坦福大学"],
      "affiliationEvidence": "原文作者区出现 Stanford University",
      "tldr": "一句话周报价值判断",
      "valueHighlight": "高分信号或核心短板",
      "reviewReason": "入选后的具体判断",
      "evidenceBasis": "full-text | abstract-fallback",
      "selectionReason": "threshold | fallback"
    }
  }
}
```

## 输出：Weekly Paper Section Result

```json
{
  "section_markdown": "### 1. Paper Title\n\n- 发表单位：...\n- 阅读价值评分：...\n- 符合维度：...\n- 主问题域：...\n- 关键支撑技术：...\n- 链接：...\n\n**研究问题**\n...\n",
  "list_item": {
    "paper": "Paper Title",
    "one_sentence": "一句话介绍，概括文章做了什么或为什么值得关注。",
    "reading_level": "本周必读 | 值得跟进 | 快速扫读",
    "link": "https://..."
  },
  "topic_signals": [
    "可供总生成器提炼本周趋势的主题信号"
  ],
  "warnings": [
    "单位线索不足",
    "仅基于摘要和已有分析生成"
  ]
}
```

## 字段约束

- `section_markdown` 必须是一篇论文的小节，不包含 YAML front matter、报告导读、本周趋势判断、推荐阅读顺序或完整论文清单。
- `list_item.one_sentence` 不写分数、发表单位或符合维度。
- `topic_signals` 用短语表达，例如 `意图驱动的闭环自治评估`、`网络智能体工具调用可靠性`、`数字孪生仿真到真实网络迁移`。
- `warnings` 只供系统日志或调试使用，不直接拼进发布正文，除非系统希望显式展示证据边界。
- 如果 `paper.originalText.status` 不是 `available`，正文中涉及方法、结果和局限时必须用「基于摘要和已有分析看」或等价表述标明证据边界。
