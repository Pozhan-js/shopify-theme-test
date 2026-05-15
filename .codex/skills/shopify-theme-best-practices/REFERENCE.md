# Shopify Theme Best Practices Reference

本参考文档基于 Shopify 官方文档 `Best practices for building Shopify themes` 整理，并映射到当前仓库的主题开发场景。

官方来源：

- HTML: `https://shopify.dev/docs/storefronts/themes/best-practices`
- Markdown: `https://shopify.dev/docs/storefronts/themes/best-practices.md`

## 一、官方核心原则

### Be performant

- 主题默认应追求更少的 JavaScript、更低的资源负担和更稳定的交互。
- 新功能先考虑浏览器原生能力、CSS、Liquid 服务端输出，最后才是前端脚本补强。

### Be purpose-built

- 主题不应追求“什么都支持一点”，而应围绕具体商家或品类做有倾向性的默认设计。
- 设计和代码结构都应服务转化目标，而不是堆叠炫技功能。

### Offer best-in-class UX

- 页面层级、内容组织、交互反馈、状态切换都应以顾客理解成本最低为目标。
- 商家配置体验也属于 UX，主题编辑器中的字段组织和命名同样重要。

### Be mobile first

- 先考虑窄屏信息层级、触控命中区域、滚动路径，再扩展到桌面端。
- 任何“桌面端很好看”的实现，都要回到移动端重新检查可读性和点击路径。

### Be accessible

- 语义、焦点、可读文本、颜色对比、替代文本、表单标签都不是补丁项，而是基线要求。

### Make customization simple

- 商家需要的是少量但清晰的可控项，而不是几十个难理解的开关。
- 默认值和字段文案应让商家不看说明也能大致理解用途。

## 二、对本仓库的落地方式

### 文件归位建议

- 全局骨架改动：`layout/theme.liquid`
- 页面模板选择：`templates/`
- 可配置页面区块：`sections/`
- 局部复用片段：`snippets/`
- 样式与交互资源：`assets/`
- 主题编辑器 schema / 运行配置：`config/`
- 文案：`locales/`

### 结构拆分建议

- 如果功能主要服务单个页面区域，并需要商家控制内容或展示形式，优先做 section。
- 如果一个 section 内存在可重复的小单元，优先考虑 block，而不是硬编码多个平级字段。
- 只有跨 section 或跨模板重复出现的结构才抽成 snippet。

### 性能审查点

- 是否新增了本可避免的 JS。
- 是否把非首屏资源变成了首屏阻塞。
- 是否在多个 section/snippet 中重复引用同一资源。
- 是否为很小的收益引入很重的第三方库。

### 无障碍审查点

- 是否存在只靠颜色传达状态的设计。
- 是否存在无标签输入、无替代文本图片、无可读名称按钮。
- 是否能只用键盘完成关键操作。

### 主题编辑器审查点

- schema 名称是否面向运营可读。
- 配置项是否过多、过碎、过难理解。
- 默认值是否能形成“开箱可用”的效果。

## 三、推荐的执行顺序

1. 先确认这次改动服务的页面、商家目标和顾客路径。
2. 再决定功能落在哪个主题层级。
3. 先写移动端和基础语义结构，再补桌面端与增强交互。
4. 补 schema 配置项时，只保留真实有用的控制项。
5. 最后跑 `shopify theme check`，并做页面和主题编辑器手动验证。

## 四、适合这个 skill 的典型请求

- “帮我做一个可配置的商品卖点 section”
- “这个 Shopify section 结构是不是太重了”
- “帮我按最佳实践 review 这个主题改动”
- “这个需求应该放 section、block 还是 snippet”
- “帮我优化主题的移动端和性能”
