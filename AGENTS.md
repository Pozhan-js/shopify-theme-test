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

## Agent Skills
仓库内置项目级 skill：`.codex/skills/shopify-theme-best-practices/`。
当需求涉及 Shopify 主题开发、section/snippet/template 设计、主题代码 review、性能优化、无障碍、移动端优先、自定义配置设计、或“是否符合 Shopify 最佳实践”时，优先读取该 skill 再实施修改。
如果只是想让 skill 出现在 Codex 的全局 skill 列表中，仅放在仓库里还不够，还需要额外安装到用户级 skill 目录。


## 问题所在

来自 Andrej 的推文：

> "模型会代你做错误假设，然后不假思索地执行。它们不管理自身的困惑，不寻求澄清，不呈现矛盾，不展示权衡，在应该提出异议时也不反驳。"

> "它们真的很喜欢把代码和 API 搞复杂，堆砌抽象概念，不清理死代码……明明 100 行能搞定的事情，非要实现成 1000 行的臃肿架构。"

> "它们有时仍会改动或删除自己理解不足的代码和注释，即使这些内容与任务本身无关。"

## 解决方案

四个原则，集中在一个文件中，直接解决这些问题：

| 原则 | 解决什么问题 |
|-----------|-----------|
| **编码前思考** | 错误假设、隐藏困惑、缺少权衡 |
| **简洁优先** | 过度复杂、臃肿抽象 |
| **精准修改** | 无关编辑、触碰不应碰的代码 |
| **目标驱动执行** | 通过测试优先、可验证的成功标准 |

## 四个原则详解

### 1. 编码前思考

**不要假设。不要隐藏困惑。呈现权衡。**

LLM 经常默默选择一种解释然后执行。这个原则强制明确推理：

- **明确说明假设** — 如果不确定，询问而不是猜测
- **呈现多种解释** — 当存在歧义时，不要默默选择
- **适时提出异议** — 如果存在更简单的方法，说出来
- **困惑时停下来** — 指出不清楚的地方并要求澄清

### 2. 简洁优先

**用最少的代码解决问题。不要过度推测。**

对抗过度工程的倾向：

- 不要添加要求之外的功能
- 不要为一次性代码创建抽象
- 不要添加未要求的"灵活性"或"可配置性"
- 不要为不可能发生的场景做错误处理
- 如果 200 行代码可以写成 50 行，重写它

**检验标准：** 资深工程师会觉得这过于复杂吗？如果是，简化。

### 3. 精准修改

**只碰必须碰的。只清理自己造成的混乱。**

编辑现有代码时：

- 不要"改进"相邻的代码、注释或格式
- 不要重构没坏的东西
- 匹配现有风格，即使你更倾向于不同的写法
- 如果注意到无关的死代码，提一下 —— 不要删除它

当你的改动产生孤儿代码时：

- 删除因你的改动而变得无用的导入/变量/函数
- 不要删除预先存在的死代码，除非被要求

**检验标准：** 每一行修改都应该能直接追溯到用户的请求。

### 4. 目标驱动执行

**定义成功标准。循环验证直到达成。**

将指令式任务转化为可验证的目标：

| 不要这样做... | 转化为... |
|--------------|-----------------|
| "添加验证" | "为无效输入编写测试，然后让它们通过" |
| "修复 bug" | "编写重现 bug 的测试，然后让它通过" |
| "重构 X" | "确保重构前后测试都能通过" |

对于多步骤任务，说明一个简短的计划：

```
1. [步骤] → 验证: [检查]
2. [步骤] → 验证: [检查]
3. [步骤] → 验证: [检查]
```

强有力的成功标准让 LLM 能够独立循环执行。弱标准（"让它工作"）需要不断澄清。

## 安装

**选项 A：Claude Code 插件（推荐）**

在 Claude Code 中，首先添加插件市场：
```
/plugin marketplace add forrestchang/andrej-karpathy-skills
```

然后安装插件：
```
/plugin install andrej-karpathy-skills@karpathy-skills
```

这会将指南安装为 Claude Code 插件，使其在你所有项目中可用。

**选项 B：CLAUDE.md（按项目）**

新项目：
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```

已有项目（追加）：
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```

## 在 Cursor 中使用

本仓库包含一个已提交的 Cursor 项目规则 ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc))，因此在 Cursor 中打开项目时同样适用这些指南。详情请参见 **[CURSOR.md](CURSOR.md)**，包括如何在其他项目中使用该规则，以及它与 Claude Code 的关系。

## 核心洞察

来自 Andrej：

> "LLM 非常擅长循环执行直到达成特定目标……不要告诉它该做什么，给它成功标准，然后看着它完成。"

"目标驱动执行"原则正是捕捉了这一点：将指令式指令转化为带有验证循环的声明式目标。

## 如何判断它在起作用

如果你看到以下情况，说明这些指南正在发挥作用：

- **diff 中不必要的改动更少** —— 只有请求的改动出现
- **因过度复杂而导致的重写更少** —— 代码第一次就写得简洁
- **澄清问题在实现之前提出** —— 而不是在犯错之后
- **干净、精简的 PR** —— 没有顺带的重构或"改进"

## 定制

这些指南设计用于与项目特定指令合并。将它们添加到你现有的 `CLAUDE.md` 或创建一个新的。

对于项目特定规则，添加如下章节：

```markdown
## 项目特定指南

- 使用 TypeScript 严格模式
- 所有 API 端点必须有测试
- 遵循 `src/utils/errors.ts` 中现有的错误处理模式
```

## 权衡说明

这些指南倾向于**谨慎而非速度**。对于琐碎的任务（简单的拼写错误修复、显而易见的一行修改），请自行判断 —— 并非每个改动都需要完整的严谨流程。

目标是减少非琐碎工作中的代价高昂的错误，而不是拖慢简单任务。
