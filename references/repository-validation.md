# Repository And Story Validation

Use this checklist before importing the skill into Lumina or publishing a revised package. These checks replace executable repository scripts so every uploaded document stays within Lumina's supported formats.

## Lumina Import Format

Every uploaded document must pass all of these rules:

- Supported extensions only: `.md`, `.txt`, `.json`, `.yaml`, `.yml`.
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
- every document listed under `resources` in `manifest.json`

Also confirm:

- `SKILL.md` begins with closed YAML frontmatter;
- frontmatter contains `name: lumina-novel-generator` and a discriminating `description`;
- `manifest.json` parses as JSON and names `lumina-novel-generator`;
- YAML documents contain no tab indentation;
- no scaffold markers such as `TODO` remain;
- `LUMINA_SYSTEM_INSTRUCTIONS.md` contains the Lumina runtime contract, automatic language routing, prewrite stopping rule, connected-component boundary, language-aware anti-AI gate, and copyright boundary.
- metadata declares automatic language handling rather than a fixed output language;

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

The package passes when every imported document satisfies the Lumina filename and extension rules, all declared resources exist, the runtime instructions are self-contained, automatic language routing is declared and consistently applied, both samples demonstrate complete fiction, and no unresolved narration-level anti-AI or misleading-opening issue remains.
