<div align="center">
  <img src="./avatar.jpg" width="112" alt="Jace 的头像" />
  <h1>Jace</h1>
  <p><strong>Applied Statistics · Sichuan University</strong><br />Agent Harness · Agentic RAG · Forward Deployed Engineering</p>
</div>

四川大学应用统计学硕士研究生，研究方向为信息融合。我把统计建模与评估用在 AI 应用开发中：既写模型外面那层运行时（harness），也把 Agent 交付到真实业务里。关注的三个工程问题——结果能否核对、操作能否控制、任务失败后能否恢复——是下面每个项目的验收标准。

写代码的习惯：先定判分规则，再跑对照；边界用结构表达，不用提示词。

## 代表项目

最近的重心在 harness：先把运行时做成可回放、可审批、可续跑的东西，再谈模型能力。

### Agent 运行时与评测

#### [RepoLedger · 可续跑的仓库知识库 Agent](https://github.com/15016623369a-spe/repoledger)

**项目介绍**  
从 Git 仓库生成带源码出处的 Wiki，按变更增量更新页面，再通过只读 MCP 交给 Claude Code 查询。运行时把模型调用、工具结果和人工审批写入追加式事件日志：中断后可以续跑，完成后可以回放。核心仅使用 Python 3.10+ 标准库，离线可跑。

**技术栈**  
`Python · 标准库` · `JSONL 事件日志` · `MCP（stdio JSON-RPC）` · `Git diff + SHA-256` · `Playwright`

**项目亮点**

- **运行状态只从日志推导。** `events.jsonl` 是唯一事实来源：有工具调用但缺结果时，`resume` 只补未完成的调用；模型请求中断则按日志恢复重试。需要旧浏览器会话的调用交回模型重新规划。
- **审批在副作用之前。** 待批准的工具调用先写 `approval/request` 并退出进程，人工通过 CLI 追加 `approval/decision`，`resume` 继续执行；拒绝同样留痕。
- **上下文预算可解释。** 轮次、工具次数、输入 token 与运行时长都有上限；超长工具结果截断后存盘，模型用 `read_artifact` 取回；摘要保留原事件序号，`replay` 能回查原始证据。
- **评测给出的是否定结论。** 30 道仓库问答中 27 对可比：源码组 27/27，Wiki 组 25/27；40 道 Wiki 净收益试验为 28/40 对 30/40，配对差 95% 区间 −7.5 至 +17.5 个百分点。数据不足以证明 Wiki 稳定提高正确率，所以默认检索源码，Wiki 由使用者显式开启。

[运行时设计与边界](https://github.com/15016623369a-spe/repoledger/blob/main/docs/runtime.md) · [第四周报告](https://github.com/15016623369a-spe/repoledger/blob/main/docs/week4.md) · [演示](https://github.com/15016623369a-spe/repoledger/blob/main/docs/agent-core-demo.mp4)

#### [whetstone · Agent Harness 消融实验室](https://github.com/15016623369a-spe/whetstone)

**项目介绍**  
不是又一个 Agent，而是回答一个问题：同一个模型、同一批任务，把 harness 的某个组件换上或关掉，成绩会变多少？12 个任务、6 种配置、两个模型，用 `pass^k` 计分；不接模型也能离线跑通机制自检。

**技术栈**  
`TypeScript · Node.js` · `插件化 kernel（可逆注册）` · `node:test` · `DeepSeek / GLM`

**项目亮点**

- **日志在模型的上游。** 模型可见消息是事件日志的纯函数，循环不持有对话状态，每一步重新推导；压缩只追加 `compaction/mark` 隐藏区间，从不删除事件，所以同一会话可以关掉压缩再重放对比——模型看到的是有损的，记录不是。
- **消融是替换实现，不是加开关。** `baseline` 组合的是一个直接返回 `undefined` 的 compactor，而不是给真压缩器传 `mode: 'off'`；有测试断言「换掉插件后行为确实变了」。
- **负结果比结论更有用。** `deepseek-chat` 上压缩让输入 token **−24%**，按需加载工具 schema 反而 **+38%** token、**+31%** 步数，全开是最差配置（17,081 对 13,626 token）；`glm-4-air` 上「全开」是唯一在 `multi-bug-a` 上 0/3 的配置。所有 trial 里 subagent 工具一次都没被调用——提供能力不等于产生行为。
- **仪器先修自己。** 矩阵跑通时暴露了 4 个「看起来正常、结论其实错」的缺陷：单轮内压缩永不触发、裁剪把任务陈述挤掉、裁剪与压缩产生孤儿 tool result、压缩摘要以 system 消息发送只在某个供应商出错。后两个促成投影层的两项前置校验。
- **换一个实现再测一遍。** 同样 11 个任务交给独立实现的 RepoLedger，11/11 通过——任务在测任务，而不是在测作者。

[评测结果](https://github.com/15016623369a-spe/whetstone/blob/main/docs/results.md) · [跨实现校验](https://github.com/15016623369a-spe/whetstone/blob/main/tools/foreign-harness.py) · [局限](https://github.com/15016623369a-spe/whetstone#limitations)

### 垂直 Agent 应用

#### [见数 · Evidence-gated Agentic RAG](https://github.com/15016623369a-spe/jianshu-react)

**项目介绍**  
面向政策调查、产品手册和零售指标的垂直 Agentic RAG 工作台。系统先解析问题范围，再检索政策、公告和手册；证据不足时补查或拒答，证据齐全后才执行只读 SQL 或匹配手册章节。每个金额都能回溯到政策、公告、SQL 与绑定参数。V1 的数据库问数与表格分析保留在 [text2sql-agent](https://github.com/15016623369a-spe/text2sql-agent)。

**技术栈**  
`Python` · `FastAPI` · `LangGraph` · `Streamlit` · `SQLite / PostgreSQL` · `BM25 / BGE-M3` · `Docker Compose`

**项目亮点**

- **证据门控，而不是自由 ReAct。** 有界状态机按「范围解析 → 检索 → 证据缺口判断 → 补查 / 拒答 → 只读 SQL → 金额与引用核验」执行。正常路径不调用模型，模型只在需要改写检索词时兜底。
- **冲突与缺口在算数前拦下。** 政策和公告按商品、地区、生效期和版本校验；版本冲突、公告循环或月份覆盖缺口会在 SQL 前停止并说明原因，金额按整数分计算。
- **多域评测。** 内部 150 题（政策 60 / 手册 45 / 零售 45）：模型兜底 150/150、离线确定性 146/150、错误出数 0，引用精度与拒答正确率 100%，正常路径 P95 4.45 ms；301 项自动测试。

[三分钟演示](https://github.com/15016623369a-spe/jianshu-react/blob/main/examples/demo/v3-portfolio-demo.mp4) · [作品集案例](https://github.com/15016623369a-spe/jianshu-react/blob/main/docs/portfolio-case-study.md) · [评测报告](https://github.com/15016623369a-spe/jianshu-react/blob/main/docs/v3-m5-release-report.md)

#### [Synapse · 知脉 | 本地知识库与研究助手](https://github.com/15016623369a-spe/synapse-zhimai)

**项目介绍**  
面向个人研究与学习的自托管知识库。将 PDF、Word、网页和笔记导入后，可以用自然语言提问、沿引用回看原文，或在交互式 3D 知识图谱里按主题浏览相关资料。

**技术栈**  
`Python` · `FastAPI` · `React` · `TypeScript` · `SQLite` · `Three.js` · `PyMuPDF / RapidOCR` · `MCP`

**项目亮点**

- **混合检索与引文溯源。** 结合 BM25、向量检索与 RRF，为候选文档分配片段额度。回答附可展开的原文引用，多轮追问可沿用当前讨论的文档。
- **本地索引与可视化组织。** 扫描版 PDF 支持 OCR，知识库和索引存储在本地。离线模式支持检索和原文摘录，分区图谱可按研究主题组织文档、概念和标签。
- **可控的外部资料补充。** 可配置只读 MCP 工具查询文件系统、SQLite 和 GitHub。联网补查返回候选资料，由用户选择并确认入库。

[界面预览](https://github.com/15016623369a-spe/synapse-zhimai#这是什么) · [核心特性](https://github.com/15016623369a-spe/synapse-zhimai#核心特性) · [本地启动](https://github.com/15016623369a-spe/synapse-zhimai#快速开始)

#### [FitCheck · 语音健身记录 Agent](https://github.com/15016623369a-spe/bojililun)

**项目介绍**  
手机优先的中文健身手账，可在自己电脑上部署。说一句话、打字或拍照记录训练、饮食和体重，也可以让应用排今日训练计划；回顾页和训练统计展示容量、个人纪录、营养与热量缺口。项目由我发起和维护，与两位共创者通过 Pull Request 协作。

**技术栈**  
`Next.js 15` · `React 19` · `TypeScript` · `three.js` · `node:sqlite` · `Vercel AI SDK`

**项目亮点**

- **模型只做非结构化到结构化。** 计算、筛选和排计划都走确定性代码。未接模型时，本地规则解析动作、组数、重量和食物份量，认不准的动作按原话记录。
- **约束记忆参与决策。** 伤病和禁忌等约束类记忆始终进入上下文，并在排计划时按规则排除相关动作；记忆写入需用户确认，冲突不静默覆盖。
- **一份数据，一个口径。** 回顾页与训练统计读取同一份训练手账，同一节训练只计一次。AGENTS.md 记录产品规格、代码入口与统计口径；PR 附改动前后对照截图，CI 跑类型检查、测试与构建。

[界面一览](https://github.com/15016623369a-spe/bojililun#界面一览) · [快速开始](https://github.com/15016623369a-spe/bojililun#快速开始) · [AGENTS.md](https://github.com/15016623369a-spe/bojililun/blob/main/AGENTS.md)

### 本地创作与编辑工具

#### [学以成片 · 短视频教学 Agent](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent)

**项目介绍**  
把 Markdown 学习笔记改编成面向指定受众的教学视频。工作台先生成知识点、讲稿和分镜，创作者可以逐镜调整讲解、图解和术语读法，试做并审阅后再导出配音、字幕、成片和横竖版封面。

**技术栈**  
`Python` · `JavaScript` · `SQLite` · `Remotion / React` · `MLX` · `Qwen3-TTS` · `Whisper` · `FFmpeg / FFprobe`

**项目亮点**

- **从分镜回溯到学习原文。** 模型选择来源行号，程序回填并校验逐字引文。分镜关联对应知识点，并记录类比的适用边界，便于创作者审阅讲解内容。
- **字幕和画面事件对齐真实口播。** 用 Qwen3-TTS 本地配音，通过 Whisper 转写生成时间戳。字幕、重点弹字和图解切换沿用同一口播时间基准，再由 Remotion 和 FFmpeg 渲染、混音与合成。
- **逐镜修改与增量重做。** 任务保存分镜修订和各阶段结果，失败后可有限重试。改动某一镜的讲稿后，只重跑受影响的配音、转写、对齐和渲染阶段，再合成整片。其他镜头的有效产物经过校验后可复用。

[工作台预览](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent#工作台预览) · [架构与执行机制](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent/blob/main/docs/public/ARCHITECTURE.md) · [本地试用](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent#快速开始)

#### [ReGlyph PDF · 本地 PDF 文字编辑器](https://github.com/15016623369a-spe/reglyph-pdf)

**项目介绍**  
基于 OCRPDF-TO-PPT 扩展的桌面编辑器，用于修改扫描 PDF、扁平化宣传册和图片里的文字。工作台识别并定位原字，在选定区域修复底图，再重建可独立调整位置、字体和样式的文字框。

**技术栈**  
`Python` · `PySide6 / Qt` · `PaddleOCR` · `OpenCV` · `IOPaint / LaMa` · `PyMuPDF` · `python-pptx`

**项目亮点**

- **批量替换前逐项预览。** 预览显示页码、原文字和替换文字，用户确认后只处理勾选项。严格模式把定位不明确、容易擦到正文的候选留给人工核对。
- **限定修复区域并校验像素。** 简单底色使用快速清除，复杂纹理交给 LaMa 修复。提交前检查允许区域外的像素一致性，失败或取消时回滚该批次的文字和底图。
- **可恢复的编辑工程与多格式导出。** `.ipdfedit` 工程保留原页、修复底图和文字图层，支持撤销、重做和自动备份。可导出带文字图层的 PDF，也可导出保留可编辑文字框的 PowerPoint。

[界面与使用说明](https://github.com/15016623369a-spe/reglyph-pdf/blob/main/README.zh-CN.md) · [技术架构](https://github.com/15016623369a-spe/reglyph-pdf/blob/main/docs/architecture.md) · [上游来源与第三方说明](https://github.com/15016623369a-spe/reglyph-pdf/blob/main/THIRD_PARTY_NOTICES.md)

## 信息融合与统计建模

| 项目 | 内容 |
| --- | --- |
| [多目标跟踪实验](https://github.com/15016623369a-spe/Message-Passing-Algorithms-for-Scalable-Multitarget-Tracking-test) | 将粒子置信传播（BP）跟踪实验移植到 Python/Jupyter。另对 MT3 集合预测完成短程训练和流程验证。 |
| [Prism · 棱镜](https://github.com/15016623369a-spe/paper-prism) | 自制论文阅读技能。用可追溯阅读卡记录方法、结论和证据，维护术语库，支持跨论文比较与组会汇报。 |
| [上海房价回归实验](https://github.com/15016623369a-spe/ml-project) | 基于 3,000 条上海历史挂牌样本比较线性回归、随机森林和梯度提升。仓库提供 Notebook、数据来源与采样记录。 |
| [信贷违约预测](./PROJECT_DETAILS.md#1-credit-default-prediction-for-risk-decision-support) · [共享单车需求预测](./PROJECT_DETAILS.md#2-hourly-bike-demand-forecasting-with-ensemble-regression) | 分类与回归案例，附基线、留出集指标、模型解释和验证局限。 |

## 工程与学习记录

- [互动剧情应用交付案例](./PROJECT_DETAILS.md#3-llm-multi-agent-delivery-orchestration) · 通过 Codex 与 Claude Code 分工完成规划、实现与审查，记录场景 01–06 的浏览器回归和移动端检查。
- [Agent Learn](https://github.com/15016623369a-spe/agent-learn) · FastAPI 服务、办公任务原型、结构化输出与上下文工程练习。
- [Transformer Learning](https://github.com/15016623369a-spe/transformer-learning) · PyTorch 注意力与 Pre-Norm Block 实验。 [Hugging Learning](https://github.com/15016623369a-spe/hugging-learning) · Hugging Face 推理、生成与 KV Cache 实验。

## 联系

[15016623369@163.com](mailto:15016623369@163.com)
