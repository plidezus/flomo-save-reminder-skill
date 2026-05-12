# flomo 保存提醒 Agent Skill

面向中文 flomo 用户的本地 agent skill：在 AI 对话中发现值得保存的想法、感受、阅读启发、产品判断、工作经验或生活片段，先生成本地用户风格画像，再按用户已有 memo 风格整理成 flomo 草稿。

它不是 Codex 专用。任何支持“读取 Markdown 指令文件 + 调用 flomo MCP 或等价工具 + 写入本地画像文件”的 AI agent，都可以按这个目录使用或移植。

## 特色

- **不是简单保存提示词**：它定义的是一套保存判断协议，包括什么时候提醒、什么时候不提醒、如何确认、如何写入。
- **面向中文 flomo 语境**：默认覆盖灵感、情绪、自我观察、生活记录、阅读启发、交流反馈和工作经验，而不是只覆盖知识摘录或会议纪要。
- **先学习用户风格**：首次使用会从 `user-style.md` 的空结构开始，尝试结合 flomo memory 与 memo 样本生成本地画像。
- **画像和通用协议分离**：`SKILL.md` 放通用规则，`user-style.md` 放用户自己的格式、标签、语气和提醒偏好。
- **memory 不替代 memo 样本**：memory 只帮助理解用户和近期语境；标签、长度、语气仍以真实 memo 习惯为准。
- **保守写入**：默认只提醒和起草，不擅自写入；只有用户确认或明确授权后才写入 flomo。
- **隐私本地化**：公开仓库只包含空结构画像，不包含真实 memo、标签画像或 MCP 返回样本。

## 安装

### Codex / OpenAI Skills 兼容环境

让助手安装：

```text
Install the skill from https://github.com/plidezus/flomo-save-reminder-skill/tree/main/skills/flomo-save-reminder
```

或手动复制：

```bash
cp -R skills/flomo-save-reminder ~/.codex/skills/
```

安装后重启 Codex。

### 其他 Agent 环境

把 `skills/flomo-save-reminder/` 作为一个完整 skill 目录放到你的 agent 支持的技能/指令目录中，或直接让 agent 读取：

```text
skills/flomo-save-reminder/SKILL.md
```

使用前确认 agent 具备这些能力：

- 能读取同目录下的 `user-style.md`
- 能在本地更新 `user-style.md`
- 能访问 flomo MCP，或有等价的 memo 搜索、创建、标签规范和用户 memory 工具
- 能在写入 flomo 前先让用户确认

## 首次使用

公开仓库包含一个空结构的 `user-style.md`：

```markdown
profile_status: empty
updated_at:
sample_window:
sample_count:
```

首次使用时，skill 会把它判断为 `empty`，然后尝试通过 flomo MCP 的 memory 和 memo 样本生成本地画像。生成后会把 `profile_status` 改为 `generated`。

## 隐私边界

这个仓库只包含通用协议：

- `SKILL.md`
- `agents/openai.yaml`
- 空结构 `user-style.md`

不会包含：

- 用户真实 memo
- 用户标签画像
- flomo 导出数据
- MCP 返回样本

## 发布前检查

```bash
find skills/flomo-save-reminder -maxdepth 3 -type f -print
rg -n "<your-private-terms>" skills/flomo-save-reminder
git diff --cached
```

`find` 结果应只包含：

```text
skills/flomo-save-reminder/SKILL.md
skills/flomo-save-reminder/agents/openai.yaml
skills/flomo-save-reminder/user-style.md
```
