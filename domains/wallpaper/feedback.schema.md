# feedback.jsonl 事件规范

每行一个 JSON 事件。字段：`ts / domain / kind / card / card_version / task / verdict(up|down) / note`。
`correction` 类事件另带 `ref_ts`（指向被更正行的 ts+card）。

## 事件类别（kind）

- `preference_seed`：偏好声明/初始化。冷启动基线、用户原话录入。不计入满意度统计。
- `reference_intake`：参考图入库确认。用户确认某张图入库、代表其品味。不计入满意度统计。
- `reference_review`：参考图评价。用户对已入库参考图的 👍/👎，用于卡片调优。不计入满意度统计。
- `generation_review`：生成结果评价。用户对某次实际生成图的 👍/👎。**唯一计入满意度统计的类别。**
- `correction`：更正记录。对某条历史记录的纠正，必须追加新行并引用原行，禁止覆盖原文。

## 铁律

- 满意度统计只读 `kind=generation_review` 的行；seed/intake/review 类事件混入会产生虚假基线。
- 纠错只追加 `correction`，不改历史行；历史行是证据，不是草稿。
