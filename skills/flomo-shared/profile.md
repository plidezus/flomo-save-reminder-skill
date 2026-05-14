# flomo 共享画像

profile_status: empty
profile_kind: flomo_shared_profile
schema_version: 3
updated_at:
sample_window:
sample_count:

## 定义

这是 flomo 相关 skills 共用的 profile 文件。

它包含两个生命周期不同的部分：

- `Expression Profile`：从真实 flomo memo 样本生成。
- `User Preferences`：只能在用户明确确认后修改。

刷新 `Expression Profile` 不能顺手重写 `User Preferences`，除非用户明确要求。

## Expression Profile

### Save Tendencies

- 待生成。

### Format Habits

- 待生成。

### Tag Habits

- 待生成。

### Draft Style

- 待生成。

### Save Value Judgment

- 适合保存：待生成。
- 不适合保存：待生成。

## User Preferences

这些字段只能在用户明确确认后修改。不要从 memo 样本推断。

### Save Reminder

- reminder_mode:
- auto_save_scope:

### Daily Echo

- default_target_date: yesterday
- max_echo_count: 1
- silence_when_no_tension: true
- tips_shown:
- automation_hint_shown:

## 维护规则

- `profile_status: empty` 表示共享 profile 尚未生成。
- 从真实 memo 样本和用户确认保存过的草稿生成 `Expression Profile`。
- profile 要保持紧凑：保存模式，不保存原始 memo dump。
- 不保存大段 memo 原文。
- 不从 memo 样本推断自动化、主动性或写入权限。
- 生成后把 `profile_status` 改为 `generated`，并填写 `updated_at`、`sample_window` 和 `sample_count`。
- `updated_at` 超过 30 天视为过期；过期不阻塞当前任务，但应在合适时刷新。
