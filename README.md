# flomo 保存提醒

一个面向中文 flomo 用户的本地 agent skill。

它在 AI 对话中发现值得留下的想法、感受、阅读启发、产品判断、工作经验和生活片段，先生成本地用户风格画像，再整理成更像用户自己的 flomo memo 草稿。

## 特色

- **先判断值不值得留**：普通问答、临时任务、AI 单方面总结都不急着保存；只有未来可能回看的内容才提醒。
- **贴近中文 flomo 场景**：覆盖灵感、情绪、自我观察、生活记录、阅读启发、交流反馈和工作经验。
- **先学习用户风格**：首次使用从空结构 `user-style.md` 开始，学习格式、标签、语气和提醒偏好。
- **规则和画像分开**：`SKILL.md` 放通用流程，`user-style.md` 放每个用户自己的写法。
- **写入保持克制**：默认只提醒和起草，确认后再写入 flomo。
- **隐私留在本地**：公开仓库只放空结构画像，不放真实 memo、标签画像或 MCP 返回样本。

## 安装

### OpenAI Skills / Codex 兼容环境

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

把 `skills/flomo-save-reminder/` 作为一个完整 skill 目录放到你的 agent 支持的技能目录中，或直接让 agent 读取：

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
reminder_mode: unset
updated_at:
sample_window:
sample_count:
```

首次使用时，skill 会先询问提醒方式：

1. 只在我说“保存/记一下”时使用
2. 我让你扫描当前对话时，帮我挑值得保存的内容
3. 本轮对话里，看到特别值得留的内容可以轻提醒

选完后再尝试通过 flomo MCP 的 memory 和 memo 样本生成本地画像。生成后会把 `profile_status` 改为 `generated`，并写入用户选择的 `reminder_mode`。

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
