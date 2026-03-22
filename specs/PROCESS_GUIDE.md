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