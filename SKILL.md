---
name: short-drama-creation
description: Create original short fiction and reference-consistent character videos. Use when a user wants topic selection, story design or revision, storyboard generation, a short initial video followed by true continuous extension to a requested duration, automatic music, multilingual interaction, or targeted TTS recovery.
---

# Short Drama Creation

Build original narrative work from a character turnaround, an idea, a synopsis, or an existing draft. The skill supports text-first fiction and character-led video production while keeping every visible response in the user's language.

## Runtime entry

For Lumina Canvas Agent, use `LUMINA_SYSTEM_INSTRUCTIONS.md` as the self-contained system prompt. Repository maintenance should keep every imported file in a supported text format and under 20,000 characters.

Only rely on actions actually connected to the Agent. Never claim that an image, audio track, video, extension, duration check, or saved artifact exists unless a connected action returned it.

## Decide the route

Choose one main route for the current run:

- **Story route:** the user asks for fiction planning, drafting, continuation, analysis, or revision.
- **Character-video route:** a character turnaround or three-view image is supplied and the user asks for storyboards, motion, a short film, or a duration-controlled video.

An attached image by itself does not authorize media generation. Mixed requests may use story design internally, but the character-video state machine controls media actions.

## Shared rules

1. Read `references/interaction-and-topic-ui.md` before showing any creative choices.
2. Detect the interaction language from the latest direct user message. Apply it to visible reasoning summaries, questions, UI copy, action status, errors, and delivery notes.
3. Treat requested story language separately. An explicit story-language request wins; otherwise revisions retain the draft language; otherwise use the interaction language.
4. When two or more creative directions are possible, use the mandatory native topic selector and stop the run after invoking it.
5. Prefer original characters, settings, causality, scene order, dialogue, and visual motifs. References may contribute abstract craft functions, never protected expression.
6. Preserve completed upstream work when one downstream action fails. Retry only the failed unit when the action contract permits it.

## Story route

Read `references/story-workflow.md` for full guidance.

- For a vague request, create four genuinely different direction bundles and send them through the native topic selector. Do not draft yet.
- Once a direction is chosen, present a concise attraction strategy and outline unless the user asked to skip planning.
- Stop for confirmation after planning. On the next run, draft from the confirmed plan.
- For direct-write requests, draft immediately only when the premise and direction are already unambiguous.
- For revisions, classify the request first: structure, causality, pacing, character, dialogue, imagery, opening clarity, ending impact, or language texture. Change the smallest sufficient layer and preserve unaffected strengths.
- If the user asks for web research and a research action is connected, gather public sources, translate findings into abstract story functions, and use the topic selector if multiple new directions are offered.

Default story delivery:

```text
[localized title]
[complete story]
[brief quality note only when requested]
```

When the user requests a demonstration or explicit craft review, add localized sections for the supplied input, chosen technique mix, complete story, and concise creation self-review.

## Character-video route

Read `references/continuous-video-workflow.md` before any media call and follow its state order:

```text
character intake
→ native topic selection and stop
→ explicit duration lock
→ continuity plan
→ one seed storyboard
→ one short initial video
→ sequential true extensions of the latest complete video
→ duration verification
→ final delivery
```

Critical invariants:

- Do not generate images, speech, music, or video before the user submits a topic.
- Require a target duration from the user's direct prompt. Do not silently assume one.
- Generate one short initial video, then extend the latest successful complete video until the target is reached.
- Never compose the final result by concatenating independent clips.
- Each video action receives its own local timeline beginning at `00:00`. Never place cumulative film timecodes inside a video prompt.
- A tail-only clip is not a successful extension for this workflow.
- Do not reach the duration by looping, slowing, accelerating, freezing, padding, or silently rounding.
- Generate original instrumental background music automatically when a compatible connected action exists.
- Embed music only through a route that preserves one continuous video. Otherwise return the music separately and label it clearly.

## TTS failure handling

Read `references/audio-and-tts.md` whenever speech is requested or an audio action fails.

When an audio risk audit rejects one TTS chunk:

1. identify the exact failed chunk;
2. rewrite only its spoken text;
3. retain meaning, intent, pace, and emotional direction;
4. retry only that chunk's TTS action;
5. simplify the rejected wording progressively if it fails again;
6. when already-neutral wording is rejected, try another available voice;
7. confirm audio success before downstream video generation;
8. keep successful chunks and all completed media;
9. stop after the bounded recovery sequence and report the actual failure.

Do not restart the whole workflow unless the connected action makes targeted recovery impossible.

## Quality and safety

Read `references/originality-and-quality.md` before final delivery.

- Open on a concrete disturbance, decision, or unanswered fact rather than decorative abstraction.
- Give the protagonist a visible want and a cost for pursuing it.
- Escalate through changed circumstances, not repeated arguments.
- Make dialogue carry pressure, evasion, leverage, or subtext.
- Let images, props, and locations affect events.
- End with consequence or reinterpretation, not an explanatory summary.
- Avoid repetitive synthetic phrasing, especially narration built from formulaic contrast constructions.
- Do not imitate a living creator's recognizable style.
- Do not reproduce protected characters, dialogue, signature props, iconic shot sequences, or distinctive plot chains.
- Keep sensitive material fictionalized, non-instructional, and safe for the relevant audience.

## References

- `references/interaction-and-topic-ui.md` — language routing and the mandatory native direction card.
- `references/story-workflow.md` — original story planning, drafting, research translation, and revision.
- `references/continuous-video-workflow.md` — character intake, seed generation, true extension, local timelines, and duration checks.
- `references/audio-and-tts.md` — automatic music and rejected-chunk TTS recovery.
- `references/originality-and-quality.md` — story craft, genre promises, originality, and final review.
- `references/package-validation.md` — import, configuration, and behavioral validation.
- `examples/character-video-flow.md` — non-executable character-video acceptance example.
- `examples/story-flow.md` — non-executable multilingual story acceptance example.
