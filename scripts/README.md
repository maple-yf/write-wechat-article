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
