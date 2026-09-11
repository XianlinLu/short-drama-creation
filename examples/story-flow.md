# Story Workflow Acceptance Example

This document tests routing and originality; it is not prose to copy into user output.

## Vague request

```text
Write a short story about a night-shift archivist who receives tomorrow's incident report.
```

Expected behavior:

- all visible interaction is English;
- four distinct story directions appear only in one native single-choice card;
- no numbered or Markdown alternative is returned;
- the execution ends after the card is invoked.

## After selection

The user chooses a direction about preventing an accident that the report says the archivist caused. The next execution presents a concise attraction strategy and causal outline, then requests confirmation unless the user explicitly asks to draft immediately.

The final story should establish the archive and impossible report literally, give the archivist an observable goal, escalate through changed evidence and institutional pressure, make dialogue carry leverage, and end with a consequence that reframes the report.

## Revision failure case

If the user says in Chinese that the English opening is confusing, the workflow responds in Chinese while preserving the story's English language. It repairs opening clarity without unnecessarily changing the later plot.

## Pass conditions

- interaction and story languages are resolved independently;
- vague direction selection uses the native card;
- planning and drafting respect the requested stop point;
- revision changes the narrowest sufficient layer;
- characters, causality, wording, and imagery are original;
- the finished story has a clear opening, escalating pressure, and consequential ending.
