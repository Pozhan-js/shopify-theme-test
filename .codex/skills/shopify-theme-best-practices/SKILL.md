---
name: shopify-theme-best-practices
description: Guides Shopify theme development and review using Shopify official best practices plus this repository's conventions. Use when editing Liquid theme files, planning sections or snippets, reviewing theme code, or when the user asks about Shopify theme best practices, performance, accessibility, mobile-first design, customization strategy, or theme architecture.
---

# Shopify Theme Best Practices

使用这个 skill 时，把 Shopify 官方主题最佳实践和当前仓库约束一起考虑，不要只从“能跑”出发写主题代码。

## 快速判断

1. 先定位改动在哪一层：`layout/`、`templates/`、`sections/`、`snippets/`、`assets/`、`config/`、`locales/`。
2. 再判断这次需求主要在解决什么：架构拆分、性能、无障碍、设计、移动端、运营可配置性。
3. 自定义功能优先落在项目专属 section/snippet，不要无边界地继续堆进公共片段。

## 核心原则

### 模块化，但不过度碎片化

- 页面结构优先用 templates、sections、blocks 表达。
- 一个 section 应围绕一个明确业务目标，schema 名称对运营可读。
- 只有在多个地方真正复用时才抽 snippet。
- 不要把过多职责塞进单一 section，也不要把简单结构拆成太多小块。

### 性能优先

- 默认少用 JavaScript，优先原生浏览器能力和 CSS。
- 非关键资源延后加载，避免重复注入脚本和样式。
- 改 `assets/` 时优先复用现有资源，避免引入体积大但收益小的新依赖。
- 每次都先问一句：不用 JS 能不能完成。

### 无障碍与可发现性

- 优先语义化 HTML。
- 可点击控件必须可键盘操作。
- 图片、图标、按钮、表单都要有可理解名称。
- 不要滥加 ARIA；原生语义足够时优先原生语义。

### 设计与移动端优先

- 先按移动端布局思考，再扩展到桌面端。
- 保证信息层级清晰、触控区域足够、间距一致。
- Theme 默认方案应服务当前店铺/品类的核心转化路径，而不是做成通用大杂烩。

### 配置简单可用

- 给商家提供真正有价值的配置项，不要堆很多难理解开关。
- schema 默认值应尽量开箱可用，字段命名贴近运营语言。
- 品牌表达类配置优先做“少而明确”的控制项。

## 仓库约束

- 保持 2 空格缩进。
- 文件名使用 kebab-case。
- section 相关样式优先带 `section.id` 作用域，减少全局污染。
- 不保留 `DEBUG` 文案、测试样式、店铺专属临时逻辑。
- 修改 `config/settings_data.json` 前先提醒备份。
- 修改 `locales/` 时同步检查默认文案和 `zh-CN.json`。

## 输出要求

给出方案或完成修改时，尽量明确：

1. 改动落在哪些主题层级和文件。
2. 这次改动主要满足了哪些最佳实践。
3. 是否有性能、无障碍、移动端或可配置性的残余风险。
4. 应做哪些验证。

## 验证

- 至少执行 `shopify theme check`。
- 涉及结构或样式时，手动验证首页、商品页、集合页、购物车页。
- 同时验证主题编辑器里的相关配置是否可保存、可预览。

需要更细的原则映射、实施建议和 review 清单时，再读 [REFERENCE.md](REFERENCE.md)。
