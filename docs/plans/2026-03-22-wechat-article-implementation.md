# 微信公众号文章写作技能实施计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans 来执行这个计划。

**目标：** 使用 skill-creator 技能创建一个微信公众号文章写作技能。

**方式：** 通过 skill-creator 技能来生成 skill.json 和 skill.md 文件。

**核心功能：** AI 自动生成文章、文章润色优化、微信公众号排版美化、标题生成。

**架构：** 单一技能，包含创作流程和修改流程两种工作模式。

---

## 前置准备

### Task 0: 了解 skill-creator 技能

**Step 1: 调用 skill-creator 技能**

使用命令调用 skill-creator 技能，了解其工作方式和参数要求。

\`\`\`bash
# 调用 skill-creator 技能
skill-creator --help
\`\`\`

**Step 2: 准备技能规范**

整理以下信息供 skill-creator 使用：

- **技能名称**：write-wechat-article
- **技能描述**：微信公众号文章写作助手，帮助用户创作、优化、排版文章
- **触发词**：写一篇公众号文章、创作公众号内容、生成微信公众号文章、优化这篇文章、给文章起标题
- **输入参数**：主题、字数、风格、目标文件、输出路径等
- **输出产物**：.md、-wechat.html、cover-.html

---

## Phase 1: 使用 skill-creator 创建主技能

### Task 1: 创建主技能框架

**Step 1: 使用 skill-creator 生成技能文件**

\`\`\`bash
# 调用 skill-creator 技能
skill-creator create write-wechat-article \
  --description "微信公众号文章写作技能" \
  --author "Maple" \
  --triggers "写一篇公众号文章,创作公众号内容,生成微信公众号文章,优化这篇文章,给文章起标题"
\`\`\`

**Step 2: 验证生成的文件**

检查生成的 skill.json 和 skill.md 文件：

\`\`\`bash
ls -la skills/write-wechat-article/
cat skills/write-wechat-article/skill.json
cat skills/write-wechat-article/skill.md
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/write-wechat-article/
git commit -m "feat: create main skill using skill-creator"
\`\`\`

---

### Task 2: 完善技能内容

**文件：**
- Modify: `skills/write-wechat-article/skill.md`

**Step 1: 在 skill.md 中添加详细说明**

\`\`\`markdown
# 微信公众号文章写作技能

你是一个微信公众号文章写作助手，帮助用户创作、优化、排版文章。

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

**默认**：\`./output/[文章slug]/\`

**可配置**：
- 调用参数：\`--output ./my-output\`
- 环境变量：\`WECHAT_ARTICLE_OUTPUT_DIR\`

## 输出产物

| 文件 | 说明 |
|-----|------|
| [slug].md | 文章源文件（Markdown） |
| [slug]-wechat.html | 可粘贴到微信公众号的 HTML |
| cover-[slug].html | 封面图 HTML（浏览器打开截图） |

## 微信公众号兼容性规范

**必须遵守的规则**：

1. ✅ 使用 \`<section>\` 标签
2. ✅ 使用 \`<p>\` 标签代替 \`<ul>\`/\`<li>\`
3. ✅ 表格标题背景色应用到每个 \`<th>\` 标签
4. ✅ 只使用基础 CSS 属性：color, background-color, font-size, font-weight, text-align, margin, padding, border
5. ❌ 禁止 \`<style>\` 标签
6. ❌ 禁止 \`<div>\` 标签

**主题色**：\`rgb(0, 112, 192)\` 或 \`#0070c0\`

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
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add skills/write-wechat-article/skill.md
git commit -m "docs: add detailed skill specification"
\`\`\`

---

## Phase 2: 创建文档和脚本

### Task 3: 创建 README

**文件：**
- Create: `README.md`

**Step 1: 创建 README.md**

\`\`\`markdown
# write-wechat-article

微信公众号文章写作技能，面向 AI Agent（Claude Code、OpenCode、OpenClaw）。

## 功能

- 📝 **AI 自动生成文章** - 根据主题快速生成高质量内容
- ✨ **文章润色优化** - 改善语言表达，提升可读性
- 🎨 **微信公众号排版** - 自动转换为微信兼容格式
- 🎯 **标题生成** - 生成吸引人的标题选项

## 使用方式

### 通过 AI Agent 使用

\`\`\`
# 创作模式
"写一篇关于 AI Agent 的公众号文章"

# 修改模式
"优化这篇文章的语言表达"
"给这篇文章生成新标题"
\`\`\`

### 输出文件

\`\`\`
output/[article-slug]/
├── [article-slug].md              # Markdown 源文件
├── [article-slug]-wechat.html     # 可粘贴到微信公众号的 HTML
└── cover-[article-slug].html      # 封面图 HTML（浏览器打开截图）
\`\`\`

## 微信公众号兼容性

本技能严格遵守微信公众号 HTML 规范：

- ✅ 使用 \`<section>\` 标签
- ✅ 使用 \`<p>\` 标签代替 \`<ul>\`/\`<li>\`
- ✅ 表格标题背景色应用到每个 \`<th>\` 标签
- ✅ 只使用基础 CSS 属性
- ❌ 禁止 \`<style>\` 标签
- ❌ 禁止 \`<div>\` 标签

主题色：\`rgb(0, 112, 192)\`

## 许可

MIT
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add README.md
git commit -m "docs: add README"
\`\`\`

---

### Task 4: 创建 .gitignore

**文件：**
- Create: `.gitignore`

**Step 1: 创建 .gitignore**

\`\`\`
node_modules/
output/
*.log
.DS_Store
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add .gitignore
git commit -m "chore: add gitignore"
\`\`\`

---

## Phase 3: 测试和验证

### Task 5: 创建测试用例

**文件：**
- Create: `tests/manual-test.md`

**Step 1: 创建测试文档**

\`\`\`markdown
# 手动测试用例

## 测试环境

- AI Agent: Claude Code / OpenCode / OpenClaw
- 技能: write-wechat-article

## 测试用例

### TC1: 创作模式

**输入**：
\`\`\`
写一篇关于 AI Agent 发展趋势的公众号文章，字数约 2000 字
\`\`\`

**预期输出**：
1. 询问确认需求（如字数、风格）
2. 生成文章内容
3. 生成标题选项
4. 提供润色机会
5. 转换为 HTML 格式
6. 生成封面图
7. 返回三个文件路径

**验证点**：
- [ ] 文章内容质量
- [ ] 标题选项数量（5个）
- [ ] HTML 兼容性（可粘贴到微信公众号）
- [ ] 封面图样式

### TC2: 修改模式 - 润色

**输入**：
\`\`\`
优化 output/article/article.md 的语言表达，使其更简洁
\`\`\`

**预期输出**：
1. 读取文章内容
2. 优化语言表达
3. 保存修改后的文件
4. 返回文件路径

**验证点**：
- [ ] 文章被修改
- [ ] 核心观点保持不变
- [ ] 表达更流畅

### TC3: 修改模式 - 标题

**输入**：
\`\`\`
给这篇文章生成新标题
\`\`\`

**预期输出**：
1. 读取文章内容
2. 生成 5 个标题选项
3. 提供选择机会

**验证点**：
- [ ] 标题数量（5个）
- [ ] 标题吸引力
- [ ] 标题长度（不超过 20 字）

### TC4: 兼容性测试

**步骤**：
1. 生成测试文章
2. 打开 -wechat.html 文件
3. 全选（Cmd+A）并复制
4. 粘贴到微信公众号编辑器
5. 检查样式是否保留

**验证点**：
- [ ] 标题颜色正确
- [ ] 背景色保留
- [ ] 表格边框显示
- [ ] 列表符号显示
- [ ] 手机预览正常
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add tests/manual-test.md
git commit -m "test: add manual test cases"
\`\`\`

---

### Task 6: 兼容性验证

**Step 1: 创建验证清单**

\`\`\`markdown
# 微信公众号兼容性验证清单

## HTML 标签检查

- [ ] 使用 \`<section>\` 标签
- [ ] 使用 \`<p>\` 标签代替 \`<ul>\`/\`<li>\`
- [ ] 没有使用 \`<style>\` 标签
- [ ] 没有使用 \`<div>\` 标签

## CSS 属性检查

- [ ] 只使用基础 CSS 属性
- [ ] 没有 \`position\`、\`display\`、\`flex\` 等属性
- [ ] 没有 \`transform\`、\`animation\` 等属性

## 样式检查

- [ ] H1: 居中 + 主题蓝背景 + 白色字体
- [ ] H2: 主题蓝色 + 下边框
- [ ] H3: 深灰色 + 加粗
- [ ] 主题色: \`rgb(0, 112, 192)\`

## 表格检查

- [ ] 表格标题背景色应用到每个 \`<th>\` 标签
- [ ] \`<tr>\` 上没有设置背景色

## 内容检查

- [ ] 列表使用 \`• **标题**：内容\` 格式
- [ ] 分隔线使用 \`—— · ——\`
- [ ] 使用中文标点符号
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add tests/compatibility-checklist.md
git commit -m "test: add compatibility checklist"
\`\`\`

---

## Phase 4: 最终验证

### Task 7: 完整流程测试

**Step 1: 端到端测试**

执行完整的创作流程：

1. 调用技能生成文章
2. 验证所有输出文件
3. 测试 HTML 兼容性
4. 验证封面图

**Step 2: 提交**

\`\`\`bash
git add .
git commit -m "chore: complete initial implementation using skill-creator

- 使用 skill-creator 技能创建主技能
- 添加详细的技能规范
- 创建 README 和测试用例
- 添加兼容性验证清单

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
\`\`\`

---

## 完成

技能创建完成！下一步：

1. **安装技能** - 将技能安装到 Agent 平台
2. **测试技能** - 运行测试用例
3. **收集反馈** - 根据使用反馈优化
