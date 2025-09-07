# Workflow: blog-one-shot（编排官一键出稿）

_Powered by BMAD™ Pack_

适用：已有 2-3 份离线文本/Markdown 参考资料，想快速生成可用首稿。

前置：

- 仅离线文本/Markdown；中文输出；落盘到本仓库。
- 团队：`agent-teams/blog-team.yaml`
- 代理：`agents/blog-orchestrator.md`

步骤：

1) 将参考资料以文本/Markdown 粘贴提供。
2) 设置编排官为自动模式：`*mode auto`
3) 运行一键流程：`*run`
4) 查看产物：

   - 草稿：`docs/blog/drafts/`
   - 证据映射/角度：`docs/blog/evidence/`
   - 社媒物料：`docs/blog/social/`

说明：

- 自动模式默认选评分最高角度；如需人工介入，请改用 `*mode interactive` 按步执行。
- 任何一步均可重跑并覆盖上一版本草稿（建议保留文件历史）。
