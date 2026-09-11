# Short Drama Creation

> 从人物三视图、故事灵感或现有文本出发，完成原创选题、故事设计、角色一致性分镜、短视频生成与连续延长，并自动匹配用户语言和背景音乐。
>
> Create original short dramas from a character turnaround, story idea, or existing draft—with interactive topic selection, consistent storyboards, a short initial video, continuous duration extension, automatic language matching, and original music.

[中文](#中文介绍) · [English](#english-introduction)

## 中文介绍

`Short Drama Creation` 是一套原创短剧与叙事创作 Skill。它既能完成小说构思、续写和改写，也能把人物角色三视图转化成具有故事连续性的短视频。

视频流程不会把多个独立片段拼接起来。它会先生成一段短小的初始视频，再把上一次成功返回的完整视频作为下一次延长输入，直到达到用户在提示词中指定的时长。

### 核心流程

```text
人物三视图或故事需求
→ 原生单选卡确认创作方向
→ 读取用户指定时长
→ 建立原创故事与角色连续性
→ 生成一张初始分镜图
→ 生成一段短小的初始视频
→ 基于最新完整视频逐次延长
→ 校验最终时长与连续性
→ 生成原创背景音乐与可选配音
→ 交付最终视频
```

### 原生选题卡

只要创作流程需要用户选择方向，不论输入是人物图、故事灵感、小说梗概、已有草稿、参考作品还是研究结果，Skill 都会调用原生单选提问能力。

选题卡固定包含一个问题、四个互斥方向、首项推荐、简短说明、Other 输入以及提交控件。选题不会以 Markdown、JSON、表格、普通文本或编号列表代替。原生提问能力不可用时，流程会停在选题阶段，不会自行替用户决定，也不会提前生成媒体。

### 自动语言匹配

Skill 根据用户最新的直接请求确定交互语言。用户使用英语时，问题、选项、过程说明和错误提示均使用英语；用户使用日语时，这些内容均使用日语。附件、引用、粘贴文本和工具结果中的语言不会覆盖用户的直接语言。

故事语言独立处理：用户明确指定的语言优先；改写任务默认保留原稿语言；其他情况跟随交互语言。

### 连续视频延长

用户必须在提示词中写明目标时长，例如 `45 秒`、`1 分钟`、`1 分 30 秒` 或 `00:45`。Skill 会检查可用的初始时长、延长步长、最大累计时长和时长元数据，再规划可实现的延长链。

每一次延长都以上一次成功返回的完整视频为输入，并等待结果验证通过后才进行下一步。只返回新增尾部片段的能力不会被当作视频延长，也不会改用独立片段拼接。

### 每个视频提示词从零计时

初始视频和每一次延长都是独立的视频生成调用，因此对应提示词都从本次调用的 `00:00` 开始，并在本次调用的时长结束。

完整影片的累计时间只保存在内部计划和进度元数据中。例如，一个新增 10 秒的延长调用使用 `00:00–00:10`，不会因为它处于成片后半段而写成 `00:30–00:40`。

### 原创背景音乐

在具备相应能力时，Skill 默认生成原创器乐背景音乐，并让音乐长度、情绪和能量变化匹配最终视频。音乐只通过不改变连续视频结构的方式嵌入；如果无法安全嵌入，则作为单独音轨交付并说明同步方式。

### 精确的 TTS 恢复

当音频风险审核拒绝某个配音分块时，Skill 会定位准确分块，只改写该分块中的台词或旁白，并保留原始含义、人物意图、节奏和情绪方向。

它只重试失败的音频步骤。再次失败时会逐步简化措辞；文本已经中性时，可切换一次可用音色。成功的配音分块、分镜和视频不会被重做。音频确认成功后才继续依赖它的视频步骤。

### 原创与质量控制

参考作品只用于提取抽象叙事功能，例如节奏、悬念密度、关系压力或视觉对比。Skill 会重新创建人物、世界、因果链、场景顺序、对白和意象，不复制受保护角色、标志性道具、经典镜头或辨识度很高的剧情链，也不会直接模仿在世创作者的独特风格。

完成故事会检查开场可理解性、人物欲望、冲突升级、对白张力、画面作用、因果关系和结尾余味；视频会检查人物外观、服装、动作、场景、道具、光线、时长和音频连续性。

### 使用方法

1. 导入仓库中的受支持文本文件。
2. 加载 `SKILL.md`，并按运行入口文件配置 Agent。
3. 上传人物三视图，或输入故事创作与改写需求。
4. 如果要生成视频，请在直接提示词中写明目标时长。
5. 在原生选题卡中提交一个方向。
6. 等待初始分镜、初始视频、连续延长、时长校验和音频依次完成。

示例：

```text
读取我上传的人物三视图，用原生单选卡给我四个原创悬疑短剧方向。目标时长 60 秒。提交方向前不要生成媒体；提交后先生成一段短小的初始视频，再连续延长到 60 秒，并生成原创背景音乐。
```

## English Introduction

`Short Drama Creation` is a skill for original narrative writing and character-led short-video production. It can plan, draft, continue, or revise fiction, and it can transform a character turnaround into a visually consistent continuous short film.

The video workflow does not assemble independent clips. It generates one short initial video, then repeatedly supplies the latest successful complete video to a true extension action until the duration written in the user's prompt is reached.

### Core workflow

```text
character reference or story request
→ native single-choice direction card
→ user-specified duration lock
→ original story and continuity design
→ one initial storyboard
→ one short initial video
→ sequential extension of the latest complete video
→ duration and continuity verification
→ original music and optional speech
→ final delivery
```

### Native direction selection

Whenever the workflow needs the user to choose a creative direction, the skill invokes a native single-choice question regardless of whether the input is an image, idea, synopsis, draft, reference, or research result.

The card contains one question, four mutually exclusive directions, a recommended first option, concise descriptions, an Other field, and submit controls. It cannot be replaced by Markdown, JSON, tables, prose, or numbered choices. If the native action is unavailable, the workflow stops without selecting for the user or starting media generation.

### Automatic language matching

Visible questions, options, progress notes, and errors follow the language of the user's latest direct request. Language found only in attachments, quotations, pasted drafts, or action output does not override it. Story language follows an explicit instruction, otherwise preserves a revised draft's language, otherwise matches the interaction language.

### True continuous extension

The user supplies a duration such as `45 seconds`, `1 minute`, `1 minute 30 seconds`, or `00:45`. The skill checks available initial lengths, extension increments, cumulative limits, and returned duration metadata before generation.

Each extension consumes the previous complete video and must return a longer complete video. A tail-only clip is rejected as an extension, and independent clips are never concatenated. Duration is not faked through loops, freezes, padding, speed changes, or silent rounding.

### Zero-based prompts for every call

Every initial-video and extension prompt describes one independent generation call, so its visible timing begins at `00:00` and ends at that call's duration. Cumulative film time is kept outside generation prompts. A ten-second extension therefore uses `00:00–00:10`, never `00:30–00:40`.

### Original background music

When a compatible action is connected, the skill automatically creates original instrumental music matching the verified final duration and story energy curve. Audio is embedded only through a route that preserves one continuous video; otherwise it is delivered as a clearly labeled synchronized track.

### Targeted TTS recovery

If an audio risk audit rejects one TTS chunk, the skill identifies that exact chunk and rewrites only its spoken text while preserving meaning, intent, pace, and emotional direction. It retries only the failed audio step, simplifies wording progressively, and can try one alternate voice when neutral text is still rejected. Successful audio and upstream media remain untouched, and dependent video generation waits for confirmed audio success.

### Originality and quality

References contribute only broad craft functions such as reveal timing, relationship pressure, pacing, or visual contrast. The skill creates new characters, worlds, causality, scenes, dialogue, and imagery. It does not reproduce protected characters, signature objects, iconic shots, or distinctive plot chains, and it does not imitate the recognizable style of a living creator.

### How to use

1. Import the repository's supported text documents.
2. Load `SKILL.md` and configure the Agent with the runtime entry document.
3. Upload a character turnaround or enter a story request.
4. For video output, include the exact target duration in the direct prompt.
5. Submit one direction through the native card.
6. Let the workflow generate, extend, verify, score, and deliver the result.

Example:

```text
Read my uploaded character turnaround and show four original mystery directions in a native single-choice card. Target duration: 60 seconds. Generate no media before I submit a direction. After selection, create one short initial video, continuously extend that complete video to 60 seconds, and generate original background music.
```

## Package Structure

```text
SKILL.md
runtime instructions document
agents/
references/
examples/
manifest.json
```

All import documents use supported text formats and remain within the platform's per-document character limit.
