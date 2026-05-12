# flomo 保存提醒

和 AI 聊久了，你会发现一件事：聊得最起劲的那些句子，往往是你自己说出来的。

但 AI 不是笔记本，它不替你记。关掉窗口，那些被对方点破的判断、临时蹦出来的标题、半夜两点的灵感，就一起溜走了。

flomo 的初衷是不让有价值的瞬间溜走。这个 skill 把它延伸到了 AI 对话里——看到一段你未来真的会回来翻的内容，它会停下来提醒你存一下，按你自己的写法整理成 memo 草稿，你确认后再写进 flomo。

它不抢着记，只在该记的时候停一下。

## 它在意什么

- **宁可不提醒，也不打扰**：普通问答、临时任务、AI 自己的总结，都不值得保存。只有未来你可能回来翻的内容，才停下来问你一句。
- **写得像你，而不是像 AI**：第一次使用会学你已有 memo 的格式、标签、语气——存下去的草稿，读起来是你写的，不是 ChatGPT 风格的小作文。
- **写入前先确认**：默认只起草和提醒，你点头才存。AI 越自动，越要给你留刹车。
- **隐私只在本地发酵**：你的写作风格画像存在本地，公开仓库里只有空结构。

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

## 它是怎么组织的

skill 分两层：

- `SKILL.md`：稳定协议。判断要不要提醒、怎么采样、什么时候才能写入。
- `user-style.md`：你自己的写法画像。格式、标签、语气、提醒偏好都在这里。

前者所有人共享，后者只属于你。升级 skill 不会动你的写法，换设备同步你的写法也不会污染协议。

## 首次使用

公开仓库里的 `user-style.md` 是一个空结构：

```markdown
profile_status: empty
updated_at:
sample_window:
sample_count:
```

第一次用的时候，skill 会把它判断为 `empty`，然后通过 flomo MCP 的 memory 和 memo 样本生成你的本地画像。生成完把 `profile_status` 改成 `generated`，下次直接读。

## 隐私边界

这个公开仓库只放通用协议：

- `SKILL.md`
- `agents/openai.yaml`
- 空结构 `user-style.md`

不会放：

- 你的真实 memo
- 你的标签画像
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
