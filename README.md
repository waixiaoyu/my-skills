# 我的 Codex Skills

这个仓库用于存放我个人维护的 Codex skills。每个 skill 使用一个独立目录，并至少包含一个 `SKILL.md` 文件；相关参考材料、脚本、agent 配置、模板或说明文件可以放在该 skill 目录下的子目录中。

## 当前 Skills

### `research-team-material-insight`

基于用户提供的 PPT、PDF、论文、报告、会议纪要或文本素材，整理研究团队信息、关键技术点、能力证据和少量精准合作切入点。它不是关键词检索工具，也不假设 AI 会自动搜全网补齐材料。

适合在已经有素材时使用，例如老师团队介绍、论文材料、项目报告、会议纪要、技术路线说明，以及与目标业务场景相关的背景材料。

详细说明见 [`research-team-material-insight/README.md`](research-team-material-insight/README.md)。

### `paper-weekly-section-writer`

在论文周报最终生成阶段，为每篇已经入选的论文生成风格一致、证据边界清楚的中文正文小节，并提供完整论文清单可复用的一句话介绍。

## 目录约定

常见结构：

```text
skill-name/
  SKILL.md
  README.md
  references/
  scripts/
  agents/
```

## 维护原则

- 每个 skill 尽量保持自包含。
- 不提交私密资料、临时输出、账号凭据或本地环境文件。
- 新增 skill 时，在仓库根目录下创建同级目录。
- 修改现有 skill 时，同时检查 `SKILL.md`、`README.md` 和 `agents/openai.yaml` 是否一致。
