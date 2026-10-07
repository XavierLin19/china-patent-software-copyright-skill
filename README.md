# China Patent & Software Copyright Skill｜中国发明专利与软件著作权

面向中国发明专利撰写和软件著作权（软著）申请材料整理的一体化 Agent Skill。统一输入、证据追踪与质量门禁，分别完成技术交底、专利申请文件草稿，以及真实项目源码、申请信息和软件操作手册的整理。

An evidence-first Agent Skill for Chinese invention patent drafting and software copyright registration materials. It supports technical disclosures, prior-art research, patent claims and specifications, software user manuals, and real source-code material selection.

**搜索关键词 / Search keywords**：中国发明专利、专利撰写、专利查新、技术交底书、权利要求、专利说明书、软件著作权、软著、软著申请、源代码鉴别材料、软件操作手册、CNIPA、China patent、Chinese invention patent、patent drafting、software copyright、Agent Skills、Codex Skill、Claude Code Skill。

**Skill 标识**：`china-patent-software-copyright` · **版本**：`1.0.1` · **作者**：[XavierLin19](https://github.com/XavierLin19) · **许可证**：[MIT](LICENSE)

## 主要特点

- 一个入口，两条专利/软著子流程；两者共用来源、事实和疑点 ID。
- 专利流程分开材料审阅、检索、发明人访谈、创新对齐、起草对齐和审计。
- 软著流程基于真实项目与源码，关键字段和代码选择由用户确认。
- 用 `SUPPORTED / INFERRED / USER_CONFIRMED / UNKNOWN / CONFLICT / OUT_OF_SCOPE` 表示事实状态。
- 禁止编造实验、参数、功能、日期、权属、截图或源码。
- 附带统一台账、质量门禁和可复制模板。

## 安装

将整个 `china-patent-software-copyright/` 目录复制到当前 Agent 支持的 Skill 目录中，确保 `SKILL.md` 位于这个目录的根层。Codex 的默认用户目录为 `~/.codex/skills/`；其他 Agent 请使用其配置的技能目录。复制完成后重新加载技能列表或开始新会话。

从 GitHub 安装到 Codex（目标目录不存在时）：

```bash
git clone https://github.com/XavierLin19/china-patent-software-copyright-skill.git ~/.codex/skills/china-patent-software-copyright
```

Windows PowerShell：

```powershell
git clone https://github.com/XavierLin19/china-patent-software-copyright-skill.git "$env:USERPROFILE\.codex\skills\china-patent-software-copyright"
```

不使用 Git 时，可下载仓库 ZIP，把解压后的目录命名为 `china-patent-software-copyright`，再复制到技能目录。材料整理本身无需强制安装第三方运行时；文档转换、截图和导出按所用 Agent 的可用能力执行。

## 使用示例

显式调用：`$china-patent-software-copyright`。也可直接提出下列请求，由支持自动技能选择的 Agent 识别。

- “请阅读当前项目，先帮我梳理可能的发明点，建立证据台账并列出需要发明人确认的问题。”
- “根据这个项目准备软著材料草稿。先分析真实功能并列出申请字段和代码文件候选，等我确认后继续。”
- “专利交底和软著都要整理；共用项目事实台账，但分别输出各自材料，并标出未核实的内容。”

## 目录

```text
china-patent-software-copyright/
├── SKILL.md
├── agents/openai.yaml      # Codex 展示信息与自动调用设置
├── patent/WORKFLOW.md
├── software-copyright/WORKFLOW.md
├── references/             # 事实完整性、台账、门禁、融合来源
└── templates/              # 接入、台账、专利和软著草稿模板
```

本 Skill 提供材料起草与整理辅助，不代替专业法律意见或登记结果保证。官方规则与表单请在实际提交时核实。

## 设计来源

本项目以原创流程说明统一三个上游项目的有益设计，未打包其实现代码。来源、取舍和审阅范围见 [融合记录](references/source-review.md)。本版提供可独立安装的流程型 Skill；具体材料由 Agent 使用可用工具生成。

- [wjttdbx/patent-skill](https://github.com/wjttdbx/patent-skill)：案件证据与申请包审计。
- [Gl0Haven/patent-drafting-skill](https://github.com/Gl0Haven/patent-drafting-skill)：材料审阅、检索知情访谈与起草前对齐。
- [Fokkyp/SoftwareCopyright-Skill](https://github.com/Fokkyp/SoftwareCopyright-Skill)：真实源码、业务理解与用户确认节点。
