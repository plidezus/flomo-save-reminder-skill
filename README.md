# flomo skills：保存提醒 + 昨日回声

和 AI 聊得最起劲的那些句子，往往是你自己说出来的。

但 AI 不是笔记本，它不替你记。关掉窗口，那些被对方点破的判断、临时蹦出来的标题、半夜两点的灵感，就一起溜走了。就算偶尔记进 flomo 了，过两天也很少会再翻——存下来的内容，最容易躺着不动。

这是一组面向中文 flomo 用户的本地 agent skills。两个 skill 一头一尾，加上一份共享画像：

- **flomo-save-reminder**：在 AI 对话里发现值得留下的内容，按你自己的写法整理成 memo 草稿，确认后再写入。让有价值的瞬间不溜走。
- **flomo-daily-echo**：读取某一天的 memo 和相关旧 memo，从中找一条新旧之间真实存在的张力，生成一条可以继续讨论的「回声」，不是日报。让存下来的内容不只是躺着。
- **flomo-shared**：共享的 flomo profile，负责初始化 / 刷新你的表达画像，也保存你明确确认过的轻量偏好。

它们都不抢着替你做事，只在该停一下的时候停一下。

## 核心原则

- **触发要窄**：普通聊天不会因为出现「想法」「记录」「flomo」等词就自动触发。
- **用户保留刹车**：写入 flomo、开启主动提醒、设置定时，都需要明确授权。
- **你的写法只学一次**：两个 skill 共用同一份 profile；画像来自样本，主动性和授权只来自你的明确确认。
- **样本优先**：个性化来自真实 memo 样本和你确认保存过的内容，不来自 AI 想象。
- **失败诚实**：读不到东西就说读不到，不假装做了。

## flomo-save-reminder

它分得清「这是值得回看的判断」和「这只是一句对话」——大部分时候它选择不打扰。

它会停下来提醒你：

- 你说「保存到 flomo」「记一下」「整理成 memo」
- 你让它扫描当前对话，看有没有值得保存的内容
- 你已经授权本轮轻提醒

它不会冒出来：

- 普通事实问答
- 临时代码排障
- AI 自己冒出来一段总结，但你没接话
- 只有链接、摘录或信息转述，没有你自己的反应

首次使用会先问你希望保存提醒多主动，再生成或读取共享 profile。默认只起草和提醒，写入前确认。

## flomo-daily-echo

它的目标不是「每日总结」，而是：

> 从目标日期 memo 与相关旧 memo 中找出一个真实存在的张力，生成一条用户可能愿意继续讨论的回声。

如果新旧 memo 之间没有张力、没有延续、没有反差，就不硬写。日报最容易写成空话，回声宁可沉默。

### 触发

只在你明确要求时才运行，例如：

- 「看看昨天的 flomo」
- 「昨日回声」
- 「昨天能出来啥」
- 「每日回顾」
- 「dailyreview」

普通聊天里出现「回顾」「记录」「Echo」等词，不应该触发。

### 它会输出三种结果

- **昨日回声**：新旧 memo 之间能说明张力、延续、反差或分叉。
- **昨日观察**：有当日 memo，但旧 memo 证据不足；不叫「回声」。
- **沉默**：没东西就不硬写。

它优先看你自己反复回到的话题，而不是只按标签翻笔记。具体协议在 `SKILL.md` 里。

## 安装

### OpenAI Skills / Codex 兼容环境

完整安装：

```bash
cp -R skills/flomo-shared ~/.codex/skills/
cp -R skills/flomo-save-reminder ~/.codex/skills/
cp -R skills/flomo-daily-echo ~/.codex/skills/
```

只安装保存提醒时，也建议同时安装 `flomo-shared`：

```bash
cp -R skills/flomo-shared ~/.codex/skills/
cp -R skills/flomo-save-reminder ~/.codex/skills/
```

安装后重启 Codex。

也可以让助手从 GitHub 安装：

```text
Install the skills from https://github.com/plidezus/flomo-skills/tree/main/skills
```

### 其他 Agent 环境

把需要的 skill 目录放到你的 agent 支持的技能目录中，或让 agent 直接读取对应 `SKILL.md`：

```text
skills/flomo-save-reminder/SKILL.md
skills/flomo-daily-echo/SKILL.md
```

使用前确认 agent 具备这些能力：

- 能读取相邻 skill 目录下的 `../flomo-shared/SKILL.md` 和 `../flomo-shared/profile.md`
- 能访问 flomo MCP，或等价的 memo 搜索、相关笔记、创建、标签规范和用户 memory 工具
- 能在写入 flomo 前先让用户确认

## 目录结构

```text
skills/
  flomo-shared/
    SKILL.md
    profile.md
    agents/openai.yaml
  flomo-save-reminder/
    SKILL.md
    agents/openai.yaml
  flomo-daily-echo/
    SKILL.md
    agents/openai.yaml
```

`flomo-shared/SKILL.md` 定义 profile 的初始化、刷新和旧版本迁移；`profile.md` 存共享画像和用户明确确认过的轻量偏好。保存提醒和昨日回声不各自生成画像，只读取共享 profile。

旧版本如果已有 `flomo-save-reminder/user-style.md` 或 `state.md`，新版会在首次需要 profile 时提示迁移。画像可以迁移，明确授权过的偏好可以迁移，临时运行状态不迁移。

## 隐私边界

这个公开仓库只放通用协议和空模板：

- `SKILL.md`
- `agents/openai.yaml`
- 空结构共享画像

不会放：

- 你的真实 memo
- 你的标签画像
- flomo 导出数据
- MCP 返回样本
- 个人化后的 `flomo-shared/profile.md`

## 想 fork 或贡献的话

提交前跑一遍这几条，确保没把私人内容混进公开仓库：

```bash
find skills -maxdepth 3 -type f -print | sort
rg -n "真实用户姓名|本地绝对路径|私人标签|memo 原文" skills README.md
git diff --cached
```
