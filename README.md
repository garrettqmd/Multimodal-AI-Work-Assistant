# Multimodal-AI-Work-Assistant (企业级多模态 AI 工作助手)

> 基于 Dify 平台编排的企业级多模态 AI 智能体中枢。集成**全模态会议纪要处理**、**智能周报生成**与**内部规章 RAG 检索**。本项目旨在通过 Agentic Workflow（智能体工作流）与高阶 Prompt 工程，将大模型从“单点对话玩具”转化为深度嵌入企业协同生态的“工程生产力工具”。

[![Powered by Dify](https://img.shields.io/badge/Powered%20by-Dify-orange.svg)](https://dify.ai)
[![AI Models](https://img.shields.io/badge/AI%20Models-Gemini%20%7C%20Claude%20%7C%20GPT--4o-green.svg)](#)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

---

## 🎯 项目概述与业务价值

在现代企业协同中，AI 落地面临三大痛点：输入源非标碎片化（语音/截图/文档）、员工意图混淆、输出结果缺乏直接的工程可用性。

本项目构建了一个具备“感知-思考-执行”能力的中央控制 Agent，实现从**杂乱的多源数据摄入**，到**零幻觉的高级处理**，再到**自动化推送到飞书/钉钉**的全流程闭环。用人单位和开发者可通过本项目，直观体验复杂大模型工作流在真实业务场景中的技术落地能力。

---

## ✨ 核心模块与 AI 技术实践 (Core Features & AI Tech)

本项目包含三个同等权重的核心引擎，分别应对企业中最典型的高频场景：

### 🎙️ 1. 多模态产研会议智能中枢 (Multimodal Meeting & PRD Engine)
自动解析包含大量废话、跨频聊天的会议记录，并按指令生成高管摘要、飞书风格 PRD、Draw.io 架构图或任务分配单。
* **多模态数据摄入 (Data Normalization)**：结合 Python 脚本处理，抹平底层数据对象（List）与纯文本（String）的格式差异，实现对 PDF文档、语音转录(ASR)、图片(OCR) 的统一降噪与清洗。
* **“指令霸权”意图路由网关 (Zero-Shot Routing)**：基于系统级 Prompt 克服大模型处理长文本时的“注意力偏移（Attention Hijacking）”问题，实现用户显性指令高于上下文内容的精准分发。
* **XML 复杂数据流控制**：注入【视觉组件字典】，约束大模型直接输出携带 Node & Edge 拓扑关系的 `mxGraphModel` XML 代码，实现文字到工程图纸的降维转化。

### 📈 2. 智能周报与工作汇报引擎 (Intelligent Report Engine)
将员工一周零散的打卡记录、聊天流水账，提炼为符合大厂规范（STAR 法则 / 目标-进度-风险模型）的专业汇报。
* **上下文压缩与抽象摘要 (Abstractive Summarization)**：过滤情绪化与无意义文本，提升信息信噪比。
* **Persona 与 Tone-of-Voice 微调**：通过 Prompt 定制 AI 的职场角色“人格”，确保输出行文的客观性、专业性与逻辑连贯性。

### 🔍 3. 规章制度 RAG 检索库 (Enterprise RAG Knowledge Base)
精准响应如“报销标准”、“请假流程”等制度询问，杜绝大模型在此类确定性事实上的瞎编乱造。
* **检索增强生成 (Retrieval-Augmented Generation)**：结合向量库进行文档的分块 (Chunking) 与 Embedding。
* **事实边界约束**：严格限制大模型仅基于召回的 Top-K 文本片段进行生成，配备缺失兜底逻辑。

---

## 🧠 深度 Prompt Engineering 实践 (Andrew Ng's Principles)

本项目的灵魂在于对 LLM 提示词的极限压榨，严格遵循了吴恩达 (Andrew Ng) 提出的提示词工程核心原则，极大提升了系统的工业级稳定性：

1. **清晰的指令与分隔符 (Delimiters)**：使用 `{{}}`、Markdown 标题及 XML 标签作为强定界符，将角色定义、核心规则与用户杂乱的输入文本彻底物理隔离，有效防御 Prompt Injection（提示词注入）。
2. **强制结构化输出 (Structured Output)**：通过精准的 Few-shot 示例，迫使模型输出复杂且严谨的 JSON Payload（用于飞书 Webhook 交互）和 Markdown 嵌套代码块，并通过前端转义策略解决安全拦截问题。
3. **条件检查与零幻觉红线 (Zero-Hallucination & Condition Checking)**：设计了严密的兜底逻辑。例如在提取待办任务时，若上下文中未提及负责人或期限，强制模型输出高亮的 `[⚠️ 待认领]` 或 `[已延期]`，绝不允许其基于概率模型自行捏造业务数据。
4. **留出思考时间与步骤拆解 (Think Step-by-Step)**：在数据处理链条上，抛弃“一步到位”的黑盒，采用【文本降噪】➔【意图判定】➔【定向产出】的多级节点流转，显著降低了逻辑推理错误率。

---

## 🏗️ 系统架构与数据流 (System Architecture)

![系统架构图](./assets/architecture_placeholder.png) 
*(说明：请将 Dify 导出的架构流转截图替换此处的占位符图片)*

系统采用高内聚、低耦合的路由分发模式：
1. **统一输入层**：接收图文、语音、文件。
2. **处理控制层**：文档提取器 + Python 格式化脚本 + 降噪 LLM。
3. **中央调度网关**：分析动作指令，执行 4 选 1（或多分支）路由。
4. **生产线与服务层**：PRD 生成器 / Draw.io 代码生成器 / 周报生成器 / RAG 检索服务。
5. **生态触达层**：HTTP API 组装，自动化推送到飞书/钉钉企业群。

---

## 🛠️ 技术栈与依赖 (Tech Stack)

* **核心编排引擎**: Dify (Agentic Workflow)
* **大语言模型**: Gemini 1.5 Pro / Flash, Claude 3.5 Sonnet 等（支持灵活热插拔）
* **数据处理中间件**: Python 3 (内嵌脚本节点)
* **企业级 API 集成**: 飞书开放平台 Webhook / 多维表格 OpenAPI
* **前端展示语言**: Markdown, Mermaid.js, draw.io XML

---

## 🚀 快速上手 (Quick Start)

让该项目在你的本地或企业私有化 Dify 平台上跑起来：

### 环境要求
* 已部署好（或使用云端）的 Dify 平台账号。
* OpenAI / Gemini / Anthropic 等模型供应商的 API Key。
* （可选）一个飞书或钉钉的群自定义机器人 Webhook 地址。

### 部署与测试步骤
1. **克隆项目**: 下载本项目到本地。
2. **导入 DSL 工作流**: 登录 Dify，在“工作室”点击“导入 DSL 文件”，选择本项目 `workflows/` 目录下的 `.yml` 文件。
3. **配置模型与密钥**: 在导入后的画布中，统一检查并选择你的可用 LLM 模型。如有飞书推送需求，请在【HTTP 请求】节点填入你的 Webhook 地址。
4. **执行测试**: 打开项目 `test_cases/` 文件夹，复制一份杂乱的会议记录，输入聊天框，并在末尾加上指令（如：“请帮我生成画图代码”），观察惊艳的结果！

---

## 🔮 未来演进路线 (Roadmap)

- [ ] **接入多维表格**：将提取到的任务清单通过 OAuth 鉴权，直接自动化写入企业级多维数据库 (Bitable)。
- [ ] **多端渲染适配**：优化 XML 代码在不同前端聊天 UI 中的展示策略，全量引入 Mermaid 直出方案。
- [ ] **BI 数据分析助手**：扩展新路由节点，支持通过自然语言查询 SQL 并生成数据折线图。

---

## 📚 相关目录索引

* [📂 核心工作流配置 (DSL)](workflows/README.md)
* [📂 高阶提示词赏析](prompts_showcase/README.md)
* [📂 压力测试语料包](test_cases/README.md)

---
*💡 如果这个项目展示的架构思维和 Prompt 调优策略对你有启发，欢迎点亮右上角的 Star ⭐️！*
