# 插图风格调色板 specification

本文档定义了微信公众号文章的 14 种插图风格，包括视觉特征、色值方案、关键词权重、字体说明和适用场景。

---

## 1. 信息图蓝图风 (Infographic Blueprint)

### 模板文件
`templates/infographic.html`

### 视觉特征
- 现代信息图风格，强调数据可视化
- 网格化布局，模块化设计
- 技术蓝图质感，适合对比评测类内容

### 色值方案 (YAML)
```yaml
colors:
  background: "#F2F2F2"
  module_background: "rgba(184, 216, 190, 0.25)"
  border: "#2D2926"
  accent_primary: "#E91E63"
  accent_secondary: "#3A7BD5"
  text_primary: "#2D2926"
  text_secondary: "#4A4A4A"
```

### 关键词权重 (YAML)
```yaml
keywords:
  对比: 3
  评测: 3
  选择: 2
  数据: 3
  分析: 2
  技术: 1
  工具: 2
```

### 字体说明
- 标题：PingFang SC Bold, 20-24px
- 正文：PingFang SC Regular, 14-16px
- 数据标注：Menlo, Monaco, monospace, 12-14px

### 适用场景
- 产品对比评测
- 数据分析报告
- 技术工具选择指南
- 性能测试报告

---

## 2. 商务深蓝风 (Business Deep Blue)

### 模板文件
`templates/style-business.html`

### 视觉特征
- 专业商务风格，权威可信
- 深蓝色主调，强调企业级专业感
- 简洁大气的卡片式布局

### 色值方案 (YAML)
```yaml
colors:
  background: "#FFFFFF"
  primary: "#1B2A4A"
  accent: "#2563EB"
  text_primary: "#1E293B"
  text_secondary: "#64748B"
  card_background: "#F8FAFC"
  border: "#E2E8F0"
```

### 关键词权重 (YAML)
```yaml
keywords:
  商业: 3
  战略: 2
  分析: 2
  报告: 2
  数据: 2
  KPI: 3
  企业: 2
```

### 字体说明
- 标题：PingFang SC Semibold, 18-22px
- 正文：PingFang SC Regular, 15-16px
- 数据强调：DIN Alternate, 16-18px

### 适用场景
- 商业分析报告
- 企业战略文档
- KPI 数据展示
- 行业研究报告

---

## 3. 暗色科技风 (Dark Tech)

### 模板文件
`templates/style-dark-tech.html`

### 视觉特征
- 暗色主题，黑客终端风格
- 霓虹色点缀，赛博朋克质感
- 适合代码展示和技术深度内容

### 色值方案 (YAML)
```yaml
colors:
  background: "#0A0A0F"
  primary: "#00FFB2"
  secondary: "#3B82F6"
  warning: "#FF3366"
  text_primary: "#E5E7EB"
  text_secondary: "#9CA3AF"
  code_background: "#1F2937"
```

### 关键词权重 (YAML)
```yaml
keywords:
  代码: 3
  开发: 3
  技术: 2
  黑客: 3
  终端: 2
  编程: 3
  教程: 2
```

### 字体说明
- 标题：JetBrains Mono Bold, 18-22px
- 正文：PingFang SC Regular, 14-16px
- 代码：JetBrains Mono, 13-15px

### 适用场景
- 编程教程
- 技术深度解析
- 代码示例展示
- 开发工具介绍

---

## 4. 杂志编辑风 (Journal Editorial)

### 模板文件
`templates/style-journal.html`

### 视觉特征
- 高端杂志风格，编辑排版
- 大图配文，强调视觉冲击力
- 红色点缀，增加视觉焦点

### 色值方案 (YAML)
```yaml
colors:
  background: "#FAF8F5"
  accent: "#C8412B"
  text_primary: "#2C2C2C"
  text_secondary: "#666666"
  quote_border: "#C8412B"
  divider: "#E5E5E5"
```

### 关键词权重 (YAML)
```yaml
keywords:
  深度: 3
  报道: 2
  专题: 2
  学术: 2
  文化: 2
  评论: 2
  杂志: 2
```

### 字体说明
- 标题：PingFang SC Medium, 22-28px
- 副标题：PingFang SC Regular, 16-18px
- 正文：PingFang SC Light, 15-17px
- 引用：Georgia, 16-18px (italic)

### 适用场景
- 深度报道文章
- 学术专题内容
- 文化评论文章
- 杂志式长文

---

## 5. 实验室蓝图风 (Lab Blueprint)

### 模板文件
`templates/style-lab-blueprint.html`

### 视觉特征
- 认知架构专用风格
- 实验室蓝图质感，技术图表化
- 三色强调系统，层次分明

### 色值方案 (YAML)
```yaml
colors:
  background: "#F2F2F2"
  primary: "#E91E63"
  secondary: "#3A7BD5"
  tertiary: "#E6A817"
  text_primary: "#2D2926"
  text_secondary: "#4A4A4A"
  blueprint_lines: "#B0B0B0"
```

### 关键词权重 (YAML)
```yaml
keywords:
  架构: 3
  认知: 3
  OS: 2
  Application: 2
  原理: 2
  概念: 2
  图谱: 2
```

### 字体说明
- 标题：PingFang SC Bold, 20-24px
- 正文：PingFang SC Regular, 14-16px
- 标签：Menlo, Monaco, monospace, 12px

### 适用场景
- 系统架构说明
- 认知概念解析
- 原理图展示
- 知识图谱文章

---

## 6. 杂志现代风 (Magazine Modern)

### 模板文件
`templates/style-magazine.html`

### 视觉特征
- 现代杂志风格，时尚简约
- 大留白设计，注重视觉呼吸
- 红色点缀，提升设计感

### 色值方案 (YAML)
```yaml
colors:
  background: "#FFFFFF"
  accent: "#C8412B"
  text_primary: "#1A1A1A"
  text_secondary: "#737373"
  divider: "#E5E5E5"
  highlight: "#FFF1F0"
```

### 关键词权重 (YAML)
```yaml
keywords:
  现代: 2
  杂志: 2
  生活方式: 2
  趋势: 2
  时尚: 2
```

### 字体说明
- 标题：PingFang SC Semibold, 24-30px
- 正文：PingFang SC Regular, 15-17px
- 注释：PingFang SC Light, 13-14px

### 适用场景
- 生活方式文章
- 趋势分析内容
- 时尚设计类文章
- 现代主题内容

---

## 7. 孟菲斯波普风 (Memphis Pop)

### 模板文件
`templates/style-memphis.html`

### 视觉特征
- 孟菲斯设计风格，几何图形
- 高饱和度色彩，年轻活泼
- 波普艺术风格，趣味性强

### 色值方案 (YAML)
```yaml
colors:
  background: "#FFFFFF"
  primary: "#FFD700"
  secondary: "#FF6B6B"
  tertiary: "#4169E1"
  quaternary: "#00CED1"
  text_primary: "#2D2D2D"
  pattern_overlay: "rgba(255, 215, 0, 0.1)"
```

### 关键词权重 (YAML)
```yaml
keywords:
  创意: 3
  设计: 3
  趣味: 2
  年轻: 2
  波普: 3
  艺术: 2
```

### 字体说明
- 标题：PingFang SC Bold, 22-28px
- 正文：PingFang SC Medium, 15-17px
- 装饰文字：PingFang SC Bold, 14px

### 适用场景
- 创意设计类文章
- 趣味科普内容
- 年轻化主题文章
- 艺术设计介绍

---

## 8. 报纸新闻风 (Newspaper)

### 模板文件
`templates/style-newspaper.html`

### 视觉特征
- 经典报纸排版，传统新闻风格
- 标题密集，信息密度高
- 红色报头，强调新闻属性

### 色值方案 (YAML)
```yaml
colors:
  background: "#FBF7F0"
  accent: "#CC0000"
  text_primary: "#2C2C2C"
  text_secondary: "#5A5A5A"
  headline: "#1A1A1A"
  divider: "#D4D4D4"
  paper_texture: "rgba(0, 0, 0, 0.02)"
```

### 关键词权重 (YAML)
```yaml
keywords:
  新闻: 3
  报道: 2
  时事: 2
  政策: 2
  行业: 2
  动态: 2
```

### 字体说明
- 报头：PingFang SC Heavy, 28-36px
- 标题：PingFang SC Bold, 18-22px
- 正文：PingFang SC Regular, 14-15px
- 电头：Menlo, 12px

### 适用场景
- 新闻报道文章
- 时事分析内容
- 政策解读文章
- 行业动态报道

---

## 9. 北欧极简风 (Nordic Minimal)

### 模板文件
`templates/style-nordic.html`

### 视觉特征
- 北欧极简风格，自然质朴
- 低饱和度色彩，温和舒适
- 大留白设计，强调呼吸感

### 色值方案 (YAML)
```yaml
colors:
  background: "#FAFAF8"
  primary: "#6B7F5E"
  accent1: "#C4654A"
  accent2: "#D4B896"
  text_primary: "#3D4A3A"
  text_secondary: "#6B7280"
  divider: "#E5E7EB"
```

### 关键词权重 (YAML)
```yaml
keywords:
  极简: 3
  北欧: 2
  生活: 2
  环保: 2
  自然: 2
  设计: 2
```

### 字体说明
- 标题：PingFang SC Light, 20-26px
- 正文：PingFang SC Ultralight, 15-17px
- 注释：PingFang SC Ultralight, 13-14px

### 适用场景
- 生活方式文章
- 环保主题内容
- 极简设计介绍
- 自然主题文章

---

## 10. 复古未来主义 (Retro Future)

### 模板文件
`templates/style-retro-future.html`

### 视觉特征
- 复古未来主义风格
- 深色背景配霓虹色，赛博朋克
- 像素元素与科技感结合

### 色值方案 (YAML)
```yaml
colors:
  background: "#2D1B00"
  primary: "#FFBF00"
  secondary: "#33FF33"
  warning: "#FF4444"
  text_primary: "#FFE4B5"
  text_secondary: "#D4A574"
  scanline: "rgba(0, 0, 0, 0.3)"
```

### 关键词权重 (YAML)
```yaml
keywords:
  复古: 3
  未来: 3
  赛博: 2
  游戏: 2
  科技: 2
  像素: 2
```

### 字体说明
- 标题：Press Start 2P / PingFang SC Bold, 18-24px
- 正文：PingFang SC Regular, 14-16px
- 复古强调：Courier New, 13-15px

### 适用场景
- 复古科技文章
- 游戏相关内容
- 赛博朋克主题
- 未来科技畅想

---

## 11. 深蓝卡片科技风 (Deep Blue Card)

### 模板文件
`templates/style-card-deep-blue.html`

### 视觉特征
- 深蓝渐变背景，科技感强
- 卡片式模块化布局
- 适合 SaaS 产品和技术服务介绍

### 色值方案 (YAML)
```yaml
colors:
  background: "linear-gradient(#0A192F, #142233, #0F1F35)"
  primary: "#1E90FF"
  text_primary: "#FFFFFF"
  text_secondary: "#94A3B8"
  card_background: "rgba(30, 41, 59, 0.6)"
  card_border: "rgba(148, 163, 184, 0.2)"
```

### 关键词权重 (YAML)
```yaml
keywords:
  科技: 3
  产品: 2
  SaaS: 3
  云服务: 2
  数据: 2
  仪表盘: 3
```

### 字体说明
- 标题：PingFang SC Semibold, 18-22px
- 正文：PingFang SC Regular, 14-16px
- 卡片标题：PingFang SC Medium, 16px

### 适用场景
- SaaS 产品介绍
- 云服务文档
- 数据产品说明
- 技术平台展示

---

## 12. 红蓝对话风 (Red-Blue Dialog)

### 模板文件
`templates/style-dialog-red-blue.html`

### 视觉特征
- 双色对话系统，问答结构
- 红蓝对比色，视觉区分清晰
- 适合 FAQ 和对话式内容

### 色值方案 (YAML)
```yaml
colors:
  primary_red: "#E63946"
  primary_blue: "#1D4ED8"
  background: "#FFFFFF"
  text_primary: "#1F2937"
  text_secondary: "#6B7280"
  question_bg: "#FEF2F2"
  answer_bg: "#EFF6FF"
```

### 关键词权重 (YAML)
```yaml
keywords:
  对话: 3
  问答: 3
  问题: 2
  教程: 2
  FAQ: 3
  指南: 2
```

### 字体说明
- 问题标题：PingFang SC Semibold, 16-18px
- 回答内容：PingFang SC Regular, 14-16px
- 对话标签：Menlo, 12px

### 适用场景
- FAQ 问答文章
- 问题解决指南
- 对话式教程
- 互动式内容

---

## 13. 蓝绿流程风 (Blue-Green Process)

### 模板文件
`templates/style-process-blue-green.html`

### 视觉特征
- 蓝绿渐变，流程导向
- 步骤化设计，操作清晰
- 适合教程和操作指南

### 色值方案 (YAML)
```yaml
colors:
  primary_blue: "#007BFF"
  primary_green: "#10B981"
  background: "#FFFFFF"
  text_primary: "#1F2937"
  text_secondary: "#6B7280"
  step_background: "#F3F4F6"
  step_border: "#E5E7EB"
```

### 关键词权重 (YAML)
```yaml
keywords:
  流程: 3
  步骤: 3
  教程: 2
  操作: 2
  指南: 2
  生命周期: 2
```

### 字体说明
- 步骤标题：PingFang SC Semibold, 16-18px
- 步骤内容：PingFang SC Regular, 14-16px
- 步骤编号：DIN Alternate, 20-24px

### 适用场景
- 操作流程教程
- 步骤指南文章
- 生命周期说明
- 实施方案文档

---

## 14. 蓝色步骤风 (Step Blue)

### 模板文件
`templates/style-step-blue.html`

### 视觉特征
- 蓝色主题，步骤清晰
- 代码块友好，技术教程专用
- 深色代码背景，易读性强

### 色值方案 (YAML)
```yaml
colors:
  primary: "#007BFF"
  primary_dark: "#0056B3"
  background: "#FFFFFF"
  text_primary: "#212529"
  text_secondary: "#6C757D"
  code_bg: "#1E293B"
  code_text: "#E2E8F0"
  step_bg: "#F8F9FA"
```

### 关键词权重 (YAML)
```yaml
keywords:
  教程: 3
  入门: 2
  快速开始: 2
  编程: 3
  API: 2
  代码: 3
```

### 字体说明
- 步骤标题：PingFang SC Semibold, 18-20px
- 正文：PingFang SC Regular, 14-16px
- 代码：JetBrains Mono / Fira Code, 13-15px

### 适用场景
- 编程入门教程
- 快速开始指南
- API 文章教程
- 代码示例文章

---

## 风格选择参考表

| 文章类型 | 推荐风格 | 备选风格 |
|---------|---------|---------|
| 产品对比评测 | 信息图蓝图风 | 深蓝卡片科技风 |
| 商业分析报告 | 商务深蓝风 | 杂志编辑风 |
| 编程技术教程 | 暗色科技风 / 蓝色步骤风 | 蓝绿流程风 |
| 深度报道文章 | 杂志编辑风 | 报纸新闻风 |
| 系统架构说明 | 实验室蓝图风 | 深蓝卡片科技风 |
| 生活方式内容 | 杂志现代风 / 北欧极简风 | 信息图蓝图风 |
| 创意设计文章 | 孟菲斯波普风 | 杂志现代风 |
| 新闻报道 | 报纸新闻风 | 杂志编辑风 |
| 环保主题 | 北欧极简风 | 杂志现代风 |
| 复古科技游戏 | 复古未来主义 | 暗色科技风 |
| SaaS 产品介绍 | 深蓝卡片科技风 | 商务深蓝风 |
| FAQ 问答 | 红蓝对话风 | 蓝绿流程风 |
| 操作教程 | 蓝绿流程风 | 蓝色步骤风 |
| 快速入门 | 蓝色步骤风 | 暗色科技风 |

---

## AI 匹配说明

### 关键词到风格的映射规则

1. **关键词权重评分机制**
   - 每种风格定义了多个关键词及权重（1-3）
   - AI 分析文章标题、摘要、内容关键词
   - 计算每种风格的总权重得分
   - 选择得分最高的风格

2. **权重计算公式**
   ```
   风格得分 = Σ (关键词出现次数 × 权重)
   ```

3. **特殊规则**
   - 如果多个风格得分相近（<10% 差异），使用文章类型二次判定
   - 某些关键词组合触发特定风格（如"认知"+"架构" → 实验室蓝图风）
   - 技术类文章优先匹配代码友好风格（暗色科技风、蓝色步骤风）

4. **匹配示例**
   - "Python 入门教程快速开始"
     - 蓝色步骤风：教程(3) + 入门(2) + 快速开始(2) = 7
     - 暗色科技风：代码(3) + 编程(3) + 教程(2) = 8
     - 蓝绿流程风：教程(2) + 步骤(3) + 指南(2) = 7
     - **推荐**：暗色科技风

---

## 插图类型与风格组合说明

### 插图类型定义

1. **封面图 (Cover)**
   - 文章主视觉，通常为第一张插图
   - 建议使用：杂志编辑风、孟菲斯波普风、复古未来主义

2. **章节分隔图 (Divider)**
   - 章节之间的分隔插图
   - 建议使用：信息图蓝图风、实验室蓝图风、蓝绿流程风

3. **概念解释图 (Concept)**
   - 解释抽象概念或原理
   - 建议使用：实验室蓝图风、信息图蓝图风、深蓝卡片科技风

4. **数据展示图 (Data)**
   - 展示数据或统计信息
   - 建议使用：信息图蓝图风、商务深蓝风、深蓝卡片科技风

5. **代码示例图 (Code)**
   - 展示代码片段或配置
   - 建议使用：暗色科技风、蓝色步骤风、深蓝卡片科技风

6. **流程步骤图 (Process)**
   - 展示操作流程或步骤
   - 建议使用：蓝绿流程风、蓝色步骤风、红蓝对话风

### 风格一致性原则

1. **单篇文章建议**
   - 优先保持风格一致，使用同一风格模板
   - 如需混合，不超过 2-3 种风格
   - 混合时保持色系统一（如同为蓝色系）

2. **多篇文章系列**
   - 同一系列文章使用统一风格
   - 建立系列视觉识别系统
   - 保持标题样式、色值方案一致

---

## 色值方案使用规范

### 色值格式
- 主色值使用 HEX 格式：`#RRGGBB`
- 透明度使用 RGBA 格式：`rgba(R, G, B, A)`
- 渐变使用 CSS 格式：`linear-gradient(...)`

### 色值命名约定
```yaml
# 基础色
background: 背景色
text_primary: 主文本色
text_secondary: 次文本色

# 强调色
primary: 主强调色
secondary: 次强调色
tertiary: 第三强调色
accent: 点缀色

# 功能色
border: 边框色
divider: 分隔线色
code_background: 代码背景色
card_background: 卡片背景色
```

### 色值对比度要求
- 文本与背景对比度 ≥ 4.5:1
- 强调文本与背景对比度 ≥ 7:1
- 遵循 WCAG AA 标准

---

## 版本信息

- **创建日期**：2026-03-22
- **版本**：1.0.0
- **维护者**：微信公众号文章写作项目组

---

## 更新日志

### v1.0.0 (2026-03-22)
- 初始版本
- 定义 14 种插图风格
- 添加风格选择参考表
- 添加 AI 匹配说明
