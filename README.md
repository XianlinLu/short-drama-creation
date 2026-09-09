# Novel Generator

> 一个专注于原创中文小说策划、生成与改写的 Agent Skill。它先确定读者承诺、人物欲望、冲突引擎和故事大纲，再写出完整、强钩子、高张力、低 AI 味的小说。
>
> An Agent Skill for planning, drafting, and revising original Chinese fiction. It establishes the reader promise, character desire, conflict engines, and story outline before producing a complete, gripping, low-AI-smell story.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`Novel Generator` 把一句灵感、人物设定、故事梗概、类型偏好或已有片段，转化为可以真正吸引读者的原创小说。它不会收到一个宽泛题材就直接输出套路正文，而是先完成一套可确认的剧情设计：

```text
情绪承诺 → 高压关系 → 冲突场 → 叙事功能组合 → 剧情引擎 → 升级节奏 → 大纲确认 → 完整正文 → 语言与质量自检
```

**原创是这套 Skill 的核心作用。** 它只提取可泛化的叙事功能，不搬运现成人物、名场面、台词、世界观或连续情节。每次创作都会重新设计角色、关系、场景、利益与代价、核心物件或规则以及结局，让最终作品成为能够独立成立的新故事。

### 它解决什么问题

普通小说生成经常遇到这些问题：

- 开头缺少钩子，读者很快失去兴趣；
- 主角没有明确欲望，只能被剧情推着行动；
- 冲突只是重复发生，没有真正升级；
- 参考经典作品时容易变成换皮或情节搬运；
- 指定作者风格时只学到表面腔调；
- 语言充满总结腔、教学腔和公式化对比；
- 结尾解释主题，却没有画面和余味。

这套 Skill 会先找到故事最重要的读者快感和人物压力，再决定用什么关系、场景、物件、规则与反转承载它。

### 核心能力

#### 从一句想法到完整故事

支持从主题、人物设定、梗概、类型、桥段信号或已有片段开始，完成故事方向、结构设计、正文生成和后续改写。

#### 写前选择与确认

当输入不够明确时，先提供紧凑选项。用户可以回复：

```text
按默认
```

或：

```text
1B 2A 3C
```

当设定已经可用时，先输出“故事如何吸引人”和大纲，确认后再写正文。用户明确要求直接写时，会在内部完成规划后直接成文。

#### 原创重构

电影、电视剧、小说、游戏或类型套路只会被拆解成高层叙事功能，例如：

- 跌落神坛与公开回归；
- 被低估后的证据翻盘；
- 错认身份与迟来承认；
- 小人物被大型系统挤压；
- 多方利益冲突与公开问责；
- 线索返还与前提反转。

重构时会重新设计设定、职业或力量体系、人物关系、核心物件、风险、代价和结局，不复制原作的表达与连续情节。

#### 强化小说吸引力

- 前三段制造危险、羞辱、损失、谜题或不可逆扰动；
- 给主角一个立刻可见的欲望和一个隐藏伤口；
- 让冲突至少升级三次，并逐步关闭安全选项；
- 让对白承担威胁、试探、隐瞒、交换、指控或悲伤；
- 用具体人物、动作、物件和场景承载信息；
- 让早期细节在后文改变意义；
- 解决当前冲突，同时留下结尾回响。

#### 降低 AI 味

生成或改写时会主动检查：

- `不是 X，而是 Y`；
- `关键在于`、`值得注意的是`；
- `总之`、`综上所述`；
- `这不仅是……更是……`；
- 装饰性破折号；
- 解释主题的总结式结尾。

这些表达会优先改写为动作、物件、场景、对白和后果。

#### 修复误导性开篇

强钩子不能让读者误判基本事实或故事类型。如果一句话听起来像尸体复活、梦境或超自然现象，而故事本身并非如此，Skill 会在同一句或下一句补上清楚的现实锚点。

### 支持的故事方向

- 爽文、逆袭、打脸与复仇；
- 武侠、江湖、修仙与奇幻；
- 悬疑、推理、惊悚与黑色反转；
- 甜宠、虐恋、追妻与女性成长；
- 科幻、概念故事与记忆题材；
- 职场、商业、组织内斗与制度讽刺；
- 现实情感、生存压力与小人物故事。

### 使用示例

```text
写一个不会武功的账房先生误入江湖死局的完整短篇，先给几个方向。
```

```text
写一个从行业顶峰跌落、在底层重新理解手艺并公开翻身的原创修仙故事，先给大纲。
```

```text
写一个近未来记忆交易故事。主角卖掉了最无用的一天，却发现那一天能救他的女儿。
```

```text
这段 AI 味太重，尤其是“不是……而是……”，保留剧情信息，改得像人写的。
```

```text
这个开篇让我误以为是鬼怪故事。保留危险感，但在前两句说清楚实际发生了什么。
```

### 安装

在支持 Agent Skills 的工具中安装：

```bash
npx skills add XianlinLu/novel-generator
```

### 导入格式

仓库中的可导入文档遵循以下规则：

- 文件扩展名使用小写 `.md`、`.txt`、`.json`、`.yaml` 或 `.yml`；
- 文件和目录名只包含英文字母、数字、下划线和连字符；
- 文件和目录名不超过 64 个字符；
- 导入内容不包含脚本、图片、二进制文件或缓存文件。

详细检查规则见 [`references/repository-validation.md`](./references/repository-validation.md)。

## English Introduction

`Novel Generator` turns a rough idea, character setup, synopsis, genre preference, narrative signal, or existing excerpt into original Chinese fiction designed to hold a reader's attention. It does not jump from a broad topic to generic prose. It follows a confirmable story-design pipeline:

```text
emotional promise → high-pressure relationship → conflict arena → narrative functions → plot engines → escalation → outline confirmation → complete story → language and quality gates
```

**Originality is the skill's central purpose.** It extracts only reusable narrative functions and never carries over existing characters, famous scenes, dialogue, fictional worlds, or recognizable plot chains. Every story redesigns its characters, relationships, setting, stakes, central object or rule, and ending so the result stands independently.

### What it does

- turns a short idea into a complete Chinese story;
- presents compact choices when the premise needs direction;
- builds an attraction strategy and outline before drafting;
- translates familiar narrative signals into reusable functions without copying protected expression;
- strengthens openings, character desire, escalation, dialogue pressure, imagery, reversals, and ending resonance;
- revises existing drafts at the story-engine level instead of merely changing adjectives;
- removes formulaic AI phrasing and misleading poetic openings.

### Originality by design

References are treated as craft signals rather than copy targets. The skill may reuse broad functions such as downfall and comeback, public proof, mistaken identity, institutional pressure, or clue reversal, but it rebuilds the following elements:

- setting and profession or power system;
- character identities and pressure relationships;
- central object, evidence, or governing rule;
- stakes, costs, and moral choices;
- escalation path and final resolution.

It does not reproduce protected passages, signature dialogue, famous scenes, unique characters, iconic objects, or a recognizable sequence of events. Requests involving a living author's distinctive style are translated into general craft features.

### Quality standards

- an immediate disturbance appears within the first three paragraphs;
- the protagonist has a visible desire and a private wound;
- conflict escalates at least three times through choices and consequences;
- dialogue carries threat, testing, accusation, bargaining, concealment, or grief;
- concrete objects, actions, people, and places carry information;
- earlier details return with changed meaning;
- the current conflict resolves while the ending leaves emotional resonance;
- formulaic contrast, teaching-tone transitions, decorative dashes, and summary endings are removed;
- the opening remains literally clear and does not create an unintended genre promise.

### Example requests

```text
Write a complete wuxia suspense story about an untrained accountant trapped in a deadly jianghu misunderstanding. Give me several directions first.
```

```text
Write an original cultivation story about a fallen master who relearns the meaning of the craft at the bottom and returns in a public trial. Show me the outline first.
```

```text
Write a near-future memory-market story in Chinese. A father sells his most useless day and discovers that it contains the only way to save his daughter.
```

### Installation

For tools that support Agent Skills:

```bash
npx skills add XianlinLu/novel-generator
```

### Import format

All importable documents in this repository follow these rules:

- lowercase `.md`, `.txt`, `.json`, `.yaml`, or `.yml` extensions only;
- file and folder names contain only letters, numbers, underscores, and hyphens;
- every file and folder name stays within 64 characters;
- the import content contains no scripts, images, binaries, or cache files.

See [`references/repository-validation.md`](./references/repository-validation.md) for the complete checklist.

## Package Structure

```text
SKILL.md                       # Skill entrypoint
agents/                        # Interface metadata
references/                    # Story engines, remix, output, and quality rules
examples/                      # Full sample stories and self-evaluations
manifest.json                  # Resource map and quality gates
LICENSE.md                     # License
```

## License

Distributed under the MIT License. See [`LICENSE.md`](./LICENSE.md).
