---
name: write-wechat-article
description: Use when the user asks to "写一篇公众号文章", "创作公众号内容", "写公众号文章", "生成微信公众号文章", "优化这篇文章", "润色文章", "给文章起标题", or mentions WeChat public account (微信公众号), WeChat article formatting, WeChat-compatible HTML, article cover generation, or wants to create content for WeChat public account. Make sure to use this skill whenever the user mentions WeChat, 公众号, article writing, content creation, or needs help with article formatting or title generation.
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
- 使用 deep-research / agent-reach / browser-use skills
- 详见：[PROCESS_GUIDE.md#调研流程](specs/PROCESS_GUIDE.md#二调研流程)

**Step 2: 文案创作**
- 交互式问答确定方向和内容
- 详见：[PROCESS_GUIDE.md#创作流程](specs/PROCESS_GUIDE.md#三创作流程)

**Step 3: 生成文档**
- 生成 Markdown 文件
- 生成微信公众号 HTML
- 详见：[FORMAT_SPEC.md](specs/FORMAT_SPEC.md)

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

### 插图风格

- 14 种可用风格，详见 [STYLE_PALETTE.md](specs/STYLE_PALETTE.md)
- 风格匹配：AI 根据文章内容自动推荐合适风格
- 插图类型：comparison（对比）、ability（能力）、process（流程）

详见：[STYLE_PALETTE.md](specs/STYLE_PALETTE.md#插图风格)

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
