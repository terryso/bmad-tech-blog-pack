# ingest-sources

Powered by BMAD™ Pack

目标：从用户提供的本地文本/Markdown中提取“干净笔记”。
约束：不抓取URL；不解析PDF；中文输出。
流程：

1) 读取/粘贴多份资料（可分节）。
2) 统一清理（去噪、标准化标题、保留代码块）。
3) 产出“统一资料笔记”，供后续任务使用。
输出：标准化笔记块（source, section, content）。

## Task Spec (YAML)

```yaml
task:
  id: ingest-sources
  name: Ingest Sources (Local Text Only)
  intent: 从用户提供的本地文本/Markdown 提取“干净笔记”用于后续流程
  doc_input_type: local-text-only
inputs:
  - id: sources
    type: markdown[]
    required: true
    description: 多份文本/Markdown 资料，允许分条粘贴
outputs:
  - id: unified_notes
    type: object[]
    schema:
      - source: string
      - section: string
      - content: markdown
steps:
  - id: normalize
    description: 统一清理（去噪、保留代码块、标准化标题）
  - id: structure
    description: 切分为 {source, section, content} 结构
validation:
  - name: 非空校验
    rule: 至少包含一条 unified_notes
  - name: 结构校验
    rule: 每条均包含 source/section/content 三字段
  - name: 代码块保真
    rule: 代码块不应被误清洗；若被修改需说明
elicit:
  - id: granularity
    prompt: 请选择切分粒度
    options:
      1: 仅按文档（粗粒度）
      2: 按一级标题（默认）
      3: 按二级标题（更细）
    default: 2
  - id: preserve_elements
    prompt: 是否保留表格/列表/行内代码
    options:
      1: 全部保留（默认）
      2: 去除表格但保留列表/代码
      3: 仅保留代码块
    default: 1
```

## 运行协议（交互）

- 若用户未粘贴任何资料，先提示按编号选项提供资料与粒度选择。
- 执行完成后，给出 unified_notes 的示例与统计（条数/来源数）。
- 如发现明显乱码或混入非文本内容，提示用户修正来源再重试。
