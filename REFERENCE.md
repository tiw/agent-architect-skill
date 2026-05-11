# Agent 组件知识库（Reference）

> 基于 LangChain/LangGraph、OpenCode、OpenClaw、Claude Code、CrewAI、AutoGen 等主流框架分析

---

## 一、完整组件清单

### 1.1 推理引擎 (Reasoning Engine)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **LLM Provider** | 模型接入抽象，支持多厂商切换 | LangChain ChatModel, OpenCode Provider, pi-ai |
| **Agent Loop** | 核心循环：`think → act → observe → repeat` | ReAct, Plan-Execute, Reflexion |
| **Streaming Handler** | 流式输出处理，实时展示推理过程 | SSE, WebSocket streaming |
| **Fallback Strategy** | 模型故障降级（切换模型/重试/缓存） | Circuit breaker, Model router |
| **Token Budget** | Token 预算管理，防止上下文溢出 | Context window tracker |

### 1.2 工具系统 (Tool System)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Tool Registry** | 工具注册中心，动态发现与管理 | OpenCode Tool Registry, MCP |
| **Tool Schema** | 工具定义（名称/描述/参数/返回值） | JSON Schema, TypeBox, Zod |
| **Tool Dispatcher** | 工具调用路由与执行 | Function calling, Code generation |
| **Tool Shortlisting** | 上下文感知工具筛选 | Semantic tool retrieval |
| **Tool Schema Guard** | 参数校验与类型安全检查 | Runtime validation, MCP validation |
| **Sandbox** | 工具执行沙箱，隔离危险操作 | Docker, E2B, Local sandbox |
| **Tool Result Feedback** | 执行结果回传，驱动下一轮推理 | Observation injection |

### 1.3 记忆系统 (Memory System)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **In-Context Memory** | 当前对话窗口（短期记忆） | Message array, Conversation buffer |
| **Vector Memory** | 语义检索记忆（RAG） | Chroma, Pinecone, pgvector |
| **Episodic Memory** | 历史经验记录 | JSONL logs, Action history |
| **Semantic Memory** | 事实知识存储 | YAML frontmatter, Knowledge graph |
| **Compaction** | 上下文压缩/摘要 | Summarization, Rolling window |
| **Memory Search** | 混合检索（关键词+语义+时间） | Hybrid search, Entity index |

### 1.4 上下文管理 (Context Management)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Prompt Assembly** | 动态组装系统提示+上下文+工具描述 | Template engine, Jinja2 |
| **Context Loader** | 按需加载相关上下文 | File watcher, Git index |
| **Context Compression** | 长上下文压缩策略 | Map-reduce, Selective inclusion |
| **Scratchpad** | 中间推理痕迹 | Reasoning traces |
| **System Prompts** | 分层系统提示 | SOUL.md, IDENTITY.md, AGENTS.md |

### 1.5 规划与编排 (Planning & Orchestration)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Task Planner** | 任务分解为可执行子任务 | TodoManager, Plan tool, Goal tree |
| **Plan Validator** | 计划可行性检查与修正 | Plan approval gate |
| **Multi-Agent Orchestrator** | 多 Agent 协作调度 | LangGraph, CrewAI, AutoGen |
| **Agent Handoff** | Agent 间任务交接 | OpenAI Agents SDK handoff |
| **State Machine** | 状态流转控制 | LangGraph StateGraph |
| **Dependency Graph** | 任务依赖管理与拓扑排序 | DAG executor |

### 1.6 会话管理 (Session Management)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Session Store** | 会话持久化 | JSONL append-only, SQLite |
| **Session Lifecycle** | 创建/暂停/恢复/结束/归档 | State machine |
| **Message Compaction** | 消息历史压缩与摘要 | Rolling summary |
| **Session Isolation** | 多会话隔离 | Per-project DB, Namespace |
| **Resume & Fork** | 从任意点恢复或分支会话 | Checkpoint, Git-like fork |

### 1.7 权限与安全 (Permission & Security)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Permission System** | 分层权限 | Allowlist, Denylist, Role-based |
| **Approval Gates** | 高风险操作人工确认 | Plan approval, Destructive op check |
| **Sandbox** | 执行环境隔离 | Docker, Firejail, VM |
| **Audit Log** | 全操作审计追踪 | Structured logging, Immutable log |
| **Input Sanitization** | 输入清洗 | Filter, Escape, Validation |
| **Guardrails** | 输出安全检查与过滤 | NeMo Guardrails, Content filter |

### 1.8 触发与调度 (Triggers & Scheduling)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Event Trigger** | 事件驱动 | Event bus, Pub/Sub |
| **Cron Scheduler** | 定时任务 | Cron expression, Job queue |
| **Heartbeat** | 存活检测与状态上报 | Health check, Watchdog |
| **Channel Adapter** | 多平台消息适配 | Telegram, Slack, Discord, Email |

### 1.9 观测与调试 (Observability)
| 组件 | 说明 | 代表实现 |
|------|------|----------|
| **Tracing** | 推理链路追踪 | LangSmith, OpenTelemetry |
| **Cost Tracking** | Token 消耗与成本统计 | Usage meter, Budget alert |
| **Debug Mode** | 详细日志与中间状态导出 | Verbose logging, State dump |
| **Replay** | 会话回放与问题复现 | Session replay, Step debugger |

---

## 二、场景推荐矩阵

### 2.1 按 Agent 类型推荐

| Agent 类型 | 必选项 | 强烈建议 | 可选增强 | 预估工时 |
|-----------|--------|----------|----------|----------|
| **🤖 聊天助手** | LLM, In-Context Memory, Prompt Assembly | Streaming, System Prompts | Vector Memory, Multi-Agent | 2-4h |
| **💻 代码 Agent** | LLM, Tool Registry, File Tools, Sandbox | Permission System, Plan Approval, Subagent | Episodic Memory, IDE Integration | 1-2d |
| **🔍 研究 Agent** | LLM, Web Search Tool, Vector Memory | Planning, Multi-Agent, Session Store | Tool Shortlisting | 2-3d |
| **🛠️ 自动化 Agent** | LLM, Tool Registry, Cron Scheduler | Event Trigger, Heartbeat, Audit Log | Recovery, Concurrency | 2-4d |
| **👥 多 Agent 团队** | Multi-Agent Orchestrator, Handoff, Event Bus | Session Isolation, State Machine | Agent Discovery | 1-2w |
| **🎮 游戏/仿真 Agent** | LLM, Execution Runtime, Fast Loop | Sandbox, State Machine, Memory | Planning | 3-5d |
| **📊 数据分析 Agent** | LLM, Code Execution, File Tools | Vector Memory, Visualization Tool | Planning, Result Caching | 2-3d |
| **🏢 企业流程 Agent** | LLM, Tool Registry, Permission System, Audit Log | Approval Gates, Guardrails, Session Store | SSO, RBAC | 1-3w |

### 2.2 按复杂度分级

```
Level 1: 最小可用 Agent (MVP)
  👤 适合: 个人开发者 / 黑客松 / 快速验证
  ✅ LLM Provider
  ✅ Agent Loop
  ✅ 2-3 个基础 Tools
  ✅ In-Context Memory
  ✅ System Prompt
  ~ 200 行代码

Level 2: 可用 Agent (Production-ready)
  👤 适合: 小团队 / 内部工具 / 个人生产力
  ⬆️ 以上全部 +
  ✅ Tool Registry (10+ tools)
  ✅ Session Persistence
  ✅ Streaming Output
  ✅ Error Handling & Retry
  ✅ Basic Permission
  ✅ Cost Tracking
  ~ 2000 行代码

Level 3: 高级 Agent (Enterprise)
  👤 适合: 企业部门 / 产品化 / 多用户场景
  ⬆️ 以上全部 +
  ✅ Vector Memory / RAG
  ✅ Task Planning & Decomposition
  ✅ Multi-Agent Orchestration
  ✅ Approval Gates
  ✅ Sandbox Execution
  ✅ Audit Log & Compliance
  ✅ Cron & Event Triggers
  ✅ Context Compaction
  ✅ Tracing & Observability
  ~ 10000+ 行代码

Level 4: 平台级 Agent (Platform)
  👤 适合: 平台公司 / SaaS 产品 / 大规模部署
  ⬆️ 以上全部 +
  ✅ Gateway (HTTP/WebSocket)
  ✅ Multi-Channel
  ✅ Plugin System
  ✅ Self-improving
  ✅ Horizontal Scaling
```

---

## 三、主流框架组件对比

| 组件维度 | LangChain/LangGraph | OpenCode | OpenClaw | Claude Code | CrewAI |
|---------|---------------------|----------|----------|-------------|--------|
| **核心哲学** | 模块化组合 | 客户端-服务器分离 | Harness Engineering | 安全可控 | 角色扮演协作 |
| **Agent Loop** | AgentExecutor / Graph | SessionPrompt.loop() | pi-mono runtime | ReAct + Permission | Task-based |
| **多 Agent** | LangGraph StateGraph | Agent Teams (P2P) | Gateway routing | Subagent delegation | Crew + Process |
| **工具系统** | @tool decorator | Tool Registry (20+) | Plugin tools | 54 built-in tools | @tool decorator |
| **记忆** | Memory classes | Session compaction | Hybrid memory | File-based + Context | Short-term |
| **权限** | 基础 | 两级权限 | Auth + Trust | 分层审批 | 无 |
| **触发方式** | 手动/API | API + TUI | Cron + Heartbeat + Webhook | 手动 CLI | 手动 |
| **持久化** | 可选外部存储 | SQLite per-project | JSONL + File system | JSONL sessions | 无 |
| **上下文** | Prompt templates | Context files (.opencode/) | Workspace files | Selective file read | Task description |
| **扩展性** | 高 | 高 | 高 | 中 | 中 |
| **学习曲线** | 陡峭 | 中等 | 中等 | 低 | 低 |
| **适用场景** | 复杂工作流 | 编码 Agent | 持久化个人助手 | 代码开发 | 协作任务 |

---

## 四、设计决策参考

### 4.1 关键抉择

**Q1: 手写 Loop vs 用框架？**
- 原型验证（< 3天）→ 用 LangChain / OpenAI Agents SDK
- 生产系统（> 3个月）→ 手写核心 Loop（~200行），借用框架的模型封装和 RAG
- 平台级产品 → 参考 OpenClaw Harness 架构

**Q2: 记忆怎么选？**
- 会话 < 10 轮 → In-Context Memory
- 需要跨会话 → JSONL + 摘要
- 大量知识 → Vector DB（RAG）
- 用户个性化 → Semantic Memory
- 经验积累 → Episodic Memory

**Q3: 单 Agent vs 多 Agent？**
- 单一领域、明确输入输出 → 单 Agent
- 研究+编码+测试 → Manager + Worker
- 多人协作仿真 → CrewAI
- 长周期自主运行 → OpenClaw

**Q4: 工具怎么管理？**
- 工具 < 10 个 → 全量注入 prompt
- 工具 10-30 个 → Tool Shortlisting
- 工具 30+ 个 → 分类注册表 + MCP

**Q5: 上下文窗口爆了怎么办？**
- 第一层：Compaction（摘要旧消息）
- 第二层：Selective Loading（只加载相关文件）
- 第三层：RAG Retrieval（从向量库检索）
- 第四层：Subagent（新会话处理子任务）

### 4.2 反模式警示

| 反模式 | 后果 | 正确做法 |
|--------|------|----------|
| **过度抽象** | Debug 困难 | 核心 Loop 手写 |
| **工具过载** | Token 浪费 | Tool Shortlisting |
| **无权限控制** | 安全风险 | 破坏性操作审批 |
| **忽视上下文压缩** | 成本飙升 | Proactively compact |
| **硬编码 Prompt** | 难以维护 | 外部化 System Prompts |
| **忽略审计日志** | 无法追溯 | 结构化记录 |

---

## 五、快速启动 Checklist

```markdown
□ 1. 定义 Agent 目标和领域
□ 2. 选择复杂度级别（Level 1-4）
□ 3. 选定 LLM Provider
□ 4. 设计 Tool 集合（从最少开始）
□ 5. 实现核心 Agent Loop
□ 6. 添加 Session 持久化
□ 7. 编写 System Prompt
□ 8. 设置权限边界
□ 9. 添加错误处理和重试
□ 10. 接入观测（日志 + 成本）
□ 11. （可选）添加记忆系统
□ 12. （可选）添加 Planning 能力
□ 13. （可选）扩展为多 Agent
□ 14. （可选）添加定时/事件触发
```
