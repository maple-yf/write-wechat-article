# 微信公众号文章格式规范

> Agent + 开发者参考文档
> 包含 HTML 兼容性、CSS 属性、设计风格等所有格式规范

---

## 一、主题色规范

### 核心主题色
- **微信文章色**：`rgb(0, 112, 192)` 或 `#0070c0`
- **科技蓝主题（封面/插图）**：`#0EA5E9`、`#38BDF8`、`#0284C7`

应用场景：
- 文章标题背景色、二级标题颜色
- 表格标题背景色
- 结尾强调文字

---

## 二、HTML 兼容性规范（重要）

### 2.1 标签使用

**✅ 必须使用**
- `<section>` - 微信公众号最友好
- `<p>` - 段落
- `<strong>` - 加粗
- `<table>`, `<tr>`, `<td>`, `<th>` - 表格

**⚠️ 谨慎使用**
- `<div>` - 支持不稳定
- `<span>` - 支持有限

**❌ 禁止使用**
- `<ul>`, `<ol>`, `<li>` - 样式丢失
- `<style>` - 会被过滤

### 2.2 CSS 属性规范

**✅ 完全支持**
- color, background-color, font-size, font-weight
- text-align, margin, padding, border, line-height

**❌ 不支持**
- max-width, position, display, flex, transform, animation

### 2.3 列表实现

❌ 错误：
```html
<ul><li>项目 1</li></ul>
```

✅ 正确：
```html
<p style="margin: 5px 0;">• <strong>标题</strong>：内容描述。</p>
```

### 2.4 表格标题背景色（关键）

❌ 错误：
```html
<tr style="background-color: rgb(0, 112, 192);">
  <th>产品</th>
</tr>
```

✅ 正确：
```html
<tr>
  <th style="background-color: rgb(0, 112, 192); color: #ffffff;">产品</th>
</tr>
```
