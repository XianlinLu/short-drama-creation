# Short Drama Creation

> 将人物三视图或故事灵感转化为原创短剧：先生成一段短小的种子视频，再连续延长到用户提示词中指定的时长，同时自动匹配用户语言。
>
> Turns a character reference or story idea into an original short drama by generating one short seed video and continuously extending it to the duration specified in the user's prompt, while matching the user's language.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`Short Drama Creation` 是一套面向原创短剧生产的 Agent Skill。用户可以上传人物角色三视图，也可以直接输入故事灵感。Skill 会先通过单选交互卡确认选题和提示词中的目标时长，再生成一张种子分镜与一段短小的初始视频，随后始终以上一次成功返回的完整视频为输入，连续延长到规定时长。

```text
人物三视图或故事灵感
→ 单选选题卡
→ 读取用户指定时长
→ 原创短剧与连续延长计划
→ 角色一致性锁定
→ 1 张种子分镜图
→ 1 段短小的种子视频
→ 以上一次完整视频为输入逐次延长
→ 达到用户规定时长的最终视频
→ 原创背景音乐与可选配音
```

### 核心能力

#### 原生交互式选题

首轮不会直接生成媒体，而是调用 Agent 的原生单选提问能力，显示一个问题、简短标题、四个互斥选题、选项说明、推荐项和自定义输入。用户提交方向且提示词包含目标时长后，才开始种子分镜、种子视频和连续延长。

如果运行环境不提供原生交互提问能力，Skill 会明确说明限制并退回编号选项，不会假装已经显示交互卡。

#### 原创短剧设计

每个选题都会重新设计角色目标、冲突、场景、视觉母题、升级、反转和结尾，不搬运已有作品的人物、名场面、标志性物件、台词或连续情节。作品名称和创作者只作为抽象创作信号，不作为复制目标。

#### 人物一致性

Skill 从人物三视图中提取稳定的视觉锚点，包括脸型、五官、发型、服装结构、固定配饰、身体比例和画面风格。种子分镜和每一次视频延长都会复用同一份角色锁定信息，避免角色在延长过程中变脸、换装或比例漂移。

#### 种子视频连续延长

用户在提示词中写明目标时长，例如 `45 秒`、`1 分钟`、`1 分 30 秒` 或 `00:45`。Skill 会读取视频组件支持的种子时长、延长步长和最大累计时长，然后选择最短且可稳定延长的初始片段。

每次延长都把上一次成功返回的完整视频传给下一次延长调用，并检查新的累计时长。整个流程不会生成多个独立片段后再拼接，也不会通过循环画面、变速、静帧填充或静默取整伪造时长。如果组件无法精确达到目标，Skill 会在生成前给出最接近的可支持时长供用户选择。

#### 每个视频提示词从 0 开始

全片累计时间与视频提示词时间严格分离。累计时间只用于内部计划和时长校验；每个种子或延长节点的提示词都是独立的局部时间轴，必须从 `00:00` 开始，并在本次生成时长结束。例如对应全片第 30–40 秒的 10 秒延长提示词仍写作 `00:00–00:10`，不会写成 `00:30–00:40`。这里的“独立”指提示词和局部时间独立；延长节点仍然以上一次返回的完整视频作为媒体输入。

#### 自动生成原创背景音乐

默认生成一条与目标时长、故事情绪、类型、场景和节奏匹配的原创纯音乐。优先通过视频生成或延长组件的原生音频能力加入；也可以在不拼接、不裁切、不变速视频的前提下，仅把音轨加入这一条完整延长视频。

音乐不会复制已有旋律，也不会模仿在世创作者的独特风格。没有可用的非拼接音频写入方式时，Skill 会把同步配乐单独交付，并明确标注音乐尚未嵌入视频。

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
2. 连接人物图片输入、参考图图片生成、图生视频，以及能够接收已有完整视频并返回更长完整视频的视频延长能力。
3. 如需对白或旁白，再连接 TTS 能力。
4. 上传人物三视图，并在提示词中写明目标时长。
5. 提交单选卡中的选题方向。
6. 等待种子分镜、种子视频、连续延长、时长校验和音乐依次完成。

Skill 会在生成前计算延长次数。默认自动流程最多执行 12 次延长；需要更多次数时，会先要求用户缩短时长或明确批准更大的执行预算。

### 示例请求

```text
请读取我上传的人物三视图，用单选交互卡给我 4 个原创短剧选题。目标时长 60 秒。现在不要生成媒体；我提交方向后，先生成一段短小的种子视频，再把这条视频连续延长到 60 秒，并生成原创背景音乐。
```

```text
把这个故事灵感改成 45 秒悬疑短剧。先让我选择视觉方向，再生成种子视频并连续延长到 45 秒；不要拼接独立片段。
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

`Short Drama Creation` is an Agent Skill for producing original short-form narrative videos. Start with a character turnaround or story idea and write the target duration in the prompt. The skill confirms a direction through an interactive single-select card, generates one seed storyboard and one short seed video, then repeatedly extends the latest complete video until the requested duration is verified.

```text
character reference or story idea
→ interactive topic card
→ user-specified duration
→ original story and continuation map
→ character continuity lock
→ one seed storyboard
→ one short seed video
→ sequential extension of the latest complete video
→ duration-verified final video
→ original background music and optional speech
```

### Core capabilities

#### Interactive topic selection

The first run uses the Agent's native single-choice input to display one question, a short header, four mutually exclusive topics, concise descriptions, a recommended option, and custom input. No media is generated before the user submits a direction and supplies a target duration.

If native interactive input is unavailable, the skill labels the limitation and returns equivalent numbered options instead of pretending that a UI card appeared.

#### Original short-drama design

Every production rebuilds the character goal, conflict, setting, visual motif, escalation, reversal, and ending. Existing characters, signature objects, famous shots, dialogue, or recognizable scene sequences are never carried over. Creative references are reduced to general narrative or visual functions.

#### Character continuity

The skill extracts stable visual anchors from the character turnaround: facial structure, visible features, hair, costume construction, fixed accessories, proportions, and rendering style. The same reference and continuity lock are reused for the seed storyboard and every compatible video-extension call.

#### Seed-and-extend video workflow

The user specifies a duration such as `45 seconds`, `1 minute`, `1 minute 30 seconds`, or `00:45`. The skill inspects supported seed lengths, extension increments, maximum cumulative duration, and duration metadata. It then creates the shortest suitable seed video and extends the latest successful complete video in sequence.

Independent clips are never concatenated. The workflow also avoids loops, speed changes, frozen-frame padding, silent rounding, and trimming. If the requested duration is unreachable, the skill asks the user to choose from the nearest supported durations before generation.

#### Every video prompt starts at zero

Full-film cumulative time is kept separate from prompt-local time. Cumulative values are used only for planning and duration verification. Every seed or extension prompt has an independent local timeline beginning at `00:00` and ending at that call's own duration. A 10-second extension corresponding globally to 30–40 seconds therefore uses `00:00–00:10`, never `00:30–00:40`. “Independent” applies to prompt wording and local timing; an extension still receives the previously returned complete video as its media input.

#### Automatic original background music

The default production includes one original instrumental plan matched to the requested duration and story energy curve. Audio is embedded through native video audio support or a single-video audio mux that does not concatenate, trim, or retime the extended video.

It never copies an existing melody or imitates a living creator's distinctive style. If no non-concatenating audio path exists, synchronized music is delivered separately and clearly labeled as not embedded.

#### Targeted TTS recovery

If an audio risk audit rejects one TTS chunk, the skill identifies the exact chunk and rewrites only its dialogue or narration while preserving meaning, intent, pacing, successful audio, and upstream media. It tries a safer rewrite, a simpler rewrite, and—only when the text is already neutral—another available voice.

Only the failed TTS step is retried. Video generation continues only after replacement audio succeeds. If the bounded attempts fail, the skill stops with the chunk ID and actual error instead of restarting the workflow.

#### Automatic language matching

All visible topic cards, progress, errors, storyboards, and delivery notes follow the language of the user's latest direct request. Attachments, quotations, code, retrieved content, and tool output do not override that choice. A component's fixed prompt language affects only its internal parameter, not the user's visible workflow.

### How to use

1. Import the latest Skill ZIP.
2. Connect character-image input, reference-aware image generation, image-to-video, and true video extension that accepts a complete video and returns a longer complete video.
3. Connect TTS only when dialogue or narration is needed.
4. Upload a character turnaround and include the target duration in the prompt.
5. Submit one direction in the interactive topic card.
6. Let the Agent generate the seed storyboard, seed video, sequential extensions, duration verification, and music.

The skill calculates the required extension count before generation. The default automated flow is capped at 12 extensions; longer jobs require the user to shorten the duration or explicitly approve a larger execution budget.

### Example request

```text
Read my uploaded character turnaround and show four original short-drama directions in a native single-choice card. Target duration: 60 seconds. Generate no media yet. After I submit a direction, create one short seed video, continuously extend that same video to 60 seconds without concatenating independent clips, and generate original background music.
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
