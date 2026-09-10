# Repository And Story Validation

Use this checklist before importing the skill into Lumina or publishing a revised package. These checks replace executable repository scripts so every uploaded document stays within Lumina's supported formats.

## Lumina Import Format

Every uploaded document must pass all of these rules:

- Supported extensions only: `.md`, `.txt`, `.json`, `.yaml`, `.yml`.
- Each individual document contains no more than 20,000 characters.
- Extensions are lowercase.
- File and folder names contain only `A-Z`, `a-z`, `0-9`, `_`, and `-`.
- The base name of each file or folder is no more than 64 characters.
- Do not include hidden configuration files, executable scripts, images, binaries, nested `.git` directories, caches, or generated artifacts in the import package.

## Required Package Documents

Confirm that these documents exist:

- `SKILL.md`
- `LUMINA_SYSTEM_INSTRUCTIONS.md`
- `README.md`
- `LICENSE.md`
- `manifest.json`
- `agents/interface.yaml`
- `agents/openai.yaml`
- `references/language-routing.md`
- `references/character-video-demo.md`
- every document listed under `resources` in `manifest.json`

Also confirm:

- `SKILL.md` begins with closed YAML frontmatter;
- frontmatter contains `name: short-drama-creation` and a discriminating `description`;
- `manifest.json` parses as JSON and names `short-drama-creation`;
- YAML documents contain no tab indentation;
- no scaffold markers such as `TODO` remain;
- `LUMINA_SYSTEM_INSTRUCTIONS.md` contains the Lumina runtime contract, automatic language routing, prewrite stopping rule, connected-component boundary, language-aware anti-AI gate, and copyright boundary.
- `LUMINA_SYSTEM_INSTRUCTIONS.md` contains Character Video Demo Mode, the mandatory native single-choice topic UI, explicit target-duration locking, connected media capability checks, one seed storyboard, one short seed video, sequential extension of the latest complete video, prompt-local timelines starting at zero, automatic original background music, and final duration verification.
- `SKILL.md` and `LUMINA_SYSTEM_INSTRUCTIONS.md` contain the TTS risk-audit recovery rule, exact failed-chunk isolation, bounded retries, alternate-voice fallback, and the requirement to preserve successful upstream results.
- metadata declares automatic language handling rather than a fixed output language;
- metadata version and resource maps agree across `manifest.json` and `agents/interface.yaml`;
- every imported document passes the 20,000-character limit, with margin left for future maintenance;

## Sample Evidence

Each full sample should contain:

- `## 输入`
- `## 技法组合`
- `## 小说正文`
- `## 创作自评`
- visible checks for 开篇钩子、人物欲望、冲突升级、对白张力、画面感、反转/悬念、结尾余味
- enough story text to demonstrate a complete short story rather than an outline or synopsis

## Story Smoke Test

For a draft or sample, inspect these observable signals. They are diagnostics, not substitutes for editorial judgment.

1. The first three paragraphs contain an immediate disturbance such as danger, loss, shame, debt, exposure, interruption, or a concrete mystery.
2. The story contains sustained concrete scene work: people, objects, physical actions, places, and sensory details.
3. Dialogue is frequent enough for the chosen story and carries threat, test, accusation, bargain, concealment, grief, or choice.
4. Abstract language does not overwhelm concrete scene language.
5. The opening does not describe an alive person as a walking dead body unless the story intentionally establishes a supernatural rule.
6. Decorative em dashes do not become a repeated rhythm crutch.
7. The story resolves the present conflict and returns an earlier image with changed meaning.
8. Every visible planning step, status, option, error, outline, and conversational sentence uses the detected interaction language.
9. The title and story body use the selected story language, which may differ when explicitly requested.

## Character Video Demo Smoke Test

Use a disposable character turnaround and mock or low-cost connected components where possible.

First run:

1. Attach the turnaround and request a character video with an explicit duration such as 45 seconds.
2. Confirm that the Agent invokes exactly one native single-choice question with a localized title/header, four distinct topic options, concise descriptions, a recommended first option, and a free-form custom path.
3. Confirm that it explains the one-seed-video/continuous-extension/target-duration consequence of selection and stops without generating media.
4. If the native interaction action is intentionally unavailable for this test, confirm that the Agent labels the fallback and returns the equivalent localized numbered choices without pretending a UI card appeared.

Second run:

1. Return the selected option, topic card, target duration, and reference image when state is not preserved.
2. Confirm that the Agent inspects supported seed durations, extension increments, cumulative-output behavior, maximum duration, and duration metadata before generation.
3. Confirm that one seed storyboard receives the original turnaround and stable Character Lock.
4. Confirm that one short seed video is generated from the actual seed storyboard.
5. Confirm that every extension consumes the immediately previous successful complete video and returns a longer complete video.
6. Confirm that extensions run sequentially and cumulative duration is checked after each success.
7. Confirm that every seed and extension action prompt starts at `00:00` and ends at that call's own requested duration.
8. For an extension mapped globally to 30–40 seconds, confirm that the video prompt says `00:00–00:10`, while cumulative 30/40 values remain only in internal metadata or structured duration fields.
9. Confirm that independent or tail-only clips are never concatenated, looped, slowed, padded, trimmed, or presented as the final result.
10. Confirm that the final returned duration matches the explicit user target within the component's declared tolerance.
11. Confirm that original background music is embedded only through native audio support or a single-video audio mux without video concatenation, or is delivered separately with an honest label.
12. Confirm that failed tools are reported with actual errors and are never represented as successful outputs.
13. Confirm that every visible plan, status, error, and delivery note uses the detected interaction language, even when a media component requires a fixed prompt language.

Missing-tool test:

1. Replace the extension capability with a tail-only clip generator.
2. Confirm that the Agent rejects it as true extension, returns the seed video, and does not concatenate clips.
3. Request an unreachable duration and confirm that the Agent stops before generation with nearby supported durations.
4. Disconnect non-concatenating audio input or muxing and confirm that synchronized music is delivered separately and labeled as not embedded.

TTS risk-audit test:

1. Simulate an `audio risk audit` rejection for one known chunk such as `chunk 4` after at least one earlier chunk succeeds.
2. Confirm that the Agent identifies the exact rejected chunk and changes only its spoken text while preserving story meaning, character intent, timing, and emotional direction.
3. Confirm that it retries only the failed TTS call and keeps successful chunks plus all upstream image/video state.
4. On repeated rejection, confirm one progressively simpler rewrite and then an alternate available voice when the text is already neutral.
5. Confirm that downstream video generation waits for successful replacement audio.
6. After the bounded attempts fail, confirm that the Agent stops with the chunk ID and last actual error instead of restarting the workflow.

## Anti-AI Pattern Scan

Target zero narration-level hits for these patterns:

```text
不是.*而是
不在于.*在于
总之|综上所述|总而言之
关键在于|值得注意的是|让我们|想象一个世界
这不仅.*更是
这就是.*的意义
```

When a pattern appears, replace explanation with action, image, dialogue, or consequence. Keep a hit only when it is unmistakably deliberate character dialogue.

## Opening Clarity Scan

Review the first three paragraphs for combinations such as:

```text
死人.*走
尸体.*走
死者.*开口
尸体.*开口
```

If the story is not intentionally supernatural, rewrite with a literal anchor such as `快死的人`, `满身血的人`, `被误认为死人`, or `披着死者衣物的人`.

## Passing Standard

The package passes when every imported document satisfies the Lumina filename, extension, and 20,000-character rules, all declared resources exist, the runtime instructions retain the essential runtime contract, automatic language routing is consistently applied, the visual demo respects its topic gate, target-duration lock, and continuous extension chain without clip concatenation, both samples demonstrate complete fiction, and no unresolved narration-level anti-AI or misleading-opening issue remains.
