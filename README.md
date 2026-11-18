<div align="center">
  <img src="./avatar.jpg" width="112" alt="Jace 的头像" />
  <h1>Jace</h1>
  <p><strong>Applied Statistics · Sichuan University</strong><br />AI Agent Engineering · Forward Deployed Engineering · Information Fusion</p>
</div>

四川大学应用统计学硕士研究生，研究方向为信息融合。我把统计建模与评估用在 AI 应用开发中，关注三个工程问题：结果能否核对、操作能否控制、任务失败后能否恢复。

## 代表项目

### [见数 · Text2SQL Agent](https://github.com/15016623369a-spe/text2sql-agent)

**项目介绍**  
见数把数据库问数和文件分析放进同一个工作台。用户可以用自然语言查询 SQLite 或 PostgreSQL，也可以上传 CSV、Excel 做筛选、聚合、公式核对和绘图。数据库查询完成后，界面会展示本次使用的业务口径、实际执行的 SQL 和结果，方便复核或继续追问。

**技术栈**  
`Python` · `FastAPI` · `LangGraph` · `Streamlit` · `Plotly` · `SQLite / PostgreSQL` · `pandas` · `Docker Compose`

**项目亮点**

- **多轮口径澄清与 SQL 试跑。** LangGraph 保存已确认的时间、地区和指标口径，刷新后可恢复会话。Schema RAG 检索相关表列并补齐外键桥接表，生成的 SQL 通过 `EXPLAIN` 检查后以只读权限执行。演示库的订单备注修改需人工审批。
- **复杂表格分析与计算隔离。** 支持选择工作表、确认多级表头，并按指定单位和容差逐行核对公式。文件计算在关闭网络的容器内运行，可输出统计表、交互图表和可下载结果。
- **真实执行结果评测。** 在 BIRD Mini-Dev 的 200 道留出题上，新旧版本各运行三次，SQL 执行准确率中位数从 **32.5% 提升至 39.5%**。评测记录同时公开结果匹配口径、失败分类、延迟与 token 开销。

[逐步演示](https://github.com/15016623369a-spe/text2sql-agent/blob/main/docs/demo.md) · [快速启动](https://github.com/15016623369a-spe/text2sql-agent#快速启动) · [评测与复现](https://github.com/15016623369a-spe/text2sql-agent/blob/main/docs/ci.md#产品路径-bird-评测)

### [Synapse · 知脉 | 本地知识库与研究助手](https://github.com/15016623369a-spe/synapse-zhimai)

**项目介绍**  
面向个人研究与学习的自托管知识库。将 PDF、Word、网页和笔记导入后，可以用自然语言提问、沿引用回看原文，或在交互式 3D 知识图谱里按主题浏览相关资料。

**技术栈**  
`Python` · `FastAPI` · `React` · `TypeScript` · `SQLite` · `Three.js` · `PyMuPDF / RapidOCR` · `MCP`

**项目亮点**

- **混合检索与引文溯源。** 结合 BM25、向量检索与 RRF，为候选文档分配片段额度。回答附可展开的原文引用，多轮追问可沿用当前讨论的文档。
- **本地索引与可视化组织。** 扫描版 PDF 支持 OCR，知识库和索引存储在本地。离线模式支持检索和原文摘录，分区图谱可按研究主题组织文档、概念和标签。
- **可控的外部资料补充。** 可配置只读 MCP 工具查询文件系统、SQLite 和 GitHub。联网补查返回候选资料，由用户选择并确认入库。

[界面预览](https://github.com/15016623369a-spe/synapse-zhimai#这是什么) · [核心特性](https://github.com/15016623369a-spe/synapse-zhimai#核心特性) · [本地启动](https://github.com/15016623369a-spe/synapse-zhimai#快速开始)

### [学以成片 · 短视频教学 Agent](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent)

**项目介绍**  
把 Markdown 学习笔记改编成面向指定受众的教学视频。工作台先生成知识点、讲稿和分镜，创作者可以逐镜调整讲解、图解和术语读法，试做并审阅后再导出配音、字幕、成片和横竖版封面。

**技术栈**  
`Python` · `JavaScript` · `SQLite` · `Remotion / React` · `MLX` · `Qwen3-TTS` · `Whisper` · `FFmpeg / FFprobe`

**项目亮点**

- **从分镜回溯到学习原文。** 模型选择来源行号，程序回填并校验逐字引文。分镜关联对应知识点，并记录类比的适用边界，便于创作者审阅讲解内容。
- **字幕和画面事件对齐真实口播。** 用 Qwen3-TTS 本地配音，通过 Whisper 转写生成时间戳。字幕、重点弹字和图解切换沿用同一口播时间基准，再由 Remotion 和 FFmpeg 渲染、混音与合成。
- **逐镜修改与增量重做。** 任务保存分镜修订和各阶段结果，失败后可有限重试。改动某一镜的讲稿后，只重跑受影响的配音、转写、对齐和渲染阶段，再合成整片。其他镜头的有效产物经过校验后可复用。

[工作台预览](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent#工作台预览) · [架构与执行机制](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent/blob/main/docs/public/ARCHITECTURE.md) · [本地试用](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent#快速开始)

### [ReGlyph PDF · 本地 PDF 文字编辑器](https://github.com/15016623369a-spe/reglyph-pdf)

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
