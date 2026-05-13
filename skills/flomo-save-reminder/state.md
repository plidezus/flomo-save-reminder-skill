# flomo 保存提醒状态

state_status: empty
updated_at:

reminder_mode: unset
confirmation_policy: confirm_before_write
auto_save_policy: off_by_default

## 字段说明

- `reminder_mode`
  - `unset`：首次使用时需要先询问用户希望如何触发保存提醒。
  - `explicit_only`：只在用户明确说保存、记一下、整理成 flomo 时使用。
  - `scan_on_request`：用户要求扫描当前对话时，集中判断哪些内容值得保存。
  - `session_light`：仅本轮或用户授权的会话中，允许低频轻提醒。
- `confirmation_policy`
  - 默认写入前确认。
- `auto_save_policy`
  - 默认不自动保存。自动保存必须来自用户对某类内容的明确授权。

## 维护规则

- 这里只保存 flomo-save-reminder 自己的交互状态。
- 不保存用户标签、memo 格式、表达风格；这些属于 `../flomo-shared/user-style.md`。
- 用户完成首次设置后，把 `state_status` 改为 `configured`，填写 `updated_at` 和 `reminder_mode`。
