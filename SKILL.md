---
name: short-drama-creation
description: Create original short fiction and reference-consistent character videos. Use for topic selection, story design or revision, storyboard generation, smart aspect-ratio routing, a short initial video followed by true continuous extension, automatic music, multilingual interaction, targeted TTS recovery, or bounded media policy recovery.
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

## Smart aspect ratio

Read `references/aspect-ratio-routing.md` before the initial storyboard and every video-extension call.

- Select the initial ratio from an explicit supported user choice, stated destination, composition needs, or the shared action default, in that order.
- A three-view character sheet supplies identity but does not force the final canvas ratio.
- Use the same planned orientation for the storyboard and initial video, then lock the initial video's actual metadata ratio.
- For every video-extension action, omit the `ratio` field entirely. Do not send the locked ratio, `auto`, `null`, or an empty value. The output inherits the input video's ratio.
- If an extension returns `InvalidParameter.TaskTypeConstraint` for `ratio`, preserve the latest complete video, remove that field from the failed request, and retry only the failed extension once.
- If a connector automatically reinserts the field, stop and report a connector configuration problem. Never restart earlier generation or crop, pad, stretch, or transcode to hide the error.

## Video copyright-policy recovery

Read `references/video-copyright-recovery.md` when a video action returns error code `23007`, `OutputVideoSensitiveContentDetected.PolicyViolation`, a copyright-restriction message, or an equivalent output-side policy rejection.

- Treat it as a policy rejection, not a transient network error.
- Preserve every verified upstream artifact and isolate the failed initial-video or extension call.
- Do not bypass safeguards, disguise protected names, or repeat identical requests.
- If the character reference itself appears to contain a recognizable third-party character, celebrity, logo, branded asset, film frame, poster, or watermark, stop and request an original unbranded reference.
- Otherwise, retry the failed video step once with a prompt-only originalization that removes named works, creators, brands, likeness instructions, exact-scene language, signature props, and iconic staging while preserving story meaning, emotion, duration, continuity, and the local `00:00` timeline.
- If that retry is rejected, make one final attempt using a newly staged visual expression. Change at least three visual dimensions. For an extension, keep the last verified complete video and replace only the failed next beat.
- After two compliant recovery attempts, stop the video branch and report the real error identifiers. Never loop, restart the whole workflow, or switch providers solely to evade the rejection.

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

## Audio copyright-policy recovery

Read `references/audio-copyright-recovery.md` when an audio action returns `OutputAudioSensitiveContentDetected.PolicyViolation`, a copyright-restriction message, or an equivalent output-side audio policy result. Do not confuse it with a TTS text-risk rejection that names a failed dialogue chunk.

- Preserve the complete video, all successful TTS chunks, and all unrelated audio results.
- Do not disguise rejected audio through pitch, speed, noise, reversing, slicing, codec changes, hidden names, repeated submissions, or provider hopping.
- If the input uses a recognizable third-party recording, melody, jingle, extracted soundtrack, celebrity or character voice, or unclear reference audio, stop using that reference and ask for user-created audio or continue without it.
- Retry only the failed audio step. Attempt 1 removes named songs, performers, franchises, likeness requests, lyrics, samples, and soundalike instructions, then requests a fully original cue, neutral voice, or functional sound effect.
- If rejected again, attempt 2 creates a new audio design. Music changes at least four compositional dimensions; TTS uses one different neutral non-impersonation voice; sound effects use a new synthesis concept.
- After two compliant attempts, stop the affected audio branch and report the actual identifiers. Deliver the verified video without the rejected track when music is optional and label the omission clearly.
- Never restart successful video generation because an audio result failed.

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
- `references/aspect-ratio-routing.md` — intelligent initial ratio selection, inherited extension ratio, and invalid-ratio recovery.
- `references/audio-and-tts.md` — automatic music and rejected-chunk TTS recovery.
- `references/audio-copyright-recovery.md` — bounded, non-evasive recovery for output-side audio copyright policy rejections.
- `references/video-copyright-recovery.md` — bounded, non-evasive recovery for output-side video copyright policy rejections.
- `references/originality-and-quality.md` — story craft, genre promises, originality, and final review.
- `references/package-validation.md` — import, configuration, and behavioral validation.
- `examples/character-video-flow.md` — non-executable character-video acceptance example.
- `examples/story-flow.md` — non-executable multilingual story acceptance example.
