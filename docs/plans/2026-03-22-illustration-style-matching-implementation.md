# 插图风格智能匹配功能实现计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 为微信公众号文章写作技能增加插图风格智能匹配功能，根据文章内容自动推荐合适的视觉风格和色彩方案。

**Architecture:** 采用关键词加权匹配算法，在文章生成流程中新增风格匹配步骤。风格数据存储在 specs/STYLE_PALETTE.md，SKILL.md 更新流程说明，通过分析文章内容和关键词频率推荐风格。

**Tech Stack:** Markdown, YAML, 文本处理算法

---

## 任务概览

1. 创建风格色彩规范文档 (STYLE_PALETTE.md)
2. 更新 SKILL.md 流程说明
3. 实现风格匹配算法逻辑

---

## Task 1: 创建风格色彩规范文档

**Files:**
- Create: `specs/STYLE_PALETTE.md`

**Step 1: 创建 STYLE_PALETTE.md 文档头部**

```markdown
# 插图风格色彩规范

本文档定义了 14 种可用插图风格的主题色值、关键词权重和适用场景。

> 使用场景：当 AI 需要根据文章内容推荐插图风格时，参考本文档的关键词权重和色值方案。
```

**Step 2: 添加第一种风格 - 信息图蓝图风**

```markdown
---

## 1. 信息图蓝图风

**模板**: `templates/infographic.html`

**视觉特征**:
- 蓝图网格背景（20px小网格 + 100px大网格）
- 模块化卡片布局，浅绿色半透明背景
- 工程图纸风格的边框和标签
- 荧光笔高亮效果（黄色#FFF200）

**色值方案**:
```yaml
background: "#F2F2F2"        # 蓝图灰
module_bg: "rgba(184, 216, 190, 0.25)"  # 模块背景
border: "#2D2926"            # 深灰边框
primary: "#E91E63"           # 粉红强调
secondary: "#3A7BD5"         # 蓝色强调
warning: "#E91E63"           # 警告色
text: "#1a1a1a"              # 文字色
```

**关键词权重**:
```yaml
对比: 3
评测: 3
选择: 2
数据: 3
分析: 2
技术: 1
工具: 2
```

**字体**:
- 中英文: 系统默认 (-apple-system, PingFang SC, Noto Sans SC)
- 代码/标签: 'SF Mono', monospace

**适用场景**: 技术对比、产品选型、工具评测、数据分析报告
```

**Step 3: 添加第二种风格 - 商务深蓝风**

```markdown
---

## 2. 商务深蓝风

**模板**: `templates/style-business.html`

**视觉特征**:
- 深蓝色顶部栏（#1B2A4A）带强调色下划线
- 大号KPI数字展示
- 左侧编号圆形标识
- 光谱卡片（左色条区分等级）

**色值方案**:
```yaml
background: "#FFFFFF"        # 白色
background_alt: "#F1F5F9"    # 浅灰
primary: "#1B2A4A"           # 海军蓝
accent: "#2563EB"            # 亮蓝
success: "#059669"           # 绿色
warning: "#DC2626"           # 红色
text: "#1a1a1a"
```

**关键词权重**:
```yaml
商业: 3
战略: 2
分析: 2
报告: 2
数据: 2
KPI: 3
企业: 2
```

**字体**:
- 英文: 'DM Sans', sans-serif
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 商业分析、战略报告、产品发布、企业内参
```

**Step 4: 添加第三种风格 - 暗色科技风**

```markdown
---

## 3. 暗色科技风

**模板**: `templates/style-dark-tech.html`

**视觉特征**:
- 深色背景（#0A0A0F）带扫描线纹理
- 霓虹绿色发光效果（#00FFB2）
- 终端风格文字（带 > 前缀）
- 半透明模块卡片

**色值方案**:
```yaml
background: "#0A0A0F"        # 近黑
primary: "#00FFB2"           # 霓虹绿
secondary: "#3B82F6"         # 蓝色
warning: "#FF3366"           # 霓虹红
text: "#EEEEF0"              # 浅灰
```

**关键词权重**:
```yaml
代码: 3
开发: 3
技术: 2
黑客: 3
终端: 2
编程: 3
教程: 2
```

**字体**:
- 英文: 'IBM Plex Mono', monospace
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 开发者内容、技术文章、代码教程、黑客主题
```

**Step 5: 添加第四种风格 - 杂志编辑风**

```markdown
---

## 4. 杂志编辑风

**模板**: `templates/style-journal.html`

**视觉特征**:
- 米色纸张背景（#FAF8F5）
- 衬线字体标题
- 巨大引号装饰
- 双栏报纸风格布局

**色值方案**:
```yaml
background: "#FAF8F5"        # 米白
text: "#1a1a1a"              # 深黑
accent: "#C8412B"            # 编辑红
border: "#1a1a1a"            # 分隔线
border_light: "#ddd"
```

**关键词权重**:
```yaml
深度: 3
报道: 2
专题: 2
学术: 2
文化: 2
评论: 2
杂志: 2
```

**字体**:
- 英文标题: 'Playfair Display', serif
- 中文标题: 'Noto Serif SC', serif
- 正文: 'Noto Sans SC', sans-serif

**适用场景**: 深度报道、专题文章、学术论文、文化评论
```

**Step 6: 添加第五种风格 - 实验室蓝图风**

```markdown
---

## 5. 实验室蓝图风（认知架构专用）

**模板**: `templates/style-lab-blueprint.html`

**视觉特征**:
- 蓝图网格背景
- 大标题粗体设计（72px）
- 三层光谱卡片（带英文标识和壁垒等级）

**色值方案**:
```yaml
background: "#F2F2F2"
module_bg: "rgba(184, 216, 190, 0.25)"
text: "#1a1a1a"
primary: "#E91E63"           # 粉红
secondary: "#3A7BD5"         # 蓝
tertiary: "#E6A817"          # 橙
```

**关键词权重**:
```yaml
架构: 3
认知: 3
OS: 2
Application: 2
原理: 2
概念: 2
图谱: 2
```

**字体**:
- 中文: 系统默认
- 标签/代码: 'SF Mono', monospace

**适用场景**: 认知架构说明、概念图谱、技术原理解析
```

**Step 7: 添加第六种风格 - 杂志现代风**

```markdown
---

## 6. 杂志现代风

**模板**: `templates/style-magazine.html`

**视觉特征**:
- 与杂志编辑风类似，但更现代
- 简洁的标签系统
- 场景卡片网格

**色值方案**:
```yaml
background: "#FFFFFF"
text: "#1a1a1a"
accent: "#C8412B"            # 红
gray_medium: "#666"
gray_light: "#888"
```

**关键词权重**:
```yaml
现代: 2
杂志: 2
生活方式: 2
趋势: 2
时尚: 2
```

**字体**:
- 标题: 'Playfair Display' + 'Noto Serif SC'
- 正文: 'Noto Sans SC'

**适用场景**: 现代杂志文章、生活方式、趋势分析
```

**Step 8: 添加第七种风格 - 孟菲斯波普风**

```markdown
---

## 7. 孟菲斯波普风

**模板**: `templates/style-memphis.html`

**视觉特征**:
- 大量几何装饰（圆、三角、十字、波浪）
- 鲜艳的孟菲斯配色
- 旋转错位效果
- 粗黑边框

**色值方案**:
```yaml
background: "#FFFFFF"
primary: "#FFD700"           # 金黄
secondary: "#FF6B6B"         # 珊瑚红
tertiary: "#4169E1"          # 皇室蓝
accent1: "#98FB98"           # 淡绿
accent2: "#9B59B6"           # 紫色
border: "#1a1a1a"
```

**关键词权重**:
```yaml
创意: 3
设计: 3
趣味: 2
年轻: 2
波普: 3
艺术: 2
```

**字体**:
- 英文: 'Rubik', sans-serif
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 创意内容、设计文章、年轻化品牌、趣味科普
```

**Step 9: 添加第八种风格 - 报纸新闻风**

```markdown
---

## 8. 报纸新闻风

**模板**: `templates/style-newspaper.html`

**视觉特征**:
- 报纸质感背景（#FBF7F0）
- 经典报头设计（多层分隔线）
- 衬线字体标题
- 三线表格

**色值方案**:
```yaml
background: "#FBF7F0"        # 纸张白
ink: "#111111"               # 墨黑
gray_dark: "#444444"
gray_light: "#888888"
accent: "#CC0000"            # 红色强调
```

**关键词权重**:
```yaml
新闻: 3
报道: 2
时事: 2
政策: 2
行业: 2
动态: 2
```

**字体**:
- 报头: 'Playfair Display' + 'Noto Serif SC'
- 正文: 'Source Sans 3' + 'Noto Serif SC'

**适用场景**: 新闻报道、时事评论、政策解读、行业动态
```

**Step 10: 添加第九种风格 - 北欧极简风**

```markdown
---

## 9. 北欧极简风

**模板**: `templates/style-nordic.html`

**视觉特征**:
- 大量留白
- 柔和的自然色调（绿、橙、红）
- Outfit无衬线字体
- 细线分隔

**色值方案**:
```yaml
background: "#FAFAF8"        # 米白
primary: "#6B7F5E"           # 鼠尾草绿
accent1: "#C4654A"           # 陶土红
accent2: "#D4B896"           # 沙金
text: "#2C2C2C"              # 深灰
```

**关键词权重**:
```yaml
极简: 3
北欧: 2
生活: 2
环保: 2
自然: 2
设计: 2
```

**字体**:
- 英文: 'Outfit' + 'Inter'
- 中文: 'Noto Sans SC'

**适用场景**: 极简设计、生活方式、北欧风内容、环保主题
```

**Step 11: 添加第十种风格 - 复古未来主义**

```markdown
---

## 10. 复古未来主义

**模板**: `templates/style-retro-future.html`

**视觉特征**:
- CRT扫描线效果
- 屏幕暗角（vignette）
- 像素网格纹理
- 琥珀色荧光（#FFBF00）

**色值方案**:
```yaml
background: "#2D1B00"        # 深琥珀黑
primary: "#FFBF00"           # 琥珀荧光
secondary: "#33FF33"         # 磷绿
warning: "#FF4444"           # 霓虹红
text: "#F5E6C8"              # 米白
```

**关键词权重**:
```yaml
复古: 3
未来: 3
赛博: 2
游戏: 2
科技: 2
像素: 2
```

**字体**:
- 英文: 'Space Mono' + 'IBM Plex Mono', monospace
- 中文: 'Noto Sans SC'

**适用场景**: 复古科技、赛博朋克、未来主义、游戏主题
```

**Step 12: 添加第十一种风格 - 深蓝卡片科技风**

```markdown
---

## 11. 深蓝卡片科技风

**模板**: `templates/style-card-deep-blue.html`

**视觉特征**:
- 深蓝渐变背景
- 发光顶部线条
- 彩色数据卡片（蓝/绿/橙/紫）
- 半透明玻璃效果

**色值方案**:
```yaml
background: "linear-gradient(#0A192F, #142233, #0F1F35)"
primary: "#1E90FF"           # 道奇蓝
accent1: "#00C853"           # 绿
accent2: "#FF9800"           # 橙
accent3: "#8B5CF6"           # 紫
text: "#FFFFFF"
```

**关键词权重**:
```yaml
科技: 3
产品: 2
SaaS: 3
云服务: 2
数据: 2
仪表盘: 3
```

**字体**:
- 英文: 'Inter', sans-serif
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 科技产品、数据仪表盘、SaaS产品、云服务
```

**Step 13: 添加第十二种风格 - 红蓝对话风**

```markdown
---

## 12. 红蓝对话风

**模板**: `templates/style-dialog-red-blue.html`

**视觉特征**:
- 红蓝双色主题
- 对话气泡设计
- 问题列表（带编号圆）
- 大CTA按钮

**色值方案**:
```yaml
background: "#FFFFFF"
primary_red: "#E63946"       # 红
primary_blue: "#1D4ED8"      # 蓝
bg_red: "#FFE8E8"            # 淡红
bg_blue: "#E6F3FF"           # 淡蓝
text: "#1a1a1a"
```

**关键词权重**:
```yaml
对话: 3
问答: 3
问题: 2
教程: 2
FAQ: 3
指南: 2
```

**字体**:
- 英文: 'Inter', sans-serif
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 问答对话、问题解决、教程指南、FAQ内容
```

**Step 14: 添加第十三种风格 - 蓝绿流程风**

```markdown
---

## 13. 蓝绿流程风

**模板**: `templates/style-process-blue-green.html`

**视觉特征**:
- 蓝绿双色主题
- 流程步骤组件（带箭头）
- 双色渐变卡片
- 生命周期网格

**色值方案**:
```yaml
background: "#FFFFFF"
background_alt: "#F8F9FA"
primary_blue: "#007BFF"      # 蓝
primary_green: "#10B981"     # 绿
bg_beige: "#FFF7ED"          # 米色
bg_green: "#ECFDF5"          # 淡绿
text: "#1a1a1a"
```

**关键词权重**:
```yaml
流程: 3
步骤: 3
教程: 2
操作: 2
指南: 2
生命周期: 2
```

**字体**:
- 英文: 'Inter', sans-serif
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 流程说明、操作指南、步骤教程、用户旅程
```

**Step 15: 添加第十四种风格 - 蓝色步骤风**

```markdown
---

## 14. 蓝色步骤风

**模板**: `templates/style-step-blue.html`

**视觉特征**:
- 圆形步骤标识
- 代码块展示
- 渐变数据卡片
- 步骤列表（垂直）

**色值方案**:
```yaml
background: "#FFFFFF"
background_alt: "#F5F7FA"    # 浅蓝灰
primary: "#007BFF"           # 蓝
primary_dark: "#0056B3"      # 深蓝
code_bg: "#1E293B"           # 代码底
text: "#1a1a1a"
```

**关键词权重**:
```yaml
教程: 3
入门: 2
快速开始: 2
编程: 3
API: 2
代码: 3
```

**字体**:
- 英文: 'Inter' + 'JetBrains Mono'
- 中文: 'Noto Sans SC', sans-serif

**适用场景**: 编程教程、技术入门、快速开始、API文档
```

**Step 16: 添加风格选择参考表**

```markdown
---

## 风格选择参考表

| 文章类型 | 推荐风格 |
|---------|---------|
| 技术对比/评测 | 信息图蓝图风、实验室蓝图风 |
| 商业/战略报告 | 商务深蓝风、深蓝卡片科技风 |
| 开发/编程内容 | 暗色科技风、复古未来主义、蓝色步骤风 |
| 深度报道/学术 | 杂志编辑风、报纸新闻风 |
| 创意/设计 | 孟菲斯波普风、北欧极简风 |
| 数据/仪表盘 | 商务深蓝风、深蓝卡片科技风 |
| 对话/问答 | 红蓝对话风 |
| 流程/教程 | 蓝绿流程风、蓝色步骤风 |
| 产品/技术 | 深蓝卡片科技风、暗色科技风 |
```

**Step 17: 添加关键词匹配说明**

```markdown
---

## AI 匹配说明

### 根据内容关键词匹配

- 包含 `代码`、`编程`、`开发` → 暗色科技风、蓝色步骤风
- 包含 `数据`、`分析`、`对比` → 信息图蓝图风、商务深蓝风
- 包含 `设计`、`创意`、`艺术` → 孟菲斯波普风、北欧极简风
- 包含 `流程`、`步骤`、`教程` → 蓝绿流程风、蓝色步骤风
- 包含 `对话`、`问答`、`问题` → 红蓝对话风
- 包含 `新闻`、`报道`、`政策` → 报纸新闻风、杂志编辑风

### 插图类型与风格组合

插图类型（comparison/ability/process）决定基础结构，风格主题提供视觉样式，两者可自由组合。
```

**Step 18: 提交创建的规范文档**

```bash
git add specs/STYLE_PALETTE.md
git commit -m "docs: add illustration style palette specification"
```

---

## Task 2: 更新 SKILL.md 流程说明

**Files:**
- Modify: `SKILL.md`

**Step 1: 在 SKILL.md 中添加风格规范引用**

在 `## 关键规范速查` 部分的 `### 主题色` 小节后添加：

```markdown
详见：[FORMAT_SPEC.md#主题色](specs/FORMAT_SPEC.md#一主题色规范)

### 插图风格

- 14 种可用风格，详见 [STYLE_PALETTE.md](specs/STYLE_PALETTE.md)
- 风格匹配：AI 根据文章内容自动推荐合适风格
- 插图类型：comparison（对比）、ability（能力）、process（流程）

详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md#插图风格)
```

**Step 2: 更新 Step 4: 设计配图 部分**

将原来的：
```markdown
**Step 4: 设计配图**
- **封面图**：生成 cover-[slug].html（左侧矩形1175×500px + 右侧方形500×500px）
- **插图**：根据文章内容生成3-5张插图
  - 对比图（问题 vs 方案）
  - 核心能力图（功能/特性介绍）
  - 流程图（步骤/教程）
- 详见：[FORMAT_SPEC.md#配图设计](specs/FORMAT_SPEC.md#五配图设计规范)
```

更新为：
```markdown
**Step 4: 设计配图**
- **4.1 生成封面图**：生成 cover-[slug].html（左侧矩形1175×500px + 右侧方形500×500px）
- **4.2 风格匹配**：根据文章内容推荐插图风格
  - AI 分析全文内容和关键词
  - 推荐 1-3 种风格供用户选择
  - 用户确认后应用对应色值方案
  - 详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md)
- **4.3 生成插图**：使用确认的风格生成 3-5 张插图
  - 插图类型：comparison（对比图）、ability（能力图）、process（流程图）
  - 风格主题：应用用户确认的色值方案
  - 插图类型决定结构，风格主题提供视觉样式
- 详见：[FORMAT_SPEC.md#配图设计](specs/FORMAT_SPEC.md#五配图设计规范)
```

**Step 3: 提交更新**

```bash
git add SKILL.md
git commit -m "docs: update SKILL.md with illustration style matching workflow"
```

---

## Task 3: 在快速参考中添加插图风格说明

**Files:**
- Modify: `SKILL.md`

**Step 1: 在 `### 核心流程概览` 后添加插图风格说明**

在 `## 快速参考` 部分添加：

```markdown
**Step 4: 设计配图**
- **4.1 生成封面图**：生成 cover-[slug].html
- **4.2 风格匹配**：根据文章内容推荐插图风格（详见 STYLE_PALETTE.md）
- **4.3 生成插图**：使用确认的风格生成插图
```

**Step 2: 在 `### 配图规范速查` 部分添加风格说明**

在现有内容后添加：

```markdown

**插图风格（14种可选）**：
- 信息图蓝图风、商务深蓝风、暗色科技风、杂志编辑风
- 实验室蓝图风、杂志现代风、孟菲斯波普风、报纸新闻风
- 北欧极简风、复古未来主义、深蓝卡片科技风、红蓝对话风
- 蓝绿流程风、蓝色步骤风

详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md)
```

**Step 3: 提交更新**

```bash
git add SKILL.md
git commit -m "docs: add illustration style quick reference to SKILL.md"
```

---

## Task 4: 实现风格匹配算法（在 skill 中添加执行逻辑）

**Files:**
- Modify: `SKILL.md`

**Step 1: 在 SKILL.md 末尾添加风格匹配执行逻辑**

在 `## 详细规范` 部分之前添加：

```markdown
---

## 风格匹配执行逻辑

当执行 **Step 4.2: 风格匹配** 时，按以下流程操作：

### 4.2.1 分析文章内容

1. 读取文章标题和正文内容
2. 合并全文进行关键词提取
3. 统计关键词出现频率

### 4.2.2 计算风格得分

对 14 种风格分别计算匹配得分：

```
风格得分 = Σ(关键词频率 × 关键词权重)
```

例如：文章出现"代码"5次、"开发"3次、"教程"2次
- 暗色科技风得分 = 5×3 + 3×3 + 2×2 = 28
- 蓝色步骤风得分 = 5×3 + 2×2 = 19
- 信息图蓝图风得分 = 3×1 = 3

### 4.2.3 生成推荐报告

按得分降序排列，取前 3 种风格（得分 > 0），生成报告：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 文章插图风格分析报告
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📝 文章信息
  标题：[文章标题]
  字数：约 [字数] 字
  插图建议：[数量] 张

🎯 推荐风格

  1. 【风格名称】模板文件 ⭐ 最佳匹配
     匹配得分：[得分]
     触发关键词：
       • 关键词1 (次数)
       • 关键词2 (次数)
     推荐理由：[根据适用场景生成]

     色值方案：
       背景：#[色值]
       主色：#[色值]
       次色：#[色值]
       ...

  2. 【风格名称】模板文件
     匹配得分：[得分]
     ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

请选择风格（输入编号或名称，或直接确认推荐风格）：
```

### 4.2.4 用户确认

1. 展示推荐报告
2. 等待用户选择（输入编号、名称，或确认推荐）
3. 如果用户不满意，可重新展示所有 14 种风格列表
4. 确认后，记录选定的风格信息

### 4.2.5 应用风格到插图生成

在 Step 4.3 生成插图时：
1. 根据内容需要确定插图类型（comparison/ability/process）
2. 使用 Step 4.2 确认的风格色值
3. 组合生成最终插图

### 风格与插图类型组合示例

| 插图类型 | 风格主题 | 结果描述 |
|---------|---------|---------|
| comparison | 暗色科技风 | 深色霓虹对比图（左右两栏深色背景 + 霓虹边框） |
| ability | 孟菲斯波普风 | 孟菲斯风格能力卡片（几何装饰 + 鲜艳配色） |
| process | 商务深蓝风 | 商务风格流程图（深蓝主题 + 专业配色） |

---
```

**Step 2: 提交更新**

```bash
git add SKILL.md
git commit -m "feat: add style matching execution logic to SKILL.md"
```

---

## Task 5: 创建风格匹配测试场景

**Files:**
- Create: `tests/style-matching-test-scenarios.md`

**Step 1: 创建测试场景文档**

```markdown
# 风格匹配测试场景

本文档用于测试风格匹配功能是否按预期工作。

---

## 测试场景 1: 技术教程文章

**文章内容关键词**：代码、开发、编程、教程、入门

**预期推荐**：
1. 暗色科技风（高权重关键词匹配多）
2. 蓝色步骤风（教程、入门关键词）

---

## 测试场景 2: 商业分析文章

**文章内容关键词**：商业、战略、数据、KPI、企业

**预期推荐**：
1. 商务深蓝风
2. 深蓝卡片科技风

---

## 测试场景 3: 创意设计文章

**文章内容关键词**：创意、设计、艺术、趣味、年轻

**预期推荐**：
1. 孟菲斯波普风
2. 北欧极简风

---

## 测试场景 4: 流程教程文章

**文章内容关键词**：流程、步骤、操作、指南、生命周期

**预期推荐**：
1. 蓝绿流程风
2. 蓝色步骤风

---

## 测试场景 5: 新闻报道文章

**文章内容关键词**：新闻、报道、时事、政策、行业

**预期推荐**：
1. 报纸新闻风
2. 杂志编辑风

---

## 测试场景 6: 技术对比文章

**文章内容关键词**：对比、评测、数据、分析、工具

**预期推荐**：
1. 信息图蓝图风
2. 实验室蓝图风
```

**Step 2: 提交测试场景**

```bash
git add tests/style-matching-test-scenarios.md
git commit -m "test: add style matching test scenarios"
```

---

## Task 6: 更新 PROCESS_GUIDE.md

**Files:**
- Modify: `specs/PROCESS_GUIDE.md`

**Step 1: 更新生成流程部分**

在 `### 4.2 文件命名规范` 之后添加：

```markdown
### 4.3 插图风格匹配

**Step 1：分析文章内容**
- 读取文章标题和正文
- 提取关键词并统计频率

**Step 2：推荐风格**
- 计算每种风格的匹配得分
- 生成推荐报告（前 3 种风格）

**Step 3：用户确认**
- 展示推荐报告
- 等待用户选择或确认

**Step 4：应用色值**
- 根据确认的风格，应用对应色值方案
- 结合插图类型生成最终插图

详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md)
```

**Step 2: 提交更新**

```bash
git add specs/PROCESS_GUIDE.md
git commit -m "docs: update PROCESS_GUIDE.md with style matching workflow"
```

---

## Task 7: 更新 FORMAT_SPEC.md

**Files:**
- Modify: `specs/FORMAT_SPEC.md`

**Step 1: 在配图设计规范部分添加风格说明**

在 `### 5.2 文章配图类型` 之后添加：

```markdown

### 5.3 插图风格主题

**可选风格**（14 种）：

| 风格 | 适用场景 | 色值特征 |
|-----|---------|---------|
| 信息图蓝图风 | 技术对比、产品选型 | 蓝图灰背景 + 粉红/蓝强调 |
| 商务深蓝风 | 商业分析、战略报告 | 深蓝主题 + KPI 数字 |
| 暗色科技风 | 开发者内容、代码教程 | 深色背景 + 霓虹绿发光 |
| 杂志编辑风 | 深度报道、学术论文 | 米白背景 + 衬线字体 |
| 实验室蓝图风 | 认知架构、技术原理 | 蓝图网格 + 三层光谱 |
| 杂志现代风 | 现代杂志、生活方式 | 简洁标签 + 场景卡片 |
| 孟菲斯波普风 | 创意内容、设计文章 | 几何装饰 + 鲜艳配色 |
| 报纸新闻风 | 新闻报道、时事评论 | 报纸质感 + 经典报头 |
| 北欧极简风 | 极简设计、生活方式 | 大量留白 + 自然色调 |
| 复古未来主义 | 复古科技、赛博朋克 | CRT 扫描 + 琥珀荧光 |
| 深蓝卡片科技风 | 科技产品、SaaS | 深蓝渐变 + 玻璃效果 |
| 红蓝对话风 | 问答对话、FAQ | 红蓝双色 + 对话气泡 |
| 蓝绿流程风 | 流程说明、操作指南 | 蓝绿主题 + 流程步骤 |
| 蓝色步骤风 | 编程教程、API 文档 | 圆形步骤 + 代码块 |

**风格匹配方式**：
- AI 根据文章内容关键词自动推荐
- 用户可从推荐中选择或自定义
- 详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md)

**风格与插图类型组合**：
- 插图类型决定基础结构
- 风格主题提供视觉样式（色值、字体、特效）
- 可自由组合
```

**Step 2: 提交更新**

```bash
git add specs/FORMAT_SPEC.md
git commit -m "docs: update FORMAT_SPEC.md with illustration style themes"
```

---

## Task 8: 创建实现验证清单

**Files:**
- Create: `tests/style-matching-verification.md`

**Step 1: 创建验证清单**

```markdown
# 风格匹配功能验证清单

实现完成后，按此清单验证功能是否正常工作。

---

## 文档验证

- [ ] STYLE_PALETTE.md 文件已创建，包含 14 种风格
- [ ] 每种风格包含：名称、模板、色值、关键词权重、适用场景
- [ ] SKILL.md 已更新风格匹配流程
- [ ] PROCESS_GUIDE.md 已更新
- [ ] FORMAT_SPEC.md 已更新
- [ ] 测试场景文档已创建

---

## 功能验证

### 场景 1: 技术教程文章
- [ ] 输入包含"代码、开发、编程、教程"的文章
- [ ] 验证推荐暗色科技风为第一推荐
- [ ] 验证报告包含触发关键词和次数
- [ ] 验证色值方案正确显示

### 场景 2: 商业分析文章
- [ ] 输入包含"商业、战略、数据、KPI"的文章
- [ ] 验证推荐商务深蓝风为第一推荐
- [ ] 验证报告格式正确

### 场景 3: 创意设计文章
- [ ] 输入包含"创意、设计、艺术"的文章
- [ ] 验证推荐孟菲斯波普风为第一推荐

### 场景 4: 用户选择其他风格
- [ ] 展示推荐后，用户输入其他风格名称
- [ ] 验证可以正确应用用户选择的风格

### 场景 5: 风格与插图类型组合
- [ ] 验证 comparison + 暗色科技风 正确组合
- [ ] 验证 ability + 孟菲斯波普风 正确组合
- [ ] 验证 process + 商务深蓝风 正确组合

---

## 输出验证

- [ ] 推荐报告格式正确
- [ ] 色值方案完整显示
- [ ] 推荐理由合理准确
- [ ] 用户交互流程顺畅
```

**Step 2: 提交验证清单**

```bash
git add tests/style-matching-verification.md
git commit -m "test: add style matching verification checklist"
```

---

## 总结

### 创建的文件

1. `specs/STYLE_PALETTE.md` - 14 种风格色彩规范
2. `tests/style-matching-test-scenarios.md` - 测试场景
3. `tests/style-matching-verification.md` - 验证清单

### 修改的文件

1. `SKILL.md` - 添加风格匹配流程和执行逻辑
2. `specs/PROCESS_GUIDE.md` - 更新生成流程
3. `specs/FORMAT_SPEC.md` - 添加插图风格主题

### 实现顺序

1. 创建 STYLE_PALETTE.md 规范文档
2. 更新 SKILL.md 流程说明
3. 更新快速参考部分
4. 添加执行逻辑到 SKILL.md
5. 创建测试场景
6. 更新 PROCESS_GUIDE.md
7. 更新 FORMAT_SPEC.md
8. 创建验证清单

### 后续工作

- 根据实际使用反馈调整关键词权重
- 考虑添加自定义风格功能
- 考虑添加风格预览功能
