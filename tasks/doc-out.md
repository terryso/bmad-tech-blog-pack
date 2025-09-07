# doc-out

Powered by BMAD™ Pack

docOutputLocations:
  draft: 'docs/blog/drafts'
  evidence: 'docs/blog/evidence'
  social: 'docs/blog/social'

---

目标：将首稿、证据映射与社媒物料输出到指定目录（文件名自动建议：YYYYMMDD-{slug}.md）。
说明：若运行环境不支持自动写入，请以“可复制的 Markdown 输出块”形式提供完整内容。

## Task Spec (YAML)

```yaml
task:
  id: doc-out
  name: Document Output (Write Files)
  intent: 将文章与相关产物写入指定目录；若无法落盘则输出可复制的 Markdown
inputs:
  - id: article
    type: object
    required: true
    description: 按 blog-article-tmpl.yaml 填充后的文章对象
  - id: evidence_map
    type: object[]
    required: false
  - id: social_assets
    type: object
    required: false
    schema:
      - seo_meta: object
      - twitter_cards: object|null
outputs:
  - id: files_written
    type: object
    schema:
      - draft_path: string|null
      - evidence_path: string|null
      - social_path: string|null
  - id: fallback_markdown
    type: markdown|null
    description: 若无法写文件，则提供完整可复制的 Markdown
steps:
  - id: suggest_filename
    description: 依据 date/slug 建议文件名 YYYYMMDD-{slug}.md
  - id: write_draft
    description: 将正文写入 draft 目录
  - id: write_evidence
    description: 将 Evidence Map/角度评分等写入 evidence 目录（如提供）
  - id: write_social
    description: 将社媒物料写入 social 目录（如提供）
  - id: fallback
    description: 若无法写入文件系统，则输出可复制的 Markdown 块
validation:
  - name: 路径存在
    rule: 目标目录不可为空；不可写时需走 fallback
  - name: 文件名合法
    rule: 仅允许大小写字母、数字、连字符与下划线
  - name: 最少产物
    rule: 至少生成 draft 或 fallback_markdown 之一
elicit:
  - id: naming
    prompt: 请选择文件命名策略
    options:
      1: YYYYMMDD-<slug>.md（默认）
      2: <slug>.md
    default: 1
  - id: outputs
    prompt: 需要写入哪些产物
    options:
      1: 仅草稿（默认）
      2: 草稿 + 证据
      3: 草稿 + 证据 + 社媒
    default: 1
```

## 运行协议（交互）

- 若运行环境不支持落盘，则直接返回 fallback_markdown；同时提示目标目录以便手动保存。
- 写文件成功时，返回 files_written 中的完整路径。
