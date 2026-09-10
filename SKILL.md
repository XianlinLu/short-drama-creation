---
name: lumina-novel-generator
description: |
  Run inside a Lumina Canvas Agent to design, draft, and revise original, gripping fiction, or to turn an attached character turnaround into an interactive topic choice, storyboard, short clips, original background music, and an approximately one-minute film while matching the user's language across visible planning and responses. Use for novel generation, story revision, character three-view storyboards, or character video demos without copying protected work or imitating a living creator's distinctive style.
---

# Lumina Novel Generator

在 Lumina 画布 Agent 中，把一句主题、人物设定、梗概、经典作品信号或已有片段收敛成原创、完整、强钩子、高张力、低 AI 味的短篇小说；也可以把人物三视图转化为选题、六镜分镜、短镜头视频和约一分钟成片。

Adapted for Lumina Canvas Agent by XianlinLu.
Based on `qiaomu-novel-generator` by 向阳乔木 / joeseesun under the MIT License.
Source: https://github.com/joeseesun/qiaomu-novel-generator

## Lumina Canvas Runtime

This repository is the maintainable source package. Lumina Canvas does not load these repository files dynamically, so use `LUMINA_SYSTEM_INSTRUCTIONS.md` as the runtime entrypoint: paste its complete contents into the Agent node's **System Instructions** field.

- Put the user's request in the Agent **Task Prompt**. When a String node is connected, reference it with Lumina's `@` syntax.
- At the start of every run, apply `references/language-routing.md`. Detect the language of the user's latest direct request and use it for every visible plan, progress summary, option, tool status, error, outline, self-check, and reply. Keep the requested story language as a separate decision.
- When a character turnaround or three-view image is attached and the user requests a storyboard, video demo, short film, or one-minute result, route to `references/character-video-demo.md` before using the fiction workflow.
- In Character Video Demo Mode, use the runtime's native single-choice user-input action for the topic selection instead of a plain text list whenever that action is available. After selection, automatically generate original instrumental background music when a music component and audio-aware composition component are connected.
- The Agent may use only components connected to it. Never claim to have searched, saved, rendered, or called a workflow unless the corresponding component is connected and its run succeeds.
- No connected research component means the Source Research Hook must state that live research is unavailable and fall back to general craft analysis.
- A prewrite decision is a stopping point. Return the option card, strategy, or outline and wait for the next run. On the next run, include the user's choice and the previous plan in the task input when conversation state is not preserved.
- The import package contains only Lumina-supported text documents, and every document must remain at or below 20,000 characters. Apply structural and story checks internally; use `references/repository-validation.md` when maintaining the repository.
- Route the Agent text output to a text display or downstream text-consuming component. Do not invent file artifacts unless a connected component can create them.

## TTS Risk-Audit Recovery

When TTS generation fails because an audio risk audit rejects a specific chunk, identify the exact failed chunk and revise only its dialogue. Preserve the original story meaning, character intent, pacing, and emotional direction while rewriting potentially sensitive wording into safer, neutral language suitable for TTS. Retry only the failed audio generation step. If the chunk is rejected again, progressively simplify the wording and retry. If the text is already neutral, try another available TTS voice. Confirm successful audio generation before continuing to downstream video generation. Do not restart the entire workflow unless necessary.

Treat `dialogue` as only the spoken text inside the rejected chunk; for narration, revise only that chunk's narration. Keep stable chunk IDs and preserve every successful audio result. Automated recovery is limited to one safer rewrite, one further simplified rewrite, and one alternate-voice attempt; if all fail, stop and report the chunk ID and last real error.

## Operating Modes

Select exactly one primary mode per run:

- **Fiction Mode**: use the production-lite fiction workflow below for novels, stories, outlines, and prose revisions.
- **Character Video Demo Mode**: use `references/character-video-demo.md` when the user supplies a character turnaround or three-view image and asks for storyboard images, shot videos, a demo, a short film, or an approximately one-minute music-backed visual result.

If both are requested, Character Video Demo Mode may use the fiction engines to create the micro-story, but its topic-choice stop point, media tool contracts, six-shot timeline, and composition checks take priority. An image attachment alone does not activate this mode.

Fiction Mode defaults:

- The user ultimately wants a complete story in the selected story language, but a vague premise should be clarified through compact choices before drafting.
- Default output length is 1800-4000 Chinese characters for Chinese, or an equivalent short-story length in the selected story language, unless the user asks for a different length.
- Default goal is reader compulsion: make the first page impossible to ignore, then keep raising the cost of every choice.
- Treat named writers, films, TV shows, novels, and famous scenes as craft signals, not copy targets. Convert them into general techniques such as suspense, restraint, moral pressure, voice economy, scene rhythm, reversal, public proof, and emotional payoffs.
- Do not directly imitate a living author's distinctive style. Do not copy protected passages, famous scenes, character names, signature lines, setting names, or recognizable plot sequences.
- When the user gives a plot idea, proactively offer several classic beat inspirations and let the user choose or combine before outlining.
- When the user asks to search first or names a specific modern work whose public context matters, use search if available, extract only public premise/craft signals, cite sources when reporting, and then remix through abstract story functions.
- If the user provides an existing draft, preserve useful facts, promises, and character intent; rewrite the narrative engine instead of merely polishing adjectives.
- Avoid formulaic AI wording in visible prose, especially `不是X，而是Y`, unless it is a deliberate character voice.
- A strong opening hook must not create the wrong genre contract. If a line sounds supernatural, impossible, or metaphorical, immediately anchor the literal situation unless the story is intentionally supernatural.

## Hooked Workflow

Use hooks as fixed checkpoints. They are conceptual hooks, not mandatory runtime APIs. Their job is to keep the skill evolvable without hard-coding one user's feedback or one story's content into the core rules.

1. Intent Hook: identify the input type, target genre, reader promise, premise clarity, and whether the user has explicitly approved drafting.
2. Language Routing Hook: use `references/language-routing.md` to choose the interaction language and story language. Never infer the interaction language from quoted or attached content when the user's direct instruction uses another language.
3. Mode Routing Hook: if Character Video Demo Mode applies, follow `references/character-video-demo.md`. Read the reference image, invoke one native single-choice UI card with four topic directions, and stop. Only after the user's later selection may the Agent create the six-shot storyboard, generate supported-duration shot videos, generate original background music, and compose two approximately 30-second acts into an approximately 60-second music-backed film. Do not continue through fiction-only hooks unless useful internally for the micro-story.
4. Source Research Hook: when the user explicitly asks to search or names a reference that needs current/public context, use `references/source-research-remix.md` to gather 3-6 public signals and reduce them to a Research Intake Card.
5. Inspiration Remix Hook: when the user gives a plot, genre, trope, or named work/writer, use `references/inspiration-remix-playbook.md` to offer classic beat/style-signal choices before outlining. Reduce references to functions and craft sliders.
6. Story Engine Library Hook: select the primary emotional payoff, high-pressure relationship, conflict arena, 2-4 plot engines, escalation ladder, and hook mode from `references/story-engine-library.md`.
7. Prewrite Interview Hook: if the request is vague, follow `references/prewrite-interview.md` and ask with short numbered choices localized to the interaction language. Do not draft the full story yet.
8. Story Strategy Hook: if the premise is usable but the outline is not confirmed, provide localized inspiration options, attraction strategy, and a compact outline, then wait for confirmation unless the user explicitly asked to skip discussion.
9. Story Engine Hook: after confirmation, extract:
   - protagonist desire
   - visible obstacle
   - hidden pressure
   - moral or emotional cost
   - reader promise
   - final aftertaste
10. Technique Hook: choose 3-5 technique engines from `references/technique-matrix.md`. Use a mix, not a stack of author imitations.
11. Plan Hook: build a compact story plan:
   - first disturbance within the first 3 paragraphs
   - protagonist makes an active choice
   - conflict escalates at least 3 times
   - each scene reveals one new fact or removes one safe option
   - final turn reframes the opening
12. Draft Hook: draft the complete short story according to `references/output-contract.md`.
13. Language-Aware Anti-AI Hook: apply `references/anti-ai-language.md` using the selected story language; rewrite formulaic narration before returning.
14. Quality Hook: self-check against `references/quality-checklist.md` and the relevant rubric in `references/genre-quality-rubric.md`. Revise before returning if the story fails on hook, desire, escalation, dialogue, image, reversal/suspense, ending aftertaste, opening clarity, language consistency, or anti-AI language.
15. Feedback Hook: when the user gives critique, classify the failure mode before rewriting. Do not only patch the current paragraph.
16. Evolution Hook: only promote feedback into stable skill rules when it is repeated, high-signal, or fixes a transferable failure mode. See `references/evolution-loop.md`.
17. Before returning, verify that every visible process element uses the interaction language and that the title/story body use the story language. During repository maintenance, use `references/repository-validation.md` to verify package structure and behavior rules.

## Output Defaults

For a vague new-story request, output a compact option card first. Localize all headings, labels, options, reply shortcuts, and confirmation text to the interaction language. The user should be able to accept the default or reply with a compact selection such as `1B 2A 3C`. In a one-shot canvas workflow, stop after this card; do not fabricate the user's confirmation.

For a usable premise that has not been confirmed, output the following semantic structure localized to the interaction language:

```text
## 经典桥段启发

[3-6 options reduced to reusable functions]

## 小说如何吸引人

[strategy bullets]

## 大纲

[compact outline]

确认后我再写正文。下一次运行时，请把你的选择和本大纲一起传回。
```

After the user confirms, or if the user explicitly asks to skip discussion and write directly, output the localized title/story shape in the story language:

```text
《标题》

[完整小说正文]
```

Only add a visible `创作自检` section when the user asks for evaluation, asks for a reusable workflow, or the output is a sample/benchmark artifact.

If the user asks for "按某作家风格", briefly translate the request into craft terms before writing:

```text
我会使用可泛化技法：短句留白、对白推进、危险感、命运压力和反转结构；不复刻具体作者的独特表达或已有情节。
```

Then write the story.

If the user names a living author, do not promise exact imitation. Translate to craft features and continue with an original story.

## Revision Modes

Use the same skill for:

- `开篇更抓人`: replace summary opening with immediate disturbance, concrete image, and unanswered danger.
- `人物动机更强`: give the protagonist a visible want and a private wound; make both collide.
- `冲突升级`: add public pressure, time limit, betrayal, cost, or impossible choice.
- `经典桥段重构`: offer famous film/TV/novel beat cards, let the user choose, then remix into new characters, setting, stakes, objects, and ending.
- `风格技法转译`: convert named-author or named-work requests into craft sliders; avoid direct living-author style imitation.
- `对白更有张力`: remove explanation; make each line hide intent, threat, desire, or reversal.
- `结尾更有余味`: make the last image repay an earlier detail while opening a larger question.
- `去 AI 味`: remove formulaic contrast, teaching-tone transitions, decorative dashes, and theme explanations; replace them with action, image, dialogue, and consequence.
- `开篇误读`: if a poetic or compressed hook makes readers misunderstand the literal event or genre, rewrite with a concrete anchor in the same sentence or next sentence.
- `改成完整短篇`: convert outline or fragment into a beginning-middle-end story, not a synopsis.
- `用户反馈迭代`: classify the critique, choose one dominant failure mode, rewrite against that failure, then decide whether the lesson is only task-local or should be added to skill references.

## Boundaries

- Do not copy protected text, famous scenes, unique names, signature artifacts, or recognizable plot beats from copyrighted fiction.
- Do not copy a recognizable sequence of beats from one source. If using inspiration, change at least setting, relationship, stakes, object/rule, and ending.
- Do not present a story as "in the exact style of" a living author. Transform the request into general craft features.
- Do not use real private persons as fictional criminals, villains, abusers, or scandal subjects without clear fictionalization and safety framing.
- Do not output sexual content involving minors, explicit sexual coercion, instructions for real violence, or content that glamorizes criminal abuse.
- For sensitive requests, pivot to fictionalized, non-instructional, emotionally focused storytelling.

## Reference Files

- `references/technique-matrix.md`: technique engines and author-signal-to-craft translation.
- `references/source-research-remix.md`: search-first protocol for turning public source context into abstract remix cards.
- `references/inspiration-remix-playbook.md`: classic film/TV/novel beat cards, style-signal translation, and remix boundary.
- `references/story-engine-library.md`: emotional payoff types, high-pressure relationships, plot engines, escalation ladders, arenas, and hook bank.
- `references/prewrite-interview.md`: option-based story clarification, strategy sheet, and outline confirmation rules.
- `references/output-contract.md`: story output shape, length, and drafting rules.
- `references/anti-ai-language.md`: anti-trope language gate for formulaic AI wording.
- `references/language-routing.md`: automatic interaction-language detection, separate story-language selection, template localization, and visible-process consistency rules.
- `references/character-video-demo.md`: character-turnaround intake, native topic-choice UI, six-shot 30+30-second timeline, storyboard/video/music tool contracts, composition, and delivery quality gates.
- `references/quality-checklist.md`: seven-part self-check and repair rules.
- `references/genre-quality-rubric.md`: genre-aware quality rubrics for loading the right reader promise.
- `references/evolution-loop.md`: hook-based feedback, rule promotion, and anti-overfitting process.
- `examples/sample-01-wuxia-suspense.md`:江湖悬疑完整短篇样例与自评。
- `examples/sample-02-sci-fi-memory.md`:近未来科幻完整短篇样例与自评。
- `references/repository-validation.md`: Lumina import-format rules plus structural, sample, opening, scene, dialogue, and anti-AI validation checks.
- `LUMINA_SYSTEM_INSTRUCTIONS.md`: self-contained runtime prompt to paste into Lumina Canvas Agent System Instructions.
