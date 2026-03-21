# write-wechat-article

微信公众号文章写作技能，面向 AI Agent（Claude Code、OpenCode、OpenClaw）。

## 功能

- 📝 **AI 自动生成文章** - 根据主题快速生成高质量内容
- ✨ **文章润色优化** - 改善语言表达，提升可读性
- 🎨 **微信公众号排版** - 自动转换为微信兼容格式
- 🎯 **标题生成** - 生成吸引人的标题选项

## 使用方式

### 通过 AI Agent 使用

```
# 创作模式
"写一篇关于 AI Agent 的公众号文章"

# 修改模式
"优化这篇文章的语言表达"
"给这篇文章生成新标题"
```

### 输出文件

```
output/[article-slug]/
├── [article-slug].md              # Markdown 源文件
├── [article-slug]-wechat.html     # 可粘贴到微信公众号的 HTML
└── cover-[article-slug].html      # 封面图 HTML（浏览器打开截图）
```

## 微信公众号兼容性

本技能严格遵守微信公众号 HTML 规范：

- ✅ 使用 `<section>` 标签
- ✅ 使用 `<p>` 标签代替 `<ul>`/`<li>`
- ✅ 表格标题背景色应用到每个 `<th>` 标签
- ✅ 只使用基础 CSS 属性
- ❌ 禁止 `<style>` 标签
- ❌ 禁止 `<div>` 标签

主题色：`rgb(0, 112, 192)`

## 许可

MIT
