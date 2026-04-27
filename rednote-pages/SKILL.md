---
name: rednote-pages
description: 小红书内容页生成 — 根据正文生成多张3:4竖版内容页PNG
triggers:
  - "做内容页"
  - "生成配图"
  - "小红书内容页"
---

# RedNote Pages

小红书内容页生成模块。根据正文生成内容页图（3:4 竖版，1080×1440px）。

**输入：** 正文文本（可能需要分多页）
**输出：** `~/.hermes/research/imgs/Page1.png`, `Page2.png`, ...

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
SKILL_DIR="$HOME/.hermes/skills/rednote-skills/rednote-pages"
OUT_DIR="$HOME/.hermes/research/imgs"
mkdir -p "$OUT_DIR"

# 内容页1
node "$SKILL_DIR/scripts/render_html_to_png.js" \
  "/tmp/content_page.html" \
  "$OUT_DIR/Page1.png" 1080 1440

# 内容页2（如有）
node "$SKILL_DIR/scripts/render_html_to_png.js" \
  "/tmp/content_page2.html" \
  "$OUT_DIR/Page2.png" 1080 1440
```

**必须验证文件：**
```bash
ls -lh $OUT_DIR/
```
- 检查文件大小 > 10KB（说明渲染成功）

---

## 交付格式

```
**Page 1**
MEDIA:/path/to/Page1.png

**Page 2**（如有）
MEDIA:/path/to/Page2.png
```

---

## 注意事项

- 内容页一个主题一大页，不要塞太多信息
- 渲染前检查 HTML 中变量是否都已替换完毕
- 输出目录统一用 `~/.hermes/research/imgs/`
