# China Patent & Software Copyright Skill

## 中国发明专利与软件著作权（软著）一体化 Skill

**把论文、代码、技术交底、实验与产品资料，整理成有证据可追溯的专利和软著材料。**

本 Skill 为支持 Agent Skills 的 AI 助手提供统一入口：先阅读真实材料、梳理技术与业务事实，再分别推进中国发明专利起草或软件著作权材料整理。适合研发人员、科研团队、软件开发者，以及需要与发明人或代理人共同完善材料的人。

An evidence-first Agent Skill for **Chinese invention patent drafting** and **software copyright registration materials**. It guides material review, prior-art comparison, technical disclosures, claims and specifications, software user manuals, and real source-code selection through shared evidence records and quality checks.

**调用名**：`china-patent-software-copyright` · **Skill 版本**：`1.0.1` · **作者**：[XavierLin19](https://github.com/XavierLin19) · **许可证**：[MIT](LICENSE)

[快速开始](#快速开始) · [准备材料](#准备哪些材料) · [使用示例](#可直接复制的使用示例) · [工作流程](#两条工作流程) · [输出物](#会获得哪些输出物) · [常见问题](#常见问题)

## 一分钟了解

| 你的需求 | Skill 如何推进 | 可以获得的成果 |
|---|---|---|
| 有代码、论文或实验资料，希望梳理发明点 | 阅读材料，建立技术图谱，检索相关现有技术，列出发明人需要回答的问题 | 发明点候选、技术特征对比、证据索引、待确认清单 |
| 技术方案明确，需要交底书或申请文件草稿 | 确认保护方向与起草范围，依据已支持事实起草并审查一致性 | 技术交底草稿，或权利要求、说明书、摘要与附图说明草稿 |
| 软件已开发，需要整理软著材料 | 阅读真实项目，确认业务理解、申请字段和代表性代码，再编写手册与材料草稿 | 申请信息辅助稿、操作手册、代码来源清单及源码材料草稿 |
| 同一项目同时准备专利和软著 | 共用来源与事实台账，分别审查技术方案和软件功能 | 两套材料及共同的证据记录 |
| 已有草稿，需要补材料或纠错 | 核对版本与冲突，定位受影响段落、权项或功能 | 修订草稿、变更摘要和仍需确认的事项 |

**本仓库提供流程说明、模板、证据规则和质量检查清单，由你的 Agent 执行。**联网检索、PDF/Office 阅读、截图、源码抽取和 Word/PDF 导出使用 Agent 可用的工具，具体能力取决于运行环境。首次使用可以只提供项目路径和目标，不必先自行总结创新点。

## 核心设计

- **一个入口，两条子流程**：专利和软著分别有阶段说明，共用材料登记、事实状态与修订轨迹。
- **事实能定位到来源**：关键陈述对应来源 ID 与事实 ID，并回指页码、段落、图表、文件路径或代码位置。
- **先审阅，再起草**：材料未读、信息缺失或来源冲突时，先形成问题清单。
- **确认节点具体可审阅**：专利确认保护方向与起草范围；软著确认业务理解、申请字段、代码选择和完整草稿。
- **使用真实项目代码**：从已有源码选取材料，并记录选择理由、版本与排除范围。
- **事实状态明确**：区分材料支持、推断、用户确认、未知、冲突和未纳入审阅的内容。
- **质量检查覆盖两类材料**：检查权项支持、术语与图文关系、软件身份和版本、代码来源、手册内容及实际导出格式。

## 快速开始

### 1. 安装 Skill

Codex 的默认用户 Skill 目录为 `~/.codex/skills/`。若配置了其他目录，请使用对应位置。以下命令适用于目标文件夹尚不存在的情况。

**Windows PowerShell**

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\skills" | Out-Null
git clone https://github.com/XavierLin19/china-patent-software-copyright-skill.git "$env:USERPROFILE\.codex\skills\china-patent-software-copyright"
```

**macOS / Linux**

```bash
mkdir -p "$HOME/.codex/skills"
git clone https://github.com/XavierLin19/china-patent-software-copyright-skill.git "$HOME/.codex/skills/china-patent-software-copyright"
```

**不使用 Git**

从 [Releases](https://github.com/XavierLin19/china-patent-software-copyright-skill/releases) 下载发布包，或在仓库主页选择 **Code → Download ZIP** 获取主分支文件。将实际 Skill 文件夹命名为 `china-patent-software-copyright`，复制到你的 Agent 技能目录。安装后的结构应为：

```text
<Agent 技能目录>/
└── china-patent-software-copyright/
    ├── SKILL.md
    ├── agents/
    ├── patent/
    ├── software-copyright/
    ├── references/
    └── templates/
```

其他支持 Agent Skills 的助手（如 Claude Code）可使用其配置的项目级或用户级技能目录。确保 `SKILL.md` 位于 Skill 文件夹根层。复制后刷新技能列表，或启动新会话。

### 2. 指定项目与目标

在 Codex 中发送：

```text
请使用 $china-patent-software-copyright。
材料目录：D:\Projects\MyApp
目标：先判断适合整理哪些发明专利与软著材料。
请先阅读真实项目，建立来源清单、证据台账和待确认问题，
说明下一步需要我补充什么，并将产物写入项目内的 ip-work 目录。
```

也可以直接提出“帮我整理技术交底书”或“准备软著材料”等需求，由支持自动技能选择的 Agent 识别。不同助手的显式调用方式可能不同；无法识别 `$` 调用时，明确指定 Skill 名称或让助手读取安装目录中的 `SKILL.md`。

### 3. 审阅第一轮结果并逐步确认

第一轮通常先形成材料清单、技术/业务理解、来源与事实台账，以及影响下一步的关键问题。专利工作按阶段推进检索对比；软著工作会确认软件身份、业务口径和代码候选范围。根据具体事项确认或补材料后，再推进目标文档。

## 准备哪些材料

不必一次提供全部材料。给出可访问路径或上传当前已有资料，并指出权威版本、保密范围和希望得到的文件即可。

| 材料 | 有助于确认的内容 |
|---|---|
| 论文、技术交底、设计文档 | 技术对象、问题、方案、术语与实施方式 |
| 实验/仿真报告、数据、日志 | 实施条件、结果依据、参数、效果与适用边界 |
| 代码仓库、README、需求/产品文档 | 已实现流程、真实功能、软件入口与使用场景 |
| PDF、Word、PPT、图片、表格 | 公式、流程图、附图、界面与说明文字 |
| 已有专利或软著草稿 | 需要保留的表述、版本差异与修订范围 |
| 用户或发明人补充说明 | 原材料未说明的事实、项目身份、日期与关键选择 |

**专利任务**：尽量提供可复核的技术方案、实际实现/实验资料、已公开或已申请的时间线，以及可向发明人确认问题的渠道。有公式或附图时，可提供能保持版式的 PDF；材料无法完整读取时，会记录限制并请求可读副本。

**软著任务**：提供真实源码、软件名称/版本候选、产品或操作说明、界面资料，以及著作权主体、开发与发表情况。名称、日期、权属和运行环境等关键事实由用户确认；未提供时保留待填写项。

## 可直接复制的使用示例

### 场景 A：从论文或项目资料梳理发明点

```text
请使用 $china-patent-software-copyright，阅读 D:\Research\ProjectA。
材料包括论文、仿真报告和代码。目标是梳理中国发明专利候选方向。
请先建立问题—技术手段—效果图谱，登记来源与未知项，
检索并对比相关现有技术，再给我一页保护方向对齐卡。
实验效果只能采用材料中可定位的结果；缺失项列为待发明人确认。
```

### 场景 B：完善交底书与申请文件草稿

```text
请使用 $china-patent-software-copyright，审阅交底书和补充材料。
目标是完善权利要求、说明书、摘要及附图说明草稿。
请先检查技术事实、权项支持和材料冲突，向我确认保护方向与起草范围，
再完成草稿、证据支持映射和未关闭问题清单。
```

### 场景 C：从真实软件项目整理软著资料

```text
请使用 $china-patent-software-copyright，阅读 D:\Projects\MyApp。
目标是软著申请材料草稿，包括申请信息、操作手册和源码材料。
请先形成有证据来源的业务理解，列出需要确认的字段和代码候选。
名称、版本、权属、日期和运行环境由我确认；源码只从本项目抽取。
确认草稿后，再根据可用工具导出 DOCX 或其他约定格式。
```

### 场景 D：同一个项目准备两类材料

```text
请使用 $china-patent-software-copyright，为当前项目同时整理专利与软著材料。
共用来源清单、事实台账和待确认清单，分别执行两条子流程。
先说明哪些技术和功能已有材料支持、哪些需要补充，
再给出专利方向建议和软著材料计划，逐步确认后起草。
```

## 两条工作流程

### 中国发明专利

1. **预检与接入**：明确任务和材料版本，检查文档、图表、公式、代码与结果，建立技术图谱。
2. **检索与差异分析**：保留查询、来源、筛选与全文核验状态，比较本案特征、差异与效果依据。
3. **访谈与创新对齐**：提出关键问题，提供保护主题、差异特征、支持效果与风险供用户确认。
4. **起草范围对齐**：确认文件范围、权利要求类别、实施方式、附图、脱敏与交付格式。
5. **起草与审计**：依据已支持事实组织草稿，检查权项支持、术语/数字/符号一致性、图文对应和未决事项。

详见 [专利工作流](patent/WORKFLOW.md)。检索能力、全文可得性与已审阅范围都会写明；只读到摘要或元数据的文献不会被描述成已完成全文核验。

### 中国软件著作权

1. **识别真实项目**：确认项目、材料目标、软件身份候选及第三方/敏感内容边界。
2. **形成业务理解**：阅读需求、README、入口、界面与核心代码，整理有来源的功能和典型流程，交用户确认。
3. **确认字段与代码选择**：列出待填申请事实、代码候选与选择理由，记录确认和抽取范围。
4. **制作材料草稿**：编写申请信息和用户操作手册，选取真实源码，使用真实截图或保留待补位置。
5. **导出与验收**：按可用工具生成约定格式，检查软件名/版本、代码来源、实际分页、页眉与手册可读性。

详见 [软著工作流](software-copyright/WORKFLOW.md)。官方字段、页数与格式口径在实际提交时核实，代码排版以实际导出件检查为准。

## 会获得哪些输出物

根据任务范围裁剪输出，不要求每次生成完整材料包。

| 类别 | 典型输出 |
|---|---|
| 共用记录 | 来源清单、审阅记录、证据台账、待确认问题、版本/变更摘要、质量检查结果 |
| 专利分析 | 技术图谱、现有技术对比与检索边界、发明点候选、保护方向对齐卡 |
| 专利草稿 | 技术交底书，或按确认范围起草的权利要求、说明书、摘要、附图说明与支持映射 |
| 软著草稿 | 业务理解、申请信息、操作手册、代码候选与来源清单、真实源码材料、截图记录 |
| 导出文件 | 按 Agent 能力与约定生成 Markdown、TXT、DOCX 或 PDF；不可用格式会明确说明 |

默认在当前工作区建立 `ip-work/<案件标识>/`，也可由你指定。内部证据记录与可交付文件分别存放：

```text
ip-work/<案件标识>/
├── source-index.md         # 来源、版本、读取范围
├── evidence-ledger.md      # 事实、依据、状态与使用位置
├── questions.md            # 待确认或缺失事项
├── review-notes.md         # 材料审阅记录
├── patent/                 # 专利分析与草稿
├── software-copyright/     # 软著分析与草稿
└── delivery/               # 约定交付文件
```

## 证据追踪与防止虚构

来源使用 `S001…`、事实使用 `F001…`、问题使用 `Q001…` 编号。

| 状态 | 含义 |
|---|---|
| `SUPPORTED` | 材料直接支持，且能定位到具体内容 |
| `INFERRED` | 从已登记事实推导的解释，保留前提与推理 |
| `USER_CONFIRMED` | 用户明确确认，尚无独立材料佐证 |
| `UNKNOWN` | 信息不足或无法判定 |
| `CONFLICT` | 不同来源互相矛盾 |
| `OUT_OF_SCOPE` | 尚未审阅或不属于本次范围 |

例如，日志显示某项结果时会登记具体记录与适用条件；只有推测的性能改进则保留为推断或待验证。软件功能需要项目依据，代码材料需要真实源码，截图需要实际界面。材料和用户确认均未支持的参数、因果关系、实验数据、日期或权属不会自行补齐。

详细规则见 [证据台账](references/evidence-ledger.md)、[事实完整性](references/fact-integrity.md) 和 [质量门禁](references/quality-gates.md)。关键确认或材料缺口未解决时，当前产物会标明阶段与待办，便于继续补充。

## 仓库结构与模板

```text
china-patent-software-copyright/
├── SKILL.md                          # 唯一技能入口与共同规则
├── agents/openai.yaml                # Codex 展示信息与调用设置
├── patent/WORKFLOW.md                # 中国发明专利子流程
├── software-copyright/WORKFLOW.md    # 中国软件著作权子流程
├── references/
│   ├── evidence-ledger.md            # 来源/事实/问题登记
│   ├── fact-integrity.md             # 防止虚构与冲突处理
│   ├── quality-gates.md              # 两类材料质量检查
│   └── source-review.md              # 上游来源与融合取舍
├── templates/
│   ├── intake.md                     # 统一接入表
│   ├── evidence-ledger.md            # 统一台账模板
│   ├── patent/                       # 交底书骨架、对齐卡
│   └── software-copyright/           # 业务理解、申请信息、手册、代码来源
└── LICENSE
```

模板用于收集事实和组织草稿，填写方法见 [模板说明](templates/README.md)。官方表单以提交时系统为准。

## 常见问题

**需要安装 Python、Conda 或固定 Office 后端吗？**

加载这套流程型 Skill 本身不需要。材料阅读、截图、源码处理或导出若需要额外工具，助手应说明所需能力和限制，再按用户允许的方式执行。

**没有交底书，只有论文、代码或实验记录，可以用吗？**

可以从材料阅读、发明点候选和缺口清单开始。资料不足时提出可回答的问题，再逐步确认技术事实和保护方向。

**只有产品设想，没有真实源码，可以生成软著代码材料吗？**

代码材料阶段需要真实项目源码。可以先整理已有产品资料和待补事项，后续根据实际源码继续；不会生成代码来冒充已完成的软件。

**为什么会先确认，再生成完整文档？**

保护方向、软件身份、关键技术事实和代码选择会影响后续材料。先展示可审阅的对齐卡、字段表或候选清单，让修改发生在依赖这些选择的起草之前。

**材料会默认上传到外部服务吗？**

本 Skill 要求在指定工作区处理，并在向外部服务发送私有材料前取得许可。你使用的 AI 助手和工具自身的数据处理方式，仍由其设置与服务规则决定。

**如何更新安装？**

通过 Git 安装且没有需保留的本地修改时，可在安装目录运行 `git pull --ff-only`。ZIP 安装者可备份自己的修改后使用新发布包更新。主分支上的文档可能比已发布版本更新。

**整理好的材料是否就能直接提交？**

本 Skill 提供材料起草与整理辅助。实际提交前需核实事实、当前官方格式与办理要求；保护范围和法律判断应由具备相应专业能力的人员复核。

## 设计来源与致谢

本项目借鉴以下上游的有效设计，以原创流程说明形成统一入口和共享证据规则。当前仓库提供独立可用的流程型 Skill；各上游的工具实现与自动化能力请分别查看其仓库。

- [wjttdbx/patent-skill](https://github.com/wjttdbx/patent-skill)：案件证据追踪、申请材料审计与修订记录。
- [Gl0Haven/patent-drafting-skill](https://github.com/Gl0Haven/patent-drafting-skill)：材料审阅、检索背景、发明人访谈与起草前对齐。
- [Fokkyp/SoftwareCopyright-Skill](https://github.com/Fokkyp/SoftwareCopyright-Skill)：真实项目业务理解、源码选择与用户确认节点。

来源与融合取舍见 [审阅记录](references/source-review.md)。欢迎通过 [Issues](https://github.com/XavierLin19/china-patent-software-copyright-skill/issues) 提供使用反馈；公开反馈请使用脱敏案例。

**搜索关键词 / Search keywords**：中国发明专利、专利撰写、专利查新、技术交底书、权利要求、专利说明书、软件著作权、软著、软著申请、源代码鉴别材料、软件操作手册、CNIPA、China patent、Chinese invention patent、patent drafting、software copyright、Agent Skills、Codex Skill、Claude Code Skill。
