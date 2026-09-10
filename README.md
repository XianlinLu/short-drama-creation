# Novel Generator

> 一个能够自动匹配用户语言的原创叙事 Agent Skill。它既能完成小说策划、生成与改写，也能把人物三视图转化为交互式选题、分镜图、分镜视频、原创背景音乐和约一分钟的最终成片。
>
> An original-narrative Agent Skill that automatically matches the user's language. It can plan, draft, and revise fiction, or turn a character turnaround into an interactive topic choice, storyboard images, shot videos, original background music, and an approximately one-minute final film.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`Novel Generator` 把一句灵感、人物设定、故事梗概、类型偏好或已有片段，转化为可以真正吸引读者的原创小说。它会自动识别用户最新请求的语言，并让所有可见规划步骤与回答保持同一种语言。它不会收到一个宽泛题材就直接输出套路正文，而是先完成一套可确认的剧情设计：

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

#### 自动匹配用户语言

- 用户用英文提问，所有可见规划、选项、大纲、状态、错误、自检和回答都使用英文；
- 用户用日文提问，所有可见过程和回答都使用日文；
- 用户使用中文或其他语言时遵循相同规则；
- 语言识别以用户最新的直接请求为准，不会被附件、引用文本、粘贴稿件或工具结果带偏；
- 交流语言和小说正文语言可以不同，例如“用英文解释大纲，但用日文写小说”；
- 固定模板会动态本地化，不会在英文或日文回答中残留中文标题。

模型的私有推理不会展示；画面上实际可见的思考摘要或分步进度会严格使用识别到的用户语言。详细规则见 [`references/language-routing.md`](./references/language-routing.md)。

#### 人物三视图到一分钟成片 Demo

当用户上传人物角色三视图并提出分镜、视频 Demo 或一分钟短片需求时，Skill 会切换到独立的视觉生产模式。它不会立刻消耗生成资源，而是先读取角色的稳定外观锚点，再调用 Agent 的原生单选提问能力，以交互卡形式呈现四个原创选题方向。卡片包含短标题、选项名称、简短说明、推荐项和自定义输入；用户提交后才进入生成阶段。

用户选定方向后，默认流程是：

```text
人物三视图 → 单选选题卡 → 用户选择 → 角色一致性锁定 → 6 张分镜图 → 6 段约 10 秒视频 → 原创背景音乐 → 前 30 秒 + 后 30 秒 → 约 60 秒成片
```

默认的六镜时间线：

| 镜头 | 时间 | 叙事作用 |
| --- | --- | --- |
| 01 | 00:00–00:10 | 视觉钩子与场景规则 |
| 02 | 00:10–00:20 | 角色目标与第一阻碍 |
| 03 | 00:20–00:30 | 冲突升级与上半段转折 |
| 04 | 00:30–00:40 | 发现或反转 |
| 05 | 00:40–00:50 | 有代价的选择与高潮动作 |
| 06 | 00:50–01:00 | 回报与首尾呼应 |

这不是把 30 秒当作一次视频生成。Skill 会先读取已连接视频组件的真实时长限制；支持 10 秒时采用 `6 × 10 秒`，不支持时则改用组件能够接受的镜头时长，让上下两段都尽量接近 30 秒，并把最终成片控制在约 58–62 秒。

完整 Demo 需要连接以下能力：

- 人物参考图或多模态输入；
- 支持参考图的图片生成；
- 图生视频或首尾帧视频生成；
- 原创音乐生成；
- 支持视频顺序拼接和音轨混合的合成组件。

第二轮至少包含六次图片生成、六次视频生成、一次音乐生成和一次以上合成调用，因此应给 Agent 留出足够的执行步数。若运行环境提供最大迭代次数设置，建议从 `28` 或更高开始；当上下两段与最终成片需要分三次合成时可继续上调。

背景音乐默认自动生成：优先生成一条约 58–62 秒的原创纯音乐，并在 30 秒处配合剧情转折；组件不支持该时长时，改为生成两条风格、速度和调性兼容的约 30 秒音乐。配音、对白、音效、字幕、预览和保存属于可选能力。缺少音乐或混音能力时会交付视觉版并明确标记，不会假装完整音乐成片已经生成。

当配音因音频风险审核拒绝某个分块时，只定位并安全改写该分块的台词，保留故事含义、人物意图、节奏及已经成功的音频和媒体结果。系统只重试失败的 TTS 步骤；文字已经中性时再更换可用音色，确认音频成功后才继续视频生成。

可直接用于演示的首轮请求：

```text
请读取我上传的人物三视图，用单选交互卡给我 4 个适合这个角色的一分钟原创短片选题。现在不要生成媒体；我提交选择后，再生成分镜、视频、原创背景音乐和最终成片。
```

用户第二轮只需回复选题编号。选择即启动六张分镜图、六段短视频和最终拼接流程；如果当前 Agent 不保存上下文，应把选题卡与人物三视图一并传回。详细执行规则见 [`references/character-video-demo.md`](./references/character-video-demo.md)。

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
- 每个文档不超过 20,000 个字符；
- 文件和目录名只包含英文字母、数字、下划线和连字符；
- 文件和目录名不超过 64 个字符；
- 导入内容不包含脚本、图片、二进制文件或缓存文件。

详细检查规则见 [`references/repository-validation.md`](./references/repository-validation.md)。

## English Introduction

`Novel Generator` turns a rough idea, character setup, synopsis, genre preference, narrative signal, or existing excerpt into original fiction designed to hold a reader's attention. It detects the language of the user's latest direct request and keeps every visible planning step and response in that language. It does not jump from a broad topic to generic prose. It follows a confirmable story-design pipeline:

```text
emotional promise → high-pressure relationship → conflict arena → narrative functions → plot engines → escalation → outline confirmation → complete story → language and quality gates
```

**Originality is the skill's central purpose.** It extracts only reusable narrative functions and never carries over existing characters, famous scenes, dialogue, fictional worlds, or recognizable plot chains. Every story redesigns its characters, relationships, setting, stakes, central object or rule, and ending so the result stands independently.

### What it does

- turns a short idea into a complete story in the selected language;
- automatically matches the user's language across visible planning, options, outlines, status, errors, self-checks, and replies;
- presents compact choices when the premise needs direction;
- builds an attraction strategy and outline before drafting;
- translates familiar narrative signals into reusable functions without copying protected expression;
- strengthens openings, character desire, escalation, dialogue pressure, imagery, reversals, and ending resonance;
- revises existing drafts at the story-engine level instead of merely changing adjectives;
- removes formulaic AI phrasing and misleading poetic openings.

### Automatic language matching

- English requests produce an English visible workflow and response.
- Japanese requests produce a Japanese visible workflow and response.
- Chinese and other languages follow the same rule.
- Detection uses the latest direct user request, not language found only in attachments, quotations, pasted drafts, or tool output.
- The interaction language and story language are selected separately, so a user may request an English explanation and a Japanese story.
- Fixed templates are localized dynamically instead of leaking Chinese headings into other-language output.

Private chain-of-thought is never exposed. Any concise reasoning summary or step-by-step progress that is actually visible uses the detected interaction language. See [`references/language-routing.md`](./references/language-routing.md) for the complete routing rules.

### Character turnaround to one-minute film demo

When the user supplies a character turnaround and requests storyboards, a video demo, or a one-minute short film, the skill enters a dedicated visual-production mode. It extracts stable visible character anchors, then uses the Agent's native single-choice input to show four original directions as an interactive card with concise labels, descriptions, a recommended option, and custom input. Media generation starts only after submission.

After selection, the default pipeline is:

```text
character turnaround → interactive topic card → user choice → character lock → six storyboard images → six approximately 10-second videos → original background music → 30-second act A + 30-second act B → approximately 60-second final film
```

The six shots cover the hook, goal, escalation, reversal, climax choice, and closing echo. The skill does not assume that one video call can produce 30 seconds. It reads the connected component's real duration limits, uses `6 × 10 seconds` when supported, and otherwise rebuilds the timing from supported shot lengths. The target final duration is approximately 58–62 seconds.

The complete demo requires connected capabilities for:

- character-reference or multimodal image input;
- reference-aware image generation;
- image-to-video or first/last-frame video generation;
- original music generation;
- ordered multi-clip video composition with audio mixing.

The second run needs at least six image calls, six video calls, one music call, and one or more composition calls. Give the Agent enough execution steps to finish. If the runtime exposes a maximum-iterations setting, `28` or higher is a practical starting point; increase it when Act A, Act B, and the final film require separate composition calls.

Background music is generated automatically by default. The skill prefers one original 58–62-second instrumental track with an energy turn near 00:30; when unsupported, it creates two compatible approximately 30-second cues. Speech, dialogue, sound effects, subtitles, preview, and save are optional. Without music or audio-mixing capability, it labels the delivery visual-only instead of claiming a complete music-backed film.

If an audio risk audit rejects one TTS chunk, the skill rewrites and retries only that chunk while preserving meaning, intent, pacing, successful audio, and upstream media. It progressively neutralizes the wording, tries another available voice only when the text is already neutral, and waits for successful audio before continuing to video.

First-run demo prompt:

```text
Read my uploaded character turnaround and show four original one-minute short-film directions in a native single-choice card. Generate no media yet. After I submit a direction, create the storyboards, videos, original background music, and final film.
```

On the next run, the user can reply with the option number to start the six-image, six-video, and final-composition sequence. If the Agent does not preserve state, return the topic card and reference image with that choice. See [`references/character-video-demo.md`](./references/character-video-demo.md) for the complete runtime contract.

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
- the opening remains literally clear and does not create an unintended genre promise;
- every visible process element uses the detected interaction language, while the title and story body use the selected story language.

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
- no individual document exceeds 20,000 characters;
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
