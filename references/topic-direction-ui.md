# Topic Direction UI Gate

Apply this gate in every operating mode whenever the Agent is about to ask the user to choose a creative topic or direction.

## Trigger

The gate is mandatory when the Agent would otherwise present two or more alternatives for:

- story topic, theme, premise, or concept;
- genre or emotional direction;
- character-video direction;
- classic-beat or inspiration direction;
- researched story-function combination;
- remake, adaptation, or revision direction.

The input format does not matter. Apply the same gate to a character image, one-line idea, synopsis, existing draft, named reference, search result, or mixed-media request.

Do not invoke this gate when the user has already selected one unambiguous direction and explicitly asks to proceed, or for a purely technical clarification such as missing duration, aspect ratio, or file format.

## Mandatory Native Action

Call the runtime's native interactive-question or user-input action with a single-choice configuration. Do not merely describe the UI, print radio characters, or return the options as Markdown, prose, a table, JSON, or a numbered list.

The action must create one question containing:

- stable id: `topic_direction`;
- localized short header equivalent to `故事主题`;
- one localized question asking which direction the user wants;
- exactly four mutually exclusive direction options;
- the recommended option first and visibly marked as recommended;
- one short label and one concise description per option;
- the runtime's native free-form `Other` field;
- the runtime's native submit and ignore controls.

Do not manually add a fifth `Other` option when the runtime supplies it automatically. Keep labels short and descriptions within one or two UI lines when possible.

## Language

Apply `language-routing.md` first. Localize the header, question, labels, descriptions, recommended marker, missing-capability error, and follow-up instruction to the interaction language. Ignore attachment or reference language when selecting the UI language.

## Stop Rule

Invoke the native action and end the current run. Do not select for the user, generate downstream media, draft the full story, or continue to an outline that assumes a choice.

If conversation state is not preserved, ask the user to return the selected option together with the relevant reference, prior card, and target duration when applicable.

## Fail Closed

If the native single-choice action is missing, unavailable, or fails:

1. stop at the topic-direction stage;
2. report that the required native single-choice UI capability is unavailable;
3. do not output the four topics in text;
4. do not substitute a numbered list or Markdown card;
5. do not continue as if a direction had been selected.

Retry the UI action once only when the failure is clearly transient and the retry does not duplicate a successfully created card. After a second failure, stop with the actual error.

## Semantic Shape

Adapt field names only to the connected action's real schema:

```text
questions:
  - id: topic_direction
    header: [localized equivalent of Story Topic]
    question: [localized direction question]
    options:
      - label: [recommended concise direction]
        description: [story promise, conflict, and distinguishing visual or narrative hook]
      - label: [direction]
        description: [one concise sentence]
      - label: [direction]
        description: [one concise sentence]
      - label: [direction]
        description: [one concise sentence]
```

The runtime owns the actual radio controls, Other field, submit button, ignore button, and card styling.

## Gate Check

Before returning any direction choices, verify:

- the native single-choice action was actually called;
- exactly one question was submitted;
- exactly four mutually exclusive options were supplied;
- the first option is recommended;
- no text duplicate of the option list was returned;
- no generation continued after the action;
- any failure stopped without a text fallback.
