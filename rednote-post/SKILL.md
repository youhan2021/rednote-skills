---
name: rednote-post
description: 小红书图文发布总调度 — 调用 rednote-writer-title 写标题，rednote-cover 做封面，rednote-writer-body 写正文，rednote-pages  做内容页，交付完整图文帖子
triggers:
  - "发小红书"
  - "生成小红书"
  - "rednote"
  - "xhs"
---

# RedNote Post

小红书图文发布总调度模块。

---

## 调用子 skill 前的准备工作

在调用任何子 skill 之前，先读取本目录的 `references/account 属性.md`，将账号人设/调性作为 context 传递给子 skill。

子 skill 目录结构（rednote-skills 仓库）：
```
rednote-skills/
├── rednote-post/              ← 本调度模块
│   └── references/
│       └── account 属性.md    ← 账号属性（人设/调性/目标受众）
├── rednote-writer-title/      ← ① 写标题
├── rednote-cover/             ← ② 做封面
├── rednote-writer-body/       ← ③ 写正文
└── rednote-pages/             ← ④ 做内容页
```

---

## 工作流程

```
用户说"发小红书"/"生成小红书"
↓
① 调用 rednote-writer-title（写标题）
   • 读取 references/account 属性.md，作为 context 传入
   • 生成 3 个备选标题（每个 ≤ 20字）
   ↓ 用户选定标题（未满意 → 继续修改，直到满意为止）
② 调用 rednote-cover（做封面）
   • 用用户选定的标题生成封面 PNG
   ↓ 用户确认封面（未满意 → 重新生成，直到满意为止）
③ 调用 rednote-writer-body（写正文）
   • 读取 references/account 属性.md，作为 context 传入
   • 根据确认的标题定制正文
   • 推荐 Hashtag
   ↓ 用户确认正文（未满意 → 继续修改，直到满意为止）
④ 调用 rednote-pages（做内容页）
   • 用正文内容生成内容页 PNG（可选，内容较长时分多页）
   ↓ 用户确认内容页（未满意 → 重新生成，直到满意为止）
⑤ 交付完整帖子（封面 + 内容页 + 标题 + 正文 + Hashtag）
```

**重要规则：标题未经用户选定，不得做封面。正文未经用户确认，不得做内容页。**

---

## Step 1 — 调用 rednote-writer-title

加载 `rednote-writer-title` skill，先把 `references/account 属性.md` 的内容作为 context 传入，然后按其 SKILL.md 流程执行：
- 提炼 3 个核心要点
- 生成 3 个备选标题（每个 ≤ 20字）

**确认通过后，进入 Step 2。**

---

## Step 2 — 调用 rednote-cover

加载 `rednote-cover` skill，按其 SKILL.md 流程执行：
- 用用户选定的标题生成封面 HTML → 渲染 PNG
- 验证文件大小 > 10KB

封面图必须以用户最终选定的标题为准。

完成后，把封面图发给用户确认。

**确认通过后，进入 Step 3。**

---

## Step 3 — 调用 rednote-writer-body

加载 `rednote-writer-body` skill，先把 `references/account 属性.md` 的内容作为 context 传入，按其 SKILL.md 流程执行：
- 根据确认的标题定制正文
- 推荐 Hashtag

完成后，把标题 + 正文 + Hashtag 完整发给用户确认。

**确认通过后，进入 Step 4。**

---

## Step 4 — 调用 rednote-pages

加载 `rednote-pages` skill，按其 SKILL.md 流程执行：
- 用正文内容生成内容页 HTML → 渲染 PNG（内容较长时分多页）
- 验证文件大小 > 10KB

完成后，把内容页发给用户确认。

---

## Step 5 — 交付

用户确认内容页后，交付完整帖子：

```
**封面**
MEDIA:/path/to/Cover.png

**Page 1**
MEDIA:/path/to/Page1.png

**Page 2**（如有）
MEDIA:/path/to/Page2.png

标题（每个 ≤ 20字）：
1. [标题1]
2. [标题2]
3. [标题3]

正文：
[正文内容]

推荐 Hashtag：
#XXX #YYY #ZZZ ...
```

---

## 注意事项

- 文案未经用户确认，不得生成图片
- 图片未经用户确认，不得交付
- 图片发送：每张单独一行，不放同一个 code block
- 输出目录：`~/.hermes/research/imgs/`
