# Lumina Novel Generator

> 面向 **Lumina 画布 Agent** 的原创中文短篇小说生成 Skill：先锁定读者承诺、经典桥段功能、人物欲望、冲突引擎和大纲，再生成完整、强钩子、高张力、低 AI 味的故事。
>
> A fiction-writing skill adapted for **Lumina Canvas Agent**. It designs the reader promise, reusable story functions, character desire, conflict engines, and outline before drafting a complete, gripping, original Chinese short story.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`lumina-novel-generator` 严格继承 [qiaomu-novel-generator](https://github.com/joeseesun/qiaomu-novel-generator) 的核心写作体系，并针对 Lumina 画布 Agent 的运行方式完成适配。它不会收到一个模糊题材就马上输出套路正文，而是先完成一套可确认的剧情设计：

```text
情绪承诺 → 高压关系 → 冲突场 → 经典桥段功能化 → 剧情引擎 → 升级节奏 → 大纲确认 → 完整正文 → 去 AI 味与质量自检
```

它尤其适合：

- 根据一句主题、人物设定、梗概或已有片段创作完整中文短篇；
- 先给多个剧情方向，让用户用 `按默认`、`1B 2A 3C` 等方式快速确认；
- 将电影、电视剧、小说和类型套路拆成可复用的叙事功能，再重构为原创故事；
- 强化开篇钩子、人物欲望、冲突升级、对白张力、反转和结尾余味；
- 改写带有总结腔、教学腔和公式化 `不是 X，而是 Y` 的 AI 味文本；
- 修复为了诗性而让读者误判人物生死、现实规则或故事类型的开篇。

### Lumina 适配内容

Lumina Agent 通过 **System Instructions、Task Prompt 和已连接的组件工具**运行。本仓库因此提供两个入口：

- [`LUMINA_SYSTEM_INSTRUCTIONS.md`](./LUMINA_SYSTEM_INSTRUCTIONS.md)：已经把运行时必需规则编译成单文件，可直接粘贴到 Lumina Agent 节点的 System Instructions。
- [`SKILL.md`](./SKILL.md)：完整、可维护的 Skill 入口，配合 `references/`、`examples/` 和 `scripts/` 用于版本维护、审查与本地 Agent Skills 兼容。

适配版还增加了 Lumina 专属约束：

- Agent 只能调用画布上实际连接的组件，不能假装联网、保存或执行工作流；
- 只有连接了公开资料搜索组件时，才执行“先搜索再重构”；
- 选项卡和大纲是一次运行的停止点，不会虚构用户确认；
- 如果画布不保留上次运行状态，下一次 Task Prompt 需要带回用户选择和上一版大纲；
- Python 校验脚本仅用于仓库开发，不会被误当成 Lumina 运行时工具。

### 在 Lumina 画布中使用

1. 新建一个 Agent 节点。
2. 将 [`LUMINA_SYSTEM_INSTRUCTIONS.md`](./LUMINA_SYSTEM_INSTRUCTIONS.md) 的完整内容粘贴到 **System Instructions**。
3. 在 **Task Prompt** 中写用户请求；如果请求来自 String 节点，用 Lumina 的 `@` 语法引用该输入。
4. 将 Agent 的文本输出连接到 Show Text 或下游文本组件。
5. 可选：连接一个公开资料搜索组件，并在工具描述中明确参数、必填项和返回内容。没有连接时，Skill 会诚实回退到通用叙事分析。
6. 首次运行若输出选项或大纲，在下一次运行中传回选择，例如：

```text
按建议写。沿用下面的大纲，直接生成 2500–3000 字完整短篇：
@previous_plan
```

一个最小 Task Prompt 示例：

```text
@story_request

如果信息不足，先给紧凑选项，不要直接写正文；如果我已经明确说“直接写”，就在内部完成大纲后输出完整故事。
```

### 可以这样提问

```text
写一个不会武功的账房先生误入江湖死局的完整短篇，先给方向。
```

```text
把“跌落神坛后公开翻身”的结构改造成修仙故事，先给经典桥段启发和大纲。
```

```text
这段 AI 味太重，尤其是“不是……而是……”，保留剧情信息，改得像人写的。
```

```text
这个开篇让我误以为是鬼怪故事。保留危险感，但在前两句说清楚实际发生了什么。
```

### 核心质量门槛

- 前三段出现危险、羞辱、损失、谜题或不可逆扰动；
- 主角有一个立刻可见的欲望和一个隐藏伤口；
- 冲突至少升级三次，并持续关闭安全选项；
- 对白包含威胁、试探、隐瞒、交换、指控或悲伤；
- 具体人物、动作、物件和场景承担信息，不靠设定说明；
- 早期细节在后文改变意义，结尾回收开篇意象；
- 经典作品只作为功能信号，不复制人物、名场面、台词、世界观或连续情节链；
- 不直接模仿在世作者的独特文风，只转译为可泛化技法；
- 删除公式化对比、总结式结尾、教学腔和装饰性破折号；
- 开篇的物理事实与类型承诺清晰，不用误导换取钩子。

### 本地安装与验证

兼容 Agent Skills 的本地工具可直接安装：

```bash
npx skills add XianlinLu/novel-generator
```

仓库开发校验：

```bash
python3 scripts/validate_skill.py
python3 scripts/evaluate_story.py examples/sample-01-wuxia-suspense.md --fail-on-warning
python3 scripts/evaluate_story.py examples/sample-02-sci-fi-memory.md --fail-on-warning
```

## English Introduction

`lumina-novel-generator` faithfully carries over the core writing system from [qiaomu-novel-generator](https://github.com/joeseesun/qiaomu-novel-generator) and adapts it to Lumina's Canvas Agent runtime. Instead of jumping from a vague idea to generic prose, it follows a confirmable story-design pipeline:

```text
emotional promise → high-pressure relationship → conflict arena → functionalized classic beats → plot engines → escalation → outline confirmation → complete story → anti-AI and quality gates
```

The skill is designed to:

- create complete Chinese short fiction from a theme, character setup, synopsis, or draft;
- provide compact options before drafting so the user can answer with `use defaults` or selections such as `1B 2A 3C`;
- translate films, series, novels, genres, and author signals into reusable narrative functions without copying protected expression;
- strengthen opening hooks, character desire, escalation, dialogue pressure, reversals, imagery, and ending resonance;
- remove formulaic AI phrasing, teaching-tone transitions, decorative dashes, and summary endings;
- repair poetic openings that accidentally mislead readers about literal events or genre.

### What was adapted for Lumina

Lumina Agents run from **System Instructions**, a **Task Prompt**, and connected component tools. This repository therefore contains two entrypoints:

- [`LUMINA_SYSTEM_INSTRUCTIONS.md`](./LUMINA_SYSTEM_INSTRUCTIONS.md) is the self-contained runtime prompt to paste into a Lumina Agent node.
- [`SKILL.md`](./SKILL.md) is the maintainable package entrypoint, supported by `references/`, `examples/`, and repository-side validation scripts.

The Lumina version adds explicit runtime safeguards:

- the Agent may use only components actually connected on the canvas;
- source research runs only when a public-research component is connected;
- an option card or outline ends the current execution—the Agent never fabricates user approval;
- when state is not preserved, the next Task Prompt must include both the user's choice and the previous plan;
- Python validators are development tools, not runtime capabilities.

### Use it in Lumina Canvas

1. Create an Agent node.
2. Paste the complete contents of [`LUMINA_SYSTEM_INSTRUCTIONS.md`](./LUMINA_SYSTEM_INSTRUCTIONS.md) into **System Instructions**.
3. Put the user's request in **Task Prompt**. Use Lumina's `@` syntax when the request comes from a connected String input.
4. Connect the Agent text output to Show Text or another text-consuming component.
5. Optionally connect a public-source research component and describe its required parameters and returned evidence. Without one, the Agent falls back transparently to general craft analysis.
6. If the first run returns choices or an outline, pass the selection and previous plan back in the next run.

Example follow-up Task Prompt:

```text
Use the recommended direction. Keep the plan below and write a complete 2,500–3,000 Chinese-character story now:
@previous_plan
```

### Local installation and validation

For local tools that support Agent Skills:

```bash
npx skills add XianlinLu/novel-generator
```

For repository development:

```bash
python3 scripts/validate_skill.py
python3 scripts/evaluate_story.py examples/sample-01-wuxia-suspense.md --fail-on-warning
python3 scripts/evaluate_story.py examples/sample-02-sci-fi-memory.md --fail-on-warning
```

## Package Structure

```text
LUMINA_SYSTEM_INSTRUCTIONS.md  # Paste into Lumina Agent System Instructions
SKILL.md                       # Maintainable Agent Skill entrypoint
agents/                        # Interface metadata
references/                    # Story engines, remix, output, and quality rules
examples/                      # Full sample stories and visible self-evaluations
scripts/                       # Repository-side validators
manifest.json                  # Package resource map and quality gates
```

## Attribution and License

This adaptation is based on [`joeseesun/qiaomu-novel-generator`](https://github.com/joeseesun/qiaomu-novel-generator), created by 向阳乔木 / joeseesun, and is distributed under the MIT License. The original copyright notice is preserved in [`LICENSE`](./LICENSE).

Lumina product behavior referenced by this adaptation includes Canvas Agent System Instructions, Task Prompt `@` references, connected tools/components, multimodal inputs, and iteration-controlled step execution. See the [Lumina Canvas guide](https://seedancelumina.com/guide) for the current interface.
