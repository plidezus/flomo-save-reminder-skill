# flomo skills: 保存提醒 + 昨日回声

一组面向中文 flomo 用户的本地 agent skills。

它们不是“更会聊天的 AI”，而是把 flomo 的记录场景拆成两个更窄的操作协议：

- **flomo-save-reminder**：在 AI 对话里发现值得留下的内容，整理成 flomo memo 草稿，确认后再写入。
- **flomo-daily-echo**：读取某一天的 memo 和相关旧 memo，生成一条可讨论的“回声”，而不是日报。
- **flomo-shared**：共享 flomo 用户表达画像，供多个 flomo skills 复用。

## 核心原则

- **触发要窄**：普通聊天不会因为出现“想法”“记录”“flomo”等词就自动触发。
- **用户保留刹车**：写入 flomo、开启主动提醒、设置定时，都需要明确授权。
- **画像共享，状态分 skill**：用户怎么写 flomo 是共享画像；每个 skill 多主动、是否自动化，是各自状态。
- **样本优先**：个性化来自真实 memo 样本和用户确认保存的内容，不来自 AI 想象。
- **失败诚实**：读不到 memo、找不到旧证据、画像缺失时，要降级说明，不伪装成正常结果。

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
Install the skills from https://github.com/plidezus/flomo-save-reminder-skill/tree/main/skills
```

### 其他 Agent 环境

把需要的 skill 目录放到你的 agent 支持的技能目录中，或让 agent 直接读取对应 `SKILL.md`：

```text
skills/flomo-save-reminder/SKILL.md
skills/flomo-daily-echo/SKILL.md
```

使用前确认 agent 具备这些能力：

- 能读取同级或相邻 skill 目录下的 `state.md` 和 `../flomo-shared/user-style.md`
- 能访问 flomo MCP，或等价的 memo 搜索、相关笔记、创建、标签规范和用户 memory 工具
- 能在写入 flomo 前先让用户确认

## 目录结构

```text
skills/
  flomo-shared/
    user-style.md
  flomo-save-reminder/
    SKILL.md
    state.md
    user-style.md
    agents/openai.yaml
  flomo-daily-echo/
    SKILL.md
    state.md
    agents/openai.yaml
```

### flomo-shared/user-style.md

共享表达画像，只描述：

- 用户在 flomo 里怎么写
- 常用标签和格式习惯
- 什么内容值得保存
- 草稿应该多短、多克制、多像用户自己

不保存：

- `reminder_mode`
- `echo_mode`
- 自动保存策略
- 每日回顾频率
- 真实 memo 原文大段样本

### flomo-save-reminder/state.md

保存提醒自己的交互状态：

- `reminder_mode`
- `confirmation_policy`
- `auto_save_policy`

### flomo-daily-echo/state.md

昨日回声自己的交互状态：

- `echo_mode`
- `max_echoes`
- `silence_when_no_tension`
- `evidence_depth`
- `discussion_style`

## flomo-save-reminder

用于这些场景：

- 用户说“保存到 flomo”“记一下”“整理成 memo”
- 用户要求扫描当前对话，看是否有值得保存的内容
- 用户已授权本轮轻提醒

不用于这些场景：

- 普通事实问答
- 临时代码排障
- AI 单方面总结但用户没有确认
- 只有链接、摘录或信息转述，没有用户自己的反应

首次使用会先问用户希望保存提醒多主动，再生成或读取共享表达画像。默认只起草和提醒，写入前确认。

## flomo-daily-echo

V0.1 的目标不是“每日总结”，而是：

> 从目标日期 memo 与相关旧 memo 中找出一个真实存在的张力，生成一条用户可能愿意继续讨论的回声。

### 触发

只在用户明确要求时运行，例如：

- “看看昨天的 flomo”
- “昨日回声”
- “昨天能出来啥”
- “每日回顾”
- “dailyreview”

普通聊天里出现“回顾”“记录”“Echo”等词，不应该触发。

### 检索策略

V0.1 使用三路混合检索：

```text
相关笔记：50%
关键词 / 主题：30%
标签：20%
```

- 相关笔记是证据主干。
- 关键词 / 主题负责补足跨标签的长期母题。
- 标签负责领域约束和近期问题簇，不单独构成强证据。

最终按“关系质量”排序，而不是按来源排序。

### 输出状态

- **昨日回声**：有目标日期 memo，有旧 memo 证据，新旧记录之间能说明张力、延续、反差或分叉。
- **昨日观察**：有目标日期 memo，但旧 memo 证据不足或关系弱；不叫“回声”。
- **沉默 / 简短说明**：没有 memo、只有流水事实、或没有明显张力；不硬写日报。

### 能力露出

第一次成功输出后，可以用独立 Tips 轻露出持续能力：

```markdown
💡 Tips：
我也可以按这个方式看每天的记录：只有发现新旧笔记之间有明显张力时才开口，没东西就沉默。
```

这不是订阅邀约。真正的定时由外层 automation / scheduler 实现，skill 只定义被调用后的执行协议。

## 隐私边界

这个公开仓库只放通用协议和空模板：

- `SKILL.md`
- `state.md`
- `agents/openai.yaml`
- 空结构共享画像

不会放：

- 用户真实 memo
- 用户标签画像
- flomo 导出数据
- MCP 返回样本
- 个人化后的 `flomo-shared/user-style.md`

## 发布前检查

```bash
find skills -maxdepth 3 -type f -print | sort
rg -n "真实用户姓名|本地绝对路径|私人标签|memo 原文" skills README.md
git diff --cached
```
