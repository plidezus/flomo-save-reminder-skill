# flomo 用户风格画像

profile_status: empty
reminder_mode: unset
updated_at:
sample_window:
sample_count:

## 保存倾向

## 格式习惯

## 标签习惯

## 草稿风格

## 提醒偏好

## 维护规则

- `profile_status: empty` 表示尚未生成画像。
- `reminder_mode: unset` 表示首次使用时需要先询问用户希望如何触发保存提醒。
- 首次使用时先完成提醒模式选择，再根据 flomo memory 和 memo 样本生成画像。
- 生成后把 `profile_status` 改为 `generated`，并填写 `updated_at`、`sample_window` 和 `sample_count`。
- 只记录统计、偏好和风格，不保存大段原始 memo。
- `updated_at` 超过 30 天时视为过期；过期不阻塞保存，但应在合适时机刷新。若草稿连续不符合用户风格，也可提前刷新。
