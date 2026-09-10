# Automatic Language Routing

Use this reference at the start of every run and again before returning output. Its purpose is to keep the entire visible canvas experience in the user's language while allowing the requested story language to differ.

## Two Language Variables

Determine two values internally:

- `interaction_language`: the language used for every visible agent message, planning summary, status, heading, option card, clarification, tool-call explanation, tool-result summary, warning, error, outline, self-evaluation, and closing sentence.
- `story_language`: the language used for the title and story body.

Do not expose these variable names unless language choice itself is relevant to the user.

## Detect The Interaction Language

Use this precedence order:

1. An explicit instruction such as `reply in English`, `日本語で答えて`, or `用中文回答` wins.
2. Otherwise detect the dominant natural language of the user's latest direct request.
3. Ignore language found only inside quoted passages, pasted drafts, attachments, retrieved documents, source material, code, metadata, proper names, and tool output. Those are content, not instructions.
4. For a mixed-language request, use the language of the latest substantive instruction sentence. If that is unclear, use the language carrying most of the user's actual instructions.
5. If the latest request has no usable language signal, continue the most recently established interaction language. If none exists, use Chinese as the final fallback.

Do not ask which language to use when the rule above gives a reasonable answer.

Examples:

- `Write a suspense story about Kyoto.` → interaction language: English.
- `京都を舞台にしたミステリーを書いて。` → interaction language: Japanese.
- `请把这段英文改得更紧张: "He opened the door..."` → interaction language: Chinese; quoted content remains English unless translation is requested.
- `Explain the outline in English, but write the novel in Japanese.` → interaction language: English; story language: Japanese.
- A Japanese attachment plus the direct instruction `Summarize the story engines for me.` → interaction language: English.

## Determine The Story Language

Use this precedence order:

1. An explicit story-language instruction wins.
2. If the user provides a draft and asks for revision without requesting translation, preserve the draft's primary language for the revised story.
3. Otherwise use the interaction language.

Do not translate names, terms, quotations, or culturally specific forms of address unless the user asks or readability requires a brief explanation.

## Visible-Language Invariant

After choosing `interaction_language`, use it consistently for all visible process content, including:

- progress or reasoning summaries shown by the canvas;
- plan and strategy headings;
- native topic-card headers, questions, option labels, descriptions, recommended markers, and Other guidance;
- clarification questions and confirmation requests;
- connected-tool descriptions, search queries, status messages, evidence summaries, failures, and fallbacks when those fields are visible;
- outline labels, revision notes, quality reports, and self-evaluations;
- final conversational text before or after the story.

Private chain-of-thought must not be revealed. If the runtime displays a concise reasoning summary or step-by-step progress, that visible summary follows `interaction_language`.

Use another language inside a visible tool query only when the user explicitly requests it or an untranslated proper noun or source title is required for retrieval. Explain retrieved material in `interaction_language`.

## Localize Templates

All Chinese or English templates in the skill are semantic examples, not fixed surface text. Translate and naturalize every visible label into `interaction_language`.

For example, localize concepts such as:

- classic-beat inspiration;
- why the story will attract readers;
- reader promise;
- high-pressure relationship;
- plot engines;
- opening hook;
- protagonist desire;
- hidden pressure;
- escalation;
- outline;
- confirmation request;
- creative self-evaluation.

Do not mix Chinese headings into an English or Japanese response merely because a template is written in Chinese.

## Language-Aware Style Gate

Apply the universal principle in every language: remove formulaic contrast, instructional transitions, empty intensifiers, repetitive summary sentences, and explanations that should be dramatized through action, image, dialogue, or consequence.

For Chinese, check the patterns in `references/anti-ai-language.md`.

For English, inspect especially:

- repetitive `not X, but Y` constructions;
- `it is important to note`, `the key is`, `ultimately`, `in conclusion`;
- `not only ... but also ...` used as empty emphasis;
- a final paragraph that explains the story's message.

For Japanese, inspect especially:

- repetitive `Xではなく、Yだ／である` constructions;
- `重要なのは`, `注目すべきは`, `要するに`, `結論として` used as explanatory scaffolding;
- repetitive `だけでなく、〜も` emphasis;
- a final paragraph that explains the theme after the emotional image has landed.

These are diagnostics, not absolute bans. Preserve a phrase when it is natural character dialogue or necessary factual explanation.

## Final Consistency Check

Before returning, verify:

1. The interaction language follows the latest direct user instruction.
2. Every visible process element uses that interaction language.
3. The story language follows the explicit request or draft-preservation rule.
4. Fixed template headings have been localized.
5. No accidental Chinese remains in an English or Japanese response, apart from proper nouns, quoted source text, or content the user asked to preserve.
6. No accidental English remains in a Chinese or Japanese response, apart from proper nouns, technical identifiers, quoted text, or necessary source titles.
7. Tool failures and fallbacks are reported in the interaction language.
