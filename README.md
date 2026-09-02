<div align="center">
  <img src="./avatar.jpg" width="130" alt="头像" />
  # 黄晖杰 · Huijie Huang

  **Applied Statistics M.S. @ Sichuan University**  
  **Information Fusion · Algorithm Engineering · FDE · AI Agent Engineering**

  [![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
  [![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
  [![XGBoost](https://img.shields.io/badge/XGBoost-EB5B2A?style=flat-square)](https://xgboost.ai/)
  [![LightGBM](https://img.shields.io/badge/LightGBM-02569B?style=flat-square)](https://lightgbm.readthedocs.io/)
  [![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=mysql&logoColor=white)](https://en.wikipedia.org/wiki/SQL)
  [![AI Agent](https://img.shields.io/badge/AI-Agent_Engineering-6F42C1?style=flat-square)](#featured-projects)
</div>

---

## About Me

四川大学应用统计学硕士研究生，研究方向为信息融合。目前重点关注多源异构信息融合、多模态学习、凸优化与多 Agent 协作，并持续积累机器学习建模、模型解释和 AI 原生工程交付经验。

我的兴趣不止于训练模型，也包括把业务问题转译为可计算目标、构建可复现的数据与评估流程，并通过 Agent 编排、自动化验收和快速原型完成端到端交付。

- 🎯 **Target Roles:** Algorithm Engineer / Forward Deployed Engineer / AI Agent Engineer
- 🔬 **Research:** Information Fusion / Multimodal Learning / Convex Optimization
- 🧠 **Machine Learning:** Classification, Regression, Ensemble Learning, Model Interpretability
- 🤖 **AI Engineering:** Agent Orchestration, Context Control, Quality Gates, Human–AI Collaboration
- 📍 **Location:** Chengdu, China

## Tech Stack

| Area | Skills |
|---|---|
| Programming & Data | Python, SQL, NumPy, Pandas, Jupyter |
| Modeling | scikit-learn, XGBoost, LightGBM, SVM/SVR, Random Forest, Stacking |
| Evaluation | Cross-validation, Grid Search, ROC-AUC, RMSE, Imbalanced Learning |
| Interpretability | SHAP, Partial Dependence Plot, Feature Interaction Analysis |
| AI / FDE Engineering | Requirement Abstraction, Rapid Prototyping, Agent Orchestration, Code Review, Automated Validation |
| Engineering Tools | Git, VS Code, Codex, Claude Code, GPT, Vite/React/TypeScript Project Collaboration |

<a id="featured-projects"></a>

## Featured Projects

### 01 · Credit Default Prediction for Risk Decision Support

> 面向贷前风险识别的不平衡分类与可解释建模项目。

- 分析 **7,500 条、18 字段**借款人数据，违约样本占 **28.2%**；完成缺失模式识别、数值填补、尺度统一与类别编码。
- 采用固定 8:2 留出法，并仅在训练集应用 SMOTE，将训练数据扩充至 **8,656 条**。
- 构建线性评分、SVM、朴素贝叶斯及树模型 Benchmark，使用 3 折 GridSearchCV 和 ROC-AUC 评价；Notebook 留存最高 AUC 为 **0.7688**。
- 利用 SHAP 汇总、依赖与交互分析构建模型解释层，支持信用风险变量的影响复核。

`Python` `scikit-learn` `SMOTE` `XGBoost` `LightGBM` `SHAP`

[Read project details →](./PROJECT_DETAILS.md#1-credit-default-prediction-for-risk-decision-support)

---

### 02 · Hourly Bike Demand Forecasting with Ensemble Regression

> 面向共享单车供需调度的小时级需求预测与集成回归项目。

- 基于 **10,886 条**小时级记录，从时间戳派生月、日、星期与小时特征，并处理温度变量共线性与训练集湿度异常值。
- 从线性/正则化回归基线逐步扩展至 SVR、近邻模型与树集成模型，以 RMSE 统一评价预测误差。
- 经 5 折网格搜索，调参后 XGBoost 测试 RMSE 为 **53.1942**。
- 实现 Stacking 与异构模型加权融合，并结合 SHAP/PDP 分析小时、温度、月份和湿度的非线性效应。

`Time Feature Engineering` `Regression` `XGBoost` `Stacking` `SHAP` `PDP`

[Read project details →](./PROJECT_DETAILS.md#2-hourly-bike-demand-forecasting-with-ensemble-regression)

---

### 03 · LLM Multi-Agent Delivery Orchestration for an Interactive Story App

> 将多 Agent 从“辅助对话”转化为具备职责边界、质量门禁与返工闭环的软件交付流程。

- 设计 **Supervisor–Executor–Reviewer** 架构：Codex 负责任务规划、上下文控制和审查，Claude Code 承担受约束实现。
- 将剧情需求结构化为场景状态、资产清单、修改白名单与验收矩阵，并通过上下文隔离和文件校验和控制回归风险。
- 建立基线构建、变更审查、浏览器端到端回归、响应式检查和控制台监控等质量门禁。
- 完成 **47 个模块**构建及 Scene 01→06 全链路验证；**390×844** 窄屏无溢出，运行错误为 **0**。

`Agent Orchestration` `Quality Gate` `Codex` `Claude Code` `React` `TypeScript`

[Read project details →](./PROJECT_DETAILS.md#3-llm-multi-agent-delivery-orchestration)

## Current Exploration

### Multimodal E-commerce Design Agent

正在探索“创意 Brief → 结构化提示词 → 图片/视频资产 → 规格与一致性校验 → 应用接入”的多模态内容工作流，重点关注角色一致性、素材约束、生成失败兜底与人机协作评审。

### Information Fusion & Optimization

围绕多源异构数据的表示、对齐与联合建模持续开展文献阅读和方法学习，并关注凸优化思想在融合建模与 Agent 系统中的应用。

## Contact

- Email: `15016623369@163.com`
- GitHub: `https://github.com/15016623369a-spe`


