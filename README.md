# Short Drama Creation

> 将人物三视图或故事灵感转化为原创短剧、交互式选题、分镜图、视频片段、背景音乐和约一分钟成片，并自动匹配用户语言。
>
> Turns a character reference or story idea into an original short drama with interactive topic selection, storyboards, video clips, background music, and an approximately one-minute final film while matching the user's language.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`Short Drama Creation` 是一套面向原创短剧生产的 Agent Skill。用户可以上传人物角色三视图，也可以直接输入故事灵感。Skill 会先通过单选交互卡确认选题，再完成短剧结构、人物一致性控制、分镜场景、镜头视频、原创配乐和最终合成。

```text
人物三视图或故事灵感
→ 单选选题卡
→ 原创短剧结构
→ 角色一致性锁定
→ 6 张分镜图
→ 6 段短镜头视频
→ 原创背景音乐与可选配音
→ 前 30 秒 + 后 30 秒
→ 约 60 秒最终成片
```

### 核心能力

#### 原生交互式选题

首轮不会直接生成媒体，而是调用 Agent 的原生单选提问能力，显示一个问题、简短标题、四个互斥选题、选项说明、推荐项和自定义输入。用户提交方向后，才开始分镜、视频、音乐及合成。

如果运行环境不提供原生交互提问能力，Skill 会明确说明限制并退回编号选项，不会假装已经显示交互卡。

#### 原创短剧设计

每个选题都会重新设计角色目标、冲突、场景、视觉母题、升级、反转和结尾，不搬运已有作品的人物、名场面、标志性物件、台词或连续情节。作品名称和创作者只作为抽象创作信号，不作为复制目标。

#### 人物一致性

Skill 从人物三视图中提取稳定的视觉锚点，包括脸型、五官、发型、服装结构、固定配饰、身体比例和画面风格。每次分镜生成都重新使用原始参考图和同一份角色锁定信息，避免角色在镜头间变脸、换装或比例漂移。

#### 30 秒 + 30 秒结构

默认采用六个约 10 秒镜头：

| 镜头 | 时间 | 作用 |
| --- | --- | --- |
| 01 | 00:00–00:10 | 视觉钩子与场景规则 |
| 02 | 00:10–00:20 | 人物目标与第一阻碍 |
| 03 | 00:20–00:30 | 冲突升级与上半段转折 |
| 04 | 00:30–00:40 | 发现或反转 |
| 05 | 00:40–00:50 | 有代价的选择与高潮 |
| 06 | 00:50–01:00 | 回报与首尾呼应 |

Skill 会先读取视频组件的真实时长限制。支持 10 秒时采用 `6 × 10 秒`；不支持时改用组件允许的镜头长度，让上下两段分别接近 30 秒，并把最终成片控制在约 58–62 秒。

#### 自动生成原创背景音乐

默认生成一条与故事情绪、类型、场景和节奏匹配的原创纯音乐，并在 30 秒处配合剧情转折。音乐组件不支持约一分钟时，会生成两条速度、调性、配器和氛围兼容的约 30 秒音乐，再按上下半段合成。

音乐不会复制已有旋律，也不会模仿在世创作者的独特风格。缺少音乐或混音能力时，结果会明确标记为视觉版，不会声称已经完成带配乐成片。

#### TTS 分块恢复

当配音的某个分块被音频风险审核拒绝时，Skill 会定位准确的分块编号，只安全改写该分块的台词或旁白，并保留故事含义、人物意图、节奏以及所有已经成功的音频和媒体结果。

恢复顺序为：安全改写、进一步简化、文字已经中性时更换可用音色。只重试失败的 TTS 步骤，确认音频成功后才继续视频生成；达到重试上限后会停止并报告真实错误，不会重启整条流程。

#### 自动匹配用户语言

- 用户用中文输入，选题、进度、错误、分镜和交付说明使用中文；
- 用户用英文输入，所有可见流程使用英文；
- 用户用日文输入，所有可见流程使用日文；
- 语言识别以用户最新的直接请求为准，不受附件、引用、代码或工具输出影响；
- 媒体模型需要固定语言提示词时，只转换内部技术参数，不改变用户看到的交互语言。

模型的私有推理不会展示。实际可见的步骤摘要和状态始终使用识别到的用户语言。

### 使用方法

1. 导入最新 Skill ZIP。
2. 连接人物图片输入、参考图图片生成、图生视频、原创音乐生成及支持音轨的视频合成能力。
3. 如需对白或旁白，再连接 TTS 能力。
4. 上传人物三视图，或输入一个短剧故事灵感。
5. 提交单选卡中的选题方向。
6. 等待分镜、视频、音乐和最终成片依次完成。

第二轮至少需要六次图片生成、六次视频生成、一次音乐生成和一次以上合成调用。若运行环境提供最大迭代次数设置，建议从 `28` 或更高开始。

### 示例请求

```text
请读取我上传的人物三视图，用单选交互卡给我 4 个适合这个角色的一分钟原创短剧选题。现在不要生成媒体；我提交方向后，再生成分镜、视频、原创背景音乐和最终成片。
```

```text
把这个故事灵感改成一分钟悬疑短剧：上半段建立误会，下半段反转真相。先让我选择视觉方向。
```

### 安装

```bash
npx skills add XianlinLu/short-drama-creation
```

### 导入规范

- 仅使用小写 `.md`、`.txt`、`.json`、`.yaml` 或 `.yml` 扩展名；
- 每个文档不超过 20,000 个字符；
- 文件和目录名只包含英文字母、数字、下划线和连字符；
- 文件和目录名不超过 64 个字符；
- 导入内容不包含脚本、图片、二进制文件或缓存文件。

## English Introduction

`Short Drama Creation` is an Agent Skill for producing original short-form narrative videos. Start with a character turnaround or a story idea. The skill asks the user to choose a direction through an interactive single-select card, then develops the short-drama structure, character continuity, storyboard scenes, video shots, original music, optional speech, and final composition.

```text
character reference or story idea
→ interactive topic card
→ original short-drama structure
→ character continuity lock
→ six storyboard images
→ six short video shots
→ original background music and optional speech
→ 30-second act A + 30-second act B
→ approximately 60-second final film
```

### Core capabilities

#### Interactive topic selection

The first run uses the Agent's native single-choice input to display one question, a short header, four mutually exclusive topics, concise descriptions, a recommended option, and custom input. No media is generated before the user submits a direction.

If native interactive input is unavailable, the skill labels the limitation and returns equivalent numbered options instead of pretending that a UI card appeared.

#### Original short-drama design

Every production rebuilds the character goal, conflict, setting, visual motif, escalation, reversal, and ending. Existing characters, signature objects, famous shots, dialogue, or recognizable scene sequences are never carried over. Creative references are reduced to general narrative or visual functions.

#### Character continuity

The skill extracts stable visual anchors from the character turnaround: facial structure, visible features, hair, costume construction, fixed accessories, proportions, and rendering style. The original reference and the same continuity lock are reused for every storyboard call.

#### 30 + 30-second structure

The default timeline uses six approximately 10-second shots covering the hook, goal, escalation, reversal, climax choice, and closing echo. The skill reads the real duration limits of the connected video component. If 10 seconds is unsupported, it rebuilds the timeline from accepted durations while keeping each act near 30 seconds and the final result near 58–62 seconds.

#### Automatic original background music

The default production includes an original instrumental track matched to the story's emotion, genre, setting, and energy curve, with a planned turn around 00:30. When one minute is unsupported, the skill generates two compatible approximately 30-second cues and joins them with the two-act structure.

It never copies an existing melody or imitates a living creator's distinctive style. If music generation or audio mixing is unavailable, the result is labeled visual-only rather than presented as a complete music-backed film.

#### Targeted TTS recovery

If an audio risk audit rejects one TTS chunk, the skill identifies the exact chunk and rewrites only its dialogue or narration while preserving meaning, intent, pacing, successful audio, and upstream media. It tries a safer rewrite, a simpler rewrite, and—only when the text is already neutral—another available voice.

Only the failed TTS step is retried. Video generation continues only after replacement audio succeeds. If the bounded attempts fail, the skill stops with the chunk ID and actual error instead of restarting the workflow.

#### Automatic language matching

All visible topic cards, progress, errors, storyboards, and delivery notes follow the language of the user's latest direct request. Attachments, quotations, code, retrieved content, and tool output do not override that choice. A component's fixed prompt language affects only its internal parameter, not the user's visible workflow.

### How to use

1. Import the latest Skill ZIP.
2. Connect character-image input, reference-aware image generation, image-to-video, original music generation, and video composition with audio input.
3. Connect TTS only when dialogue or narration is needed.
4. Upload a character turnaround or enter a short-drama idea.
5. Submit one direction in the interactive topic card.
6. Let the Agent generate storyboards, videos, music, and the final film in sequence.

The production run needs at least six image calls, six video calls, one music call, and one or more composition calls. If a maximum-iterations setting exists, `28` or higher is a practical starting point.

### Example request

```text
Read my uploaded character turnaround and show four original one-minute short-drama directions in a native single-choice card. Generate no media yet. After I submit a direction, create the storyboards, videos, original background music, and final film.
```

### Installation

```bash
npx skills add XianlinLu/short-drama-creation
```

### Import format

- lowercase `.md`, `.txt`, `.json`, `.yaml`, or `.yml` extensions only;
- no individual document exceeds 20,000 characters;
- file and folder names contain only letters, numbers, underscores, and hyphens;
- every file and folder name stays within 64 characters;
- no scripts, images, binaries, or cache files are included in the import content.

## Package Structure

```text
SKILL.md                       # Skill entrypoint
agents/                        # Interface metadata
references/                    # Story, storyboard, media, and quality rules
examples/                      # Original narrative examples
manifest.json                  # Resource map and quality gates
LICENSE.md                     # License
```

## License

Distributed under the MIT License. See [`LICENSE.md`](./LICENSE.md).
