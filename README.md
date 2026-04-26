# 4个月后端转 Agent 开发工程师：系统 Roadmap

> 面向**有良好编程基础的后端开发工程师**，16 周系统路径，从 LLM API 工程化到生产级 Agent 系统设计与落地。

**核心主线：** 会用 → 会造 → 会调优 → 会生产化

**更现实的结果预期：**
- 这份路线的目标不是“16 周成为行业顶级专家”，而是**在 4 个月内建立可独立完成中小型 Agent 项目、能清楚解释工程取舍、具备面试与落地能力的系统能力**。
- 如果你已经具备扎实的后端开发经验，完成主线后可达到“Agent 开发工程师”较强候选人的水平；要成为“专家”，通常还需要后续 6-12 个月的真实项目迭代。

**设计原则：**
- Agent 工程师的核心是**工程能力**，不是 ML 研究能力。LLM 理解到"能解释其能力边界、知道它会在哪里失败"即可。
- 生产级 Agent 最常见的失败原因（LangChain 2025 行业调查）：Tool Schema 设计差、Context 管理混乱、过早引入多 Agent、缺乏可观测性——这四个问题在路径中每个都有专门章节。
- 每个实践节点明确三要素：**使用的具体库** + **参考的具体文档** + **可量化的交付标准**。
- **先做最简单可行系统，再按证据加复杂度**：很多任务先用单次 LLM 调用、RAG 或固定 workflow 就够了，不要默认上 autonomous agent。
- **优先学 workflow，再学 agent**：先掌握 prompt chaining、routing、parallelization、orchestrator-worker、evaluator-optimizer，再进入开放式 agent loop。
- **先学 pattern catalog，再学框架名字**：真正可迁移的是 workflow pattern、tool contract、state model、eval loop 和 side-effect control，不是某个框架的专有 API。
- **优先学 context engineering，而不只是 prompt wording**：真正决定效果上限的往往是上下文选择、压缩、路由、记忆和工具结果组织方式。
- **先直接用 API，再用框架**：框架是加速器，不是理解替代品；只有当你能解释底层消息流、状态流和工具流时，框架才真正有价值。
- **Trustworthy agents 默认不是附加项**：评测、审批点、回滚、停止条件、审计日志、权限边界，应当和 agent 主链路一起设计，而不是最后补。

---

## 目录

- [开始前先看](#开始前先看)
- [总体阶段划分](#总体阶段划分)
- [执行方式：主线必修 vs 进阶选修](#执行方式主线必修-vs-进阶选修)
- [贯穿全程背景资源](#贯穿全程背景资源)
- [Phase 1：LLM工程化 + 单Agent原理（Week 1–4）](#phase-1llm工程化--单agent原理week-14)
- [Phase 2：RAG全栈 + 主流框架（Week 5–8）](#phase-2rag全栈--主流框架week-58)
- [Phase 3：多Agent + 多模态 + 生产工程（Week 9–12）](#phase-3多agent--多模态--生产工程week-912)
- [Phase 4：两个真实项目落地（Week 13–16）](#phase-4两个真实项目落地week-1316)
- [建议新增但不要一开始全学的工程内容](#建议新增但不要一开始全学的工程内容)
- [核心论文清单](#核心论文清单)
- [核心资源总索引](#核心资源总索引)
- [每周学习节奏](#每周学习节奏)

---

## 开始前先看

### 适合人群

- 目标人群只包括**有良好编程基础的后端开发工程师**，默认你已经熟悉服务开发、数据库、接口设计、日志排障、部署和基本测试。
- 推荐技术基础：Python 或至少能快速切到 Python；能独立写脚本、调 API、读英文文档。
- 如果你还不熟悉 `asyncio`、HTTP、Docker、`pytest`、SQL/SQLite，建议先补 1 周工程基础再进入主线。
- 如果你不是后端工程师，这份路线图不是为你设计的，执行成本和理解门槛会明显更高。

### 时间投入假设

| 档位 | 每周投入 | 建议走法 |
|------|---------|---------|
| 高强度 | 15-20 小时 | 可按本文主线推进，大部分实验都能做 |
| 标准 | 10-12 小时 | 主线必修全做，进阶选修择优做 |
| 低强度 | 6-8 小时 | 每周只保留 1 个核心实验，项目阶段延长到 6 个月更合理 |

### 环境 / 预算假设

- **API 预算**：若全程都做量化实验，建议预留 300-1000 RMB 的 API 成本空间；如果预算紧张，优先保留主线实验，减少多模型横评次数。
- **本地机器**：CPU 即可完成大部分基础实验；如果希望本地跑多模态、reranker、LoRA 或 vLLM，最好有 NVIDIA GPU。没有 GPU 时，Week 11/12 的本地模型实验可改为 API 或 Colab。
- **系统环境**：建议 Linux / macOS；Windows 也能做，但在 Ollama、Playwright、PaddleOCR、Docker、minikube 环节会更折腾。

### 执行建议

- 这份路线故意覆盖得比较全，但**不是每个点都必须在 16 周内做完**。
- 判断是否达标，看“是否能解释设计取舍并交付稳定代码”，不要只看“是否把每个名词都碰过”。
- 这份路线默认你会用后端工程师的视角来学习 Agent：把它当成一个需要接口、状态、工具、安全、评测、部署和运维的系统，而不是只会写 Prompt 的应用层 Demo。

---

## 总体阶段划分

| 阶段 | 周次 | 核心能力 |
|------|------|---------|
| Phase 1 | Week 1–4 | LLM工程化使用 + 单Agent原理与手写实现 |
| Phase 2 | Week 5–8 | RAG系统 + LangGraph工作流 + 上下文管理 |
| Phase 3 | Week 9–12 | Evals + 服务化 + 安全 + 多Agent / 多模态进阶 |
| Phase 4 | Week 13–16 | 两个真实项目落地 |

---

## 执行方式：主线必修 vs 进阶选修

### A. 主线必修

- 每周至少完成 1 个可运行实验和 1 份量化结果。
- 优先级顺序固定：**单 Agent 原理 → RAG → LangGraph → Evals → 服务化 → 项目落地**。
- 如果时间不够，优先砍掉“横向比较”和“前沿专题”，不要砍掉主线闭环。
- 在任何一周，只要更简单的方案已经满足需求，就不要为了“更像 Agent”而强行升级架构。

### B. 进阶选修

- GraphRAG、DSPy、Dify、Browser Agent、LoRA、vLLM、K8s、MCP Server 都属于“进阶选修”。
- 这些内容很有价值，但不应阻塞主线进度。做不完时，允许先做最小可运行版本，再在项目阶段回补。

### C. 核心知识 vs 工具类

- **核心知识**：LLM API 工程化、Tool Use、工作流状态管理、RAG、上下文工程、Evals、服务化、安全治理。
- **工具类**：Ollama、vLLM、Dify、Langfuse、browser-use、MCP Server、Milvus、K8s 这类具体产品或基础设施。
- 学核心知识时要理解“为什么这样设计、有哪些 trade-off、失败点在哪里”。
- 学工具类时只要求达到“会安装、会接入、会排错、知道适用边界”，不要花大量时间研究产品细节。
- 如果某个工具不影响主线闭环，就不要让它占用本周主要时间。
- 文档里出现这些工具名时，应默认理解为“可替换示例”，不是必须绑定的唯一技术栈。

### D. 每周顺延规则

- 某周若未完成核心交付标准，不必死卡所有扩展实验，但至少要保证“主线最小成果”达标后再进下一周。
- 建议为每周定义两个结果：`Minimum`（必须完成）和 `Stretch`（做完更好）。下面各周如未显式写出，可按该原则自行裁剪。

---

## 后端工程师版学习重点排序

> 这份路线不是按“概念覆盖面最大化”设计，而是按“后端工程师转 Agent 开发后，最先能打的能力”排序。

### 优先级从高到低

1. **LLM API 工程化**：结构化输出、超时、重试、token 成本、配置管理、日志。
2. **Tool Use 与状态工作流**：工具协议、状态管理、检查点、错误恢复、人机审批。
3. **RAG 与 Context Engineering**：检索、重排、压缩、记忆、长上下文取舍。
4. **Evals 与可观测性**：Golden Dataset、回归评测、Tracing、成本/延迟/质量看板。
5. **服务化与生产治理**：FastAPI、鉴权、限流、队列、幂等、容器化、部署。

### 一条重要认知

- `Prompt Engineering` 在这份路线里只是入口能力，不是最高优先级能力。
- 真正贯穿全程的是 `Context Engineering + Workflow Design + Evals + Reliability`。
- `Framework Knowledge` 在这份路线里也不是第一优先级，真正值钱的是“把 agent 系统做稳”的能力。

### 默认降级为次优先级的内容

- Tree of Thoughts、GraphRAG、DSPy、MCP Server、Browser Agent、LoRA、K8s、vLLM 都是重要能力，但对“后端转 Agent”的前四周和前八周不是最短路径。
- 这些内容保留在文档中，但默认都按“进阶选修”或“按需会用”理解，除非你的目标岗位明确要求。
- Ollama、Dify、Langfuse、Milvus 这类工具更不应被当成“知识点”去学透，够用即可。

### 16 周主线判断标准

- 到 Week 8 时，你应该已经能独立做出一个**可评测、可持久化、有状态的单 Agent / Agentic RAG 服务**。
- 到 Week 12 时，你应该已经能把这个系统**服务化、加上评测和基础安全治理**。
- 到 Week 16 时，你应该至少有 **1 个完整项目 + 1 个精简项目**，而不是 2 个都做成半成品。

---

## 贯穿全程背景资源

> 以下资源体量大、跨越多个阶段，作为持续追进的参考资源，不在某一周集中消化。

| 资源 | 覆盖阶段 | 说明 |
|------|---------|------|
| [Berkeley CS294 LLM Agents (Fall 2024)](https://rdi.berkeley.edu/llm-agents/f24) | Phase 1–3 | Dawn Song 主讲，涵盖推理/框架/评估/安全，最权威的 Agent 系统课 |
| [Berkeley CS294 Agentic AI (Fall 2025)](https://rdi.berkeley.edu/agentic-ai/f25) | Phase 2–4 | 2025 最新版，多智能体/部署/自我改进 |
| [Berkeley CS294 Advanced LLM Agents (Spring 2025)](https://rdi.berkeley.edu/adv-llm-agents/sp25) | Phase 3 | 推理/多模态/安全进阶 |
| [mlabonne/llm-course](https://github.com/mlabonne/llm-course) | Phase 1–3 | GitHub 3万+ stars，LLM 工程师完整路径，含大量 Colab notebook |
| [Datawhale hello-agents](https://github.com/datawhalechina/hello-agents) | Phase 1–2 | 配合各周主题阅读对应章节 |
| [AgentGuide](https://github.com/adongwanai/AgentGuide) | Phase 1–4 | 全程参考手册，面试题库 Phase 4 重点刷 |
| [2025 AI Engineering Reading List](https://www.latent.space/p/2025-papers) | Phase 3–4 | 年度必读论文索引 |

### 国内开源模型生态（贯穿全程并行）

> 不建议把国内模型生态全部并行深学。**这份 roadmap 只建议 1 主线 + 1 对照 + 若干了解项。**

**推荐组合：**
- 主线用 `Qwen / DashScope`
- 对照用 `DeepSeek`
- `GLM / 文心` 了解接口差异即可
- `Ollama / vLLM` 不算模型学习主线，放到后面的服务化阶段按需使用

| 角色 | 模型/平台 | 用法 |
|------|----------|------|
| 主线模型 | [Qwen2.5 / Qwen-VL / Qwen-Audio](https://github.com/QwenLM/Qwen2.5) | 贯穿大部分实验，作为默认模型生态 |
| 对照模型 | [DeepSeek-V3 / DeepSeek-R1](https://github.com/deepseek-ai/DeepSeek-V3) | 少量横评，理解推理/成本/延迟取舍 |
| 了解即可 | [ChatGLM / GLM-4](https://github.com/THUDM/GLM-4) / [文心大模型 4.0](https://cloud.baidu.com/doc/WENXINWORKSHOP/index.html) | 只看 API 形态、Function Call 差异、适用场景 |

> 说明：
>- `Ollama` 的学习成本很低，核心就是安装、拉模型、暴露兼容接口，知道怎么用于本地原型验证就够了。
>- `vLLM` 也不该放在前期模型选型里学，它属于 Phase 3 的部署与推理服务主题。

---

## Phase 1：LLM工程化 + 单Agent原理（Week 1–4）

> 核心目标：把 LLM 当成工具用到极致，然后手写一个完整 Agent，不依赖任何框架。

> **后端视角：** 这一阶段不是学“怎么和模型聊天”，而是学“怎么把模型接进一个可靠的后端系统”。

<details>
<summary><strong>Week 1：LLM API 工程化 + Prompt/Context 基础</strong></summary>

**本周建议拆分：**
- `Minimum`：打通 2 家模型 API + 结构化输出 + Pydantic 校验 + 超时/重试/日志
- `Stretch`：做 3 家模型横评；需要本地模型时再用 Ollama 做对照

**知识点：**
- 主流 API 接入：OpenAI / Anthropic / DashScope（Qwen）/ DeepSeek / 文心 的 Chat Completion 格式；`role` 字段含义（system/user/assistant/tool）
- Token 机制：`tiktoken` 库计数；中文 token 效率差异；上下文窗口限制与成本关系
- Sampling 参数实验：temperature（0/0.7/1.5 对比）、top-p、frequency_penalty、presence_penalty
- **Structured Output**：OpenAI `response_format={"type": "json_schema"}`；Anthropic `tool_use` 强制 JSON；Pydantic v2 校验与容错解析（`model_validate_json` + try/except）
- System Prompt 设计基础：角色定义、输出格式约束、few-shot 示例的位置效果
- **Context 基础**：消息角色分层、示例放置位置、工具结果如何组织进上下文、哪些信息不该塞进 prompt
- **后端工程补充**：配置管理（`.env` / settings）、超时、重试、请求日志、错误分级

**重点学习资源：**
- 🎓 [DeepLearning.AI: ChatGPT Prompt Engineering for Developers](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) — 免费，2小时，先看
- 📚 [OpenAI Cookbook: Structured Outputs Intro](https://cookbook.openai.com/examples/structured_outputs_intro) — 逐章跟做
- 📚 [Qwen2.5 Function Call 文档](https://qwen.readthedocs.io/en/latest/framework/function_call.html)
- 📚 [DeepSeek API 文档](https://platform.deepseek.com/docs)

**论文精读：**
- [Lilian Weng: LLM Powered Autonomous Agents (2023)](https://lilianweng.github.io/posts/2023-06-23-agent/) ⭐ — Agent 领域全局地图，本周通读

**实践：**
- `pip install openai anthropic dashscope tiktoken pydantic`；Ollama 安装并拉取 `qwen2.5:7b`
- 定义 `ProductReview` Pydantic 模型，用 **OpenAI + DashScope + DeepSeek** 三套 SDK 各实现结构化提取（输入 20 条非结构化评论）
- 用 `tiktoken` 统计同一段 500 字中文在不同模型下的 token 数对比
- **交付标准：** 至少 2 家模型结构化提取稳定可跑；有 JSON 遵循率对比；基础日志/超时/重试机制到位

</details>

<details>
<summary><strong>Week 2：高级 Prompt/Context 设计 + 推理增强</strong></summary>

**本周建议拆分：**
- `Minimum`：理解 Zero-shot CoT / Few-shot CoT / Self-Consistency 对质量和成本的影响
- `Stretch`：实现 ToT 或深入对比推理模型

**知识点：**
- CoT 两种：Zero-shot CoT（"请一步步思考"）vs Few-shot CoT（手写 3 条含推理过程的示例）
- Self-Consistency：多路采样（temperature=0.7，5次）→ `collections.Counter` 取多数票
- Tree of Thoughts：树状推理搜索（BFS + LLM 评估每步）；适合规划问题，不适合简单问答
- 推理模型特点：DeepSeek-R1 的 `<think>` 慢思考；在 Agent 中的选择策略（规划用推理模型，工具调用用普通模型）
- XML 结构化提示：`<context>`, `<instructions>`, `<examples>`, `<output_format>` 对遵循率的影响
- **Context 进阶**：把任务做对，往往比“写更花的 prompt”更依赖上下文结构和输入分解
- **后端工程补充**：本周重点是建立“效果/成本/延迟”的取舍意识，不是沉迷花式 Prompt 技巧

**重点学习资源：**
- 📚 [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) — 重点读 XML 结构、复杂指令、减少幻觉章节
- 📺 [Berkeley CS294 Fall 2024 Lecture 1](https://rdi.berkeley.edu/llm-agents/f24) — Denny Zhou（CoT 原作者）主讲
- 📚 [mlabonne/llm-course Prompt Engineering 章节](https://github.com/mlabonne/llm-course) — 含可运行 Colab

**论文精读：**
- Chain-of-Thought Prompting Elicits Reasoning in LLMs (Wei et al., 2022)
- Self-Consistency Improves CoT Reasoning (Wang et al., 2022)
- Tree of Thoughts (Yao et al., 2023)
- Large Language Models are Zero-Shot Reasoners (Kojima et al., 2022)

**实践：**
- 取 [GSM8K](https://github.com/openai/grade-school-math) 前 30 道题，对比 4 种方法准确率：Standard / Zero-shot CoT / Few-shot CoT / Self-Consistency（5路采样）
- 用 DeepSeek-R1 跑同一批题，记录 `think` 过程长度与正确率的相关性
- **交付标准：** 至少完成 3 种推理策略对比；写出“什么场景值得为更高质量付出更多 token 成本”的结论

</details>

<details>
<summary><strong>Week 3：Tool Use & Function Calling — 核心工程</strong></summary>

**知识点：**
- Tool Use 完整协议，逐步手写每个环节：定义 JSON Schema → 传入 `tools=[]` → 检测 `tool_calls` → 执行工具 → 以 `role="tool"` 回传结果 → 再次调用获取最终答案
- **Tool Schema 设计原则**（生产失败 #1 来源）：`description` 必须说明"何时用"而非"是什么"；参数含取值范围、单位、边界情况；对比实验：模糊版 vs 精确版，预期成功率差 ≥ 20%
- ReAct 循环手动实现：while 循环处理 `tool_calls`，手动管理 `messages` list
- 并行工具调用：`asyncio.gather` 并发执行多个 tool，批量回传
- 错误处理：将异常信息作为 `tool` message 回传，指数退避重试（最多 3 次）
- **Agent-Computer Interface（ACI）意识**：工具定义、参数命名、输入格式、错误返回本质上都是给模型设计接口，重要性不低于 prompt 本身
- **安全前置**（从本周开始，不要等到生产化阶段再补）：
  - 代码执行工具默认禁用网络、限制运行目录、设置超时和输出长度上限
  - 文件工具必须做路径白名单，只允许访问工作目录
  - 每个工具都要有审计日志：输入参数、执行耗时、异常信息

**重点学习资源：**
- 🎓 [DeepLearning.AI: Agent Skills with Anthropic](https://www.deeplearning.ai/short-courses/agent-skills-with-anthropic/) — 官方课程，重点跟做
- 📚 [Anthropic Tool Use 文档](https://docs.anthropic.com/en/docs/build-with-claude/tool-use) — 必读：Best practices、Parallel tool use、Error handling
- 📚 [OpenAI Function Calling Guide](https://platform.openai.com/docs/guides/function-calling) — 对比格式差异

**论文精读：**
- **ReAct: Synergizing Reasoning and Acting** (Yao et al., ICLR 2023) ⭐
- **Toolformer** (Schick et al., Meta 2023) ⭐

**实践：**
- `pip install numexpr duckduckgo-search`；实现 4 个工具：搜索（`DDGS().text()`）/ 计算器（`numexpr.evaluate()`）/ 代码执行（`subprocess.run`）/ 文件读写（`pathlib`）
- **完全不使用框架**，用原生 OpenAI API 手写完整 ReAct 循环
- **Schema 质量对比实验**：模糊版 vs 精确版，各跑 50 条指令，统计调用成功率差异
- 如果你是第一次做 Agent：可把“代码执行”降级为受限 Python 表达式执行，先不开放任意 shell
- **交付标准：** ReAct Agent 完成 10/10 测试任务；Schema 实验报告（两版成功率差 ≥ 15%）

</details>

<details>
<summary><strong>Week 4：Agent 核心模式 — Planning / Memory / Reflection</strong></summary>

**本周建议拆分：**
- `Minimum`：Plan-Execute + Episodic Memory（SQLite）+ Reflection 打通
- `Stretch`：语义记忆蒸馏、复杂长期记忆策略

**知识点：**
- Planning 两种模式：**ReAct**（边想边做，适合开放任务）vs **Plan-Execute**（先规划 JSON 列表再执行，适合结构固定任务）；步骤失败时触发重规划
- Memory 四类工程实现：
  - 短期（Context Window）：`ConversationBufferWindowMemory(k=10)` 保留最近 k 轮
  - 长期（向量DB）：`faiss-cpu` + `sentence-transformers`，`IndexFlatL2` + 语义检索
  - 情景（Episodic）：`{task, steps, outcome, timestamp}` 存 SQLite，按相似度检索历史经验
  - 语义（Semantic）：LLM 定期从 Episodic 中蒸馏规律为 facts 文本，注入下次对话 System Prompt
- Reflection 机制：任务结束后 LLM 自评分（1-10）+ 50 字教训 → 写入 Episodic Memory → 下次相似任务前检索注入
- Context Window 管理：`summarize` 策略（LLM 压缩最旧 4 条为 1 条摘要）vs `truncate` 策略
- **后端工程补充**：本周重点不是“记忆概念多完整”，而是把状态、历史和经验真正落到可存储、可回放、可复现的结构里

**重点学习资源：**
- 🎓 [DeepLearning.AI: Agentic AI](https://learn.deeplearning.ai/courses/agentic-ai/) — 吴恩达主讲 ⭐ 免费
- 📺 [Berkeley CS294 Fall 2024 Lecture 2](https://rdi.berkeley.edu/llm-agents/f24) — Shunyu Yao（ReAct 作者）主讲
- 📚 [FAISS Getting Started](https://github.com/facebookresearch/faiss/wiki/Getting-started)

**论文精读：**
- **Reflexion** (Shinn et al., 2023) ⭐
- **Generative Agents** (Park et al., 2023) ⭐ — 记忆+规划+反思完整架构
- **LATS: Language Agent Tree Search** (Zhou et al., 2023)

**实践：**
- `pip install faiss-cpu sentence-transformers`
- 在 Week 3 手写 Agent 基础上添加：Plan-Execute 架构 + FAISS 长期记忆（`all-MiniLM-L6-v2` embedding）+ Reflection 模块（写入 `episodes.json`）
- 测试：同一个 10 步任务连跑 5 次，记录有无 Reflection 的成功率曲线
- **交付标准：** 至少完成 Plan-Execute + 结构化历史存储 + Reflection 回写；能复盘失败轨迹而不是只展示成功 demo

**Phase 1 阶段检验：** 不依赖任何框架，用原生 API 实现含 Planning + Memory + Tool Use + Reflection 的完整 Agent，并能清晰讲解每部分的工作原理和设计取舍。

</details>

---

## Phase 2：RAG全栈 + 主流框架（Week 5–8）

> 核心目标：掌握 **检索增强系统 + 有状态工作流 + 上下文工程** 的完整工程能力。

> **后端视角：** 这一阶段要把 Agent 看成“检索、路由、状态、评测”构成的后端系统，而不是只会回答问题的聊天机器人。

> **关键融合观点：** 在很多真实系统里，先做 `workflow + retrieval + evals` 往往比直接上开放式 agent 更稳、更便宜、更容易调试。

<details>
<summary><strong>Week 5：RAG 系统完整链路</strong></summary>

**本周建议拆分：**
- `Minimum`：先把 parse → chunk → index → retrieve → answer → eval 的单条链路打通
- `Stretch`：多 embedding、多检索策略横评；Milvus 只做产品认知级对比

**知识点：**
- **文档解析**：程序型 PDF（`PyMuPDF`/`pdfplumber`）/ 扫描型（`pdf2image` → `PaddleOCR`）/ HTML（`trafilatura`）/ Word（`python-docx`）
- **分块策略对检索质量的影响**：
  - Fixed：`RecursiveCharacterTextSplitter(chunk_size=512, chunk_overlap=64)`
  - Semantic：`SemanticChunker`（LangChain），比 Fixed 召回率高 10-20%
  - Hierarchical（父子块）：小块（256 tokens）检索，结果扩展到大块（1024 tokens）喂给 LLM
- **Embedding 选型**（参考 [MTEB 排行榜](https://huggingface.co/spaces/mteb/leaderboard)）：`text-embedding-3-small`（OpenAI）/ `BAAI/bge-large-zh-v1.5`（国内中文最强开源）/ `text-embedding-v3`（阿里）
- **向量库选型**：
  - `FAISS`（本地，无服务，原型）→ `Chroma`（本地持久化，开发）→ `Qdrant`（生产，Docker 一行部署）
  - `Milvus`（`pip install pymilvus`）：了解其在企业向量检索中的常见定位即可，不必前期深挖产品细节；本地可用 `milvus-lite`
- **检索策略**：Dense（向量相似度）/ Sparse（`rank_bm25`，关键词精确匹配）/ **Hybrid RRF**（两路融合，通常提升 10-15%）
- **RAGAS 评估**：`Faithfulness`（答案忠实度）/ `Answer Relevancy`（答案相关性）/ `Context Recall`（检索完整性）
- **后端工程补充**：关注 ingest pipeline 的可重跑性、索引更新策略、离线评测脚本，而不是只关注 prompt 写法

**重点学习资源：**
- 🎓 [DeepLearning.AI: LangChain Chat with Your Data](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/)
- 🎓 [DeepLearning.AI: Building and Evaluating Advanced RAG](https://www.deeplearning.ai/short-courses/building-evaluating-advanced-rag/) — LlamaIndex 官方课 ⭐
- 📚 [Datawhale llm-universe](https://github.com/datawhalechina/llm-universe)
- 📺 [Berkeley CS294 Fall 2024: RAG 系统评估](https://rdi.berkeley.edu/llm-agents/f24) — Jerry Liu（LlamaIndex 创始人）主讲

**论文精读：**
- RAG for Knowledge-Intensive NLP Tasks (Lewis et al., 2020)
- **Self-RAG** (ICLR 2024) ⭐
- **RAGAS** (2023)

**实践：**
- `pip install langchain langchain-community llama-index faiss-cpu chromadb qdrant-client pymilvus[model] ragas sentence-transformers rank-bm25 PyMuPDF pdfplumber trafilatura`
- 3 种分块 × 2 种 Embedding × 3 种检索 = **18 组合**，用 RAGAS 评估每种 `Context Recall@5`
- 用 **LlamaIndex** 重写同一个 RAG，写 300 字 LangChain vs LlamaIndex 对比笔记
- 如有余力，再用 `milvus-lite` 替换 FAISS 重跑最优组合，了解它在生产中的定位
- **时间不够时的主线裁剪**：保留“1 种分块 + 1 种 embedding + Dense/Hybrid 两种检索 + 1 次框架改写”；Milvus 改为选修
- **交付标准：** 18 组合 RAGAS 对比报告；Hybrid RRF 比最差组合提升 ≥ 15%

</details>

<details>
<summary><strong>Week 6：LangGraph — 有状态工作流 Agent</strong></summary>

**本周建议拆分：**
- `Minimum`：StateGraph + 条件分支 + 持久化检查点 + Human-in-the-Loop
- `Stretch`：Subgraph 复用和更复杂循环

**知识点：**
- LangGraph 核心抽象（逐一手写）：
  - `StateGraph`：状态类型用 `TypedDict`
  - `add_node(name, func)`：处理函数签名 `def node(state) -> dict`
  - `add_edge` / `add_conditional_edges`：无条件/条件转移
  - `compile(checkpointer=...)`：`invoke` / `stream` 两种调用
- **五种 workflow pattern 要能用代码解释清楚**：
  - **Prompt Chaining**：上一步输出稳定地作为下一步输入，适合固定管线
  - **Routing**：先分类再走不同处理分支，适合多意图入口
  - **Parallelization**：可并行子任务同时跑，适合多数据源或多候选生成
  - **Orchestrator-Worker**：一个节点拆任务，多个 worker 执行并汇总
  - **Evaluator-Optimizer**：先生成，再用规则/模型评估，不达标则修正
- 为什么需要 LangGraph：需要**循环**（质量不达标则重搜索）+ **条件分支** + **状态持久化**（中断续跑）+ **可视化**（`draw_mermaid()`）
- `Checkpointer`：`MemorySaver` → `SqliteSaver` → `PostgresSaver`；`thread_id` 实现中断续跑
- **Human-in-the-Loop**：`interrupt_before=["node"]` 暂停 → `graph.update_state()` 修改 → `invoke(None, config)` 继续
- `Subgraph`：子 Agent 封装为独立 Graph，父 Graph 引用，实现复用和 Context 隔离
- **Workflow vs Agent 区分**：固定路径、可预测任务优先用 workflow；只有当步骤数、路径或工具选择不可预判时，才升级成更开放的 agent loop
- **后端工程补充**：把 LangGraph 当成“工作流编排引擎”，不是“又一个 Agent 框架”

**重点学习资源：**
- 🎓 [DeepLearning.AI: AI Agents in LangGraph](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/) ⭐ 免费，必做
- 📚 [LangGraph 官方 Tutorials](https://langchain-ai.github.io/langgraph/tutorials/) — 顺序精读 4 篇：basic chatbot → agent → memory → human-in-the-loop
- 📺 [LangChain YouTube: LangGraph 系列](https://www.youtube.com/@LangChain)

**论文精读：**
- **AgentBench** (Liu et al., 2023)
- **The Landscape of Emerging AI Agent Architectures** (arxiv 2404.11584)

**实践：**
- `pip install langgraph langchain-openai tavily-python`
- 用同一个 Research 任务分别实现 `routing` 和 `evaluator-optimizer` 两种 workflow，对比“固定 workflow 是否已经够用”
- 实现 Research Agent：State（query/sources/draft/score/iterations）→ search → read → evaluate → 条件分支（分<7 循环，≥7 综合）→ output
- `SqliteSaver` 持久化；手动中断后同一 `thread_id` 续跑；Human-in-the-Loop 审批 sources
- **交付标准：** 除可视化外，必须能证明状态可恢复、流程可中断续跑、人工审批能改变执行路径

</details>

<details>
<summary><strong>Week 7：高级 RAG + Memory 系统</strong></summary>

**本周建议拆分：**
- `Minimum`：HyDE / Reranker / Multi-Query 三选二 + 跨会话记忆打通
- `Stretch`：GraphRAG 跑完整 pipeline，并整理一份“什么问题值得用 GraphRAG”的判断标准

**知识点：**
- 高级检索技术栈（每个都要手写，理解背后的问题和解法）：
  - **HyDE**：LLM 先生成假设答案 → 用假设答案 embedding 去检索，解决查询与文档措辞差异大的问题
  - **Multi-Query Retrieval**：`MultiQueryRetriever`，3 个不同角度子查询并行检索后去重合并
  - **Re-ranking**：向量检索 Top-20 → `BAAI/bge-reranker-large` 精排取 Top-5，精排比初排高 10-20%（`pip install FlagEmbedding`）
  - **Contextual Compression**：`LLMChainExtractor` 只保留与查询相关的句子，减少 Context 噪声
  - **Agentic RAG**：Agent 自己决定是否检索，置信度低才检索（LangGraph 条件分支实现）
- **GraphRAG**（Microsoft）：实体抽取 → 知识图谱 → 社区检测 → 社区摘要 → 检索；解决跨文档关联查询问题；`pip install graphrag`
- **Memory 四类完整工程实现**：短期（`ConversationBufferWindowMemory`）/ 长期（`VectorStoreRetrieverMemory`）/ 情景（SQLite 结构化日志）/ 语义（LLM 蒸馏 facts）
- 跨会话持久化：对话结束 `write_memory()` → 下次对话 `load_memory()` → 存入/遗忘策略（低评分 + 超期可丢弃）

**重点学习资源：**
- 🎓 [DeepLearning.AI: Agent Memory](https://www.deeplearning.ai/short-courses/agent-memory-building-memory-aware-agents/)
- 📚 [Microsoft GraphRAG Getting Started](https://microsoft.github.io/graphrag/get_started/) — 跑完整个 pipeline
- 📚 [FlagEmbedding BGE Reranker 示例](https://github.com/FlagOpen/FlagEmbedding/tree/master/FlagEmbedding/reranker) — 20 行接入

**论文精读：**
- **Agentic RAG Survey** (arxiv 2501.09136, 2025) ⭐
- **GraphRAG: From Local to Global** (Microsoft, 2024) ⭐
- **Agent Workflow Memory** (Zhou et al., 2024) ⭐
- **MemGPT** (2023)

**实践：**
- 在 Week 5 RAG 基础上逐步升级，每步验证 RAGAS 指标变化：HyDE → Reranker → Multi-Query
- GraphRAG 对 20 篇文章建索引，测试 3 个普通 RAG 答不好的跨文档关联问题
- 为 Week 6 Agent 添加完整四类记忆，验证"第二次对话能正确引用第一次的特定细节"
- 如果每周投入不足 12 小时，建议先把 GraphRAG 顺延到项目一中作为增强项
- **交付标准：** HyDE + Reranker 组合 Context Recall@5 比基础 Dense 提升 ≥ 15%；跨会话记忆可演示

</details>

<details>
<summary><strong>Week 8：上下文工程（Context Engineering）+ DSPy（选修）</strong></summary>

**本周建议拆分：**
- `Minimum`：把 Context Engineering 四策略落到已有 LangGraph Agent 中，并做 token / 质量对比
- `Stretch`：DSPy 自动优化跑通；如果手头没有稳定标注集，DSPy 可以延后到项目阶段

**知识点：**
- **Context Engineering 四策略**（Agent 工程最核心的能力之一）：
  - **Write**：工具结果 < 500 tokens 全写，否则先摘要；System Prompt（永久指令）vs User Message（本轮输入）职责边界；Scratchpad 设计（中间思考不暴露给用户但写入 Context）
  - **Select**：基于语义相似度动态从 Memory 检索注入；时间权重（近期记忆权重更高）；Token 预算约束（Memory 用量 ≤ 总窗口 20%）
  - **Compress**：对话超 N 轮时 LLM 将最旧 M 条压缩为 1 条摘要；`LLMChainExtractor` 文档压缩；量化评估信息损失
  - **Isolate**：多 Agent 的 StateGraph 只含各自需要的字段；Orchestrator 做信息路由，不共享 Context
- **长上下文工程**（与压缩并列的另一种思路）：
  - **Lost-in-the-middle 现象**（Liu et al., 2023）：模型对 Context 中间信息记忆最差，首尾最强 → 关键信息放开头或紧靠问题结尾
  - **Needle-in-a-haystack 测试**：`pip install needlehaystack`，验证模型在你的任务长度下的可靠性
  - **长上下文 vs RAG 决策**：< 50K tokens + 需要全局理解 → 直接放 Context；> 100K tokens 或需要精确定位 → RAG；跨多文档 → RAG + 结构化提取
  - **Context Caching**：Anthropic `cache_control: {"type": "ephemeral"}` 可降低成本 60-90%
- **DSPy（Declarative Self-Improving Python）**：
  - 核心思想：将 Prompt 优化变成有 Metric 的优化问题，自动搜索最优 Prompt 和示例
  - `dspy.Signature` → `dspy.ChainOfThought` / `dspy.Predict` → `dspy.MIPROv2` 优化器
  - 适合场景：有清晰的可程序化 metric + 20-100 条示例数据
- **后端工程补充**：这一周的本质是“上下文预算管理”，这比 DSPy 本身更通用、更核心
- **工具边界**：DSPy 在这份路线里是“可选的优化框架”，不是必须掌握的核心知识

**重点学习资源：**
- 📺 [Berkeley CS294 Fall 2024: Compound AI & DSPy](https://rdi.berkeley.edu/llm-agents/f24) — Omar Khattab（DSPy 作者）主讲 ⭐
- 📚 [DSPy 官方 Tutorials](https://dspy.ai/tutorials/) — Intro → Optimizers → RAG
- 📚 [Anthropic: Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)

**论文精读：**
- **DSPy** (Khattab et al., 2023) ⭐

**实践：**
- `pip install dspy-ai`
- Context Compression：LangGraph Agent 添加 `compress_context` 节点（触发条件：messages > 8000 tokens；压缩最旧 4 条为 200 token 摘要）
- DSPy 优化：定义 ResearchReport Signature → 30 条训练数据 → LLM-as-Judge metric → `MIPROv2` 优化 10 轮
- **交付标准：** Context 压缩后 token 用量降低 ≥ 30%；DSPy 优化后评分比手写 Prompt 提升 ≥ 1 分（10分制）

**Phase 2 阶段检验：** 能独立构建生产质量的 Agentic RAG 系统；用 LangGraph 实现含持久化和 Human-in-the-Loop 的有状态工作流；用 RAGAS 量化评估并优化。

</details>

---

## Phase 3：多Agent + 多模态 + 生产工程（Week 9–12）

> **后端视角：** 这一阶段的主线其实是 `Evals + 服务化 + 安全治理`。多 Agent 和多模态是有业务需求时再加的系统能力，不是默认必选项。

> **关键融合观点：** 多 Agent 不代表更高级。只有当单 Agent 在并行性、上下文隔离、专业化工具集或长程任务上确实不够时，才值得升级。

<details>
<summary><strong>Week 9：多 Agent 架构 + Code Agent + Protocols（了解）</strong></summary>

> **核心认知：** 市场上有数十个多 Agent 框架（LangGraph、CrewAI、AutoGen、AgentScope……），它们会持续迭代。不应该逐一学习每个框架，而应该理解框架存在的原因和解决的共性问题，做到举一反三。

> **执行建议：** 这一周的重点不是“会多少框架”，而是“知道何时不该上多 Agent”。如果单 Agent 还不稳定，优先回头修单 Agent + Evals。

**本周建议拆分：**
- `Minimum`：做出一个 Orchestrator-Worker 最小样例，并写清“为什么单 Agent 不够”
- `Stretch`：Code Agent、自定义路由策略；协议层只做接口认知即可

**知识点：**

**何时需要多 Agent（不要过早引入复杂度）：**
- ✅ 任务天然可并行（多数据源同时采集）
- ✅ 子任务需要专业化（代码 Agent 的工具集 ≠ 搜索 Agent，隔离更安全）
- ✅ 单 Agent Context Window 装不下（任务分治）
- ❌ 只是想"看起来高级"——单 Agent 够用绝对不引入多 Agent

**从 workflow 升级到 agent 的判断问题：**
1. 任务路径是否无法提前写死？
2. 是否必须依赖环境反馈持续重规划？
3. 是否存在明确的暂停点、审批点、停止条件？
4. 是否有 evals 能证明 agent 比 workflow 更值得？

**框架背后的共性问题（5个问题快速评估任何框架）：**
1. **状态模型**：用什么数据结构表示 Agent 状态？
2. **路由机制**：Agent 之间如何决定下一步由谁执行？
3. **循环控制**：如何控制循环终止？
4. **状态持久化**：框架如何实现中断续跑？
5. **可观测性**：是否原生支持 Tracing？

**多 Agent 四种架构模式（语言无关）：**
- **Orchestrator-Worker**：Orchestrator 分解，Workers 并行执行，结果汇总（最常见）
- **Peer-to-Peer**：`asyncio.Queue` 互相协调，适合协商类任务
- **Hierarchical**：多层级 Manager → Worker，适合大规模任务
- **Parallel Sub-Graphs**（LangGraph `Send()` API）：动态广播并行节点

**Code Agent 专项：**
- **执行沙箱设计**：`subprocess.run(["python", "-c", code], timeout=30)` → 容器级隔离（Docker 容器池，生产用）
- **测试驱动循环**：生成代码 → 执行 → 捕获完整 stderr（含 Traceback 和行号）→ 分析 → 修改 → 重试（最多 3-5 次）
- **代码搜索工具**：`ast` 模块（Python AST 解析）/ `tree-sitter`（多语言）/ `ripgrep`

**Agentic Protocols（了解即可）：**
- **MCP**：工具/资源服务的标准化接口，核心价值是“跨框架复用同一批工具与资源”
- **A2A**：Agent 与 Agent 的通信协议，核心价值是跨系统协作而非单进程内编排
- **NLWeb**：把网站能力以自然语言接口暴露给 Agent，更偏生态方向认知
- `pip install mcp`；Server 端用 `@server.tool()` 暴露 JSON Schema；Client 端可用适配器加载工具
- **后端工程补充**：协议层是“接口标准认知”，不是这份路线前半段的产出重点
- **工具边界**：知道这些协议分别解决什么问题、适合什么边界即可，不必一口气把三类协议都搭起来

**重点学习资源：**
- 🎓 [DeepLearning.AI: Multi AI Agent Systems with crewAI](https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/) — **目的是理解多 Agent 概念抽象，不是学 CrewAI**
- 📚 [LangGraph Multi-Agent Architectures 文档](https://langchain-ai.github.io/langgraph/concepts/multi_agent/)
- 📚 [MCP 官方文档: Building Your First Server](https://modelcontextprotocol.io/quickstart/server)
- 📚 [Microsoft ai-agents-for-beginners](https://github.com/microsoft/ai-agents-for-beginners) — 看 lessons 列表即可，重点关注 `Multi-Agent`、`Protocols`、`Context Engineering`
- 📺 [Berkeley CS294 Fall 2025: 多智能体 AI](https://rdi.berkeley.edu/agentic-ai/f25) — Noam Brown 主讲

**论文精读：**
- **MetaGPT** (2023) ⭐ — 理解 SOP 思想，用结构化角色分工减少 Agent 间歧义
- **ChatDev** (2023) — 测试驱动的多 Agent 编程流程
- **Voyager** (Wang et al., 2023) — 技能库 + 自我进化

**实践：**
- LangGraph `Send()` Orchestrator-Worker：5 家公司并行分析，记录并行比串行加速比
- 如有余力，再手写一个最小 MCP Server（`search_arxiv` + `get_paper_abstract`）做接口理解；A2A / NLWeb 只需写 200 字边界判断
- Code Agent 沙箱实验：实现"写代码→执行→看报错→修改→重试"循环，测试 3 个任务的平均重试次数和成功率
- **框架评估练习**：花 2 小时阅读任意一个新框架，用 5 个问题写 300 字评估报告
- **交付标准：** 至少完成 1 个真正有并行价值的多 Agent 样例；如果没有业务必要，允许协议层 / Code Agent 顺延

</details>

<details>
<summary><strong>Week 10：Agent 评估体系（Evals）</strong></summary>

**本周建议拆分：**
- `Minimum`：Unit Test + Golden Dataset + 端到端回归评测
- `Stretch`：LLM-as-Judge 双模型交叉验证 + 任一 tracing/eval 平台历史趋势看板

**知识点：**
- **Evals 三层体系**（每层都要实现）：
  - **Unit Test 层**：`pytest` + `unittest.mock.patch` 验证工具调用正确性（工具名、参数类型、无幻觉工具名）
  - **Trajectory 层**：记录完整 `[(thought, action, observation)]` 序列，LLM-as-Judge 评估步骤合理性
  - **End-to-End 层**：人工标注 Golden Dataset + LLM-as-Judge 打分 + 程序化验证（代码运行率/格式正确率）
- **Golden Dataset 构建**：每条数据 `{input, expected_tool_sequence, reference_answer, difficulty, category}`；覆盖 4 类：直接回答 / 单工具 / 多步工具 / 模糊输入；20-30 条覆盖核心场景
- **LLM-as-Judge 设计原则**：每分有具体 Rubric；Judge Prompt 中加 CoT（先分析再打分）；多模型交叉验证（GPT-4o + Qwen-Max，差异 > 2 分时人工复核）
- **Tracing / 观测平台接入**（从本周起，之后每个项目都用）：可用 `Langfuse` 或任一同类平台；核心是把 trace、score、成本和错误样本记录下来
- CI 式评测流水线：每次修改 Prompt 后必须先跑 `python eval_pipeline.py`，输出对比报告后才决定是否保留
- **Harness 思维**：长任务的稳定性不只取决于模型能力，更取决于停止条件、上下文重置、错误恢复、审批点和日志设计
- **Metacognition / Self-Critique 的正确位置**：它是可选优化环节，不是默认开关。只有当 eval 证明“自检一轮”能稳定提升质量时，才值得付出额外延迟和成本
- **Trustworthy Agent 最小清单**：高风险工具有人审；外部副作用可回滚；失败时可停止；关键动作可追溯；每次改 Prompt 都能回归测试
- **后端工程补充**：这一周是整个 roadmap 的硬主线，不是附属主题。没有评测，后面的优化、服务化和项目都会失真

**重点学习资源：**
- 📚 [Anthropic Engineering: Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) ⭐ 必读
- 📚 [Langfuse Tracing Quickstart](https://langfuse.com/docs/tracing) — 作为 tracing 平台示例，30 分钟跟做
- 📚 [Evidently AI: LLM Evaluation Applied Course](https://www.evidentlyai.com/llm-evaluation-course-practice) — 10 个 Python 实战教程

**论文精读：**
- **SWE-bench** (2023) ⭐
- **GAIA** (2023)
- **AgentBench** (Liu et al., 2023)

**实践：**
- `pip install langfuse pytest deepeval`
- 为 Week 6 Research Agent 构建三层评测流水线：Unit Test（`test_tools.py`）+ Golden Dataset（20条）+ LLM-as-Judge（Qwen-Max + GPT-4o 交叉验证）+ 一套 tracing 记录
- 做一次 Prompt 改进实验：改 System Prompt → 跑评测 → 输出前后对比报告
- 选一个节点加 `self-critique` 或 `evaluator-optimizer` 回路，用相同数据集验证是否真的值得保留
- **交付标准：** 一键运行评测是硬要求；没有这套评测，不建议进入项目阶段

</details>

<details>
<summary><strong>Week 11：多模态 Agent</strong></summary>

**本周建议拆分：**
- `Minimum`：PDF 解析 + 一个最小多模态问答链路
- `Stretch`：多模态 RAG、Browser Agent、语音 Agent 按资源条件择优选 1-2 个，不建议一周内全做，也不需要深入研究具体工具框架

> **适用边界：** 如果你的目标岗位更偏通用 Agent 平台、企业知识库、工作流编排，而不是文档智能 / GUI 自动化，这一周可以整体降为选修，把时间挪给 Week 10 和 Week 12。

**知识点：**
- **VLM 核心概念**：图文 Token 化（patches → token embeddings）；传入图片格式（base64 或文件路径）；VLM 能力边界（OCR/图表解读/GUI 元素定位/空间关系）
- **PDF 解析完整工具链**（生产中最常见的多模态需求）：
  - 程序型 PDF：`PyMuPDF`（`page.get_text()`/`get_images()`）+ `pdfplumber`（`page.extract_table()`）
  - 扫描型 PDF：`pdf2image`（`convert_from_path()`）→ `PaddleOCR`（`ocr.ocr(img_array, cls=True)`，中文精度 ≥ 95%）
  - 混合型：`get_text()` 返回 < 10 字符则走 OCR 路径
  - 版式理解（进阶）：`layoutparser` 区分标题/正文/图片/表格
- **多模态 RAG**：CLIP（`openai/clip-vit-base-patch32`）生成图片 512 维向量；文字 + 图片 chunk 存同一 Qdrant（`payload.type` 区分）；文字查询触发图文混合检索 → base64 传给 VLM
- **Web Agent / Browser Agent**：`pip install browser-use playwright && playwright install chromium`；Agent 接收截图 → VLM 识别元素 → 输出动作（click/type/scroll）→ Playwright 执行 → 截图反馈
- **语音 Agent 基础**（了解）：Whisper API 做 ASR；`edge-tts` 或 OpenAI TTS 做语音输出；以工具形式接入 Agent
- **工具边界**：`browser-use`、`playwright` 只是实现载体，核心是理解“感知-决策-执行-反馈”闭环

**重点学习资源：**
- 📚 [Qwen2.5-VL 官方文档](https://qwen.readthedocs.io/en/latest/multimodal/vl_guide.html) — 图片输入格式 + 工具调用示例，必读
- 📚 [PyMuPDF 文档: Working with Images](https://pymupdf.readthedocs.io/en/latest/how-to-work-with-images.html)
- 📚 [PaddleOCR 快速开始](https://paddlepaddle.github.io/PaddleOCR/latest/quick_start.html)
- 🎓 [DeepLearning.AI: Building Multimodal Search and RAG](https://www.deeplearning.ai/short-courses/building-multimodal-search-and-rag/)
- 📚 [browser-use Quickstart](https://docs.browser-use.com/quickstart) — 作为 Browser Agent 工具示例

**论文精读：**
- **LLaVA** (Liu et al., 2023) ⭐ — VLM 指令微调奠基
- **Qwen2.5-VL Technical Report** (2025) ⭐ — 读 Architecture 和 Document Understanding 部分
- **CogAgent** (Hong et al., 2023) — GUI 操作专用 VLM
- **WebArena** (2023) — Web Agent 标准评测体系

**实践：**
- `pip install PyMuPDF pdfplumber pdf2image paddlepaddle paddleocr transformers browser-use playwright && playwright install chromium`
- **PDF 解析**：实现 `parse_pdf(path) -> list[{page_num, type, content}]`；人工验证 5 页识别率 ≥ 95%
- **多模态 RAG**：Qdrant 混合索引（文字用 bge-large-zh，图片用 CLIP）→ `qwen2.5-vl`（本地或 API）→ 测试 5 个图表问题
- **Web Agent**：任选一种浏览器自动化组合实现 arxiv 搜索任务（自动搜索 → 找到第一篇 → 提取标题摘要 → 返回 JSON）
- 机器资源不足时：优先用 API 跑 VLM；Browser Agent 可降级为“截图理解 + Playwright 固定流程”
- **交付标准：** PDF 解析识别率 ≥ 95%；多模态 RAG 5/5 图表问题正确；Web Agent 任务成功（3 次尝试）

</details>

<details>
<summary><strong>Week 12：LLMOps + 服务化部署 + 微调</strong></summary>

**本周建议拆分：**
- `Minimum`：FastAPI 服务化 + Docker Compose + 一套 tracing/日志方案 + 基础安全治理
- `Stretch`：vLLM、K8s、LoRA 三选一做深，不建议第一次就一周内全部做满

> **后端优先级提醒：** 对后端工程师转 Agent 而言，`鉴权 / 限流 / 队列 / 幂等 / 监控` 的优先级高于 `LoRA / DPO / GRPO`。

**知识点：**
- **FastAPI 流式服务化**：`StreamingResponse` + SSE（`yield f"data: {token}\n\n"`）；`async def` + `asyncio.gather()` 并发工具调用；`/health` + `/metrics`；`asyncio.wait_for(timeout=60)` 超时控制
- **Docker 容器化**：多阶段 Dockerfile（builder → runner，镜像体积减小 50%+）；`docker-compose.yml`（FastAPI + Redis + Qdrant + Langfuse）；`.env` 文件管理密钥
- **Kubernetes 基础**（理解概念，不要求深度运维）：`Deployment`/`Service`/`Ingress`/`ConfigMap`（Prompt 热更新）/`CronJob`（定时触发）/`HPA`（自动扩缩容）；知道这些对象如何支撑 Agent 服务即可
- **vLLM 私有化部署**：了解它作为推理服务层的定位即可，需要私有化部署时再深入
- **LLMOps / Tracing 工具**：Langfuse 只是示例；核心是成本监控、trace 记录、问题样本回放
- **生产接口治理**（这部分在真实项目里非常常见，建议至少做最小版）：
  - 鉴权：API Key / JWT 二选一
  - 限流：按用户 / IP 做 rate limit
  - 后台任务：长耗时任务放入队列，不阻塞请求线程
  - 幂等：重复请求不重复扣费、不重复执行副作用工具
  - 配置分环境：dev / staging / prod 独立配置和 secrets
  - 审批点：发邮件、写库、调用外部系统前可插入人工确认
  - Kill Switch / 回滚：发现异常时可立刻停掉 agent 执行或回退到上一个稳定 Prompt / 配置
- **Agent 安全完整体系**（4 个层面）：
  - **Prompt Injection 防护**：直接注入（正则过滤）+ 间接注入（tool 结果过一层 sanitizer LLM）
  - **越狱攻击识别**：角色扮演绕过/编码绕过/分步绕过；输出层 Llama Guard 或 `detoxify` 分类器
  - **PII 过滤**：`pip install presidio-analyzer`；中国身份证/手机号/银行卡正则检测 + 脱敏替换
  - **工具调用审计日志**：`{timestamp, agent_id, tool_name, args, result_hash, user_id}` append-only 存储；最小权限原则
- **运维最小闭环**：异常报警、成本阈值报警、超时熔断、重试上限、事故复盘模板。能不能“出问题后可控”，比能不能跑出漂亮 demo 更重要
- **微调精要**（跑通 pipeline > 深入算法）：
  - SFT 数据构造：从 Agent 运行日志提取高质量 tool use 轨迹，格式化为 JSONL
  - `pip install transformers peft trl datasets`；`SFTTrainer` 10 行核心代码；`LoraConfig(r=16, lora_alpha=32)`
  - DPO：`{"prompt", "chosen", "rejected"}` 格式；`DPOTrainer`
  - GRPO 原理（了解）：DeepSeek-R1 用可验证奖励信号（代码执行结果/答案正确性）做强化学习，无需人工标注
  - **微调 vs RAG vs Prompt Engineering 决策树**：行为可描述 → Prompt Engineering；需外部知识 → RAG；需固定格式/风格 → 微调
- **后端工程补充**：如果你的目标是尽快拿下 Agent 开发岗，微调理解到“会判断是否该用”通常就够了，不必在 16 周内深挖训练体系

**重点学习资源：**
- 📚 [FastAPI 文档: Async](https://fastapi.tiangolo.com/async/)
- 📚 [vLLM 文档: Docker 部署](https://docs.vllm.ai/en/latest/serving/deploying_with_docker.html)
- 📚 [Kubernetes 官方交互式教程](https://kubernetes.io/docs/tutorials/kubernetes-basics/) — 6 个模块，1 天完成
- 📚 [Langfuse Self-Hosting](https://langfuse.com/docs/deployment/self-host) — 作为 tracing 平台自托管示例
- 📚 [Datawhale self-llm: Qwen2.5 LoRA 教程](https://github.com/datawhalechina/self-llm)

**论文精读：**
- **InstructGPT** (Ouyang et al., 2022)
- **DPO** (Rafailov et al., 2023) ⭐
- **DeepSeek-R1** (2025)

**实践：**
- FastAPI + Docker：`POST /chat`（SSE）+ `GET /health`；多阶段 Dockerfile + docker-compose；`curl` 验证流式输出
- vLLM 压测：10 并发 5 分钟，记录 QPS/P99/GPU 显存；AWQ 量化前后对比
- K8s 实验：`minikube`，写 Deployment + ConfigMap（存 System Prompt）；理解它如何支持服务发布与热更新
- LoRA 微调：50 条 Tool Use 数据 → `SFTTrainer` 微调 `Qwen2.5-1.5B-Instruct`（Colab T4 可跑）→ 对比 20 条测试集格式正确率
- 最小生产化补充：给 `/chat` 增加鉴权、限流和请求日志；若时间有限，优先于 LoRA
- **交付标准：** 至少完成可用 API 服务 + 鉴权/限流/日志 + Docker 化；vLLM / K8s / LoRA 完成其一即可作为加分项

**Phase 3 阶段检验：** 能解释多 Agent 选型标准；多模态 Agent 能处理图文混合 PDF；Agent 服务能 Docker 部署并接入一套 tracing/日志方案；能判断何时该用 LoRA 而不是盲目微调。

</details>

---

## Phase 4：两个真实项目落地（Week 13–16）

> 核心目标：综合运用所有能力，构建代码开源、有量化评测基线的真实 Agent 系统。

> **后端视角：** 项目阶段的评价标准不是“功能堆了多少”，而是“系统边界是否清晰、接口是否稳定、评测是否可信、部署是否可复现”。

<details>
<summary><strong>Week 13–14：项目一 — 多模态智能文档分析 Agent</strong></summary>

**本项目定位：**
- 适合目标岗位偏文档智能、企业知识库、投研/咨询、报告生成。
- 如果你的目标岗位更偏通用 Agent 平台或工作流系统，这个项目可以做成“精简副项目”，重点保留 PDF 解析 + RAG + 报告生成主链路。

**目标：** 支持上传 PDF/扫描件/含图表的报告，Agent 自主理解图文内容 → 跨文档检索 → 结合搜索引擎补充最新信息 → 生成结构化分析报告。

**为什么选这个项目：** 文档智能是企业使用 AI Agent 最高频的真实场景；OCR + 图文检索 + 多模态 RAG 三个技术难点的组合在简历上区分度高。

**技术架构：**

```
upload_docs → parse → chunk_and_index
     ↓
[用户提问] → query_rewrite(HyDE) → hybrid_retrieve(Dense+BM25 RRF)
     → rerank(BGE Reranker) → check_need_web_search
                                 ↓(是)           ↓(否)
                            tavily_search        merge_context
                                 ↓
                       generate_report → quality_check(LLM打分)
                            ↓(分<7)          ↓(分≥7)
                        refine_report      output
```

- **基础模型**：Qwen2.5-VL-7B（图文理解，本地或 API）+ Qwen2.5-72B-Instruct API（报告生成）
- **文档处理**：PyMuPDF + pdfplumber（程序型）/ pdf2image + PaddleOCR（扫描型）；版式自动分类
- **索引**：文字 chunk（bge-large-zh + Qdrant）+ 图片 chunk（CLIP + 同一 Qdrant Collection）
- **工具集**：Tavily 搜索 / Python 代码执行 / matplotlib 图表生成
- **Memory**：可选用 DSPy 优化报告生成 Prompt；用户偏好存 Redis
- **部署**：FastAPI + Docker Compose（Qdrant + 模型服务 + Redis + tracing/logging）

**关键工程挑战（面试重点）：**
- 大 PDF 内存管理：分页流式处理，不全量加载
- OCR 错误容错：低置信度字符标记，不 crash 整体流程
- 图文 chunk 权重：纯文字查询如何触发图片检索（CLIP 图片描述增强）
- Context Window 压缩：Week 8 的 Compress 策略实际应用

**交付物：**
1. 图文混合问答准确率 ≥ 85%（20 条 Golden Dataset）
2. FastAPI + Docker Compose 一键启动，含一套 tracing / logging 能力
3. RAGAS + LLM-as-Judge 完整 Evals 报告（检索质量 + 报告质量 + 延迟/Token 成本）
4. GitHub README 含系统架构图 + 10 分钟 demo 录屏

</details>

<details>
<summary><strong>Week 15–16：项目二 — 企业多 Agent 自动化工作流</strong></summary>

**本项目定位：**
- 这是**更贴近后端工程师转 Agent 开发主战场**的项目，默认应作为主项目认真完成。
- 如果时间只够做 1 个重项目，优先把这个项目做完整，再把项目一做成精简版本。

**目标：** 构建多 Agent 编排的自动化系统。示例场景：竞品监控与分析工作流（可替换为任何业务场景）——数据采集 → 分析 → 报告生成 → 通知推送，定时触发，支持人工干预。

**为什么多 Agent 是合理的（量化论证，写进架构文档）：**
- 4 个数据源并发采集，天然可并行（串行会慢 4x）
- 分析 Agent 需要 Code Interpreter，通知 Agent 需要 Webhook，工具集隔离更安全
- 报告生成 Agent 的 Context 可能很长，与采集 Agent 完全隔离，避免互相污染

**技术架构：**

**第一阶段：Dify 快速原型（Day 1–2）**
- HTTP Request → LLM → Code → HTTP Request，2 天内跑通端到端，验证业务逻辑；重点是验证流程，不是研究 Dify 产品本身

**第二阶段：LangGraph 完整实现（Day 3–14）**
- **Orchestrator**：接收任务 → `Send()` 并行分发给 4 个 Worker（Context 隔离）
  - `search_agent`：Tavily + requests，工具集仅限搜索/读取
  - `analyze_agent`：Code Interpreter，接入 DeepSeek-V3 API
  - `report_agent`：Qwen2.5-72B + 可选的 Prompt 优化
  - `notify_agent`：调用 webhook 或等价通知接口
- **通知接口**：`send_webhook()` + `save_report()`（持久化到 SQLite）；MCP 只是可选封装方式
- **K8s 生产部署**：Helm Chart（Deployment + ConfigMap + CronJob 定时 + HPA 自动扩容）；理解其生产价值即可
- **LLMOps**：接入任一 tracing 平台 + Token 成本告警 + Prompt Injection 过滤中间件

**交付物：**
1. 工作流快速原型截图 + LangGraph 代码 + 原型工具 vs 自研实现对比分析（200字）
2. Helm Chart 可在 minikube 一键部署，CronJob 可演示定时触发
3. Evals 报告：10 次运行，质量/成本/延迟三维度；并行比串行加速比（目标 ≥ 3x）
4. 系统架构图 + 多 Agent 选型量化论证文档
5. 生产就绪检查清单（安全/成本/可观测性/错误处理）
6. 代码开源 GitHub，README 含完整部署指南

</details>

---

## 建议新增但不要一开始全学的工程内容

> 下面这些内容在真实 Agent 项目里经常比“前沿框架名词”更重要。建议在 Phase 3-4 按需加入，但不要在前 4 周全部摊开。

| 模块 | 为什么重要 | 建议插入位置 |
|------|-----------|-------------|
| 鉴权 / RBAC | Agent 往往会调工具和读数据，没有权限边界很危险 | Week 12 / 项目阶段 |
| Rate Limit / 配额 | 控成本、防刷、防雪崩 | Week 12 |
| 队列与异步任务 | OCR、检索、报告生成通常超过同步请求时长 | Week 12 / 项目阶段 |
| 幂等与重试 | Agent 天生容易多次调用工具，副作用控制很关键 | Week 9 / 12 |
| Prompt / Dataset 版本管理 | 不做版本化就无法科学比较实验结果 | Week 10 |
| CI/CD 与回滚 | Prompt、代码、模型配置都需要可回滚 | 项目阶段 |
| SLO / 告警 | 线上问题首先体现为延迟、错误率、成本飙升 | Week 12 / 项目阶段 |
| 失败案例复盘机制 | Agent 工程进步主要来自失败样本，不来自“成功 demo” | Week 10 之后持续执行 |

---

## 核心论文清单

<details>
<summary><strong>必读基础（Phase 1–2）</strong></summary>

| 论文 | 重要性 | 核心贡献 |
|------|--------|---------|
| [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) — Lilian Weng, 2023 | ⭐⭐⭐ | Agent 领域全局地图 |
| ReAct (Yao et al., ICLR 2023) | ⭐⭐⭐ | 推理-行动循环范式 |
| Reflexion (Shinn et al., 2023) | ⭐⭐⭐ | 自我反思与错误修正 |
| Toolformer (Schick et al., Meta 2023) | ⭐⭐⭐ | 工具自学习 |
| Chain-of-Thought (Wei et al., 2022) | ⭐⭐⭐ | 推理能力激发 |
| Generative Agents (Park et al., 2023) | ⭐⭐⭐ | 记忆+规划+反思完整架构 |
| Tree of Thoughts (Yao et al., 2023) | ⭐⭐ | 树状规划搜索 |
| HuggingGPT (Shen et al., 2023) | ⭐⭐ | 多模型编排 |
| Self-RAG (ICLR 2024) | ⭐⭐⭐ | RAG 自评估与自改进 |
| RAGAS (2023) | ⭐⭐ | RAG 量化评估框架 |

</details>

<details>
<summary><strong>进阶必读（Phase 2–3）</strong></summary>

| 论文 | 重要性 | 核心贡献 |
|------|--------|---------|
| DSPy (Khattab et al., 2023) | ⭐⭐⭐ | 程序化 Prompt 优化，告别手工调提示 |
| MetaGPT (2023) | ⭐⭐⭐ | 多 Agent 角色分工 SOP |
| GraphRAG (Microsoft, 2024) | ⭐⭐⭐ | 知识图谱增强检索 |
| Agent Workflow Memory (2024) | ⭐⭐⭐ | 跨任务工作流记忆 |
| LATS (Zhou et al., 2023) | ⭐⭐ | MCTS 引入 Agent 决策 |
| Voyager (Wang et al., 2023) | ⭐⭐ | 技能库 + 终身学习 Agent |
| Agentic RAG Survey (arxiv 2501.09136, 2025) | ⭐⭐⭐ | Agentic RAG 全景综述 |
| MemGPT (2023) | ⭐⭐ | 虚拟内存管理 |
| SWE-bench (2023) | ⭐⭐⭐ | 代码 Agent 评测标准 |

</details>

<details>
<summary><strong>前沿必读（Phase 3–4）</strong></summary>

| 论文 | 重要性 | 核心贡献 |
|------|--------|---------|
| InstructGPT (Ouyang et al., 2022) | ⭐⭐⭐ | RLHF 对齐奠基 |
| DPO (Rafailov et al., 2023) | ⭐⭐⭐ | 简化对齐训练 |
| DeepSeek-R1 (2025) | ⭐⭐⭐ | GRPO 训练推理能力 |
| Constitutional AI (Anthropic, 2022) | ⭐⭐ | 输出安全对齐 |
| Memory in the Age of AI Agents Survey (2025) | ⭐⭐⭐ | 记忆系统全景综述 |
| WebArena (2023) | ⭐⭐ | 网页自动化 Agent 评测 |

</details>

<details>
<summary><strong>多模态（Phase 3 Week 11）</strong></summary>

| 论文 | 重要性 | 核心贡献 |
|------|--------|---------|
| LLaVA (Liu et al., 2023) | ⭐⭐⭐ | VLM 指令微调奠基 |
| InternVL2 (Chen et al., 2024) | ⭐⭐⭐ | 顶级开源 VLM，多模态 Agent 备选 |
| CogAgent (Hong et al., 2023) | ⭐⭐ | GUI 理解专用 VLM |
| Qwen2.5-VL Technical Report (2025) | ⭐⭐⭐ | 多模态 Agent 基础模型 |
| OS-Atlas (2024) | ⭐⭐ | GUI 操作通用基础模型 |

</details>

---

## 核心资源总索引

### GitHub 项目

| 项目 | 使用阶段 | 用途 |
|------|---------|------|
| [mlabonne/llm-course](https://github.com/mlabonne/llm-course) | 贯穿全程 | LLM 工程师完整路径，含大量 Colab notebook |
| [datawhalechina/hello-agents](https://github.com/datawhalechina/hello-agents) | Phase 1–2 | Agent 原理与实践 |
| [datawhalechina/diy-llm](https://github.com/datawhalechina/diy-llm) | Phase 3 Week 12 | SFT/RLHF/GRPO 按需查阅 |
| [adongwanai/AgentGuide](https://github.com/adongwanai/AgentGuide) | 贯穿全程 | 工程化全景 + 面试题库 |
| [datawhalechina/llm-cookbook](https://github.com/datawhalechina/llm-cookbook) | Week 1–3 | Prompt + API 实战（吴恩达系列中文版）|
| [datawhalechina/llm-universe](https://github.com/datawhalechina/llm-universe) | Week 5 | RAG 应用开发 |
| [datawhalechina/self-llm](https://github.com/datawhalechina/self-llm) | Week 12 | LoRA/QLoRA 微调实操，含 Qwen2.5 章节 |
| [stanfordnlp/dspy](https://github.com/stanfordnlp/dspy) | Week 8 | DSPy 程序化 Prompt 优化 |
| [assafelovic/gpt-researcher](https://github.com/assafelovic/gpt-researcher) | Week 13–14 | 项目一工作流设计参考 |
| [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) | Week 9 | MCP 服务器生态，参考已有 Server 实现 |
| [QwenLM/Qwen-Agent](https://github.com/QwenLM/Qwen-Agent) | Week 9 | 阿里官方 Agent 框架，Function Call / Code Interpreter |
| [QwenLM/Qwen2.5-VL](https://github.com/QwenLM/Qwen2.5-VL) | Week 11 | 多模态 Agent 首选基础模型 |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | Week 12 | 生产级 LLM 推理服务，Docker 一行部署 |
| [langfuse/langfuse](https://github.com/langfuse/langfuse) | Week 10 / 12 | LLMOps 全链路观测，支持私有化部署 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Week 11 | 开源 Web Agent 框架，VLM 驱动浏览器 |
| [langgenius/dify](https://github.com/langgenius/dify) | Week 9 / 15–16 | 企业 Agent 落地低代码平台，快速原型 |
| [OpenGVLab/InternVL](https://github.com/OpenGVLab/InternVL) | Week 11 | 顶级开源 VLM，多模态 RAG 备选 |
| [FlagOpen/FlagEmbedding](https://github.com/FlagOpen/FlagEmbedding) | Week 7 | BGE Reranker + BGE Embedding，RAG 精排 |

### 学术课程

| 课程 | 机构 | 用途 |
|------|------|------|
| [CS294 LLM Agents (Fall 2024)](https://rdi.berkeley.edu/llm-agents/f24) | Berkeley | Agent 领域最权威系统课，贯穿 Phase 1–3 |
| [CS294 Agentic AI (Fall 2025)](https://rdi.berkeley.edu/agentic-ai/f25) | Berkeley | 最新进展，Phase 2–4 |
| [CS294 Advanced LLM Agents (Spring 2025)](https://rdi.berkeley.edu/adv-llm-agents/sp25) | Berkeley | 推理/多模态/安全进阶，Phase 3 |
| [CS329A: Self-Improving AI Agents](https://cs329a.stanford.edu/) | Stanford | 自我改进 + 工具增强，Phase 3 参考 |
| [DeepLearning.AI 短课系列](https://www.deeplearning.ai/short-courses/) | Andrew Ng | 全部免费，各周按需使用 |

### YouTube 频道

| 频道 | 用途 |
|------|------|
| [LangChain 官方](https://www.youtube.com/@LangChain) | 框架实战，Phase 2–3 |
| [LangChain Interrupt 2025](https://www.youtube.com/playlist?list=PLfaIDFEXuae3LIv3FbwVmqxw-s_WqP8iy) | 行业前沿，Phase 3–4 |
| [Arize AI](https://www.youtube.com/@ArizeAI) | Agent Evals，Phase 3 |
| [DeepLearning.AI](https://www.youtube.com/@Deeplearningai) | 短课补充 |

---

## 每周学习节奏

| 类型 | 占比 | 原则 |
|------|------|------|
| 视频课程 | 25% | 边看边做笔记，核心课不超速 |
| 论文精读 | 15% | 每周 1–2 篇，做批注 + 100字摘要 |
| 动手实践 | 50% | **每周必须有可运行代码产出，不接受"大概理解了"** |
| 输出沉淀 | 10% | 博客/GitHub，强迫自己把模糊理解变成清晰表达 |

**实践原则（来自行业教训）：**
1. **先手写，再用框架**：Week 1–4 手写 Agent 是刻意设计，不能跳过，否则后续框架学习只会用不会理解
2. **量化一切**：每个实践的"交付标准"是硬性要求，不达标不进入下一周；不接受"跑通了但没有量化"
3. **先单 Agent 后多 Agent**：不要在 Week 9 前引入多 Agent，单 Agent 做不好多 Agent 一定更差
4. **不要逐一学框架**：理解框架解决的共性问题，用 5 个评估问题快速上手任何新框架
5. **先保主线闭环，再做进阶专题**：GraphRAG / DSPy / Browser Agent / LoRA / K8s 都很有价值，但它们不该阻塞主线项目落地
6. **高风险工具先做最小权限**：代码执行、文件系统、Webhook、浏览器自动化都默认按“不可信输入”处理

---

> 论文均可在 [arxiv.org](https://arxiv.org) 免费获取。Berkeley CS294 课程视频在课程主页免费观看。DeepLearning.AI 短课全部免费。
