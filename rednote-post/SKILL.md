---
name: rednote-post
description: 小红书图文发布总调度 — 调用 rednote-writer 写文案，再调用 rednote-image 生成封面和配图，交付完整图文帖子
triggers:
  - "发小红书"
  - "生成小红书"
  - "rednote"
  - "xhs"
---

# RedNote Post

小红书图文发布总调度模块。先调用 `rednote-writer` 写文案，再调用 `rednote-image` 生成封面和配图，交付完整帖子。

---

## 工作流程

```
用户说"发小红书"/"生成小红书"
↓
① 调用 rednote-writer（写文案）
   • 提炼素材核心要点
   • 生成 3 个备选标题（每个 ≤ 20字）
   • 写出正文
   • 推荐 Hashtag
   ↓ 用户确认文字
② 调用 rednote-image（生成图片）
   • 生成封面图（封面 = 标题）
   • 生成内容页配图（可选）
   ↓ 用户确认图片
③ 交付完整帖子（图片 + 标题 + 正文 + Hashtag）
```

**重要：文案未经用户确认，不得生成图片。**

---

## Step 1 — 调用 rednote-writer

加载 `rednote-writer` skill，按其 SKILL.md 流程执行：
- 收集素材 / 理解主题
- 提炼 3 个核心要点
- 生成 3 个备选标题
- 写出正文
- 推荐 Hashtag

完成后，把标题 + 正文 + Hashtag 完整发给用户确认。

**确认通过后，进入 Step 2。**

---

## Step 2 — 调用 rednote-image

加载 `rednote-image` skill，按其 SKILL.md 流程执行：
- 用用户确认的标题生成封面 HTML → 渲染 PNG
- 用正文内容生成配图 HTML（可选，内容较长时分多页）→ 渲染 PNG
- 验证文件大小 > 10KB

封面图必须以用户最终选定的标题为准。

完成后，把封面图和配图发给用户确认。

---

## Step 3 — 交付

用户确认图片后，交付完整帖子：

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
