# rednote-skills

小红书图文发布 Skill 套件，包含三个互相调用的模块：

| Skill | 描述 | 触发词 |
|-------|------|--------|
| `rednote-post` | 总调度：调用 writer → image → 交付 | 发小红书 / 生成小红书 |
| `rednote-writer` | 文案写作：提炼要点、写标题、写正文、Hashtag | 写小红书文案 |
| `rednote-image` | 图片生成：封面图 + 内容页图（3:4 竖版） | 做封面 / 生成配图 |

## 安装

通过 ClawHub 安装 `rednote-post`，三个 skill 会一起安装。

## 架构

```
用户："发小红书"
  → rednote-post（总调度）
      → rednote-writer（Step 1：写文案，用户确认）
      → rednote-image（Step 2：生成封面+配图，用户确认）
      → 交付完整帖子
```

## Skill 开发

```bash
# 克隆
git clone https://github.com/youhan2021/rednote-skills.git
cd rednote-skills

# 本地修改后推送
git add . && git commit -m "描述" && git push
```
