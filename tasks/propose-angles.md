# propose-angles

Powered by BMAD™ Pack

template: '../templates/angle-scorecard-tmpl.yaml'

---
目标：基于证据，生成 7-10 个选题角度并打分，推荐 Top 3。
输出：按模板填充评分卡；每个角度附：

- hook（一句话引子）
- why（证据支撑/读者价值）
- 风险与避坑（如：争议点、可验证性）。

## Task Spec (YAML)

```yaml
task:
  id: propose-angles
  name: Propose Angles & Score
  intent: 基于 Evidence Map 生成候选角度并量化评分，产出 Top 3 推荐
inputs:
  - id: evidence_map
    type: object[]
    required: true
    schema:
      - claim: string
      - quote: string
      - source: string
      - link: string|null
outputs:
  - id: angle_candidates
    type: object[]
    schema:
      - angle: string
      - hook: string
      - why: string
      - scores:
          novelty: number
          controversy: number
          utility: number
          relevance: number
          writability: number
      - risks: string
      - total_score: number
  - id: recommendations
    type: object[]
    schema:
      - angle: string
      - reason: string
steps:
  - id: generate
    description: 生成 7-10 个候选角度，并给出 hook/why/风险
  - id: score
    description: 按五维度 0-5 打分并给出简要理由，计算总分
  - id: recommend
    description: 汇总推荐 Top 3 并说明取舍
validation:
  - name: 数量校验
    rule: 候选角度不少于 7 个
  - name: 字段完整性
    rule: 每个角度包含 angle/hook/why/scores/risks/total_score
  - name: 风险显式化
    rule: 高风险角度必须给出避坑建议
elicit:
  - id: candidate_count
    prompt: 生成候选角度数量
    options:
      1: 7（默认）
      2: 8-9
      3: 10+
    default: 1
  - id: weight_style
    prompt: 是否自定义评分权重
    options:
      1: 默认等权（推荐）
      2: 自定义（需用户提供 5 维权重）
    default: 1
```

## 运行协议（交互）

- 若 evidence_map 为空，提示先运行 `extract-claims-quotes`。
- 输出时给出 Top 3 摘要与完整评分表的复制块。
