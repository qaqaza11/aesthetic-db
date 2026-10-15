# 审美偏向数据库（aesthetic-db）

一个人的审美档案：让 AI 生成的内容（图片、视频、文档、文词、PPT……）更符合**你**的品味。

## 结构

```
profile.yaml                  全局：各领域默认卡 + 铁律
domains/<领域>/
  styles/<卡名>.yaml           风格卡：同一领域可有多张卡
  references/                 参考图（正反例，正文只存路径+哈希）
  feedback.jsonl              反馈账本：每条 verdict 带时间戳，可回溯、可撤销
_templates/                   新风格卡模板
```

## 任何 AI 怎么用它（5 步）

1. 读 `profile.yaml`：知道任务所属领域默认用哪张卡
2. 读 `domains/<领域>/styles/<卡>.yaml`：likes / dislikes / 约束 / 参考图
3. 把卡里的 `prompt_semantics`（语义级描述）**翻译**成你所用模型的 prompt 方言，不要硬抄原文
4. 只在**真实创作任务**里加载；普通聊天不注入
5. 铁律：内容准确性、可读性、任务要求永远优先于审美偏好；用户点名用哪张卡 > 默认卡；拿不准换卡时先问用户

## 反馈账本

`feedback.jsonl` 每行一条：`{"ts": "...", "domain": "...", "card": "...", "card_version": 1, "task": "...", "verdict": "up|down", "note": "..."}`。只追加不覆盖，旧偏好随时间自动淡化。

## 设计来源

- [tastetrainer](https://github.com/chweintr/tastetrainer)：个人审美数据集的记法（偏好分/标签/色板/构图）
- [PMG](https://github.com/tianyixu686/pmg-bench)：参考图→文字偏好→注入生成器（轻量路线，不训练模型）
- [Pick-a-Pic](https://stability.ai/research/pick-a-pic)：二选一采集偏好
- Codex 与 Claude Opus 的两轮评审（含互相辩论）：结论是轻量可用，但偏好条目必须带上下文（场景/强度/来源/时间），反馈必须可归因，审美会漂移

## 隐私

本仓库是公开的（主人明确选择 public）。内容只有审美偏好，不含账号、密码、个人身份信息。
