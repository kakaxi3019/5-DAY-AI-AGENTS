# 5-Day AI Agent Course (LangChain Local Edition)

本项目是 Google Kaggle [5-Day AI Agents Course](https://www.kaggle.com/learn-guide/5-day-agents) 的**本地化重构版本**。在保证内容与原课程尽量保持一致的前提下，使用通用的 **LangChain** 框架进行改写，可以在本地运行。  

##  项目目的

原始的 Kaggle 课程依赖于 Google ADK (Agent Development Kit) 和特定的 Kaggle Notebook 环境。本项目的目的是**脱离平台依赖**，使用通用的 **LangChain** 框架在本地实现所有课程内容，为没有Gemini API Key 和 Kaggle 账号的用户提供了便利。

如果你想查看原版课程，请访问：[Google 5-Day AI Agents Course](https://www.kaggle.com/learn-guide/5-day-agents)

## 🛠️ API 资源准备

本课程选用了 ModelScope 和 Tavily 作为 API 服务，它们都提供了免费调用额度。如果你有自己的 API Key 或者本地部署的 LLM，可以替换 `.env` 文件中的相应字段。

### 1. 大语言模型 (LLM) - ModelScope
本项目默认使用 **ModelScope (魔搭社区)** 提供的 Serverless API，特别是 Qwen (通义千问) 系列模型。

*   **推荐模型**: `Qwen/Qwen3-235B-A22B-Instruct-2507` (或其他 Qwen2.5/Qwen-Max 系列)
*   **申请地址**: [ModelScope 灵积模型服务](https://modelscope.cn/my/myaccesstoken)
*   **配置**: 获取 Access Token 后，填入 `.env` 文件的 `OPENAI_API_KEY` 字段，并将 `OPENAI_API_BASE` 设置为 ModelScope 的端点（通常是 `https://api-inference.modelscope.cn/v1`）。

### 2. 搜索工具 - Tavily
本项目使用 **Tavily** 作为 Agent 的联网搜索工具（替代原课程中使用的 `google_search` 工具）。Tavily 是专为 AI Agent 设计的搜索引擎，返回结果更适合 LLM 处理，更关键的是它提供免费调用额度

*   **申请地址**: [Tavily 官网](https://tavily.com/)
*   **配置**: 注册后获取 API Key，填入 `.env` 文件的 `TAVILY_API_KEY` 字段。

### 3. 环境配置文件 (.env) 说明
项目根目录下提供了 `.env` 配置文件模板，主要参数说明如下：

```env
# === LLM Model Configuration ===
MODEL_NAME="Qwen/Qwen3-235B-A22B-Instruct-2507"       # 模型名称ID
OPENAI_API_KEY=ms-xxxxxxxxxxxx                         # ModelScope的API Token
OPENAI_API_BASE=https://api-inference.modelscope.cn/v1 # API调用地址

# === Tool Configuration ===
TAVILY_API_KEY="tvly-xxxxxxxxxxxx"                     # Tavily搜索API Key

# === Optional Parameters ===
EXTRA_BODY={"enable_thinking": false}                  # 可选：传递给模型的额外参数(JSON格式)
```

---

##  如何运行 

本项目推荐使用 [uv](https://github.com/astral-sh/uv) 进行高效的 Python 依赖管理。

### 1. 环境初始化

在终端中执行以下命令来创建和激活虚拟环境：

```bash
# 创建虚拟环境（首次执行即可）
uv venv

# 激活环境 (Mac/Linux)
source .venv/bin/activate
# Windows 用户请使用: .venv\Scripts\activate

# 安装项目依赖（首次执行即可）
uv pip install -r requirements.txt
```

### 2. 配置 Jupyter Notebook 内核

为了在 Jupyter Notebook 中正确运行代码，请创建一个专用的 Python 内核：

```bash
# 安装内核工具（首次执行即可）
uv pip install ipykernel

# 注册名为 "5-DAY-AGENTS" 的内核（首次执行即可）
python -m ipykernel install --user --name=5-DAY-AGENTS --display-name "Python (5-DAY-AGENTS)"
```

**使用方法**：打开任意 `.ipynb` 文件后，点击右上角的 **Kernel** 选择器，选择 **Python (5-DAY-AGENTS)**。

## 课程目录 (Course Directory)

本项目包含 5 天的课程内容，所有代码均已适配本地运行：

### **Day 1: Agent 基础与架构**
*   **Day 1a: From Prompt to Action** (`day1a/`)
    *   构建你的第一个 Agent，学习 ReAct 模式（推理+行动），并使用工具进行网络搜索。
*   **Day 1b: Agent Architectures** (`day1b/`)
    *   探索多 Agent 协作系统，掌握 **顺序 (Sequential)**、**并行 (Parallel)** 和 **循环 (Loop)** 三种核心工作流。

### **Day 2: 工具使用 (Tool Use)**
*   **Day 2a: Agent Tools** (`day2a/`)
    *   学习如何将自定义 Python 函数封装为工具，并赋予 Agent 编写和执行代码的能力。
*   **Day 2b: Tool Best Practices** (`day2b/`)
    *   集成外部标准工具 (MCP) 以及处理 Human-in-the-Loop (人机交互) 场景。

### **Day 3: 记忆管理 (Memory)**
*   **Day 3a: Agent Sessions** (`day3a/`)
    *   管理短期会话记忆 (Short-term Memory)，利用数据库持久化对话状态。
*   **Day 3b: Agent Memory** (`day3b/`)
    *   构建长期记忆 (Long-term Memory) 系统，让 Agent 能够存储和检索跨会话的关键信息。

### **Day 4: 可观测性与评估**
*   **Day 4a: Observability** (`day4a/`)
    *   了解如何通过日志 (Logs) 和追踪 (Traces) 来调试 Agent 的行为。
*   **Day 4b: Evaluation** (`day4b/`)
    *   建立评估数据集，使用 LLM-as-a-Judge 模式对 Agent 的表现进行自动化打分。

### **Day 5: 部署与通信**
*   **Day 5a: Agent2Agent Communication** (`day5a/`)
    *   **Agent 互联**: 将 Agent 部署为服务 (LangServe)，并实现 Agent 之间的远程调用与协作。

---

##  声明 (Disclaimer)

*   **来源**: 本项目内容改编自 Google 的开源教程，仅供学习和研究使用。
*   **框架**: 原教程使用 Google ADK，本项目将其移植到了 LangChain 生态。
*   **模型**: 代码默认配置为兼容 OpenAI 接口的模型（可使用 DeepSeek, Qwen 等），你需要自行准备 API Key。

如有问题，欢迎提交 Issue 反馈！
## ©️ 许可证与版权 (License & Copyright)

*   **原课程内容**: 版权归 Google 及 Kaggle 所有，基于 **Apache 2.0 License** 许可。
*   **改编代码**: 本项目的 LangChain 重构代码及新的说明文档，同样遵循 **Apache 2.0 License** 开源。

