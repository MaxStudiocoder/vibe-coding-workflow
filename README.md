# Vibe Coding Workflow Skill

[中文](#中文) | [English](#english)

---

## 中文

一个用于 **文档驱动、多 Agent Vibe Coding 工作流** 的 Codex Skill。

它把模糊的软件想法整理成一套可执行的工程流程：

1. 需求澄清
2. 高层设计
3. 详细设计
4. 任务拆分
5. 多 Agent 实现
6. 测试与验收

适合用在复杂项目、AI 辅助编程、Claude Code / Codex / Cursor 工作流，以及任何不适合“一句话直接生成代码”的软件开发任务中。

## 功能

- 将粗略想法整理成 `doc/proposal.md`
- 生成高层设计与详细设计文档
- 将项目拆分成小粒度任务
- 使用 `progress.md` 追踪任务状态
- 支持主 Agent / 子 Agent 分工
- 强制引入测试、lint、类型检查和 diff 审查
- 降低 Vibe Coding 中的上下文混乱和不可控修改风险

## 目录结构

```text
vibe-coding-workflow/
  SKILL.md
  agents/
    openai.yaml
  references/
    prompt-templates.md
