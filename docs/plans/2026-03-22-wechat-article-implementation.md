# 微信公众号文章写作技能实施计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**目标：** 为 AI Agent（Claude Code、OpenCode、OpenClaw）开发一个模块化的微信公众号文章写作技能，包含生成、润色、排版、标题四个子技能。

**架构：** 模块化多技能架构 - 主技能负责意图解析和任务分发，四个子技能各司其职。内容由 Agent 处理，技能负责组织和格式化。严格遵守微信公众号 HTML 兼容性规范。

**技术栈：** JavaScript / Node.js，AI 模型由 Agent 平台管理（国产大模型），测试框架 Vitest

---

## 前置准备

### Task 0: 初始化项目结构

**文件：**
- Create: `package.json`
- Create: `README.md`
- Create: `.gitignore`
- Create: `shared/prompts.js`
- Create: `shared/format-utils.js`
- Create: `scripts/install.js`
- Create: `scripts/build.js`
- Create: `scripts/test.js`

---

## Phase 1: 基础设施

### Task 1: 创建 package.json

**文件：**
- Create: `package.json`

**Step 1: 创建 package.json**

```json
{
  "name": "write-wechat-article",
  "version": "1.0.0",
  "description": "微信公众号文章写作技能",
  "main": "index.js",
  "scripts": {
    "test": "vitest",
    "build": "node scripts/build.js",
    "install": "node scripts/install.js"
  },
  "keywords": ["wechat", "article", "ai", "agent"],
  "author": "Maple",
  "license": "MIT",
  "devDependencies": {
    "vitest": "^2.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

**Step 2: 创建 README.md**

```markdown
# write-wechat-article

微信公众号文章写作技能，面向 AI Agent（Claude Code、OpenCode、OpenClaw）。

## 功能

- AI 自动生成文章
- 文章润色优化
- 微信公众号排版美化
- 标题生成

## 安装

\`\`\`bash
npm install
\`\`\`

## 使用

通过 AI Agent 调用本技能。

## 输出

- `.md` - 文章源文件
- `-wechat.html` - 可粘贴到微信公众号编辑器的 HTML
- `cover-.html` - 封面图 HTML

## 许可

MIT
```

**Step 3: 创建 .gitignore**

\`\`\`
node_modules/
output/
*.log
.DS_Store
\`\`\`

**Step 4: 提交**

\`\`\`bash
git add package.json README.md .gitignore
git commit -m "chore: initialize project structure"
\`\`\`

---

### Task 2: 创建共享模块框架

**文件：**
- Create: `shared/prompts.js`
- Create: `shared/format-utils.js`

**Step 1: 创建 prompts.js**

```javascript
/**
 * Prompt 模板
 * 参考 WECHAT_TEMPLATE.md 规范
 */

// 文章主题色
const THEME_COLOR = 'rgb(0, 112, 192)';
const THEME_COLOR_HEX = '#0070c0';

// 文章结构模板
const ARTICLE_STRUCTURE_TEMPLATE = `
# 主标题 🦞

Tech and Talk，关注 AI、科技与生活。 | 约 XXXX 字

[引言段落]

—— · ——

## 第一个主要章节

[章节内容]

—— · ——

## 总结

[总结段落]
`;

// Prompt 生成器
function generatePrompt(type, params) {
  const { topic, wordCount, style, figureCount } = params;

  switch(type) {
    case 'generate':
      return generateArticlePrompt(topic, wordCount, style, figureCount);
    case 'polish':
      return polishPrompt(params.content, params.direction);
    case 'title':
      return titlePrompt(params.content, params.count);
    case 'format':
      return formatPrompt(params.content);
    default:
      throw new Error(`Unknown prompt type: ${type}`);
  }
}

function generateArticlePrompt(topic, wordCount, style, figureCount) {
  return `
请根据以下要求创作一篇微信公众号文章：

**主题：** ${topic}
**字数：** 约 ${wordCount} 字
**风格：** ${style}
**插图数量：** ${figureCount} 张（根据文章长度自动决定：短文1-2张，中篇3-4张，长篇4-5张，上限5张）

**文章结构：**
${ARTICLE_STRUCTURE_TEMPLATE}

**格式要求：**
1. 使用中文标点符号
2. 段落之间空一行
3. 列表使用 \`• **标题**：内容\` 格式
4. 在合适位置插入插图标记：\`<!-- FIGURE:类型 -->\`（类型：comparison/ability/process）

**插图位置规则：**
- 章节开头或结尾
- 重要观点之后
- 对比内容处
- 避免在段落中间插入

**插图类型说明：**
- comparison：对比图（问题 vs 方案）
- ability：核心能力图（功能/特性介绍）
- process：流程图（步骤/教程）

请开始创作，确保内容有深度、有观点、有价值。
  `.trim();
}

function polishPrompt(content, direction) {
  return `
请优化以下文章内容：

**优化方向：** ${direction}

**原文：**
${content}

**优化要求：**
1. 保持原文核心观点和逻辑
2. 改善语言表达，使其更流畅
3. 增强文章的可读性和吸引力
4. 保持中文标点符号
5. 返回完整的优化后文章

请开始优化：
  `.trim();
}

function titlePrompt(content, count = 5) {
  return `
请根据以下文章内容，生成 ${count} 个吸引人的标题：

**文章内容：**
${content.substring(0, 500)}...

**标题要求：**
1. 简洁有力，概括核心主题
2. 有吸引力，能引发点击欲望
3. 符合微信公众号标题风格
4. 每个标题不超过 20 字
5. 按吸引力从高到低排序

请生成 ${count} 个标题：
  `.trim();
}

function formatPrompt(content) {
  return `
请将以下 Markdown 内容转换为微信公众号兼容的 HTML 格式。

**转换要求（重要）：**
1. 使用 \`<section>\` 标签而不是 \`<div>\`
2. 使用 \`<p>\` 标签代替 \`<ul>\` 和 \`<li>\`
3. 表格标题背景色必须应用到每个 \`<th>\` 标签，不能在 \`<tr>\` 上设置
4. 只使用基础 CSS 属性：color, background-color, font-size, font-weight, text-align, margin, padding, border
5. 主题色：${THEME_COLOR}
6. 所有样式使用内联方式，不使用 \`<style>\` 标签

**标题样式：**
- H1：居中 + 主题蓝背景 + 白色字体
- H2：主题蓝色 + 下边框
- H3：深灰色 + 加粗

**列表格式：**
\`<p style="margin: 5px 0;">• <strong>标题</strong>：内容</p>\`

**分隔线：**
\`<section style="text-align: center; color: #999; margin: 30px 10px;">—— · ——</section>\`

**原文：**
${content}

请开始转换，确保兼容微信公众号编辑器。
  `.trim();
}

module.exports = {
  THEME_COLOR,
  THEME_COLOR_HEX,
  ARTICLE_STRUCTURE_TEMPLATE,
  generatePrompt
};
```

**Step 2: 创建 format-utils.js**

```javascript
/**
 * 微信公众号格式工具函数
 * 参考 WECHAT_TEMPLATE.md 和 WECHAT_COMPATIBILITY_NOTES.md
 */

const { THEME_COLOR, THEME_COLOR_HEX } = require('./prompts');

/**
 * 将 Markdown 转换为微信公众号兼容的 HTML
 * @param {string} markdown - Markdown 内容
 * @param {Object} options - 转换选项
 * @returns {string} HTML 字符串
 */
function markdownToWechatHTML(markdown, options = {}) {
  const {
    title = '文章标题',
    wordCount = 'XXXX',
    slug = 'article'
  } = options;

  // 解析 Markdown 并转换为 HTML
  let html = markdown;

  // 1. 处理一级标题 (H1)
  html = html.replace(/^#\s+(.+)$/gm, (match, h1Text) => {
    return `<section style="text-align: center; background-color: ${THEME_COLOR}; color: #ffffff; padding: 15px 20px; margin: 20px 10px; font-size: 22px; font-weight: bold;">
  ${h1Text}
</section>`;
  });

  // 2. 处理二级标题 (H2)
  html = html.replace(/^##\s+(.+)$/gm, (match, h2Text) => {
    return `<h2 style="font-size: 18px; color: ${THEME_COLOR}; border-bottom: 2px solid ${THEME_COLOR}; padding-bottom: 8px; margin: 30px 0 15px 0; font-weight: bold;">${h2Text}</h2>`;
  });

  // 3. 处理三级标题 (H3)
  html = html.replace(/^###\s+(.+)$/gm, (match, h3Text) => {
    return `<p style="margin: 15px 0 5px 0; font-weight: bold; color: #333;">${h3Text}</p>`;
  });

  // 4. 处理列表项
  html = html.replace(/^•\s+\*\*(.+?)\*\*：(.+)$/gm, (match, title, content) => {
    return `<p style="margin: 5px 0;">• <strong>${title}</strong>：${content}</p>`;
  });

  // 5. 处理普通列表项
  html = html.replace(/^•\s+(.+)$/gm, (match, content) => {
    return `<p style="margin: 5px 0;">• ${content}</p>`;
  });

  // 6. 处理分隔线
  html = html.replace(/^—— · ——$/gm, () => {
    return `<section style="text-align: center; color: #999; margin: 30px 10px; font-size: 14px;">—— · ——</section>`;
  });

  // 7. 处理加粗
  html = html.replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>');

  // 8. 处理段落（非标签行）
  const lines = html.split('\n');
  const processedLines = lines.map(line => {
    // 跳过已经是 HTML 的行
    if (line.trim().startsWith('<')) return line;
    // 跳过空行和标题行
    if (!line.trim() || line.startsWith('#')) return line;
    // 包装段落
    return `<p style="margin: 15px 0;">${line}</p>`;
  });
  html = processedLines.join('\n');

  // 9. 包装为完整的 HTML 文档
  html = wrapInHTMLDocument(html, title, wordCount);

  return html;
}

/**
 * 包装为完整的 HTML 文档
 */
function wrapInHTMLDocument(content, title, wordCount) {
  return `<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<body>

<section style="margin: 0; padding: 0; font-size: 16px; color: #333; line-height: 1.8; font-family: -apple-system-font, BlinkMacSystemFont, 'Helvetica Neue', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei UI', 'Microsoft YaHei', Arial, sans-serif;">

${content}

</section>

</body>
</html>`;
}

/**
 * 生成封面图 HTML
 * @param {Object} params - 封面图参数
 * @returns {string} HTML 字符串
 */
function generateCoverHTML(params) {
  const {
    title = '文章标题',
    subtitle = '副标题',
    tags = ['AI', '科技'],
    accentColor = '#0EA5E9'
  } = params;

  return `<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  body {
    margin: 0;
    padding: 0;
    display: flex;
    gap: 20px;
    background: #f0f0f0;
  }
  .cover-container {
    display: flex;
    gap: 20px;
  }
  .cover-left {
    width: 1175px;
    height: 500px;
    background: linear-gradient(135deg, ${accentColor} 0%, #0369A1 100%);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 40px;
    box-sizing: border-box;
  }
  .cover-right {
    width: 500px;
    height: 500px;
    background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 40px;
    box-sizing: border-box;
  }
  .title {
    font-size: 68px;
    font-weight: 900;
    color: #ffffff;
    text-align: center;
    margin-bottom: 20px;
  }
  .subtitle {
    font-size: 28px;
    font-weight: 500;
    color: rgba(255,255,255,0.9);
    text-align: center;
  }
  .tags {
    display: flex;
    gap: 15px;
    margin-top: 30px;
  }
  .tag {
    padding: 10px 20px;
    background: rgba(255,255,255,0.2);
    border-radius: 20px;
    color: #ffffff;
    font-size: 16px;
    font-weight: 600;
  }
  .logo {
    font-size: 48px;
    font-weight: 900;
    color: ${accentColor};
    margin-bottom: 20px;
  }
  .tag-right {
    padding: 8px 16px;
    background: rgba(14, 165, 233, 0.2);
    border: 1px solid ${accentColor};
    border-radius: 20px;
    color: ${accentColor};
    font-size: 14px;
    font-weight: 600;
  }
</style>
</head>
<body>
<div class="cover-container">
  <div class="cover-left">
    <div class="title">${title}</div>
    <div class="subtitle">${subtitle}</div>
    <div class="tags">
      ${tags.map(tag => `<div class="tag">${tag}</div>`).join('')}
    </div>
  </div>
  <div class="cover-right">
    <div class="logo">🦞</div>
    <div class="tags">
      ${tags.map(tag => `<div class="tag-right">${tag}</div>`).join('')}
    </div>
  </div>
</div>
</body>
</html>`;
}

/**
 * 生成对比图 HTML
 * @param {Object} params - 对比图参数
 * @returns {string} HTML 字符串
 */
function generateComparisonFigure(params) {
  const {
    leftTitle = '传统方式',
    leftItems = ['痛点1', '痛点2', '痛点3'],
    rightTitle = '新方案',
    rightItems = ['优势1', '优势2', '优势3']
  } = params;

  return `<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  body {
    margin: 0;
    padding: 20px;
    background: #f5f5f5;
    font-family: -apple-system-font, BlinkMacSystemFont, 'Helvetica Neue', 'PingFang SC', sans-serif;
  }
  .comparison-container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    max-width: 1200px;
    margin: 0 auto;
  }
  .side {
    flex: 1;
    padding: 40px;
    border-radius: 16px;
  }
  .side-left {
    background: linear-gradient(135deg, #FEE2E2 0%, #FECACA 100%);
  }
  .side-right {
    background: linear-gradient(135deg, #DBEAFE 0%, #BFDBFE 100%);
  }
  .side-title {
    font-size: 24px;
    font-weight: 700;
    text-align: center;
    padding: 15px;
    border-radius: 12px;
    margin-bottom: 20px;
  }
  .side-left .side-title {
    background: #EF4444;
    color: #ffffff;
  }
  .side-right .side-title {
    background: #0EA5E9;
    color: #ffffff;
  }
  .item {
    background: #ffffff;
    padding: 15px 20px;
    margin: 10px 0;
    border-radius: 8px;
    font-size: 16px;
  }
  .vs-badge {
    width: 80px;
    height: 80px;
    background: linear-gradient(135deg, #F59E0B 0%, #EF4444 100%);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 28px;
    font-weight: 900;
    color: #ffffff;
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  }
</style>
</head>
<body>
<div class="comparison-container">
  <div class="side side-left">
    <div class="side-title">${leftTitle}</div>
    ${leftItems.map(item => `<div class="item">${item}</div>`).join('')}
  </div>
  <div class="vs-badge">VS</div>
  <div class="side side-right">
    <div class="side-title">${rightTitle}</div>
    ${rightItems.map(item => `<div class="item">${item}</div>`).join('')}
  </div>
</div>
</body>
</html>`;
}

/**
 * 生成核心能力图 HTML
 * @param {Object} params - 核心能力图参数
 * @returns {string} HTML 字符串
 */
function generateAbilityFigure(params) {
  const {
    abilities = [
      { name: '能力1', desc: '描述1', icon: '🚀' },
      { name: '能力2', desc: '描述2', icon: '⚡' },
      { name: '能力3', desc: '描述3', icon: '🎯' }
    ]
  } = params;

  return `<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  body {
    margin: 0;
    padding: 40px;
    background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%);
    font-family: -apple-system-font, BlinkMacSystemFont, 'Helvetica Neue', 'PingFang SC', sans-serif;
  }
  .abilities-container {
    display: flex;
    gap: 30px;
    justify-content: center;
    max-width: 1200px;
    margin: 0 auto;
  }
  .ability-card {
    flex: 1;
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 16px;
    padding: 40px 30px;
    text-align: center;
  }
  .ability-icon {
    font-size: 64px;
    margin-bottom: 20px;
  }
  .ability-name {
    font-size: 24px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 15px;
  }
  .ability-desc {
    font-size: 16px;
    color: #94A3B8;
    line-height: 1.6;
  }
</style>
</head>
<body>
<div class="abilities-container">
  ${abilities.map((ability, index) => {
    const colors = ['#0EA5E9', '#22C55E', '#F59E0B'];
    const color = colors[index % colors.length];
    return `
  <div class="ability-card" style="border-top: 4px solid ${color};">
    <div class="ability-icon">${ability.icon}</div>
    <div class="ability-name">${ability.name}</div>
    <div class="ability-desc">${ability.desc}</div>
  </div>`;
  }).join('')}
</div>
</body>
</html>`;
}

/**
 * 生成流程图 HTML
 * @param {Object} params - 流程图参数
 * @returns {string} HTML 字符串
 */
function generateProcessFigure(params) {
  const {
    steps = [
      { num: 1, title: '步骤1', desc: '描述1' },
      { num: 2, title: '步骤2', desc: '描述2' },
      { num: 3, title: '步骤3', desc: '描述3' }
    ],
    code = 'command --option'
  } = params;

  return `<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  body {
    margin: 0;
    padding: 40px;
    background: #f5f5f5;
    font-family: -apple-system-font, BlinkMacSystemFont, 'Helvetica Neue', 'PingFang SC', sans-serif;
  }
  .process-container {
    max-width: 1000px;
    margin: 0 auto;
  }
  .steps {
    display: flex;
    gap: 30px;
    margin-bottom: 40px;
  }
  .step {
    flex: 1;
    text-align: center;
  }
  .step-num {
    width: 50px;
    height: 50px;
    background: #0EA5E9;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    font-weight: 700;
    color: #ffffff;
    margin: 0 auto 15px;
  }
  .step-title {
    font-size: 18px;
    font-weight: 700;
    color: #1E293B;
    margin-bottom: 10px;
  }
  .step-desc {
    font-size: 14px;
    color: #64748B;
  }
  .code-block {
    background: #0F172A;
    padding: 20px;
    border-radius: 12px;
    overflow-x: auto;
  }
  .code {
    color: #38BDF8;
    font-family: 'Courier New', monospace;
    font-size: 16px;
    margin: 0;
  }
</style>
</head>
<body>
<div class="process-container">
  <div class="steps">
    ${steps.map(step => `
    <div class="step">
      <div class="step-num">${step.num}</div>
      <div class="step-title">${step.title}</div>
      <div class="step-desc">${step.desc}</div>
    </div>`).join('')}
  </div>
  <div class="code-block">
    <code class="code">${code}</code>
  </div>
</div>
</body>
</html>`;
}

/**
 * 验证 HTML 兼容性
 * @param {string} html - HTML 内容
 * @returns {Object} 验证结果
 */
function validateWechatCompatibility(html) {
  const issues = [];

  // 检查禁止使用的标签
  if (html.includes('<style>')) {
    issues.push('包含 <style> 标签（会被过滤）');
  }
  if (html.match(/<div[^>]*>/gi)) {
    issues.push('包含 <div> 标签（支持不稳定）');
  }
  if (html.match(/<ul[^>]*>/gi)) {
    issues.push('包含 <ul> 标签（样式易丢失）');
  }

  // 检查表格标题背景色
  const trWithBg = html.match(/<tr[^>]*style="[^"]*background-color:[^"]*"/gi);
  if (trWithBg) {
    issues.push('在 <tr> 标签上设置背景色（会被过滤），请将背景色应用到每个 <th> 标签');
  }

  // 检查不支持的 CSS 属性
  const unsupportedProps = ['position:', 'display:', 'flex', 'transform:', 'animation:'];
  unsupportedProps.forEach(prop => {
    if (html.includes(prop)) {
      issues.push(`包含不支持的 CSS 属性: ${prop}`);
    }
  });

  return {
    valid: issues.length === 0,
    issues
  };
}

/**
 * 估算插图数量
 * @param {number} wordCount - 文章字数
 * @returns {number} 插图数量
 */
function estimateFigureCount(wordCount) {
  if (wordCount < 1500) return 1;
  if (wordCount < 3000) return 3;
  return Math.min(5, Math.floor(wordCount / 800));
}

module.exports = {
  markdownToWechatHTML,
  generateCoverHTML,
  generateComparisonFigure,
  generateAbilityFigure,
  generateProcessFigure,
  validateWechatCompatibility,
  estimateFigureCount
};
```

**Step 3: 运行测试验证基础框架**

\`\`\`bash
# 安装依赖
npm install

# 验证模块可以加载
node -e "console.log(require('./shared/prompts').THEME_COLOR)"
node -e "console.log(require('./shared/format-utils').estimateFigureCount(2000))"
\`\`\`

预期输出：
\`\`\`
rgb(0, 112, 192)
3
\`\`\`

**Step 4: 提交**

\`\`\`bash
git add shared/prompts.js shared/format-utils.js
git commit -m "feat: add shared modules for prompts and format utilities"
\`\`\`

---

## Phase 2: 主技能

### Task 3: 创建主技能框架

**文件：**
- Create: `skills/write-wechat-article/skill.json`
- Create: `skills/write-wechat-article/skill.md`

**Step 1: 创建 skill.json**

```json
{
  "name": "write-wechat-article",
  "version": "1.0.0",
  "description": "微信公众号文章写作技能 - 主技能，负责意图解析和任务分发",
  "author": "Maple",
  "triggers": [
    "写一篇公众号文章",
    "创作公众号内容",
    "生成微信公众号文章",
    "优化这篇文章",
    "给文章起标题"
  ],
  "subSkills": [
    "wechat-generate",
    "wechat-polish",
    "wechat-format",
    "wechat-title"
  ],
  "config": {
    "defaultOutputDir": "./output",
    "themeColor": "rgb(0, 112, 192)"
  }
}
```

**Step 2: 创建 skill.md**

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
2. **调用 wechat-generate** - 生成文章内容
3. **调用 wechat-title** - 生成标题选项
4. **调用 wechat-polish** - 润色优化
5. **调用 wechat-format** - 生成 HTML 和封面图
6. **返回文件路径** - .md、-wechat.html、cover-.html

### 修改模式

当用户说"优化这篇文章"或"修改标题"时：

1. **解析意图** - 识别用户想修改什么
2. **提取参数** - 目标文件、修改要求
3. **调用对应子技能** - 单步执行
4. **返回更新文件路径**

## 意图识别

| 用户输入 | 意图 | 子技能 |
|---------|------|--------|
| "写一篇关于AI的文章" | 创作 | wechat-generate |
| "优化这篇文章" | 润色 | wechat-polish |
| "调整排版" | 格式化 | wechat-format |
| "生成标题" | 标题 | wechat-title |

## 输出位置

默认：\`./output/[文章slug]/\`

用户可以通过参数指定：
- \`--output ./my-output\`
- 环境变量 \`WECHAT_ARTICLE_OUTPUT_DIR\`
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/write-wechat-article/
git commit -m "feat: add main skill framework"
\`\`\`

---

### Task 4: 实现意图解析逻辑

**文件：**
- Modify: `skills/write-wechat-article/skill.md`

**Step 1: 在 skill.md 中添加意图解析逻辑**

在 skill.md 末尾添加：

\`\`\`markdown
## 意图解析算法

```javascript
function parseIntent(userInput) {
  const input = userInput.toLowerCase();

  // 创作意图关键词
  const createKeywords = ['写', '创作', '生成', '新建', '写一篇'];
  const isCreate = createKeywords.some(kw => input.includes(kw));

  // 修改意图关键词
  const polishKeywords = ['优化', '润色', '改进', '提升'];
  const isPolish = polishKeywords.some(kw => input.includes(kw));

  // 格式化意图关键词
  const formatKeywords = ['排版', '格式', '美化', '调整格式'];
  const isFormat = formatKeywords.some(kw => input.includes(kw));

  // 标题意图关键词
  const titleKeywords = ['标题', '起标题', '生成标题', '改标题'];
  const isTitle = titleKeywords.some(kw => input.includes(kw));

  // 判断意图
  if (isCreate) {
    return {
      mode: 'create',
      subSkill: 'wechat-generate',
      params: extractCreateParams(userInput)
    };
  } else if (isPolish) {
    return {
      mode: 'modify',
      subSkill: 'wechat-polish',
      params: extractModifyParams(userInput)
    };
  } else if (isFormat) {
    return {
      mode: 'modify',
      subSkill: 'wechat-format',
      params: extractModifyParams(userInput)
    };
  } else if (isTitle) {
    return {
      mode: 'modify',
      subSkill: 'wechat-title',
      params: extractModifyParams(userInput)
    };
  }

  return null;
}

function extractCreateParams(input) {
  // 提取主题（去除关键词后的内容）
  const topic = input.replace(/写一篇?|创作|生成|新建/g, '').trim();

  return {
    topic,
    wordCount: 2000,  // 默认值
    style: '专业',
    targetAudience: '普通用户'
  };
}

function extractModifyParams(input) {
  // 提取文件路径和修改要求
  return {
    targetFile: null,  // 需要用户指定或从上下文获取
    instructions: input
  };
}
```
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add skills/write-wechat-article/skill.md
git commit -m "feat: add intent parsing logic to main skill"
\`\`\`

---

## Phase 3: 子技能实现

### Task 5: 实现 wechat-generate 子技能

**文件：**
- Create: `skills/wechat-generate/skill.json`
- Create: `skills/wechat-generate/skill.md`

**Step 1: 创建 skill.json**

```json
{
  "name": "wechat-generate",
  "version": "1.0.0",
  "description": "生成微信公众号文章内容",
  "author": "Maple",
  "triggers": [
    "生成文章",
    "写文章",
    "创作内容"
  ],
  "sharedModules": [
    "../shared/prompts.js",
    "../shared/format-utils.js"
  ]
}
```

**Step 2: 创建 skill.md**

\`\`\`markdown
# 微信公众号文章生成技能

## 功能

根据用户提供的主题，生成完整的微信公众号文章内容。

## 输入参数

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|-----|------|------|--------|------|
| topic | string | 是 | - | 文章主题 |
| wordCount | number | 否 | 2000 | 目标字数 |
| style | string | 否 | 专业 | 文章风格 |
| targetAudience | string | 否 | 普通用户 | 目标读者 |
| outputDir | string | 否 | ./output | 输出目录 |

## 工作流程

1. **生成 Prompt** - 使用 \`shared/prompts.js\` 的 \`generatePrompt('generate', params)\`
2. **返回给 Agent** - 让 Agent 处理内容生成
3. **接收生成内容** - 从 Agent 获取 Markdown 内容
4. **插入插图标记** - 根据字数决定插图数量，在合适位置插入 \`<!-- FIGURE:类型 -->\`
5. **保存 .md 文件** - 写入 \`outputDir/[slug]/[slug].md\`

## 插图插入规则

### 数量规则

- 短文章（<1500字）：1-2张
- 中等文章（1500-3000字）：3-4张
- 长文章（>3000字）：4-5张（上限5张）

### 位置规则

优先级：
1. 章节开头或结尾
2. 重要观点之后
3. 对比内容处
4. 避免在段落中间插入

### 类型分配

| 文章类型 | 推荐插图类型 |
|---------|-------------|
| 问题分析类 | comparison（对比图） |
| 产品介绍类 | ability（核心能力图） |
| 教程指南类 | process（流程图） |

## 输出

返回生成的 Markdown 文件路径。

## 示例

**输入：**
\`\`\`json
{
  "topic": "AI Agent 的发展趋势",
  "wordCount": 2500,
  "style": "专业",
  "targetAudience": "开发者"
}
\`\`\`

**输出：**
\`\`\`
文章已生成：./output/ai-agent-trends/ai-agent-trends.md
字数：2487 字
插图：3 张（对比图、核心能力图、流程图）
\`\`\`
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/wechat-generate/
git commit -m "feat: add wechat-generate subskill"
\`\`\`

---

### Task 6: 实现 wechat-polish 子技能

**文件：**
- Create: `skills/wechat-polish/skill.json`
- Create: `skills/wechat-polish/skill.md`

**Step 1: 创建 skill.json**

```json
{
  "name": "wechat-polish",
  "version": "1.0.0",
  "description": "润色和优化微信公众号文章",
  "author": "Maple",
  "triggers": [
    "优化文章",
    "润色内容",
    "改进表达"
  ],
  "sharedModules": [
    "../shared/prompts.js"
  ]
}
```

**Step 2: 创建 skill.md**

\`\`\`markdown
# 微信公众号文章润色技能

## 功能

对已有文章进行润色和优化，改善语言表达和可读性。

## 输入参数

| 参数 | 类型 | 必填 | 说明 |
|-----|------|------|------|
| content | string | 是 | 待优化的文章内容 |
| direction | string | 否 | 优化方向（如"更简洁"、"更专业"） |
| targetFile | string | 是 | 目标文件路径 |

## 工作流程

1. **读取原文** - 从 targetFile 读取 Markdown 内容
2. **生成 Prompt** - 使用 \`shared/prompts.js\` 的 \`generatePrompt('polish', params)\`
3. **返回给 Agent** - 让 Agent 处理优化
4. **接收优化内容** - 从 Agent 获取优化后的内容
5. **保存文件** - 覆盖原文

## 输出

返回优化后的文件路径。

## 示例

**输入：**
\`\`\`json
{
  "content": "原文内容...",
  "direction": "更简洁，突出重点",
  "targetFile": "./output/article/article.md"
}
\`\`\`

**输出：**
\`\`\`
文章已优化：./output/article/article.md
\`\`\`
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/wechat-polish/
git commit -m "feat: add wechat-polish subskill"
\`\`\`

---

### Task 7: 实现 wechat-title 子技能

**文件：**
- Create: `skills/wechat-title/skill.json`
- Create: `skills/wechat-title/skill.md`

**Step 1: 创建 skill.json**

```json
{
  "name": "wechat-title",
  "version": "1.0.0",
  "description": "生成吸引人的文章标题",
  "author": "Maple",
  "triggers": [
    "生成标题",
    "起标题",
    "优化标题"
  ],
  "sharedModules": [
    "../shared/prompts.js"
  ]
}
```

**Step 2: 创建 skill.md**

\`\`\`markdown
# 微信公众号文章标题生成技能

## 功能

根据文章内容生成多个吸引人的标题选项。

## 输入参数

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|-----|------|------|--------|------|
| content | string | 是 | - | 文章内容或摘要 |
| count | number | 否 | 5 | 生成标题数量 |

## 工作流程

1. **生成 Prompt** - 使用 \`shared/prompts.js\` 的 \`generatePrompt('title', params)\`
2. **返回给 Agent** - 让 Agent 生成标题
3. **接收标题列表** - 从 Agent 获取按吸引力排序的标题
4. **返回给用户** - 让用户选择

## 输出

返回标题列表，按吸引力从高到低排序。

## 示例

**输入：**
\`\`\`json
{
  "content": "文章内容...",
  "count": 5
}
\`\`\`

**输出：**
\`\`\`
请选择标题：

1. AI Agent 来了：普通人如何抓住新机遇
2. 从 ChatGPT 到 Agent：AI 的下一个进化方向
3. 不只是聊天：AI Agent 正在改变工作方式
4. 揭秘 AI Agent：让 AI 帮你完成复杂任务
5. Agent 时代：如何用一句话调动电脑生产力

请输入序号选择，或说"自定义"提供自己的标题。
\`\`\`
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/wechat-title/
git commit -m "feat: add wechat-title subskill"
\`\`\`

---

### Task 8: 实现 wechat-format 子技能

**文件：**
- Create: `skills/wechat-format/skill.json`
- Create: `skills/wechat-format/skill.md`

**Step 1: 创建 skill.json**

```json
{
  "name": "wechat-format",
  "version": "1.0.0",
  "description": "将 Markdown 转换为微信公众号格式并生成封面图",
  "author": "Maple",
  "triggers": [
    "排版文章",
    "生成 HTML",
    "格式化"
  ],
  "sharedModules": [
    "../shared/prompts.js",
    "../shared/format-utils.js"
  ]
}
```

**Step 2: 创建 skill.md**

\`\`\`markdown
# 微信公众号文章排版技能

## 功能

将 Markdown 文章转换为微信公众号兼容的 HTML，生成封面图和插图。

## 输入参数

| 参数 | 类型 | 必填 | 说明 |
|-----|------|------|------|
| markdownFile | string | 是 | Markdown 文件路径 |
| title | string | 是 | 文章标题 |
| outputDir | string | 否 | 输出目录 |

## 工作流程

1. **读取 Markdown** - 从 markdownFile 读取内容
2. **解析插图标记** - 找到所有 \`<!-- FIGURE:类型 -->\` 标记
3. **生成文章 HTML** - 使用 \`format-utils.markdownToWechatHTML()\`
4. **生成插图 HTML** - 根据标记类型生成对应插图
5. **嵌入插图** - 将插图 HTML 嵌入到文章对应位置
6. **生成封面图** - 使用 \`format-utils.generateCoverHTML()\`
7. **验证兼容性** - 使用 \`format-utils.validateWechatCompatibility()\`
8. **保存文件** - 保存 -wechat.html 和 cover-.html

## 兼容性检查

在生成 HTML 后，必须进行兼容性检查：

\`\`\`javascript
const validation = validateWechatCompatibility(html);
if (!validation.valid) {
  console.warn('兼容性问题：');
  validation.issues.forEach(issue => console.warn(\`  - \${issue}\`\`));
}
\`\`\`

## 输出

| 文件 | 说明 |
|-----|------|
| [slug]-wechat.html | 可粘贴到微信公众号的文章 HTML |
| cover-[slug].html | 封面图 HTML |

## 插图嵌入规则

插图直接嵌入到文章 HTML 中，不生成单独文件。

### 插图类型映射

| 标记 | 生成函数 |
|-----|---------|
| <!-- FIGURE:comparison --> | generateComparisonFigure() |
| <!-- FIGURE:ability --> | generateAbilityFigure() |
| <!-- FIGURE:process --> | generateProcessFigure() |

### 嵌入方法

将 \`<!-- FIGURE:类型 -->\` 替换为：

\`\`\`html
<section style="text-align: center; margin: 30px 10px;">
  <img src="figure-[n].html" style="max-width: 100%; height: auto;" alt="插图">
</section>
\`\`\`

**注意**：由于微信公众号不支持外部 HTML，插图需要转换为内联 HTML 或图片。

## 示例

**输入：**
\`\`\`json
{
  "markdownFile": "./output/article/article.md",
  "title": "AI Agent 发展趋势",
  "outputDir": "./output/article"
}
\`\`\`

**输出：**
\`\`\`
文章 HTML 已生成：./output/article/article-wechat.html
封面图已生成：./output/article/cover-article.html
插图已嵌入：3 张
\`\`\`
\`\`\`

**Step 3: 提交**

\`\`\`bash
git add skills/wechat-format/
git commit -m "feat: add wechat-format subskill"
\`\`\`

---

## Phase 4: 测试

### Task 9: 创建测试框架

**文件：**
- Create: `tests/format-utils.test.js`
- Create: `vitest.config.js`

**Step 1: 创建 vitest.config.js**

```javascript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node'
  }
});
```

**Step 2: 创建测试文件**

\`\`\`javascript
import { describe, it, expect } from 'vitest';
import {
  markdownToWechatHTML,
  generateCoverHTML,
  generateComparisonFigure,
  generateAbilityFigure,
  generateProcessFigure,
  validateWechatCompatibility,
  estimateFigureCount
} from '../shared/format-utils.js';

describe('format-utils', () => {
  describe('markdownToWechatHTML', () => {
    it('should convert H1 to styled section', () => {
      const input = '# 测试标题';
      const result = markdownToWechatHTML(input);
      expect(result).toContain('background-color: rgb(0, 112, 192)');
      expect(result).toContain('测试标题');
      expect(result).toContain('text-align: center');
    });

    it('should convert H2 to styled h2', () => {
      const input = '## 测试章节';
      const result = markdownToWechatHTML(input);
      expect(result).toContain('color: rgb(0, 112, 192)');
      expect(result).toContain('border-bottom: 2px solid');
    });

    it('should convert list items to styled paragraphs', () => {
      const input = '• **标题**：内容';
      const result = markdownToWechatHTML(input);
      expect(result).toContain('<p style="margin: 5px 0;">• <strong>标题</strong>：内容</p>');
    });

    it('should convert separator', () => {
      const input = '—— · ——';
      const result = markdownToWechatHTML(input);
      expect(result).toContain('—— · ——');
      expect(result).toContain('text-align: center');
    });
  });

  describe('generateCoverHTML', () => {
    it('should generate valid HTML', () => {
      const result = generateCoverHTML({
        title: '测试标题',
        subtitle: '副标题',
        tags: ['AI', '科技']
      });
      expect(result).toContain('<!DOCTYPE html>');
      expect(result).toContain('测试标题');
      expect(result).toContain('副标题');
    });

    it('should have correct dimensions', () => {
      const result = generateCoverHTML({ title: '测试' });
      expect(result).toContain('width: 1175px');
      expect(result).toContain('height: 500px');
    });
  });

  describe('generateComparisonFigure', () => {
    it('should generate comparison figure', () => {
      const result = generateComparisonFigure({
        leftTitle: '旧方式',
        leftItems: ['痛点1'],
        rightTitle: '新方案',
        rightItems: ['优势1']
      });
      expect(result).toContain('旧方式');
      expect(result).toContain('新方案');
      expect(result).toContain('VS');
    });
  });

  describe('generateAbilityFigure', () => {
    it('should generate ability figure', () => {
      const result = generateAbilityFigure({
        abilities: [
          { name: '能力1', desc: '描述1', icon: '🚀' }
        ]
      });
      expect(result).toContain('能力1');
      expect(result).toContain('描述1');
      expect(result).toContain('🚀');
    });
  });

  describe('generateProcessFigure', () => {
    it('should generate process figure', () => {
      const result = generateProcessFigure({
        steps: [
          { num: 1, title: '步骤1', desc: '描述1' }
        ],
        code: 'command'
      });
      expect(result).toContain('步骤1');
      expect(result).toContain('command');
    });
  });

  describe('validateWechatCompatibility', () => {
    it('should detect style tags', () => {
      const result = validateWechatCompatibility('<style>body{}</style>');
      expect(result.valid).toBe(false);
      expect(result.issues).toContain('包含 <style> 标签（会被过滤）');
    });

    it('should detect div tags', () => {
      const result = validateWechatCompatibility('<div>content</div>');
      expect(result.valid).toBe(false);
    });

    it('should detect table background on tr', () => {
      const result = validateWechatCompatibility('<tr style="background-color: red">');
      expect(result.valid).toBe(false);
      expect(result.issues[0]).toContain('在 <tr> 标签上设置背景色');
    });

    it('should pass valid HTML', () => {
      const result = validateWechatCompatibility('<section style="color: red;">content</section>');
      expect(result.valid).toBe(true);
    });
  });

  describe('estimateFigureCount', () => {
    it('should return 1 for short articles', () => {
      expect(estimateFigureCount(1000)).toBe(1);
    });

    it('should return 3 for medium articles', () => {
      expect(estimateFigureCount(2000)).toBe(3);
    });

    it('should return 5 for long articles', () => {
      expect(estimateFigureCount(5000)).toBe(5);
    });

    it('should cap at 5', () => {
      expect(estimateFigureCount(10000)).toBe(5);
    });
  });
});
\`\`\`

**Step 3: 运行测试**

\`\`\`bash
npm test
\`\`\`

预期输出：所有测试通过

**Step 4: 提交**

\`\`\`bash
git add tests/ vitest.config.js
git commit -m "test: add format-utils test suite"
\`\`\`

---

## Phase 5: 脚本和文档

### Task 10: 创建脚本

**文件：**
- Create: `scripts/install.js`
- Create: `scripts/build.js`
- Create: `scripts/test.js`

**Step 1: 创建 install.js**

\`\`\`javascript
#!/usr/bin/env node

const fs = require('fs');
const path = require('path');

console.log('🔧 安装 write-wechat-article 技能...\n');

// 创建必要的目录
const dirs = [
  'output',
  'docs/plans',
  'tests'
];

dirs.forEach(dir => {
  const fullPath = path.join(process.cwd(), dir);
  if (!fs.existsSync(fullPath)) {
    fs.mkdirSync(fullPath, { recursive: true });
    console.log(\`✅ 创建目录: \${dir}\`);
  }
});

console.log('\n✨ 安装完成！');
console.log('\n下一步:');
console.log('  1. npm install      # 安装依赖');
console.log('  2. npm test         # 运行测试');
console.log('  3. npm run build    # 构建技能');
\`\`\`

**Step 2: 创建 build.js**

\`\`\`javascript
#!/usr/bin/env node

const fs = require('fs');
const path = require('path');

console.log('🏗️  构建技能...\n');

// 验证技能文件结构
const skillDirs = fs.readdirSync(path.join(process.cwd(), 'skills'));
let allValid = true;

skillDirs.forEach(skillDir => {
  const skillPath = path.join(process.cwd(), 'skills', skillDir);
  const skillJson = path.join(skillPath, 'skill.json');
  const skillMd = path.join(skillPath, 'skill.md');

  if (!fs.existsSync(skillJson)) {
    console.error(\`❌ \${skillDir}/skill.json 不存在\`);
    allValid = false;
  }
  if (!fs.existsSync(skillMd)) {
    console.error(\`❌ \${skillDir}/skill.md 不存在\`);
    allValid = false;
  }
});

if (allValid) {
  console.log('✅ 所有技能文件结构正确\n');
  console.log('📦 技能列表:');
  skillDirs.forEach(dir => {
    const skillJson = require(path.join(process.cwd(), 'skills', dir, 'skill.json'));
    console.log(\`  - \${skillJson.name}: \${skillJson.description}\`);
  });
  console.log('\n✨ 构建完成！');
} else {
  console.error('\n❌ 构建失败：技能文件结构不完整');
  process.exit(1);
}
\`\`\`

**Step 3: 创建 test.js**

\`\`\`javascript
#!/usr/bin/env node

const { execSync } = require('child_process');

console.log('🧪 运行测试...\n');

try {
  execSync('npx vitest run', {
    stdio: 'inherit',
    cwd: process.cwd()
  });
  console.log('\n✅ 所有测试通过！');
} catch (error) {
  console.error('\n❌ 测试失败');
  process.exit(1);
}
\`\`\`

**Step 4: 提交**

\`\`\`bash
git add scripts/
git commit -m "chore: add build, install, and test scripts"
\`\`\`

---

### Task 11: 更新 README

**文件：**
- Modify: `README.md`

**Step 1: 更新 README.md**

\`\`\`markdown
# write-wechat-article

微信公众号文章写作技能，面向 AI Agent（Claude Code、OpenCode、OpenClaw）。

## 功能

- 📝 **AI 自动生成文章** - 根据主题快速生成高质量内容
- ✨ **文章润色优化** - 改善语言表达，提升可读性
- 🎨 **微信公众号排版** - 自动转换为微信兼容格式
- 🎯 **标题生成** - 生成吸引人的标题选项

## 架构

模块化多技能架构：

- **write-wechat-article** - 主技能，负责意图解析和任务分发
- **wechat-generate** - 生成子技能
- **wechat-polish** - 润色子技能
- **wechat-format** - 排版子技能
- **wechat-title** - 标题子技能

## 安装

\`\`\`bash
git clone <repo-url>
cd write-wechat-article
npm install
npm run install  # 初始化目录结构
\`\`\`

## 使用

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

## 测试

\`\`\`bash
npm test              # 运行所有测试
npm run build         # 验证技能结构
\`\`\`

## 微信公众号兼容性

本技能严格遵守微信公众号 HTML 规范：

- ✅ 使用 \`<section>\` 标签
- ✅ 使用 \`<p>\` 标签代替 \`<ul>\`/`<li>\`
- ✅ 表格标题背景色应用到每个 \`<th>\` 标签
- ✅ 只使用基础 CSS 属性
- ❌ 禁止 \`<style>\` 标签
- ❌ 禁止 \`<div>\` 标签

主题色：\`rgb(0, 112, 192)\`

## 开发

\`\`\`bash
npm run build    # 构建技能
npm test         # 运行测试
\`\`\`

## 许可

MIT
\`\`\`

**Step 2: 提交**

\`\`\`bash
git add README.md
git commit -m "docs: update README with full project documentation"
\`\`\`

---

## 完成检查

### Task 12: 最终验证

**Step 1: 运行完整测试**

\`\`\`bash
# 安装依赖
npm install

# 初始化目录
npm run install

# 构建验证
npm run build

# 运行测试
npm test
\`\`\`

预期输出：所有命令成功，测试通过

**Step 2: 验证文件结构**

\`\`\`bash
ls -la skills/
ls -la shared/
ls -la scripts/
ls -la tests/
\`\`\`

预期输出：所有目录和文件存在

**Step 3: 最终提交**

\`\`\`bash
git add .
git commit -m "chore: complete initial implementation of wechat article skill

- 模块化多技能架构
- 主技能 + 4个子技能
- 共享模块（prompts、format-utils）
- 完整测试套件
- 构建和测试脚本

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
\`\`\`

---

**实施计划完成！**

保存位置：\`docs/plans/2026-03-22-wechat-article-implementation.md\`
