<div align="center">
  <img src="./avatar.jpg" width="112" alt="Jace 的头像" />
  <h1>Jace</h1>
  <p><strong>Applied Statistics · Sichuan University</strong><br />AI Agent Engineering · Forward Deployed Engineering · Information Fusion</p>
</div>

四川大学应用统计学硕士研究生，研究方向为信息融合。我把统计建模与评估用在 AI 应用开发中，关注三个工程问题：结果能否核对、操作能否控制、任务失败后能否恢复。

## 代表项目

### [见数 · Text2SQL Agent](https://github.com/15016623369a-spe/text2sql-agent)

面向业务数据的自然语言问数与表格分析助手。支持 SQLite 多轮问数，以及 CSV、Excel 的筛选、聚合、公式核对和绘图。查询结果可展开查看实际执行的 SQL。

- 用 LangGraph 管理口径澄清和会话恢复，用 Schema RAG 选取相关表列。生成的 SQL 先经 SQLite `EXPLAIN` 试跑，再以只读权限执行。演示写操作须经显式审批，上传文件在隔离容器内分析。
- 在 BIRD Mini-Dev 的 200 道留出题上，候选表扩展将最终表召回率从 **94.48% 提高到 98.09%**。另一次 50 道开发题对照中，加入 SQL 纠错后，执行准确率仍为 **26/50**。

`Python` · `FastAPI` · `LangGraph` · `Streamlit` · `SQLite` · `Docker Compose`

[本地演示](https://github.com/15016623369a-spe/text2sql-agent#快速开始) · [检索评测](https://github.com/15016623369a-spe/text2sql-agent/blob/main/docs/v2-schema-recall-fix.md) · [SQL 执行对照](https://github.com/15016623369a-spe/text2sql-agent/blob/main/docs/v3-step5-dry-run.md)

### [学以成片 · 短视频教学 Agent](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent)

把 Markdown 学习笔记转成可审阅、可修改的教学视频。工作台保留知识点对应的原文引文，支持逐镜修改讲稿和画面、试做单镜，再合成配音、字幕、视频和封面。

- 本地工作台 **v0.2.0** 支持修订历史、任务恢复和有界重试。修改某一镜后，仍有效的产物可从缓存复用。

`Python` · `React` · `Remotion` · `FFmpeg` · `Qwen3-TTS` · `Whisper`

[工作台与示例](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent#已经可以做什么) · [架构与执行机制](https://github.com/15016623369a-spe/xue-yi-cheng-pian-agent/blob/main/docs/public/ARCHITECTURE.md)

### [ReGlyph PDF · 本地 PDF 文字编辑器](https://github.com/15016623369a-spe/reglyph-pdf)

在 OCRPDF-TO-PPT 基础上维护并扩展桌面编辑器，让扫描件和扁平化宣传册中的文字重新可编辑。工作台把识别、替换预览、底图修复与文字排版串起来，支持导出带文字图层的 PDF 和可编辑的 PowerPoint 文字框。

- 严格替换模式校验选区外像素一致性，失败或取消时回滚该批操作。可移植工程保存原页、修复底图与文字图层，便于对照和继续编辑。

`Python` · `PySide6` · `PaddleOCR` · `LaMa`

[工作台与使用说明](https://github.com/15016623369a-spe/reglyph-pdf/blob/main/README.zh-CN.md) · [上游与新增部分](https://github.com/15016623369a-spe/reglyph-pdf/blob/main/THIRD_PARTY_NOTICES.md)

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
