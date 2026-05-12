# flomo 保存提醒 Skill

面向中文 flomo 用户的 Codex skill：在 AI 对话中发现值得保存的想法、感受、阅读启发、产品判断、工作经验或生活片段，先生成本地用户风格画像，再按用户已有 memo 风格整理成 flomo 草稿。

## 安装

在 Codex 中让助手安装：

```text
Install the skill from https://github.com/plidezus/flomo-save-reminder-skill/tree/main/skills/flomo-save-reminder
```

或手动复制：

```bash
cp -R skills/flomo-save-reminder ~/.codex/skills/
```

安装后重启 Codex。

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
