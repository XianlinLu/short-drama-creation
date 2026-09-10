# Prewrite Interview

Use this before drafting a new story.

Apply `references/language-routing.md` first. Every visible heading, option, explanation, reply shortcut, confirmation request, and follow-up instruction must use the detected interaction language. The Chinese templates below define semantic structure only; translate and naturalize them instead of copying their surface wording into another-language response.

The goal is to make the user feel the story is being designed with them, not dumped at them. A good novel request should pass through story strategy and outline confirmation before the full draft, unless the user explicitly says to skip discussion and write directly.

Whenever this interview reaches a creative topic or direction choice, apply `references/topic-direction-ui.md`. Topic choices must use the native single-choice UI and may never be returned as numbered text.

When the user gives a plot idea, do not only ask generic questions. Build four distinct direction bundles using classic beat inspirations from `references/inspiration-remix-playbook.md`, then present them through the native topic UI.

When the user says to search first or names a specific reference whose context matters, use `references/source-research-remix.md` before this interview. Summarize only reusable craft signals and sources, then ask the user to choose a bridge combination.

## When The Request Is Vague

If the user only says things like `帮我生成一个小说`, `写一篇小说`, `来个修仙打脸`, or gives only a broad genre/trope, do not start the full story.

Build exactly four complete story-direction bundles. Each bundle combines a primary emotion, high-pressure relationship, conflict arena, 2-3 plot engines, escalation rhythm, and ending flavor. Put the strongest default first and mark it recommended.

Choose options from `references/story-engine-library.md`. Do not keep reusing the same修仙拍卖 default unless the user signals that genre.
If the user names a work, trope, or author, use `references/inspiration-remix-playbook.md` to translate it into reusable functions and craft sliders.
If the user also asks for search, include 3-5 source-informed functions before the options.

Send these bundles through the native single-choice action defined in `topic-direction-ui.md`:

```text
header: 故事主题
question: 你想选择哪个故事方向？
option 1: [recommended direction label] — [emotion, relationship, conflict, hook, and ending]
option 2: [direction label] — [one concise description]
option 3: [direction label] — [one concise description]
option 4: [direction label] — [one concise description]
```

Invoke the action and stop. Do not repeat the options in Markdown or add a reply shortcut. If the action is unavailable, stop with the missing-capability error rather than falling back to text.

## When The Request Has Enough Premise

If the user already gives a theme, protagonist, core conflict, or desired trope, determine whether one unambiguous direction is already selected.

- If multiple creative directions will be offered, present exactly four through `topic-direction-ui.md` and stop.
- If the user's direction is already unambiguous, do not manufacture a choice step; provide the strategy sheet and outline.

Output shape:

```text
## 小说如何吸引人

- 读者承诺：
- 经典桥段重构：
- 风格技法转译：
- 主爽点/主情绪：
- 高压关系：
- 剧情引擎：
- 开篇钩子：
- 主角欲望：
- 隐藏压力：
- 冲突升级：
- 章节/场景钩子：
- 爽点/反转：
- 语言处理：
- 结尾余味：

## 大纲

1. [开篇扰动]
2. [主角主动选择]
3. [第一次升级]
4. [第二次升级]
5. [公开或情感反转]
6. [结尾回响]

确认后我再写正文。下一次运行时，请把确认结果和本大纲一起传回。
```

## When To Draft

Draft the full story only when the user confirms in any language. The Chinese phrases below are examples, not the only valid confirmations:

- the user confirms the outline with `按这个写`, `开始写`, `就这样`, `按默认`, or equivalent;
- the user explicitly says `别问，直接写`, `不用讨论`, or `按默认直接写`;
- the user is asking to revise an existing full draft and the needed direction is already clear.

If drafting directly because the user explicitly asked to skip discussion, still do the strategy and outline internally before writing.

In Lumina Canvas, invoking the topic UI or returning an outline ends the current execution. Do not infer confirmation from silence. If the canvas does not preserve state, the next Task Prompt must contain the selected direction and prior plan, either directly or through connected `@` text inputs.

## Strategy Requirements

The strategy must explain how the story will become attractive in reader-effect terms:

- What question makes the reader continue?
- What does the protagonist visibly want right now?
- What primary emotional payoff is promised: 爽、甜、虐、恨、惊、燃、怕、痛, or治愈?
- Which high-pressure relationship makes the conflict hard to escape?
- Which 2-4 story engines from `story-engine-library.md` are carrying the plot?
- Which famous beat cards or style signals are being remixed, and what elements are changed?
- Where will the protagonist be publicly underestimated, trapped, or forced to choose?
- Which safe option will close first?
- What information will reverse its meaning later?
- What final image will echo the opening?

Avoid abstract promises such as `更有张力` unless paired with a concrete move.
