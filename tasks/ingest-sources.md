## Powered by BMAD™ Pack

docInputType: local-text-only

---

# ingest-sources

目标：从用户提供的本地文本/Markdown中提取“干净笔记”。
约束：不抓取URL；不解析PDF；中文输出。
流程：
1) 读取/粘贴多份资料（可分节）。
2) 统一清理（去噪、标准化标题、保留代码块）。
3) 产出“统一资料笔记”，供后续任务使用。
输出：标准化笔记块（source, section, content）。
