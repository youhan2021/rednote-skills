---
name: rednote-cover
description: 小红书封面图生成 — 根据标题生成3:4竖版封面PNG
triggers:
  - "做封面"
  - "生成封面"
  - "小红书封面"
---

# RedNote Cover

小红书封面图生成模块。根据标题生成封面图（3:4 竖版，1080×1440px）。

**输入：** 标题文本
**输出：** `~/.hermes/research/imgs/Cover.png`

---

## 封面设计规范（硬性规则，违反返工）

### 尺寸
- 竖版 **3:4**（1080 × 1440px）
- 渲染命令：`node scripts/render_html_to_png.js <input.html> <output.png> 1080 1440`

### 配色方案（已验证最佳）
- 背景：#E8F4FC（几乎白色的浅钴蓝）
- 正文：#1e1b4b（深色）
- 高亮：#f97316（橙色），无下划线
- 全文不超过 2 种颜色

### 字体（已验证可渲染）
```css
font-family: 'WenQuanYi Zen Hei', 'WenQuanYi Zen Hei Sharp', 'DejaVu Sans', sans-serif;
-webkit-font-smoothing: antialiased;
```

### 字间距（正值，拉开间距）
- 主标题：letter-spacing: 4px
- 副标题：letter-spacing: 3px

### 点击预告
- 标题下方，margin-top ≥ 54px
- 深色 #1e1b4b，FontWeight 900，≥ 64px
- 箭头用 → 而非 ↓

### 禁止在封面出现
- 任何数据列表
- 多个 pill 标签并排
- 功能点列表
- 第四行正文以上的任何文字
- 词断开：每个词完整在一行里
- 颜色过多：金色、红色、绿色等禁止
- 顶栏/底栏黑边色块（去掉）

### 封面设计原则
- 小红书封面权重 > 标题，99%流量差"死"在封面上
- 字越大越醒目，缩略图里能看清才行
- 文字越少越有力，不要堆砌
- 同一账号封面色系统一，形成识别度

---

## 渲染命令

```bash
SKILL_DIR="$HOME/.hermes/skills/rednote-skills/rednote-cover"
OUT_DIR="$HOME/.hermes/research/imgs"
mkdir -p "$OUT_DIR"

node "$SKILL_DIR/scripts/render_html_to_png.js" \
  "/tmp/cover.html" \
  "$OUT_DIR/Cover.png" 1080 1440
```

**必须验证文件：**
```bash
ls -lh $OUT_DIR/
```
- 检查文件大小 > 10KB（说明渲染成功）

---

## 交付格式

```
**封面**
MEDIA:/path/to/Cover.png
```

---

## 注意事项

- 封面只放标题 + 点击预告，居中，不放其他元素
- 渲染前检查 HTML 中变量是否都已替换完毕
- 输出目录统一用 `~/.hermes/research/imgs/`
