# Repository Guidelines

## Project Structure & Module Organization
这是一个 Shopify Online Store 2.0 主题导出仓库。`layout/` 放全局骨架，核心入口是 `layout/theme.liquid`；`templates/` 定义页面模板；`sections/` 放可组合页面区块；`snippets/` 放可复用片段；`assets/` 放 CSS、JavaScript、SVG 与图片资源；`config/` 保存主题 schema 和运行中的主题配置；`locales/` 管理多语言文案。自定义功能优先落在 `sections/custom-table.liquid`、`sections/my_custom_section.liquid` 这类项目专属文件，避免直接堆进公共片段。

## Build, Test, and Development Commands
本仓库未包含 `package.json` 或 Makefile，默认走 Shopify CLI 工作流：

- `shopify theme dev`：本地预览主题并热更新。
- `shopify theme check`：执行 Liquid/主题规范检查。
- `shopify theme push`：把本地修改推送到开发主题。
- `shopify theme pull`：同步远端主题变更，覆盖前先确认差异。

如果本机未安装 CLI，先安装 Shopify CLI，再在仓库根目录运行上述命令。

## Coding Style & Naming Conventions
保持与现有主题风格一致：Liquid、JSON、CSS、JS 统一用 2 空格缩进。Section、snippet、asset 文件名使用 kebab-case，例如 `main-product.liquid`、`component-card.css`。新增 section 时，schema `name` 要面向运营可读，CSS 类名建议带 `section.id` 作用域，避免污染全局。不要在提交中保留 `DEBUG` 文案或明显的测试样式。

## Testing Guidelines
当前没有自动化测试目录，至少执行 `shopify theme check`。涉及页面结构或样式时，在 `shopify theme dev` 下手动验证首页、商品页、集合页、购物车页，以及对应的主题编辑器配置是否可保存。模板 JSON 建议用页面用途命名，如 `page.contact.json`、`page.normal.json`。

## Commit & Pull Request Guidelines
该导出包不含 `.git` 历史，无法从本地仓库提取既有提交规范。建议使用简短祈使句并带作用域，例如 `feat: add custom size table section`、`fix: correct cart drawer spacing`。PR 应说明修改的模板或 section、关联需求、主题编辑器操作步骤；涉及 UI 时附前后截图，并注明是否需要同步 `config/settings_data.json`。

## Security & Configuration Tips
`config/settings_data.json` 可能包含当前店铺配置，修改前先备份。避免提交店铺专属密钥、临时调试内容或无关的语言包改动；改 `locales/` 时同步检查 `zh-CN.json` 与默认文案是否一致。
