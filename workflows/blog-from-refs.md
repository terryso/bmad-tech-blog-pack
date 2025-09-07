## Powered by BMAD™ Pack

# Workflow: blog-from-refs （离线资料 → 中文技术博客）

前置：用户提供多份本地文本/Markdown 资料（粘贴或指出路径内容后粘贴）。

步骤：
1) 任务 ingest-sources：清理并标准化资料笔记。
2) 任务 extract-claims-quotes：生成观点-引文配对表。
3) 任务 propose-angles：产出 7-10 个选题角度及评分卡，选定 Top 1。
4) 任务 generate-outline：生成双层结构大纲（含每节要回答的问题）。
5) 任务 write-draft：依据模板生成中文首稿。
6) 任务 seo-polish：标题/摘要/关键词等优化。
7) 任务 doc-out：输出到 docs/blog/drafts|evidence|social。

交互规则：
- 每一步都允许用户微调输入/输出再进入下一步；
- 如运行环境无法自动落盘，提供完整 Markdown 供复制保存。
