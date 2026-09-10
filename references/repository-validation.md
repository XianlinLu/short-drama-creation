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
- frontmatter contains `name: lumina-novel-generator` and a discriminating `description`;
- `manifest.json` parses as JSON and names `lumina-novel-generator`;
- YAML documents contain no tab indentation;
- no scaffold markers such as `TODO` remain;
- `LUMINA_SYSTEM_INSTRUCTIONS.md` contains the Lumina runtime contract, automatic language routing, prewrite stopping rule, connected-component boundary, language-aware anti-AI gate, and copyright boundary.
- `LUMINA_SYSTEM_INSTRUCTIONS.md` contains Character Video Demo Mode, the mandatory topic-choice stop, connected media capability checks, the default six-shot timeline, character continuity rules, ordered composition, and final duration verification.
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

1. Attach the turnaround and request a one-minute character video.
2. Confirm that the Agent returns exactly four distinct topic directions in the interaction language.
3. Confirm that it recommends one direction, explains the six-image/six-video/approximately-60-second consequence of selection, and stops without generating media.

Second run:

1. Return the selected option, topic card, and reference image when state is not preserved.
2. Confirm that the planned timeline defaults to six 10-second shots only when the connected video component accepts 10 seconds.
3. Confirm that every storyboard call receives the original turnaround and stable Character Lock.
4. Confirm that each video call receives the matching actual storyboard output.
5. Confirm that failed tools are reported with actual errors and are never represented as successful outputs.
6. Confirm that clips are composed strictly in Shot 01–06 order.
7. Confirm that Act A and Act B each total approximately 30 seconds and that the final output is approximately 58–62 seconds.
8. Confirm that every visible plan, status, error, and delivery note uses the detected interaction language, even when a media component requires a fixed prompt language.

Missing-tool test:

1. Disconnect the composition capability.
2. Confirm that the Agent returns an ordered clip manifest when all shot clips exist.
3. Confirm that it explicitly says no final film was created.

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

The package passes when every imported document satisfies the Lumina filename, extension, and 20,000-character rules, all declared resources exist, the runtime instructions retain the essential runtime contract, automatic language routing is declared and consistently applied, the visual demo respects its topic gate and connected-tool contracts, both samples demonstrate complete fiction, and no unresolved narration-level anti-AI or misleading-opening issue remains.
