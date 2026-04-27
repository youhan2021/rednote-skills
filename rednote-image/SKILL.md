---
name: rednote-image
description: 小红书封面图和配图生成 — 读取纯文本输出（H titre + 正文 + Hashtag），生成 3:4 竖版 PNG 图片
triggers:
  - "做封面"
  - "生成配图"
  - "小红书图片"
---

# RedNote Image

小红书图片生成模块。根据标题 + 正文 + Hashtag 生成封面图和内容页图（3:4 竖版，1080×1440px）。

**输入：** 纯文本（标题 / 正文 / Hashtag），图片输出到 `~/.hermes/research/imgs/`

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
系统可用中文字体：文泉驿 WenQuanYi Zen Hei（首选）、文泉驿点阵 WenQuanYi Zen Hei Sharp。

### 字间距（正值，拉开间距）
- 主标题：letter-spacing: 4px
- 副标题：letter-spacing: 3px

### HTML 模板（封面标准结构）
```html
<!DOCTYPE html>
<html><head><meta charset="UTF-8"><style>
* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { width: 1080px; height: 1440px; font-family: 'WenQuanYi Zen Hei', 'WenQuanYi Zen Hei Sharp', 'DejaVu Sans', sans-serif; overflow: hidden; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }
body { background: #E8F4FC; }
.center { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; padding: 0 80px; }
.title-block { display: flex; flex-direction: column; align-items: center; gap: 8px; }
.line { display: block; font-weight: 900; line-height: 1.1; text-align: center; }
.l1 { font-size: 177px; color: #f97316; letter-spacing: 4px; }
.l2 { font-size: 109px; color: #1e1b4b; letter-spacing: 3px; }
.l2 .h { color: #f97316; }
.l3 { font-size: 95px; color: #1e1b4b; letter-spacing: 3px; }
.l3 .h { color: #f97316; }
.teaser { font-size: 64px; font-weight: 900; color: #1e1b4b; margin-top: 54px; letter-spacing: 2px; }
</style></head><body>
<div class="center">
  <div class="title-block">
    <div class="line l1">OpenClaw</div>
    <div class="line l2">已经<span class="h">过气</span>了？！</div>
    <div class="line l3"><span class="h">Hermes</span>崛起！</div>
  </div>
  <div class="teaser">点我看完整横评 →</div>
</div>
</body></html>
```

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

## 内容页排版规范

| 元素 | 规范 |
|------|------|
| 小标题 | `font-size:22px; font-weight:bold; color:#e55c00;` + 底部 `border-bottom: 2px solid #f5c89a; padding-bottom:6px; margin-bottom:12px;` |
| 正文 | `font-size:17px; line-height:1.9; margin:14px 0;` 段落间距宽松 |
| 列表 | `font-size:17px; font-weight:bold;` **全文不加粗**，关键词用 `strong style="color:#e55c00;font-weight:bold;"` 橙色高亮；不用 `ul/li` |
| 结尾 | 分隔线 `<hr style="border:none;border-top:2px solid #f5c89a;margin:24px 0 12px;">` + `<p style="font-size:12px;color:#999;">来源：XXX</p>` |
| 配图 | `width:640px; border-radius:10px; display:block;` + 图注 `<p style="font-size:13px;color:#666;text-align:center;margin-top:6px;">图注文字</p>` |

每张内容页最多包含 **2 个对比维度**，每个维度一张表格。

### 布局规范（内容页）
- 顶栏：30-40px高，紫色标签
- 主体：2 个 section 垂直排列，每个 section 含：
  - 维度标题（72px，FontWeight 900，高亮词橙色）
  - 白色圆角表格
- 两个 section 之间留 gap（24px）
- 底栏：数据来源，26px深灰色

### 表格规范
- 表头行：深色背景 #1e1b4b，白色字体，44px
- 数据行：白色背景，38px深色字体，2px 浅灰分隔线
- 第一列：灰色字体（维度名），32px
- 最优项：★ 五角星（橙色）标记

### 字体大小（内容页基准）
- 维度标题：72px
- 表头：44px
- 表格内容：38px
- 第一列：32px
- 底栏来源：26px

### 配色速查
| 用途 | 颜色 |
|------|------|
| 背景 | #E8F4FC |
| 深色文字 | #1e1b4b |
| 高亮/★ | #f97316 |
| 第一列标签 | #64748b |
| 表头背景 | #1e1b4b |
| 表格卡片背景 | #ffffff |
| 行分隔线 | #e2e8f0 |
| 紫色标签 | #7c3aed |

### 尺寸
- 3:4（1080 × 1440px），同封面

---

## 渲染命令

```bash
SKILL_DIR="$HOME/.hermes/skills/rednote-skills/rednote-image"
OUT_DIR="$HOME/.hermes/research/imgs"
mkdir -p "$OUT_DIR"

# 封面
node "$SKILL_DIR/scripts/render_html_to_png.js" \
  "/tmp/cover.html" \
  "$OUT_DIR/Cover.png" 1080 1440

# 内容页
node "$SKILL_DIR/scripts/render_html_to_png.js" \
  "/tmp/content_page.html" \
  "$OUT_DIR/Page1.png" 1080 1440
```

**必须验证文件：**
```bash
ls -lh $OUT_DIR/
```
- 检查文件大小 > 10KB（说明渲染成功）

---

## 交付格式

图片发送格式：**一个 message 里每张图单独一行**（不在同一个 code block 里），示例：

```
**封面**
MEDIA:/path/to/Cover.png

**Page 1**
MEDIA:/path/to/Page1.png

**Page 2**
MEDIA:/path/to/Page2.png
```

---

## 注意事项

- 封面只放标题 + 点击预告，居中，不放其他元素
- 内容页一个主题一大页，不要塞太多信息
- 渲染前检查 HTML 中变量是否都已替换完毕
- 输出目录统一用 `~/.hermes/research/imgs/`
