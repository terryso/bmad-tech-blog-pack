# extract-claims-quotes

Powered by BMAD™ Pack

目标：从标准化笔记中抽取“观点-引文（claim-quote）配对表”。
输出字段：claim, quote, source, link(optional), note(optional)
规则：
- claim 必须可验证；quote 尽量为原文摘录或近似复述。
- 保留出处与段落线索，便于回溯。

## Task Spec (YAML)

```yaml
task:
  id: extract-claims-quotes
  name: Extract Claims & Quotes
  intent: 从统一资料笔记中抽取可验证的“观点-引文配对表”
inputs:
  - id: unified_notes
    type: object[]
    required: true
    schema:
      - source: string
      - section: string
      - content: markdown
outputs:
  - id: evidence_map
    type: object[]
    schema:
      - claim: string
      - quote: string
      - source: string
      - link: string|null
      - note: string|null
steps:
  - id: parse
    description: 遍历 unified_notes，识别候选 claim 与其支持性 quote
  - id: pair
    description: 将 claim 与 quote 配对并补齐 source/link/note
  - id: verify
    description: 对 claim 可验证性进行基本检查与备注
validation:
  - name: 非空校验
    rule: 至少包含一条 evidence_map
  - name: 字段完整性
    rule: 每条至少包含 claim/quote/source 三字段
  - name: 可验证性
    rule: claim 需对应可定位的 quote/source
elicit:
  - id: quote_style
    prompt: 请选择引用风格
    options:
      1: 尽量原文摘录（默认）
      2: 允许近似复述（保留关键术语）
    default: 1
  - id: strictness
    prompt: 请选择可验证性严格度
    options:
      1: 标准（默认）
      2: 更严格（无法验证则丢弃）
    default: 1
```

## 运行协议（交互）

- 若输入为空，提示先运行 `ingest-sources` 产出 unified_notes。
- 输出 Evidence Map 同时给出统计（条目数/来源覆盖数），并标注缺乏可验证性的条目。
