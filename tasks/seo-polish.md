# seo-polish

Powered by BMAD™ Pack

目标：在不改变事实的前提下进行 SEO 优化。
检查清单：

- 标题/副标题是否含主关键词；
- 摘要长度与关键信息；
- 关键词密度与同义词覆盖；
- OG/Twitter 文案是否吸引点击；
- 内链/外链的锚文本是否清晰。

## Task Spec (YAML)

```yaml
task:
  id: seo-polish
  name: SEO Polish (Meta & Social)
  intent: 在不改变事实与结论的前提下优化可发现性与点击率
inputs:
  - id: article
    type: object
    required: true
    description: 按 blog-article-tmpl.yaml 填充后的文章对象
outputs:
  - id: seo_meta
    type: object
    schema:
      - titles: string[]   # 3 个候选
      - meta_description: string  # 120-160 字
      - keywords: string[]        # 5-8 个
      - og_title: string
      - og_description: string
      - twitter_title: string
      - twitter_description: string
  - id: article_suggestions
    type: markdown
    description: 如需对正文做轻微调整，输出建议片段（不直接改动事实）
steps:
  - id: derive_keywords
    description: 基于文章主旨与同义词/LSI 生成关键词
  - id: craft_titles
    description: 生成 3 个标题候选，覆盖不同风格
  - id: write_meta
    description: 生成 120-160 字摘要（meta description）
  - id: social_copy
    description: 生成 OG/Twitter 文案
  - id: checklist
    description: 按检查清单逐条校验并补救
validation:
  - name: 长度与关键词覆盖
    rule: 摘要需在 120-160 字；至少 5 个关键词并涵盖同义词
  - name: 文案可读性
    rule: 不允许关键词堆砌；语句通顺
  - name: 一致性
    rule: 不改变事实与结论
elicit:
  - id: title_style
    prompt: 标题风格偏好
    options:
      1: 信息密度高（默认）
      2: 问题式
      3: 对比式
    default: 1
  - id: keyword_strictness
    prompt: 关键词严格度
    options:
      1: 标准（默认）
      2: 严格（更高密度与覆盖）
    default: 1
```

## 运行协议（交互）

- 输入为 article 对象；若为空，请先运行 `write-draft`。
- 输出 `seo_meta` 与 `article_suggestions`（如有），并列出检查清单逐条通过情况。
