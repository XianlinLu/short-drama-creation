# Interaction Language And Topic Selector

## Language decision

Maintain two variables:

- `interaction_language`: controls everything the user sees during the workflow.
- `story_language`: controls narrative, dialogue, subtitles, and spoken lines.

Choose `interaction_language` from the newest direct user instruction. Ignore language that appears only in an attachment, quotation, pasted draft, source excerpt, code block, metadata field, proper name, or action output. For mixed messages, use an explicit language instruction; otherwise use the language carrying the latest substantive request; otherwise retain the established conversation language.

Choose `story_language` in this order:

1. explicit user instruction;
2. language of an existing draft being revised;
3. `interaction_language`.

Localize every visible heading, question, option label, option description, recommendation marker, status note, error, confirmation request, and delivery summary. If a media action accepts only a particular prompt language, translate only the hidden action parameter.

## When the selector is compulsory

Whenever the workflow reaches a point where the user must choose among two or more creative directions, invoke the native single-choice user-input action. This applies across every route and input type, including:

- character images and mixed media;
- one-line ideas, synopses, and existing drafts;
- genre, theme, emotion, or visual-direction choices;
- adaptation, revision, or research-derived directions;
- references to books, films, creators, or familiar tropes.

Do not invoke it when the user has already supplied one clear direction and explicitly asks to continue, or when the missing detail is purely technical, such as duration or aspect ratio.

## Native card contract

Create exactly one single-choice question with:

- id `topic_direction`;
- a short localized header equivalent to “Story Topic”;
- a localized question asking which creative direction the user prefers;
- exactly four mutually exclusive options;
- the strongest default first and marked recommended;
- a short label and one concise differentiating description per option;
- the runtime's built-in free-form Other path;
- the runtime's built-in ignore and submit controls.

Do not add a fifth Other option when the action supplies one automatically. Do not simulate controls using Unicode circles or checkboxes.

Build options as complete direction bundles rather than four near-synonyms. Each bundle should differ in conflict, emotional payoff, setting or visual hook, escalation pattern, and ending flavor.

Map these semantic fields to the real connected action schema:

```yaml
questions:
  - id: topic_direction
    header: localized short title
    question: localized direction question
    options:
      - label: concise recommended direction
        description: conflict, promise, and distinguishing hook
      - label: concise direction
        description: one short differentiator
      - label: concise direction
        description: one short differentiator
      - label: concise direction
        description: one short differentiator
```

## Stop behavior

After invoking the native action, end the current execution. Do not choose for the user, repeat the options in prose, build an outline, or start image, audio, music, or video generation. Continue only after the user's later submission is available.

If the action is absent or fails, retry once only when the failure is clearly transient and no card was created. Otherwise stop and report the missing capability or actual error in `interaction_language`.

There is no text fallback. Never return the choices as Markdown, JSON, a table, a paragraph, or a numbered list. Never proceed as though the user made a selection.

## Preflight check

Before returning from a direction-selection run, confirm internally:

- one native action call was made;
- it contains one question and four options;
- option one is recommended;
- native Other is enabled;
- visible copy is localized;
- no duplicate option list appears in the reply;
- no downstream generation started.
