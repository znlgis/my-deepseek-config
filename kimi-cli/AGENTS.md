# AGENTS.md — Kimi CLI × DeepSeek 项目记忆

> Kimi CLI（Kimi Code CLI）会自动读取项目根目录的 `AGENTS.md`（可用 `/init` 生成）。
> 核心通用规则见 [`shared/RULES.md`](../shared/RULES.md)，本文件仅保留 Kimi CLI 专属要点。

## 模型分工（DeepSeek 双模型）

Kimi CLI 通过 `[providers.deepseek]` 走 DeepSeek 的 OpenAI 兼容端点，配置了两个模型：

- **主模型（Pro）**：`deepseek-reasoner`（`always_thinking`）—— 规划、架构、根因分析、代码审查、重型实现。
- **轻量模型（Flash）**：`deepseek-chat` —— 探索、检索、简单编辑、通用问答（默认）。

在 shell 模式用 `/model` 切换模型与思考模式，或启动时用 `--thinking` / `--no-thinking`。

## 核心原则（摘要）

1. 先辨意图再动手：「解释 X」不是「改 X」。
2. 最小改动，完整正确优先，不碰无关代码。
3. 先读后写，用真实文件验证假设。
4. 并行处理独立读取 / 搜索。
5. 未经请求不创建文件。
6. 用操作系统语言回复用户。

## 多步任务纪律

≥2 步任务先写待办清单，一次一个「进行中」，完成即标记，不批量。

## 反模式（阻断）

不建万能文件；不用 emoji / AI 填充词；不留死代码与空 catch；未经请求不创建文件。

## 提交前自检

复读改动文件；确认未破坏调用方；有测试则运行；清理未用导入 / 变量。
