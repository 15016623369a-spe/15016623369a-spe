# Project Details

本文件用于补充个人主页中三个核心项目的技术细节。所有样本量、模型指标与工程验收结果均来自现有 Notebook 或项目记录，不包含虚构成果。

## 1. Credit Default Prediction for Risk Decision Support

### Problem

该项目面向贷前风险筛查，目标是根据借款人的收入、信用历史、负债、贷款信息等特征识别潜在违约风险，并通过模型解释方法支持风险判断复核。

### Data & Pipeline

- 数据规模：7,500 条记录、18 个字段。
- 目标变量：`Credit Default`；其中违约样本 2,113 条，占 28.2%。
- 对年收入、信用评分、最近逾期月份、破产次数等缺失字段进行识别和处理。
- 完成数值特征填补与标准化、类别特征独热编码，并采用固定 8:2 训练/测试划分。
- 仅在训练集使用 SMOTE，重采样后训练集规模为 8,656 条。

### Modeling

项目建立了多层次模型 Benchmark，包括线性评分基线、SVM、Gaussian Naive Bayes、Random Forest、XGBoost 和 LightGBM，并对部分模型使用 3 折 GridSearchCV 搜索超参数。

评估指标统一采用 ROC-AUC。Notebook 留存结果中：

- 最高测试 AUC：0.7688。
- 树模型中随机森林测试 AUC：0.7492。

### Interpretability

使用 SHAP 汇总图、依赖图与交互分析观察信用历史、年收入、月债务、信用评分等变量对模型输出的影响方向与交互关系，使项目从单纯预测扩展到可解释风险分析。

### Engineering Improvements Planned

- 使用 `Pipeline` / `ColumnTransformer` 将预处理严格封装在交叉验证内部。
- 引入分层划分、PR-AUC、Recall、F1 与业务成本矩阵。
- 对阈值进行基于风险成本的校准，而非只比较排序指标。
- 重新整理环境依赖并复跑完整 SHAP 分析。

## 2. Hourly Bike Demand Forecasting with Ensemble Regression

### Problem

该项目预测共享单车每小时租赁量，为车辆调度、站点补给与高峰时段资源配置提供量化参考。与信贷项目不同，本项目重点处理连续目标、时间周期特征以及非线性需求关系。

### Data & Feature Engineering

- 数据规模：10,886 条小时级记录。
- 从 `datetime` 中提取月、日、星期与小时特征。
- 通过相关性分析识别 `temp` 与 `atemp` 的高度相关性，并删除体感温度字段以降低共线性。
- 使用固定 8:2 训练/测试划分。
- 仅在训练集上使用 IQR 处理湿度异常值，从 8,708 条训练样本中剔除 18 条。

### Modeling Strategy

项目按照“可解释基线 → 非线性模型 → 集成学习”的路径推进：

1. Linear Regression、Lasso、Elastic Net 用于建立线性与正则化基线。
2. SVR、KNN、Decision Tree 用于比较不同非线性假设。
3. Random Forest、XGBoost、LightGBM 用于捕捉时间、天气与需求量之间的复杂关系。
4. Stacking 与加权融合用于探索不同树模型之间的互补性。

经 5 折 GridSearchCV，调参后 XGBoost 测试 RMSE 为 53.1942。Notebook 还保留了 RF/XGBoost/LightGBM = 0/0.8/0.2 的探索性加权结果，对应 RMSE 52.9433。

> 注意：加权系数是在当前留出数据上搜索得到，因此 52.9433 应视为探索性结果，不作为独立泛化能力结论。后续应使用验证集或 OOF 预测确定融合权重。

### Interpretability

通过 SHAP 与 PDP 分析小时、温度、月份、湿度等特征的边际效应与交互关系，解释高峰时段和气象条件对需求变化的非线性影响。

### Engineering Improvements Planned

- 采用时间顺序划分或 rolling validation，避免随机划分弱化时间外推难度。
- 使用 OOF 预测训练 Stacking 元模型和融合权重。
- 增加 RMSLE、MAE 及高峰时段分组评估。
- 将特征生成、训练、评估和解释封装为可复现实验管线。

## 3. LLM Multi-Agent Delivery Orchestration

### Problem

互动剧情应用包含多场景状态、角色素材、交互流程和响应式界面。随着场景增加，单次大范围修改容易破坏已验收内容，因此需要建立职责明确、范围受控、结果可验证的 Agent 协作方式。

### Orchestration Architecture

采用 Supervisor–Executor–Reviewer 的串行协作结构：

- **Supervisor / Reviewer — Codex：**读取规则和必要上下文，拆解任务，定义修改范围和验收标准，审查实现并触发返工。
- **Executor — Claude Code：**根据受约束任务说明完成 React/TypeScript、样式、动画和场景交互实现。
- **Human-in-the-loop：**负责产品方向、剧情体验、视觉判断及最终验收决策。

串行执行可以避免两个 Agent 同时修改相同文件，并通过结构化反馈将审查问题转化为下一轮定向修复任务。

### Delivery Controls

- 将自然语言剧情拆解为状态转换、交互条件、资产映射和验收矩阵。
- 通过上下文读取范围和修改白名单限制 Agent 的操作边界。
- 在缺少 Git 基线时使用文件校验和保护已通过的 Scene 01–04。
- 对 AI 视频和人物素材设计固定接入位置及缺失资源 fallback。
- 建立构建、浏览器端到端回归、窄屏布局、控制台错误和关键交互检查。

### Verified Result

- Vite 最终构建通过，共 47 个模块。
- 浏览器完整跑通 Scene 01→06。
- 390×844 窄屏无横纵溢出。
- 浏览器控制台错误和未捕获错误均为 0。
- Scene 01–04 文件校验和与基线一致。

### Why It Matters

该项目的核心价值并非“使用了多个 AI 工具”，而是将 Agent 纳入受约束的软件交付流程：通过角色分工、上下文控制、可验证输出和反馈闭环，提高复杂需求的执行可控性。
