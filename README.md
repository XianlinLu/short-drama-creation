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

### 智能画面比例

画面比例只在初始分镜和初始视频阶段决定。Skill 会依次参考用户明确要求、发布场景和构图需求：单人全身或移动端短剧优先 `9:16`，多人调度、宽阔动作或环境叙事优先 `16:9`；三视图本身的画布比例不会强制成为成片比例。

初始视频生成后，Skill 会把返回元数据中的实际比例锁定为最终比例。所有视频延长调用都完全省略 `ratio` 参数，不传具体值，也不传 `auto`、`null` 或空字符串，因为延长视频会自动继承输入视频比例。

如果延长调用因 `InvalidParameter.TaskTypeConstraint` 拒绝比例参数，Skill 会保留最新完整视频，删除失败请求中的 `ratio` 字段，并只重试该次延长。若连接组件仍自动写入比例字段，流程会报告配置问题，不会重新生成、裁剪、填边、拉伸或转码。

### 视频版权风控恢复

当视频生成返回版权相似性风控错误时，Skill 不会重复提交同一请求或尝试规避审核。它会保留最后一个已验证视频和所有成功产物，只处理失败的视频步骤。

第一次恢复会删除作品名、角色名、品牌、指定创作者、真人肖像、精确复刻和标志性镜头等高相似信号，并用原创的动作、环境、光线和镜头语言重新表达同一故事功能。仍然失败时，第二次恢复会重新设计失败段落的视觉表达，同时保持原创角色、故事含义、时长、连续性和本次调用从 `00:00` 开始的时间轴。

如果参考图本身是可识别的第三方角色、名人、品牌素材、影视画面、海报或带水印图片，流程会停止自动重试并要求换成原创无标识素材。两次合规恢复仍被拒绝时，流程返回真实错误编号和最后一个成功视频，不会无限重试或重启整条工作流。

### 音频版权风控恢复

当音乐、配音、音效或混音返回音频版权相似性错误时，Skill 会保留完整视频和所有成功音频，只处理失败的音频步骤。它不会用变调、变速、加噪、倒放、切片、转码或切换服务来规避审核。

第一次恢复会删除歌曲名、歌手、影视配乐、角色音色、名人模仿、歌词、采样和“相似曲”等要求，改为原创器乐、普通非模仿音色或功能性音效。仍被拒绝时，第二次恢复会重建音频设计：音乐至少改变四项作曲维度，配音改用一个中性的非模仿音色，音效更换合成思路。

两次恢复失败后，受影响的音频分支会停止。仅背景音乐失败时，可明确标注并交付已经验证的视频；必要旁白失败时，则停止依赖该旁白的后续步骤并返回真实错误编号。

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

### 分镜制作工作流

当剧本和选题方向已经确认，并且用户选择“制作分镜表”时，Skill 会进入独立的分镜制作流程：

1. 解析最终剧本，整理全部主要角色的外貌、年龄、服装、身份、性格、关系和声音需求。
2. 为每个角色生成一张独立的 `16:9` 角色资产图，其中同时包含正面面部近景、正面全身、侧面全身和背面全身；四个视图保持五官、发型、服装、身材比例和画风一致。
3. 展示全部角色资产并停止，等待用户逐一确认或修改。只要仍有角色资产未确认，就不会生成试音。
4. 全部资产确认后，为每个角色生成一段 `20–30 秒` 的试音，展示中性表达、日常交流、紧张、坚定和柔和收尾等语气变化。
5. 展示全部试音并停止，等待逐一确认。任何角色音色未确认时，都不能开始场景对白或视频。
6. 全部音色确认后，解析场景一并生成分镜表，记录镜号、时长、景别、机位、角色版本、动作、对白、环境、道具和转场。
7. 使用角色已经确认的对应音色，按剧本顺序生成场景一对白，保留台词、情绪、停顿、打断和说话顺序。
8. 根据确认的角色资产、对白、场景描述和分镜生成场景一视频，随后停止并等待用户确认；确认后才处理下一场景。
9. 没有角色的剧本会跳过角色资产和试音，直接进入场景拆解。

Skill 会给剧本、角色设定、资产、音色、对白、分镜和场景视频分别记录版本。角色形象变化会使包含该角色的场景视频失效；音色变化会使该角色的对白和依赖视频失效。系统只重新生成受影响的内容，并始终使用最新确认版本。

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

### Smart aspect ratio

Aspect ratio is selected only for the initial storyboard and initial video. The skill prioritizes an explicit supported user choice, stated destination, and composition: a single full-body character or mobile-first short drama favors `9:16`, while multi-character blocking, wide action, or environment-led storytelling favors `16:9`. A three-view reference sheet does not force the final canvas ratio.

After the initial video returns, its actual metadata ratio becomes the locked final ratio. Every extension request completely omits the `ratio` parameter—without a numeric value, `auto`, `null`, or an empty string—because extension output inherits its input video's ratio.

If an extension rejects the ratio with `InvalidParameter.TaskTypeConstraint`, the skill keeps the latest complete video, removes the `ratio` field, and retries only that extension. If the connected wrapper continues to inject the field, the workflow reports a configuration problem instead of restarting, cropping, padding, stretching, or transcoding the video.

### Video copyright-policy recovery

When video generation returns a copyright-similarity policy error, the skill preserves the last verified video and retries only the failed step. It never repeats identical inputs or attempts to evade the safeguard.

The first recovery removes named works, characters, brands, creator or likeness requests, exact recreations, and iconic staging, then expresses the same story function with original action, environment, lighting, and camera language. If rejection persists, one final recovery creates a substantially new visual realization while retaining the original character, story meaning, duration, continuity, and the call-local timeline beginning at `00:00`.

Recognizable third-party characters, celebrities, branded assets, film frames, posters, or watermarked references stop automatic retry and require an original unbranded replacement. After two rejected recovery attempts, the workflow returns the real error identifiers and last successful video instead of looping or restarting the full process.

### Audio copyright-policy recovery

If music, TTS, sound-effect, or mux output triggers an audio copyright-similarity policy error, the skill preserves the complete video and every successful audio result, then retries only the failed audio step. It never uses pitch shifting, speed changes, noise, reversal, slicing, transcoding, or provider switching to evade review.

The first recovery removes named songs, performers, soundtracks, character or celebrity voices, lyrics, samples, and soundalike requests, replacing them with an original instrumental cue, neutral non-impersonation voice, or functional sound effect. If rejected again, one final recovery creates a new audio design: music changes at least four compositional dimensions, TTS uses a different neutral voice, and sound effects use a new synthesis concept.

After two failed attempts, the affected audio branch stops. A verified video may be delivered without optional music when clearly labeled; required narration blocks only the dependent downstream step and returns the real error identifiers.

### Zero-based prompts for every call

Every initial-video and extension prompt describes one independent generation call, so its visible timing begins at `00:00` and ends at that call's duration. Cumulative film time is kept outside generation prompts. A ten-second extension therefore uses `00:00–00:10`, never `00:30–00:40`.

### Original background music

When a compatible action is connected, the skill automatically creates original instrumental music matching the verified final duration and story energy curve. Audio is embedded only through a route that preserves one continuous video; otherwise it is delivered as a clearly labeled synchronized track.

### Targeted TTS recovery

If an audio risk audit rejects one TTS chunk, the skill identifies that exact chunk and rewrites only its spoken text while preserving meaning, intent, pace, and emotional direction. It retries only the failed audio step, simplifies wording progressively, and can try one alternate voice when neutral text is still rejected. Successful audio and upstream media remain untouched, and dependent video generation waits for confirmed audio success.

### Originality and quality

References contribute only broad craft functions such as reveal timing, relationship pressure, pacing, or visual contrast. The skill creates new characters, worlds, causality, scenes, dialogue, and imagery. It does not reproduce protected characters, signature objects, iconic shots, or distinctive plot chains, and it does not imitate the recognizable style of a living creator.

### Storyboard production workflow

When the screenplay and creative direction are confirmed and the user selects “Create Storyboard,” the skill enters a gated production route:

1. Parse the final screenplay and build profiles for every principal character, including appearance, age range, wardrobe, identity, personality, relationships, and voice needs.
2. Generate one separate `16:9` asset sheet per character containing a front facial close-up plus front, side, and back full-body views with consistent identity, clothing, proportions, and style.
3. Show all character sheets and stop for individual confirmation or revision. Auditions cannot begin while any asset remains unconfirmed.
4. After all assets are confirmed, generate one `20–30 second` audition per character with enough emotional variation to evaluate casting.
5. Show all auditions and stop for individual confirmation. Scene dialogue and video remain blocked until every voice is confirmed.
6. Parse scene one and create a storyboard table covering shot id, duration, framing, camera, character versions, action, dialogue, environment, props, and transition.
7. Generate scene-one dialogue in screenplay order with each character's confirmed voice, emotion, pauses, interruption, and timing.
8. Generate scene one from confirmed assets, verified dialogue, scene description, and storyboard, then stop for approval before processing the next scene.
9. Characterless scripts skip asset sheets and auditions and proceed directly to scene breakdown.

The skill versions the screenplay, profiles, assets, voices, dialogue, storyboards, and scene videos. Appearance changes invalidate dependent scene videos; voice changes invalidate that character's dialogue and dependent videos. Only affected work is regenerated, always from the latest confirmed versions.

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
