# generate-outline

Powered by BMAD™ Pack

目标：根据选定角度生成“双层结构”大纲。
要求：

- 顶部 TL;DR（3-5条）与 Key Takeaways。
- H2/H3 层级，每节附“本节回答的问题”。
- 标出需要插入的代码/图示/表格位点。

## Task Spec (YAML)

```yaml
task:
  id: generate-outline
  name: Generate Outline (Two-layer)
  intent: 根据选定角度生成可写性强、可导航的大纲
inputs:
  - id: selected_angle
    type: string
    required: true
    description: 已确定的写作角度
  - id: evidence_map
    type: object[]
    required: false
    description: 可选，用于标注“本节回答的问题”的依据
outputs:
  - id: outline
    type: object
    schema:
      - tldr: string[]
      - key_takeaways: string[]
      - sections: object[]  # 每个含 level/title/questions/notes
steps:
  - id: derive_tldr
    description: 产出 TL;DR（3-5 条）与 Key Takeaways（2-4 条）
  - id: build_hierarchy
    description: 输出 H2/H3 层级结构，并为每节写出“本节回答的问题”
  - id: annotate
    description: 标注需要插入的代码/图示/表格位点
validation:
  - name: 数量校验
    rule: TL;DR 3-5 条；至少 2 条 Key Takeaways
  - name: 结构校验
    rule: 至少包含一个 H2；如有 H3 必须归属到某个 H2 之下
  - name: 问题完备
    rule: 每个 H2 至少包含一个“本节回答的问题”
elicit:
  - id: depth
    prompt: 请选择大纲深度
    options:
      1: 仅 H2（默认）
      2: H2 + 核心 H3
      3: H2 + 详细 H3（更细）
    default: 1
  - id: annotations
    prompt: 是否强制标注插图/表格/代码位点
    options:
      1: 是（默认）
      2: 否（仅在必要时）
    default: 1
```

## 运行协议（交互）

- 若未提供 selected_angle，先请用户确认角度（可回到 `propose-angles`）。
- 输出为可复制的 Markdown 结构，便于后续 `write-draft` 直接引用。
