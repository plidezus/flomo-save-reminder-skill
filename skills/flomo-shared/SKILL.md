---
name: flomo-shared
description: Use when a flomo skill needs to read, initialize, refresh, or migrate the shared flomo profile. Do not use for saving a memo, producing Daily Echo, or ordinary chat.
---

# flomo Shared

## 核心定位

本 skill 负责 flomo 相关 skills 共用的 profile。

它不保存 memo 草稿，不生成昨日回声，不设置定时，也不决定其他 skill 什么时候开口。它只管理一个共享文件：

- `profile.md`：用户的 flomo 表达画像，以及用户明确确认过的轻量偏好。

## Profile 结构

`profile.md` 里有两类生命周期不同的内容：

1. **Expression Profile**
   - 从真实 flomo memo 样本生成或刷新。
   - 描述写法、标签、长度、格式，以及什么内容值得保存。

2. **User Preferences**
   - 只能在用户明确确认后修改。
   - 存保存提醒模式、昨日回声默认值、Tips 是否展示过等轻量偏好。
   - 不要从 memo 样本推断授权、主动性或自动化边界。

不要混用这两类内容。刷新 Expression Profile 时，不能顺手重写已经确认过的 User Preferences，除非用户明确要求。

## 触发边界

只在以下情况使用：

- 用户说要初始化、刷新、重建或迁移 flomo profile。
- `flomo-save-reminder` 或 `flomo-daily-echo` 需要 `profile.md`，但发现它缺失、为空、无效或过期。
- 用户问为什么 flomo skills 不像自己的 memo 写法。

不要在以下情况使用：

- 用户要求保存当前对话到 flomo。
- 用户要求生成昨日回声或每日回顾结果。
- 用户只是在讨论产品策略或 skill 设计。
- 当前任务不需要 flomo 用户 profile。

## 状态检查

使用 profile 前，先判断 `profile.md` 状态：

- `missing`：文件不存在。
- `empty`：文件存在，但 `profile_status: empty`、仍有占位内容，或没有 `updated_at`。
- `invalid`：关键 metadata 缺失、格式错误或无法解析。
- `stale`：`updated_at` 距今超过 30 天。
- `fresh`：`profile_status: generated`，`updated_at` 有效，且没有模板占位。

处理规则：

- `fresh`：直接使用。
- `stale`：当前任务可以先用，合适时再提示刷新。
- `missing` / `empty` / `invalid`：除非用户已经要求初始化，否则先征得同意，再初始化或修复。
- 如果用户拒绝初始化，其他 skill 可以用保守默认值继续，但要说明没有使用个性化 profile。

## 初始化和刷新

生成或刷新 Expression Profile 时，按顺序采样真实 flomo 数据：

1. 最近 3-6 个月随机 memo：可用时取 20-50 条。
2. 最近 7-14 天 memo：可用时取 5-10 条。
3. 每日回顾、历史推荐或相关旧 memo：可用时取 3-5 条。
4. 被其他 skill 调用时，可按当前话题补充 3-5 条相关样本。

不要只读最近几条。最近样本容易被临时项目、情绪或话题污染。

学习维度：

- 是否使用标签，标签在开头还是结尾。
- 常见标签层级，哪些宽标签应该避免滥用。
- 常见长度：一句话、短段落、bullet，还是较长反思。
- 是否保留用户原话。
- 是否记录时间、场景、人物、来源或下一步。
- 语气是口语、克制、反思、决策导向，还是正式。
- 用户倾向保存什么，什么只是噪音。

如果有 memory 接口，只能作为背景上下文。风格和标签必须来自真实 memo 样本或用户确认保存过的草稿。

## 旧版本迁移

旧版本可能有：

```text
flomo-save-reminder/
  user-style.md
  state.md
```

迁移规则：

- 如果 `flomo-shared/profile.md` 不存在，可以提示把旧 `user-style.md` 迁移进 `Expression Profile`。
- 旧 `state.md` 里只有用户明确确认过的偏好可以迁移，例如保存提醒模式。
- 不迁移临时运行状态，例如 last prompted time、本轮会话标记、临时计数器。
- 如果 `profile.md` 已存在，不自动覆盖，先询问是否合并。
- 除非用户明确要求删除或归档，否则保留旧文件不动。

推荐提示：

```markdown
我发现你有旧版 flomo 保存画像。要不要迁移到新的共享 profile？之后保存提醒和昨日回声都会共用它。
```

## 写入规则

只把 profile 数据写入 `profile.md`。

不要保存：

- 原始 memo 导出。
- 大段 memo 原文。
- 私人标签或个人样例到 `SKILL.md`。
- 未确认的主动性、自动化或授权推断。
- 只对本轮有效的运行痕迹。

写入或刷新 profile 后，简短说明：

- `updated_at`
- sample window
- sample count
- 这次学到或改动了哪些维度

如果生成失败，直接说明原因，不要假装已经加载用户 profile。
