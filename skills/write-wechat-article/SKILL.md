---
name: write-wechat-article
description: Use when the user asks to "写一篇公众号文章", "创作公众号内容", "生成微信公众号文章", "优化这篇文章", "润色文章", "给文章起标题", or mentions WeChat public account (微信公众号), WeChat article formatting, WeChat-compatible HTML, article cover generation, or wants to create content for WeChat public account. Make sure to use this skill whenever the user mentions WeChat, 公众号, article writing, content creation, or needs help with article formatting or title generation.
---

# 微信公众号文章写作技能

## 概述

帮助你创作、优化、排版微信公众号文章，生成符合微信编辑器要求的 HTML 格式和封面图。

## 功能

1. **创作模式** - 从头开始生成完整文章
2. **修改模式** - 优化现有文章的某个方面

## 工作流程

### 创作模式

当用户说"写一篇关于XXX的公众号文章"时：

1. **确认需求** - 询问字数、风格、目标读者
2. **生成文章** - 使用 Agent 的能力生成 Markdown 内容
3. **生成标题** - 提供多个标题选项供用户选择
4. **润色优化** - 根据用户反馈进行润色
5. **排版美化** - 转换为微信公众号 HTML 格式
6. **生成封面图** - 生成封面图 HTML
7. **返回文件** - 返回 .md、-wechat.html、cover-.html 的路径

### 修改模式

当用户说"优化这篇文章"或"修改标题"时：

1. **解析意图** - 识别用户想修改什么（内容/标题/排版）
2. **读取文件** - 读取目标文件
3. **执行修改** - 使用 Agent 的能力进行修改
4. **保存文件** - 保存修改后的内容
5. **返回路径** - 返回更新后的文件路径

## 意图识别

| 用户输入 | 意图 | 操作 |
|---------|------|------|
| "写一篇关于AI的文章" | 创作 | 生成新文章 |
| "优化这篇文章" | 润色 | 优化现有文章 |
| "调整排版" | 格式化 | 转换为微信 HTML |
| "生成标题" | 标题 | 生成标题选项 |

## 输出位置

**默认**：`./output/[文章slug]/`

**可配置**：
- 调用参数：`--output ./my-output`
- 环境变量：`WECHAT_ARTICLE_OUTPUT_DIR`

## 输出产物

| 文件 | 说明 |
|-----|------|
| [slug].md | 文章源文件（Markdown） |
| [slug]-wechat.html | 可粘贴到微信公众号的 HTML |
| cover-[slug].html | 封面图 HTML（浏览器打开截图） |

## 微信公众号兼容性规范

**必须遵守的规则**：

1. ✅ 使用 `<section>` 标签
2. ✅ 使用 `<p>` 标签代替 `<ul>`/`<li>`
3. ✅ 表格标题背景色应用到每个 `<th>` 标签
4. ✅ 只使用基础 CSS 属性：color, background-color, font-size, font-weight, text-align, margin, padding, border
5. ❌ 禁止 `<style>` 标签
6. ❌ 禁止 `<div>` 标签

**主题色**：`rgb(0, 112, 192)` 或 `#0070c0`

**标题样式**：
- H1：居中 + 主题蓝背景 + 白色字体
- H2：主题蓝色 + 下边框
- H3：深灰色 + 加粗

## 插图规范

**插图数量**：
- 短文章（<1500字）：1-2张
- 中等文章（1500-3000字）：3-4张
- 长文章（>3000字）：4-5张（上限5张）

**插图位置**：
- 章节开头或结尾
- 重要观点之后
- 对比内容处
- 避免在段落中间插入

**插图类型**：
- comparison：对比图（问题 vs 方案）
- ability：核心能力图（功能/特性介绍）
- process：流程图（步骤/教程）

**重要**：插图直接嵌入到文章 HTML 中，不生成单独文件。
