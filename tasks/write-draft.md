# write-draft

Powered by BMAD™ Pack

template: '../templates/blog-article-tmpl.yaml'

---

目标：依据大纲与证据，生成中文首稿，填充 blog-article-tmpl.yaml 的所有字段。
规则：

- 每节首句为“引导句”，结尾有“过渡句”。
- 引用证据使用内联标注 [n]，并在“Evidence Map”处统一列出。

## Task Spec (YAML)

```yaml
task:
  id: write-draft
  name: Write Draft (CN Tech Blog)
  intent: 根据大纲与 Evidence Map 生成结构化中文首稿
inputs:
  - id: outline
    type: object
    required: true
    schema:
      - tldr: string[]
      - key_takeaways: string[]
      - sections: object[]
  - id: evidence_map
    type: object[]
    required: true
    schema:
      - claim: string
      - quote: string
      - source: string
      - link: string|null
outputs:
  - id: article
    type: object
    description: 填充 blog-article-tmpl.yaml 的字段
  - id: markdown_render
    type: markdown
    description: 可复制的整篇 Markdown
steps:
  - id: fill_meta
    description: 填写标题/slug/author/date/audience/tags/seo-keywords/reading-time
  - id: compose_sections
    description: 按大纲生成各节内容，保证引导句与过渡句
  - id: cite_evidence
    description: 以 [n] 标注引用，保证可回溯并与 Evidence Map 对齐
  - id: finalize
    description: 汇总 TL;DR、Key Takeaways 与 Further Reading
validation:
  - name: 字段完备
    rule: 模板必填字段不得为空
  - name: 引用对齐
    rule: 所有 [n] 必须在 Evidence Map 中找到对应条目
  - name: 中文可读性
    rule: 不应出现大量英文直译腔与赘述
elicit:
  - id: tone
    prompt: 请选择写作语气
    options:
      1: 中性专业（默认）
      2: 亲切解说
      3: 简洁严谨
    default: 1
  - id: verbosity
    prompt: 请选择篇幅偏好
    options:
      1: 标准篇幅（默认）
      2: 较精简
      3: 详尽
    default: 1
  - id: code_style
    prompt: 代码/命令展示风格
    options:
      1: 标准代码块（默认）
      2: 行内为主
    default: 1
```

## 运行协议（交互）

- 未提供 outline/evidence_map 时，提示从 `generate-outline` / `extract-claims-quotes` 获取输入。
- 输出同时给出 article 对象与完整 Markdown，便于后续 `seo-polish` 与 `doc-out` 使用。
