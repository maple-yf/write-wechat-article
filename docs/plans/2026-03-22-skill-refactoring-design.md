# write-wechat-article 技能重构设计文档

**日期：** 2026-03-22
**设计者：** Claude
**状态：** 已批准

---

## 一、概述

### 1.1 目标

将 `skills/write-wechat-article/SKILL.md` 迁移到项目根目录，并参考以下文件丰富标准和规范：

- WECHAT_TEMPLATE.md
- WECHAT_COMPATIBILITY_NOTES.md
- SOUL.md
- README_WECHAT_CONVERTER.md
- MEMORY.md
- convert_html_to_wechat.js

### 1.2 方案选择

**选择方案：方案三 - 混合模式**

- SKILL.md：核心技能定义 + 快速参考
- specs/ 目录：详细规范文档
- scripts/ 目录：工具脚本

### 1.3 目标读者

所有文档的目标读者均为 **Agent + 开发者**

---

## 二、项目结构

### 2.1 最终结构

```
write-wechat-article/
├── SKILL.md                      # 核心技能定义 + 快速参考
├── specs/
│   ├── FORMAT_SPEC.md            # 格式规范（HTML/CSS/设计）
│   └── PROCESS_GUIDE.md          # 流程指南（调研/创作/发布）
├── scripts/
│   ├── convert_html_to_wechat.js # HTML 转换工具
│   ├── package.json              # 依赖：juice, cheerio
│   └── README.md                 # 脚本使用说明
├── output/                       # 文章输出目录
├── tests/
│   ├── manual-test.md            # 手动测试用例
│   └── compatibility-checklist.md # 兼容性检查清单
├── README.md                     # 项目说明
└── docs/
    └── plans/
        └── 2026-03-22-skill-refactoring-design.md
```

### 2.2 文件职责

| 文件 | 职责 | 行数限制 |
|-----|------|---------|
| SKILL.md | 技能定义、触发条件、核心流程、快速参考 | 150-200 行 |
| FORMAT_SPEC.md | HTML 兼容性、CSS 属性、设计风格、配色 | - |
| PROCESS_GUIDE.md | 调研流程、创作流程、检验流程、发布流程 | - |
| scripts/*.js | HTML 转换工具、辅助脚本 | - |

---

## 三、SKILL.md 设计

### 3.1 结构

```markdown
---
name: write-wechat-article
description: Use when the user asks to...
---

# 微信公众号文章写作技能

## 快速参考
### 触发条件
### 输出产物
### 核心流程概览

## 技能模式
### 创作模式
### 修改模式

## 意图识别

## 关键规范速查
### 微信兼容性 ✅
### 主题色
### 内容要求
### 配图规范速查

## 详细规范
- 完整格式规范：[FORMAT_SPEC.md](specs/FORMAT_SPEC.md)
- 完整流程指南：[PROCESS_GUIDE.md](specs/PROCESS_GUIDE.md)
```

### 3.2 核心流程概览

```
1. 调研 → 2. 创作 → 3. 生成文案 → 4. 设计配图 → 5. 检验 → 6. 发布
```

### 3.3 创作模式流程

包含封面图创作和插图创作：
- **Step 4: 设计配图**
  - 封面图：cover-[slug].html（左侧矩形1175×500px + 右侧方形500×500px）
  - 插图：3-5张（comparison、ability、process）

---

## 四、FORMAT_SPEC.md 设计

### 4.1 章节结构

1. 主题色规范
2. HTML 兼容性规范（重要）
   - 标签使用
   - CSS 属性规范
   - 列表实现
   - 表格标题背景色（关键）
3. 标题样式
4. 设计风格规范
   - 配色体系（封面/插图）
   - 布局模式
   - 数据可视化
   - 开发者语境元素
5. 配图设计规范
   - 封面图布局
   - 文章配图类型
   - 插图数量
6. HTML 生成检查清单
7. 完整 HTML 示例

### 4.2 关键规范

**主题色：**
- 微信文章色：`rgb(0, 112, 192)` 或 `#0070c0`
- 科技蓝主题（封面/插图）：`#0EA5E9`、`#38BDF8`、`#0284C7`

**HTML 兼容性：**
- ✅ 使用 `<section>` 而非 `<div>`
- ✅ 使用 `<p>` 而非 `<ul>`/`<li>`
- ✅ 表格背景色应用到 `<th>` 标签
- ❌ 禁止 `<style>` 标签

**配图规范：**
- 短文章（<1500字）：1-2张
- 中等文章（1500-3000字）：3-4张
- 长文章（>3000字）：4-5张

---

## 五、PROCESS_GUIDE.md 设计

### 5.1 章节结构

1. 流程总览
2. 调研流程
   - 图片处理（如有）
   - 多渠道调研
   - URL 处理优先级
3. 创作流程
   - 交互式问答（分步骤）
   - 内容要求（强制）
4. 生成流程
   - 文档生成
   - 文件命名规范
5. 质量检验流程
   - 检查项目
   - 修改完善
6. 发布流程
   - 截图预览
   - 发送预览
   - 清理和关闭
7. 反馈修改流程
8. 输出目录规范
9. 工具使用

### 5.2 内容要求（强制）

**底线要求：**
- ✅ 正确性：事实准确，数据可靠
- ✅ 合法性：符合法律法规

**高阶追求（必须达成）：**
- 🎯 **思想有深度**：有洞察、有思考、有升华
- 🎯 **立意要高**：站在更高视角，挖掘本质
- 🎯 **观点要鲜明**：敢于表达立场，有独特见解

---

## 六、scripts 目录设计

### 6.1 结构

```
scripts/
├── convert_html_to_wechat.js    # HTML 转换工具
├── package.json                  # 依赖配置
└── README.md                     # 脚本使用说明
```

### 6.2 package.json

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

### 6.3 工作原理

1. **CSS Inlining**：将 `<style>` 标签转换为内联样式
2. **伪元素处理**：将 `::before` 转换为真实 HTML 元素
3. **属性过滤**：移除不支持的 CSS 属性

**样式保留率：** 92-95%

---

## 七、README.md 更新

### 7.1 主要内容

- 项目简介和特性
- 快速开始
- 项目结构
- 文档索引
- 输出产物说明
- 使用工具
- 兼容性规范
- 设计风格
- 参考资料

---

## 八、实施计划

### 8.1 实施步骤

```
Step 1: 创建新目录结构
  ├── specs/
  └── scripts/

Step 2: 编写 FORMAT_SPEC.md
  └── 整合 WECHAT_TEMPLATE.md 兼容性规范

Step 3: 编写 PROCESS_GUIDE.md
  └── 整合 MEMORY.md 工作流程

Step 4: 重写 SKILL.md
  └── 核心定义 + 快速参考

Step 5: 整合 scripts
  └── 复制 convert_html_to_wechat.js 和配置

Step 6: 更新项目 README.md
  └── 说明新结构

Step 7: 更新测试文件
  └── 同步兼容性检查清单
```

### 8.2 文件来源映射

| 新文件 | 来源文件 |
|-------|---------|
| SKILL.md | 原 SKILL.md（重构） |
| FORMAT_SPEC.md | WECHAT_TEMPLATE.md + WECHAT_COMPATIBILITY_NOTES.md |
| PROCESS_GUIDE.md | MEMORY.md + SOUL.md |
| scripts/* | 原有转换工具 |
| README.md | 更新说明 |

---

## 九、设计原则

1. **分层组织** - 快速参考（SKILL.md）与深度查阅分离
2. **职责清晰** - 格式规范与流程指南分开
3. **向后兼容** - 保持现有技能接口不变
4. **可扩展性** - 预留扩展空间
5. **完整整合** - 所有参考文件内容纳入新结构

---

## 十、批准状态

✅ 所有部分设计已完成并获得用户批准

- 第一部分：整体架构与文件结构
- 第二部分：SKILL.md 内容设计
- 第三部分：FORMAT_SPEC.md 内容设计
- 第四部分：PROCESS_GUIDE.md 内容设计
- 第五部分：scripts 目录设计
- 第六部分：实施计划
- 第七部分：README.md 更新设计

**下一步：** 调用 writing-plans skill 创建详细实施计划
