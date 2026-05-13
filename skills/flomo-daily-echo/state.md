# flomo Daily Echo 状态

state_status: configured
updated_at:

echo_mode: manual
cadence: on_request
max_echoes: 1
silence_when_no_tension: true
evidence_depth: normal
discussion_style: restrained_challenge
save_handoff: flomo-save-reminder

## 字段说明

- `echo_mode`
  - `manual`：只在用户明确要求时运行。
  - `scheduled`：已授权定时任务可主动运行。
  - `session_light`：仅当前会话授权低频运行。
- `cadence`
  - 默认 `on_request`，不自动每日打扰。
- `max_echoes`
  - 默认只输出 1 条主回声。
- `silence_when_no_tension`
  - 没有真实张力时，不硬写日报。
- `evidence_depth`
  - `light`：少量旧 memo 证据。
  - `normal`：为候选张力找 1-3 条旧 memo。
  - `deep`：更广泛查找长期母题，但输出仍保持短。
- `discussion_style`
  - `restrained_challenge`：克制、直接、有判断，但不替用户下最终结论。
- `save_handoff`
  - 如果用户要保存讨论结果，交给 `flomo-save-reminder`。

## 维护规则

- 这里只保存 daily echo 自己的互动偏好。
- 不保存用户标签、memo 格式、表达风格；这些属于 `../flomo-shared/user-style.md`。
- 不保存保存提醒策略；这些属于 `../flomo-save-reminder/state.md`。
