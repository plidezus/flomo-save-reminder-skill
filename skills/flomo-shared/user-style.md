# flomo 用户表达画像

profile_status: empty
profile_kind: flomo_expression_profile
schema_version: 2
updated_at:
sample_window:
sample_count:

## 定义

这份文件是 flomo 相关 skills 共享的用户表达画像，只描述用户在 flomo 中的记录方式、标签习惯、内容选择和表达偏好。

它不是完整人格画像，也不保存某个 skill 的主动性、频率、自动保存策略或确认策略。skill 自己的交互偏好应放在各自的 `state.md` 中。

## 保存倾向

- 待生成。

## 格式习惯

- 待生成。

## 标签习惯

- 待生成。

## 草稿风格

- 待生成。

## 保存价值判断

- 适合保存：待生成。
- 不适合保存：待生成。

## 维护规则

- `profile_status: empty` 表示尚未生成共享表达画像。
- 首次需要按用户风格生成内容时，优先从真实 memo 样本和用户确认保存的内容中学习。
- 只记录统计、偏好和风格，不保存大段原始 memo。
- 不保存 `reminder_mode`、`echo_mode`、自动保存、提醒频率等 skill 状态。
- 生成后把 `profile_status` 改为 `generated`，并填写 `updated_at`、`sample_window` 和 `sample_count`。
- `updated_at` 超过 30 天时视为过期；过期不阻塞任务，但应在合适时机刷新。
