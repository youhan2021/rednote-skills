# rednote-skills

小红书图文发布 Skill 套件，包含四个模块：

| Skill | 描述 | 触发词 |
|-------|------|--------|
| `rednote-post` | 总调度：标题 → 正文 → 图片 → 交付 | 发小红书 / 生成小红书 |
| `rednote-writer-title` | 标题生成：3个备选，每个≤20字 | 写标题 / 小红书标题 |
| `rednote-writer-body` | 正文写作：开头+结尾+Hashtag | 写正文 / 小红书正文 |
| `rednote-image` | 封面图+内容页图（3:4竖版） | 做封面 / 生成配图 |

## 安装

通过 ClawHub 安装 `rednote-post`，四个 skill 会一起安装。

## 架构

```
用户："发小红书"
  → rednote-post（总调度）
      → rednote-writer-title（Step 1：生成3个备选标题，用户选定）
      → rednote-writer-body（Step 2：根据标题定制正文，用户确认）
      → rednote-image（Step 3：生成封面+配图，用户确认）
      → 交付完整帖子
```

## Skill 开发

```bash
git clone https://github.com/youhan2021/rednote-skills.git
cd rednote-skills

git add . && git commit -m "描述" && git push
```
