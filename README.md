# write-wechat-article

微信公众号文章写作技能，帮助创作、优化、排版符合微信公众号规范的文章。

## 特性

- ✅ 完整的创作流程（调研、创作、配图、检验、发布）
- ✅ 微信公众号 HTML 兼容性规范
- ✅ 封面图和插图设计规范
- ✅ HTML 转换工具
- ✅ 质量检验清单

## 快速开始

### 1. 安装依赖

```bash
cd scripts && npm install
```

### 2. 使用技能

在 Claude Code 中触发技能：

```
"写一篇关于 AI 的公众号文章"
"优化这篇文章"
"生成文章标题"
```

## 项目结构

```
write-wechat-article/
├── SKILL.md                      # 技能定义 + 快速参考
├── specs/
│   ├── FORMAT_SPEC.md            # 格式规范（HTML/CSS/设计）
│   └── PROCESS_GUIDE.md          # 流程指南（调研/创作/发布）
├── scripts/
│   ├── convert_html_to_wechat.js # HTML 转换工具
│   ├── package.json
│   └── README.md
├── output/                       # 文章输出目录
├── tests/
│   ├── manual-test.md
│   └── compatibility-checklist.md
└── README.md                     # 本文件
```

## 文档

| 文档 | 说明 |
|-----|------|
| [SKILL.md](SKILL.md) | 技能定义、触发条件、核心流程 |
| [FORMAT_SPEC.md](specs/FORMAT_SPEC.md) | HTML 兼容性、CSS 属性、设计风格 |
| [PROCESS_GUIDE.md](specs/PROCESS_GUIDE.md) | 调研、创作、检验、发布流程 |
| [scripts/README.md](scripts/README.md) | 转换工具使用说明 |

## 输出产物

| 文件 | 说明 |
|-----|------|
| [slug].md | Markdown 源文件 |
| [slug]-wechat.html | 微信公众号 HTML |
| cover-[slug].html | 封面图 HTML |

## 使用工具

### HTML 转换

```bash
cd scripts
node convert_html_to_wechat.js <input.html> [output.html]
```

### 质量检验

参考 [tests/compatibility-checklist.md](tests/compatibility-checklist.md)

## 兼容性规范

**关键规则：**
- 使用 `<section>` 而非 `<div>`
- 使用 `<p>` 而非 `<ul>`/`<li>`
- 表格背景色应用到 `<th>` 标签
- 禁止 `<style>` 标签

详见 [FORMAT_SPEC.md](specs/FORMAT_SPEC.md)

## 设计风格

- **主题色**：`rgb(0, 112, 192)`（微信文章）、`#0EA5E9`（封面/插图）
- **设计语言**：现代科技风、卡片式布局、玻璃拟态

详见 [FORMAT_SPEC.md#设计风格规范](specs/FORMAT_SPEC.md#四设计风格规范)

## 参考资料

本技能整合了以下来源的规范和最佳实践：
- WECHAT_TEMPLATE.md
- WECHAT_COMPATIBILITY_NOTES.md
- MEMORY.md
- SOUL.md

## 许可

MIT
