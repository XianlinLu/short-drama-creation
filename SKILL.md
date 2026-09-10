---
name: short-drama-creation
description: |
  Run inside a Lumina Canvas Agent to design, draft, and revise original fiction, or to turn an attached character turnaround into an interactive topic choice, one short seed video, and a continuously extended final video matching the duration written by the user. Use for story revision, character storyboards, or duration-controlled short-drama videos without multi-clip concatenation, copying protected work, or imitating a living creator's distinctive style.
---

# Short Drama Creation

在 Lumina 画布 Agent 中，把人物三视图或故事灵感转化为原创短剧选题、种子分镜、短小的初始视频和按用户指定时长连续延长的最终视频；也可自动生成原创背景音乐，并完成短剧故事、台词与叙事改写。

Adapted for Lumina Canvas Agent by XianlinLu.
Based on `qiaomu-novel-generator` by 向阳乔木 / joeseesun under the MIT License.
Source: https://github.com/joeseesun/qiaomu-novel-generator

## Lumina Canvas Runtime

This repository is the maintainable source package. Lumina Canvas does not load these repository files dynamically, so use `LUMINA_SYSTEM_INSTRUCTIONS.md` as the runtime entrypoint: paste its complete contents into the Agent node's **System Instructions** field.

- Put the user's request in the Agent **Task Prompt**. When a String node is connected, reference it with Lumina's `@` syntax.
- At the start of every run, apply `references/language-routing.md`. Detect the language of the user's latest direct request and use it for every visible plan, progress summary, option, tool status, error, outline, self-check, and reply. Keep the requested story language as a separate decision.
- When a character turnaround or three-view image is attached and the user requests a storyboard, video demo, short film, or duration-controlled result, route to `references/character-video-demo.md` before using the fiction workflow.
- In every mode, whenever the Agent offers creative topic or direction choices, apply `references/topic-direction-ui.md`: invoke the runtime's native single-choice user-input action and stop. Never render topic directions as Markdown, prose, tables, JSON, or numbered text. If the native action is unavailable, stop with the missing-capability error and do not provide a text fallback.
- In Character Video Demo Mode, after the user selects through the mandatory native topic UI, lock the duration from the user's direct prompt, generate one short seed video, then extend only the latest successful full video until that duration is verified. Never fall back to standalone-clip concatenation.
- Treat every video-generation prompt as an independent local timeline. Start its visible time instructions at `00:00` and end at that call's seed or added duration. Keep full-film or cumulative timecodes only in internal planning, structured duration parameters, and progress metadata; never write ranges such as `00:30–00:40` inside a 10-second video prompt.
- Automatically generate original instrumental background music when connected. Embed it only through native video audio conditioning or a single-video audio mux that does not concatenate or retime the extended video; otherwise deliver the synchronized music separately and label it honestly.
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
- **Character Video Demo Mode**: use `references/character-video-demo.md` when the user supplies a character turnaround or three-view image and asks for a storyboard, video demo, short film, or duration-controlled visual result.

If both are requested, Character Video Demo Mode may use the fiction engines to create the micro-story, but its topic-choice stop point, target-duration lock, continuous extension chain, and duration checks take priority. An image attachment alone does not activate this mode.

## Universal Topic Direction UI Gate

Before presenting two or more creative topics, themes, premises, genres, emotional directions, inspiration combinations, research-derived directions, or revision directions, read and apply `references/topic-direction-ui.md`. This gate applies regardless of input form or operating mode.

Use exactly one native single-choice question with four mutually exclusive options, a recommended first option, localized header/question/descriptions, and the runtime-provided Other field. Invoke the action and end the run. Do not duplicate its options in text.

If the native single-choice action is missing or fails after one safe transient retry, stop and report the actual missing capability or error. Text lists are prohibited at this gate.

Fiction Mode defaults:

- The user ultimately wants a complete story in the selected story language, but a vague premise should be clarified through the mandatory native topic-direction UI before drafting.
- Default output length is 1800-4000 Chinese characters for Chinese, or an equivalent short-story length in the selected story language, unless the user asks for a different length.
- Default goal is reader compulsion: make the first page impossible to ignore, then keep raising the cost of every choice.
- Treat named writers, films, TV shows, novels, and famous scenes as craft signals, not copy targets. Convert them into general techniques such as suspense, restraint, moral pressure, voice economy, scene rhythm, reversal, public proof, and emotional payoffs.
- Do not directly imitate a living author's distinctive style. Do not copy protected passages, famous scenes, character names, signature lines, setting names, or recognizable plot sequences.
- When the user gives a plot idea and multiple inspiration directions would help, build four original combinations and present them through the mandatory native topic UI before outlining.
- When the user asks to search first or names a specific modern work whose public context matters, use search if available, extract only public premise/craft signals, cite sources when reporting, and then remix through abstract story functions.
- If the user provides an existing draft, preserve useful facts, promises, and character intent; rewrite the narrative engine instead of merely polishing adjectives.
- Avoid formulaic AI wording in visible prose, especially `不是X，而是Y`, unless it is a deliberate character voice.
- A strong opening hook must not create the wrong genre contract. If a line sounds supernatural, impossible, or metaphorical, immediately anchor the literal situation unless the story is intentionally supernatural.

## Hooked Workflow

Use hooks as fixed checkpoints. They are conceptual hooks, not mandatory runtime APIs. Their job is to keep the skill evolvable without hard-coding one user's feedback or one story's content into the core rules.

1. Intent Hook: identify the input type, target genre, reader promise, premise clarity, and whether the user has explicitly approved drafting.
2. Language Routing Hook: use `references/language-routing.md` to choose the interaction language and story language. Never infer the interaction language from quoted or attached content when the user's direct instruction uses another language.
3. Mode Routing Hook: if Character Video Demo Mode applies, follow `references/character-video-demo.md`. Read the reference image, invoke one native single-choice UI card with four topic directions, and stop. After the user's later selection, require an explicit target duration, generate one seed storyboard and one short seed video, then pass each successful full video into the next extension call until the requested duration is verified. Each call's video prompt starts at local `00:00`; cumulative time stays outside the prompt. Do not concatenate independent clips. Do not continue through fiction-only hooks unless useful internally for the micro-story.
4. Source Research Hook: when the user explicitly asks to search or names a reference that needs current/public context, use `references/source-research-remix.md` to gather 3-6 public signals and reduce them to a Research Intake Card.
5. Inspiration Remix Hook: when the user gives a plot, genre, trope, or named work/writer, use `references/inspiration-remix-playbook.md` to build classic beat/style-signal directions and present them only through the universal native topic UI before outlining.
6. Story Engine Library Hook: select the primary emotional payoff, high-pressure relationship, conflict arena, 2-4 plot engines, escalation ladder, and hook mode from `references/story-engine-library.md`.
7. Prewrite Interview Hook: if the request is vague, follow `references/prewrite-interview.md` and present four bundled story directions through the universal native topic UI. Do not use numbered text choices or draft the full story yet.
8. Story Strategy Hook: if a creative direction is still undecided, invoke the universal native topic UI and stop. If one direction is already selected but the outline is not confirmed, provide the localized attraction strategy and compact outline, then wait for confirmation unless the user explicitly asked to skip discussion.
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

For a vague new-story request, invoke the native single-choice action with four complete story directions. Localize all fields to the interaction language, put the recommended option first, let the runtime provide Other, and stop immediately after the action. Never output a topic list or compact reply code in text.

For a usable premise with one selected direction that has not been confirmed, output the following semantic structure localized to the interaction language. If multiple directions would be shown, invoke the native topic UI instead and stop.

```text
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
- `references/topic-direction-ui.md`: universal mandatory native single-choice gate for every creative topic or direction selection, including fail-closed behavior with no text fallback.
- `references/character-video-demo.md`: character-turnaround intake, native topic-choice UI, target-duration parsing, seed-video generation, sequential video-extension contracts, audio handling, and delivery quality gates.
- `references/quality-checklist.md`: seven-part self-check and repair rules.
- `references/genre-quality-rubric.md`: genre-aware quality rubrics for loading the right reader promise.
- `references/evolution-loop.md`: hook-based feedback, rule promotion, and anti-overfitting process.
- `examples/sample-01-wuxia-suspense.md`:江湖悬疑完整短篇样例与自评。
- `examples/sample-02-sci-fi-memory.md`:近未来科幻完整短篇样例与自评。
- `references/repository-validation.md`: Lumina import-format rules plus structural, sample, opening, scene, dialogue, and anti-AI validation checks.
- `LUMINA_SYSTEM_INSTRUCTIONS.md`: self-contained runtime prompt to paste into Lumina Canvas Agent System Instructions.
