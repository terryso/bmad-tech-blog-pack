# bmad-tech-blog-pack (CN)

面向中文技术博客创作的 BMAD 扩展包（MVP）。支持多份“离线文本/Markdown”资料→角度发现→大纲→首稿→SEO→落盘输出的完整流程。

- 配置：`bmad-tech-blog-pack/config.yaml`（`slashPrefix: bmad-blog`）

- 代理：`agents/`（research-synthesizer, angle-finder, outline-architect, copywriter, doc-editor, seo-strategist）

- 任务：`tasks/`（ingest-sources, extract-claims-quotes, propose-angles, generate-outline, write-draft, seo-polish, doc-out）

- 模板：`templates/`（blog-article-tmpl.yaml, angle-scorecard-tmpl.yaml, citation-list-tmpl.yaml）

- 工作流：`workflows/blog-from-refs.md`

## Quickstart（离线、中文、放在本仓库）

1. 准备资料：把多份参考内容以文本/Markdown 形式粘贴作为输入（不抓取 URL、不解析 PDF）。

2. 执行流程（人工按顺序触发）：
   - ingest-sources → extract-claims-quotes → propose-angles → generate-outline → write-draft → seo-polish → doc-out

3. 输出位置：
   - 草稿：`docs/blog/drafts/`
   - 证据映射与角度：`docs/blog/evidence/`
   - 社媒物料：`docs/blog/social/`

## 示例文件

- 证据映射：`docs/blog/evidence/20250907-bmad-tech-blog-mvp-citations.md`

- 角度评分卡：`docs/blog/evidence/20250907-bmad-tech-blog-mvp-angles.yaml`

- 大纲：`docs/blog/drafts/20250907-bmad-tech-blog-mvp-outline.md`

- 首稿：`docs/blog/drafts/20250907-bmad-tech-blog-mvp.md`

## 约束（MVP）

- 仅离线本地文本/Markdown；中文输出；文件写入在本仓库。

- 不含网页抓取、PDF 解析、外部平台集成；后续可扩展。

## Orchestrator（博客编排官）与团队

- 代理：`agents/blog-orchestrator.md`

- 团队：`agent-teams/blog-team.yaml`（research-synthesizer → angle-finder → outline-architect → copywriter → doc-editor → seo-strategist）

### 两种模式

- 互动模式（interactive）：逐步问答与人工确认，质量可控。

- 一键模式（auto）：自动串联任务完成首稿与 SEO，快速出稿。

### 典型用法

1) 将 2-3 份参考资料以文本/Markdown 粘贴提供。

2) 互动模式：
   - `*mode interactive`
   - `*ingest` → `*extract` → `*angles`（选择角度）→ `*outline` → `*draft` → `*seo` → `*doc-out`

3) 一键模式：
   - `*mode auto`
   - `*run`

## 一键工作流

- `workflows/blog-one-shot.md`：面向已有资料的一次性端到端产出，产物位置：
  - 草稿：`docs/blog/drafts/`
  - 证据映射/角度：`docs/blog/evidence/`
  - 社媒物料：`docs/blog/social/`
