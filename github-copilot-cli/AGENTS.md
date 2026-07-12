# AGENTS.md — GitHub Copilot CLI × DeepSeek 项目指令

> GitHub Copilot CLI 会自动读取仓库根目录的 `AGENTS.md`（以及
> `.github/copilot-instructions.md`）作为自定义指令。核心通用规则见
> [`shared/RULES.md`](../shared/RULES.md)，本文件保留可直接落地的纪律。

## 关于模型（重要）

GitHub Copilot CLI 的**后端模型由 GitHub 托管**（Claude / GPT / Gemini 等），
**目前无法把 DeepSeek 直接作为其推理模型**。因此本目录的定位是：

1. 用 `AGENTS.md` 注入与其它工具一致的「DeepSeek 双模型」工作理念与纪律；
2. 通过 MCP 把 DeepSeek 作为**可调用的工具/子模型**接入（见 `README.md` 的桥接方案）。

选择内置模型时，用 `/model` 选一个具备强推理能力的模型承担 Pro 角色的工作。

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
