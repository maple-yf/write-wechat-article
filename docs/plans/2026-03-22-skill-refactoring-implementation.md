# write-wechat-article 技能重构实施计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 将 skills/write-wechat-article/SKILL.md 迁移到项目根目录，整合参考文件内容，创建混合模式文档结构。

**架构:** 采用混合模式：SKILL.md（核心定义+快速参考）+ specs/ 目录（详细规范）+ scripts/ 目录（工具脚本）。

**Tech Stack:** Markdown, Node.js (juice, cheerio)

---

## Task 1: 创建新目录结构

**Files:**
- Create: `specs/` (目录)
- Create: `scripts/` (目录)

**Step 1: 创建 specs 目录**

Run: `mkdir -p specs`
Expected: 目录创建成功

**Step 2: 创建 scripts 目录**

Run: `mkdir -p scripts`
Expected: 目录创建成功

**Step 3: 验证目录结构**

Run: `ls -la | grep -E "specs|scripts"`
Expected: 输出包含 specs 和 scripts 目录

**Step 4: 创建 .gitkeep 保持空目录**

Run: `touch specs/.gitkeep scripts/.gitkeep`
Expected: 文件创建成功

**Step 5: 提交**

```bash
git add specs/ scripts/
git commit -m "feat: create specs and scripts directories"
```

---

## Task 2: 编写 FORMAT_SPEC.md - 基础框架和主题色

**Files:**
- Create: `specs/FORMAT_SPEC.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/WECHAT_TEMPLATE.md`

**Step 1: 创建 FORMAT_SPEC.md 文件框架**

Write `specs/FORMAT_SPEC.md`:

```markdown
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
```

**Step 2: 验证文件创建**

Run: `ls -lh specs/FORMAT_SPEC.md`
Expected: 文件存在，非空

**Step 3: 提交**

```bash
git add specs/FORMAT_SPEC.md
git commit -m "feat: add FORMAT_SPEC.md with theme colors"
```

---

## Task 3: 编写 FORMAT_SPEC.md - HTML 兼容性规范

**Files:**
- Modify: `specs/FORMAT_SPEC.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/WECHAT_COMPATIBILITY_NOTES.md`

**Step 1: 在 FORMAT_SPEC.md 末尾添加 HTML 兼容性章节**

Append to `specs/FORMAT_SPEC.md`:

```markdown

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
```

**Step 2: 提交**

```bash
git add specs/FORMAT_SPEC.md
git commit -m "feat: add HTML compatibility rules to FORMAT_SPEC.md"
```

---

## Task 4: 编写 FORMAT_SPEC.md - 标题和设计风格规范

**Files:**
- Modify: `specs/FORMAT_SPEC.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/WECHAT_TEMPLATE.md`

**Step 1: 添加标题样式章节**

Append to `specs/FORMAT_SPEC.md`:

```markdown

---

## 三、标题样式

### H1 一级标题
- 居中 + 背景色 `rgb(0, 112, 192)` + 白色字体
- 可添加 emoji

### H2 二级标题
- 颜色 `rgb(0, 112, 192)` + 下边框
- 字体加粗

### H3 三级标题
- 深灰色 + 加粗
```

**Step 2: 添加设计风格章节**

Append to `specs/FORMAT_SPEC.md`:

```markdown

---

## 四、设计风格规范

### 4.1 配色体系（封面/插图）

```css
/* 科技蓝主题 */
--primary: #0EA5E9;         /* 主色 */
--accent: #38BDF8;          /* 强调色 */
--deep-blue: #0284C7;       /* 深色强调 */
--dark: #0F172A;            /* 深色背景 */
--success: #22C55E;         /* 成功/亮点 */
--pain: #E53935;            /* 痛点红 */
```

### 4.2 布局模式

- **卡片式布局**：圆角 12-16px，玻璃拟态效果
- **网格系统**：2×2、2×4、三栏等宽
- **对比策略**：左右并置、红蓝对比
- **F型阅读动线**：重要信息左对齐

### 4.3 数据可视化

- **大数字冲击**：48-64px 字号
- **横向条形图**：三色对比
- **彩色标签**：胶囊形状

### 4.4 开发者语境元素

- 代码片段：深色背景 `#0F172A`，等宽字体
- 技术术语：蓝色高亮 `#38BDF8`
- 角色标签：`You:` 蓝色加粗，`Claude:` 灰色
```

**Step 3: 提交**

```bash
git add specs/FORMAT_SPEC.md
git commit -m "feat: add title and design style sections to FORMAT_SPEC.md"
```

---

## Task 5: 编写 FORMAT_SPEC.md - 配图和检查清单

**Files:**
- Modify: `specs/FORMAT_SPEC.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/WECHAT_TEMPLATE.md`

**Step 1: 添加配图设计章节**

Append to `specs/FORMAT_SPEC.md`:

```markdown

---

## 五、配图设计规范

### 5.1 封面图布局

```
左侧矩形（1175×500px）+ 右侧方形（500×500px）

左侧：渐变背景 + 主标题 + 副标题 + 特性卡片
右侧：深色背景 + Logo + 核心数据 + 标签系统
```

### 5.2 文章配图类型

| 类型 | 用途 | 结构 |
|-----|------|------|
| comparison | 对比图 | 左右对比 + VS 徽章 |
| ability | 核心能力图 | 深色背景 + 三栏卡片 |
| process | 流程图 | 步骤可视化 + 代码块 |

### 5.3 插图数量

- 短文章（<1500字）：1-2张
- 中等文章（1500-3000字）：3-4张
- 长文章（>3000字）：4-5张
```

**Step 2: 添加 HTML 生成检查清单**

Append to `specs/FORMAT_SPEC.md`:

```markdown

---

## 六、HTML 生成检查清单

### 生成前检查：
- [ ] 使用 `<section>` 而非 `<div>`
- [ ] 使用 `<p>` 而非 `<ul>`/`<li>`
- [ ] 表格背景色应用到 `<th>` 标签
- [ ] 只使用基础 CSS 属性
- [ ] 使用主题色 `rgb(0, 112, 192)`

### 粘贴后检查：
- [ ] 标题颜色正确
- [ ] 背景色保留（重点检查表格）
- [ ] 列表符号显示
- [ ] 手机预览正常
```

**Step 3: 添加参考来源**

Append to `specs/FORMAT_SPEC.md`:

```markdown

---

*参考来源：WECHAT_TEMPLATE.md、WECHAT_COMPATIBILITY_NOTES.md*
```

**Step 4: 提交**

```bash
git add specs/FORMAT_SPEC.md
git commit -m "feat: complete FORMAT_SPEC.md with image and checklist sections"
```

---

## Task 6: 编写 PROCESS_GUIDE.md - 框架和流程总览

**Files:**
- Create: `specs/PROCESS_GUIDE.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/MEMORY.md`

**Step 1: 创建 PROCESS_GUIDE.md 文件框架**

Write `specs/PROCESS_GUIDE.md`:

```markdown
# 微信公众号文章创作流程指南

> Agent + 开发者参考文档
> 完整的文章创作、检验、发布流程

---

## 一、流程总览

```
调研 → 创作 → 生成 → 配图 → 检验 → 发布
```

### 各阶段说明

| 阶段 | 产出 | 依赖 skill |
|-----|------|-----------|
| 调研 | 素材库 | deep-research, agent-reach, browser-use |
| 创作 | 文案内容 | copywriting |
| 生成 | .md, .html | - |
| 配图 | 封面图、插图 | - |
| 检验 | 质量报告 | - |
| 发布 | 预览截图 | agent-browser |
```

**Step 2: 验证文件创建**

Run: `ls -lh specs/PROCESS_GUIDE.md`
Expected: 文件存在，非空

**Step 3: 提交**

```bash
git add specs/PROCESS_GUIDE.md
git commit -m "feat: add PROCESS_GUIDE.md with workflow overview"
```

---

## Task 7: 编写 PROCESS_GUIDE.md - 调研流程

**Files:**
- Modify: `specs/PROCESS_GUIDE.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/MEMORY.md`

**Step 1: 添加调研流程章节**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 二、调研流程

### 2.1 图片处理（如有图片输入）

当用户提供图片时：
1. 使用 **modelscope-k2-5-read-image** skill 读取图片内容
2. 将提取的文字加入调研素材库

### 2.2 多渠道调研

**可用 skills：**
- **deep-research**：企业级深度研究，多源综合、引用追踪
- **agent-reach**：搜索 16 个平台（Twitter/Reddit/B站/小红书/微信文章等）
- **browser-use**：浏览器自动化，访问网页、提取数据
- **defuddle**：提取网页干净 Markdown（URL 处理优先）

**调研要求：**
1. 根据主题选择合适的 skills 组合
2. 多渠道收集和汇总信息
3. 整理调研结果，形成素材库

### 2.3 URL 处理优先级

当用户提供 URL 时：
```bash
defuddle parse "<url>" --json --md
```
```

**Step 2: 提交**

```bash
git add specs/PROCESS_GUIDE.md
git commit -m "feat: add research workflow to PROCESS_GUIDE.md"
```

---

## Task 8: 编写 PROCESS_GUIDE.md - 创作和生成流程

**Files:**
- Modify: `specs/PROCESS_GUIDE.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/SOUL.md`

**Step 1: 添加创作流程章节**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 三、创作流程

### 3.1 交互式问答（分步骤）

**不一次性完成，采用对话方式：**

**第一阶段：方向确定**
1. Agent 提供 3-5 个专业建议和深度思考选项
2. 用户确定大方向
3. Agent 细化文章结构和大纲

**第二阶段：内容填充**
1. Agent 逐个填充内容要点
2. 每个要点提供详细阐述
3. 用户确认或调整

**第三阶段：润色定稿**
1. Agent 进行最终润色
2. 确保语言流畅、逻辑清晰
3. 输出完整文案

**关键原则：**
- 每次只讨论一个环节
- Agent 提供专业建议，用户拍板定调
- 确保每一步都经过确认

### 3.2 内容要求（强制）

**底线要求：**
- ✅ 正确性：事实准确，数据可靠
- ✅ 合法性：符合法律法规

**高阶追求（必须达成）：**
- 🎯 **思想有深度**：有洞察、有思考、有升华
- 🎯 **立意要高**：站在更高视角，挖掘本质
- 🎯 **观点要鲜明**：敢于表达立场，有独特见解

**写作原则：**
- 拒绝浅尝辄止，深挖逻辑和规律
- 拒绝人云亦云，提供独立思考
- 拒绝平庸堆砌，每篇都要有灵魂和锋芒
```

**Step 2: 添加生成流程章节**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 四、生成流程

### 4.1 文档生成

**Step 1：生成 Markdown**
- 按模板要求生成 .md 文件
- 保存到 `output/[文章slug]/[slug].md`

**Step 2：生成 HTML**
- 遵循 FORMAT_SPEC.md 的兼容性规范
- 保存到 `output/[文章slug]/[slug]-wechat.html`

**Step 3：生成封面图**
- 左侧矩形（1175×500px）+ 右侧方形（500×500px）
- 应用科技蓝主题和玻璃拟态效果
- 保存到 `output/[文章slug]/cover-[slug].html`

**Step 4：生成插图**
- 根据文章内容生成 3-5 张插图
- 插图类型：comparison、ability、process
- 插图直接嵌入到文章 HTML 中

### 4.2 文件命名规范

- 文章 slug：小写字母 + 连字符
- 示例：`openclaw-chain-effect`、`pinchbench-intro`
```

**Step 3: 提交**

```bash
git add specs/PROCESS_GUIDE.md
git commit -m "feat: add creation and generation workflow to PROCESS_GUIDE.md"
```

---

## Task 9: 编写 PROCESS_GUIDE.md - 检验和发布流程

**Files:**
- Modify: `specs/PROCESS_GUIDE.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/MEMORY.md`

**Step 1: 添加质量检验流程章节**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 五、质量检验流程

### 5.1 检查项目

**文字检查：**
- [ ] 错别字检查
- [ ] 标点符号规范（全中文标点）
- [ ] 病句检查
- [ ] 前后一致性（专有名词、数据、时间）

**格式检查：**
- [ ] 列表格式统一
- [ ] 表格格式正确
- [ ] 标题层级合理

**HTML 检查：**
- [ ] H1 居中 + 主题蓝背景 + 白色字体
- [ ] H2 使用主题蓝色和下边框
- [ ] 表格背景色应用到 `<th>` 标签
- [ ] 列表使用 `<p>` + `•`
- [ ] 使用 `<section>` 而非 `<div>`

### 5.2 修改完善

对发现的问题进行修改，重复检验直到所有问题解决。
```

**Step 2: 添加发布流程章节**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 六、发布流程

### 6.1 截图预览

**文章截图：**
```bash
npx agent-browser open file:///<HTML文件路径>
npx agent-browser screenshot --full <截图保存路径>
```

**封面图/配图截图：**
```bash
npx agent-browser open file:///<HTML文件路径>
npx agent-browser screenshot --full <截图保存路径>
```

### 6.2 发送预览

使用飞书发送截图：
```javascript
message(
  action=send,
  channel=feishu,
  media=<图片路径>
)
```

### 6.3 清理和关闭

```bash
rm <截图文件路径>
npx agent-browser close
```
```

**Step 3: 添加反馈修改流程和输出目录规范**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 七、反馈修改流程

当用户提供反馈需要修改时：

**必须重新走完整流程：**
1. **重新调研**：使用 skills 针对反馈点补充调研
2. **重新创作**：使用 copywriting skill 根据新调研修改
3. 重复生成 → 配图 → 检验 → 发布

**注意：**
- 不能直接修改文字，必须重新调研→创作
- 调研和创作必须充分利用已有 skills

---

## 八、输出目录规范

**默认输出位置：** `./output/[文章slug]/`

| 文件 | 说明 |
|-----|------|
| [slug].md | Markdown 源文件 |
| [slug]-wechat.html | 微信公众号 HTML |
| cover-[slug].html | 封面图 HTML |

**可配置：**
- 调用参数：`--output ./my-output`
- 环境变量：`WECHAT_ARTICLE_OUTPUT_DIR`
```

**Step 4: 添加工具使用和参考来源**

Append to `specs/PROCESS_GUIDE.md`:

```markdown

---

## 九、工具使用

### HTML 转换工具

```bash
# 安装依赖
cd scripts && npm install

# 转换 HTML
node convert_html_to_wechat.js <input.html> [output.html]
```

---

*参考来源：MEMORY.md、SOUL.md、README_WECHAT_CONVERTER.md*
```

**Step 5: 提交**

```bash
git add specs/PROCESS_GUIDE.md
git commit -m "feat: complete PROCESS_GUIDE.md with quality check and publish workflow"
```

---

## Task 10: 整合 scripts 目录

**Files:**
- Create: `scripts/convert_html_to_wechat.js`
- Create: `scripts/package.json`
- Create: `scripts/README.md`
- Reference: `/Users/mapleyf/.openclaw/workspace-poster/convert_html_to_wechat.js`

**Step 1: 复制转换工具到 scripts 目录**

Run: `cp /Users/mapleyf/.openclaw/workspace-poster/convert_html_to_wechat.js scripts/convert_html_to_wechat.js`
Expected: 文件复制成功

**Step 2: 创建 package.json**

Write `scripts/package.json`:

```json
{
  "name": "write-wechat-article-scripts",
  "version": "1.0.0",
  "description": "微信公众号文章转换工具",
  "scripts": {
    "convert": "node convert_html_to_wechat.js"
  },
  "dependencies": {
    "juice": "^10.0.0",
    "cheerio": "^1.0.0"
  }
}
```

**Step 3: 创建 scripts/README.md**

Write `scripts/README.md`:

```markdown
# Scripts 使用说明

## HTML 转换工具

将带 `<style>` 标签的 HTML 转换为微信公众号兼容格式。

### 安装依赖

```bash
cd scripts
npm install
```

### 使用方法

```bash
# 基础用法（输出到 input-wechat.html）
node convert_html_to_wechat.js <input.html>

# 指定输出文件名
node convert_html_to_wechat.js <input.html> <output.html>

# 使用 npm script
npm run convert <input.html>
```

### 工作原理

1. **CSS Inlining**：将 `<style>` 标签转换为内联样式
2. **伪元素处理**：将 `::before` 转换为真实 HTML 元素
3. **属性过滤**：移除不支持的 CSS 属性

### 样式保留率

| 样式类型 | 保留率 |
|---------|--------|
| 颜色和字体 | 100% |
| 间距 | 100% |
| 边框 | 95% |
| 背景色 | 100% |
| 圆角 | 100% |
| **总体** | **92-95%** |

---

*详细文档：../README_WECHAT_CONVERTER.md*
```

**Step 4: 删除 .gitkeep 文件**

Run: `rm specs/.gitkeep scripts/.gitkeep`
Expected: 文件删除成功

**Step 5: 提交**

```bash
git add scripts/
git commit -m "feat: integrate HTML converter tool into scripts directory"
```

---

## Task 11: 重写 SKILL.md - 核心定义和快速参考

**Files:**
- Modify: `skills/write-wechat-article/SKILL.md`
- Move: `skills/write-wechat-article/SKILL.md` → `SKILL.md`

**Step 1: 读取现有 SKILL.md**

Run: `cat skills/write-wechat-article/SKILL.md`
Expected: 显示现有内容

**Step 2: 创建新的 SKILL.md**

Write `SKILL.md`:

```markdown
---
name: write-wechat-article
description: Use when the user asks to "写一篇公众号文章", "创作公众号内容", "生成微信公众号文章", "优化这篇文章", "润色文章", "给文章起标题", or mentions WeChat public account (微信公众号), WeChat article formatting, WeChat-compatible HTML, article cover generation, or wants to create content for WeChat public account. Make sure to use this skill whenever the user mentions WeChat, 公众号, article writing, content creation, or needs help with article formatting or title generation.
---

# 微信公众号文章写作技能

## 快速参考

### 触发条件
- "写一篇公众号文章" / "创作公众号内容"
- "优化这篇文章" / "润色文章"
- "生成标题" / "给文章起标题"
- 提及微信公众号、WeChat article

### 输出产物

| 文件 | 说明 |
|-----|------|
| [slug].md | Markdown 源文件 |
| [slug]-wechat.html | 微信公众号 HTML |
| cover-[slug].html | 封面图 HTML |

### 核心流程概览

```
1. 调研 → 2. 创作 → 3. 生成文案 → 4. 设计配图 → 5. 检验 → 6. 发布
```

---

## 功能

1. **创作模式** - 从头开始生成完整文章
2. **修改模式** - 优化现有文章的某个方面

---

## 技能模式

### 创作模式

当用户说"写一篇关于XXX的公众号文章"时：

**Step 1: 深度调研**
- 使用 deep-rearch / agent-reach / browser-use skills
- 详见：[PROCESS_GUIDE.md#调研流程](specs/PROCESS_GUIDE.md#二调研流程)

**Step 2: 文案创作**
- 交互式问答确定方向和内容
- 详见：[PROCESS_GUIDE.md#创作流程](specs/PROCESS_GUIDE.md#三创作流程)

**Step 3: 生成文档**
- 生成 Markdown 文件
- 生成微信公众号 HTML
- 详见：[FORMAT_SPEC.md#HTML生成](specs/FORMAT_SPEC.md)

**Step 4: 设计配图**
- **封面图**：生成 cover-[slug].html（左侧矩形1175×500px + 右侧方形500×500px）
- **插图**：根据文章内容生成3-5张插图
  - 对比图（问题 vs 方案）
  - 核心能力图（功能/特性介绍）
  - 流程图（步骤/教程）
- 详见：[FORMAT_SPEC.md#配图设计](specs/FORMAT_SPEC.md#五配图设计规范)

**Step 5: 质量检验**
- 错别字、标点、格式检查
- 详见：[PROCESS_GUIDE.md#质量检验](specs/PROCESS_GUIDE.md#五质量检验流程)

**Step 6: 截图预览和发布**
- 使用 agent-browser 截图预览
- 发送到飞书
- 详见：[PROCESS_GUIDE.md#发布流程](specs/PROCESS_GUIDE.md#六发布流程)

### 修改模式

当用户说"优化这篇文章"或"修改标题"时：

1. **解析意图** - 识别用户想修改什么（内容/标题/排版）
2. **读取文件** - 读取目标文件
3. **执行修改** - 使用 Agent 的能力进行修改
4. **保存文件** - 保存修改后的内容
5. **返回路径** - 返回更新后的文件路径

---

## 意图识别

| 用户输入 | 意图 | 操作 |
|---------|------|------|
| "写一篇关于AI的文章" | 创作 | 生成新文章 |
| "优化这篇文章" | 润色 | 优化现有文章 |
| "调整排版" | 格式化 | 转换为微信 HTML |
| "生成标题" | 标题 | 生成标题选项 |

---

## 关键规范速查

### 微信兼容性 ✅

- 使用 `<section>` 而非 `<div>`
- 使用 `<p>` 而非 `<ul>`/`<li>`
- 表格背景色应用到 `<th>` 标签
- 禁止 `<style>` 标签

详见：[FORMAT_SPEC.md#HTML兼容性](specs/FORMAT_SPEC.md#二html-兼容性规范重要)

### 主题色

- 微信文章色：`rgb(0, 112, 192)` 或 `#0070c0`
- 科技蓝主题（封面/插图）：`#0EA5E9`、`#38BDF8`

详见：[FORMAT_SPEC.md#主题色](specs/FORMAT_SPEC.md#一主题色规范)

### 配图规范速查

**插图数量：**
- 短文章（<1500字）：1-2张
- 中等文章（1500-3000字）：3-4张
- 长文章（>3000字）：4-5张

**插图类型：**
- comparison：对比图（左右对比 + VS 徽章）
- ability：核心能力图（深色背景 + 三栏卡片）
- process：流程图（步骤可视化 + 代码块）

**封面图布局：**
- 左侧矩形（1175×500px）+ 右侧方形（500×500px）
- 科技蓝主题：#0EA5E9、#38BDF8、#0284C7
- 玻璃拟态效果、大数字冲击

详见：[FORMAT_SPEC.md#配图设计](specs/FORMAT_SPEC.md#五配图设计规范)

### 内容要求

- 🎯 **思想有深度**：有洞察、有思考、有升华
- 🎯 **立意要高**：站在更高视角，挖掘本质
- 🎯 **观点要鲜明**：敢于表达立场，有独特见解

详见：[PROCESS_GUIDE.md#内容要求](specs/PROCESS_GUIDE.md#32-内容要求强制)

---

## 输出位置

**默认**：`./output/[文章slug]/`

**可配置**：
- 调用参数：`--output ./my-output`
- 环境变量：`WECHAT_ARTICLE_OUTPUT_DIR`

---

## 详细规范

- **完整格式规范**：[FORMAT_SPEC.md](specs/FORMAT_SPEC.md)
- **完整流程指南**：[PROCESS_GUIDE.md](specs/PROCESS_GUIDE.md)
- **脚本工具说明**：[scripts/README.md](scripts/README.md)
```

**Step 3: 验证新 SKILL.md 创建**

Run: `ls -lh SKILL.md`
Expected: 文件存在，内容正确

**Step 4: 提交**

```bash
git add SKILL.md
git commit -m "feat: rewrite SKILL.md with hybrid structure and quick reference"
```

---

## Task 12: 更新项目 README.md

**Files:**
- Modify: `README.md`

**Step 1: 读取现有 README.md**

Run: `cat README.md`
Expected: 显示现有内容

**Step 2: 更新 README.md**

Write `README.md`:

```markdown
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
```

**Step 3: 提交**

```bash
git add README.md
git commit -m "docs: update README.md with new structure and documentation"
```

---

## Task 13: 清理旧文件和最终验证

**Files:**
- Remove: `skills/write-wechat-article/SKILL.md`

**Step 1: 删除旧的 SKILL.md**

Run: `rm -rf skills/write-wechat-article/SKILL.md`
Expected: 文件删除成功

**Step 2: 验证最终目录结构**

Run: `tree -L 2 -I 'node_modules|.git'`
Expected output:
```
.
├── SKILL.md
├── README.md
├── specs/
│   ├── FORMAT_SPEC.md
│   └── PROCESS_GUIDE.md
├── scripts/
│   ├── convert_html_to_wechat.js
│   ├── package.json
│   └── README.md
├── output/
├── tests/
└── docs/
```

**Step 3: 验证 SKILL.md 格式**

Run: `head -20 SKILL.md`
Expected: 显示 frontmatter 和标题正确

**Step 4: 验证规范文件链接**

Run: `grep -E "FORMAT_SPEC|PROCESS_GUIDE" SKILL.md`
Expected: 显示正确的相对路径链接

**Step 5: 最终提交**

```bash
git add -A
git commit -m "chore: remove old SKILL.md and finalize refactoring"
```

---

## Task 14: 运行兼容性检查

**Files:**
- Test: `tests/compatibility-checklist.md`

**Step 1: 验证 FORMAT_SPEC.md 包含所有关键规范**

Run: `grep -E "主题色|HTML 兼容性|表格标题|配图" specs/FORMAT_SPEC.md`
Expected: 输出包含所有关键章节

**Step 2: 验证 PROCESS_GUIDE.md 包含所有流程**

Run: `grep -E "调研|创作|检验|发布" specs/PROCESS_GUIDE.md`
Expected: 输出包含所有关键流程

**Step 3: 验证 scripts 配置正确**

Run: `cat scripts/package.json | grep -E "juice|cheerio"`
Expected: 输出包含依赖包

**Step 4: 检查所有文件是否可访问**

Run: `ls -lh SKILL.md specs/*.md scripts/*.{js,json,md}`
Expected: 所有文件都存在且非空

**Step 5: 最终提交**

```bash
git add -A
git commit -m "test: verify all refactoring components are in place"
```

---

## 完成检查清单

完成所有任务后，验证：

- [ ] SKILL.md 在项目根目录
- [ ] specs/FORMAT_SPEC.md 包含完整格式规范
- [ ] specs/PROCESS_GUIDE.md 包含完整流程指南
- [ ] scripts/ 目录包含转换工具和配置
- [ ] README.md 已更新
- [ ] 旧的 skills/ 目录已清理
- [ ] 所有文档链接正确
- [ ] 所有任务已提交到 git

---

**实施计划完成时间估计：** 14 个任务，每个任务 5-15 分钟，总计约 2-3 小时
