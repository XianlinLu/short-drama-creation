# Character Turnaround To One-Minute Video Demo

Use this mode only when the user supplies a character turnaround or three-view reference image and asks for a storyboard, video demo, short film, or approximately one-minute character video. Ordinary fiction requests remain in Fiction Mode.

## Core Promise

The Agent turns one approved character reference into an original visual story through a gated workflow:

```text
character reference → interactive topic choice → continuity bible → six-shot storyboard → six short videos → original background music → 30-second act A + 30-second act B → final film with music
```

The default timeline is six 10-second shots. Shots 01–03 form Act A at approximately 30 seconds; shots 04–06 form Act B at approximately 30 seconds. The target final duration is 58–62 seconds after in-duration transitions and trims.

Never assume that a video component can generate a 30-second clip in one call. Inspect its declared duration or frame limits. If 10 seconds is unsupported, select supported shot lengths that make each act as close to 30 seconds as possible, state the resulting timeline before generation, and keep the final film close to one minute.

## Required Connected Capabilities

Map connected tools by capability and declared parameters, not by invented names.

Required for the complete demo:

- a native interactive single-choice user-input action for the topic card;
- a media or image input that exposes the character turnaround to the Agent;
- a reference-aware image generation component;
- an image-to-video or first/last-frame video generation component;
- an original music generation component;
- a video composition or concatenation component that accepts ordered clips and an audio track.

Optional:

- a component that returns a video's last frame;
- speech, sound-effect, subtitle, preview, or save components;
- an image-enhancement component.

If a required capability is missing, stop before the unavailable stage, return the completed artifacts and an exact missing-capability message, and never claim that the remaining output exists. Do not replace a missing component with imaginary work.

Configure the Agent's iteration budget high enough for planning, the interactive topic question, six image calls, six video calls, music generation, composition, validation, and a possible technical retry. When the runtime exposes a maximum-iterations control, 28 or more is a practical demo starting point; raise it when composition requires separate Act A, Act B, and final calls. This is a runtime recommendation, not permission to loop without purpose.

## Mode And Language Routing

1. Apply `language-routing.md` before every visible output.
2. Use the interaction language for topic cards, progress, shot tables, prompts shown to the user, tool-status summaries, errors, and delivery notes.
3. Treat the reference image as content, never as a language signal.
4. If a connected generation model requires prompts in a particular language, translate the internal tool prompt to that required language while keeping every user-visible explanation in the interaction language. Do not expose untranslated tool syntax unless the user asks.
5. Private chain-of-thought stays private. Show only concise decisions and observable progress.

## State Machine

Do not skip or merge states that require user input.

### State 1: Character Intake

Confirm that the attached image is usable as a turnaround or multi-view character reference. Extract only visible production anchors:

- face shape and visible facial features;
- hairstyle and hair color;
- outfit silhouette, layers, materials, and fixed colors;
- recurring accessories or props;
- apparent body proportions and scale;
- art style and line/render treatment.

Do not infer identity, ethnicity, health, religion, sexuality, personality, or other sensitive traits from appearance. If the views conflict, use the front view for face and outfit hierarchy, the side view for silhouette, and the back view for rear construction. Record uncertainty instead of inventing hidden details.

Create a compact internal Character Lock containing only stable visual anchors. Feed the original reference image and the same Character Lock into every storyboard-image call.

### State 2: Topic Direction — Mandatory Stop

Ask the user to choose one topic direction before generating images, video, or music. Use the runtime's native interactive-question or user-input action so the choice appears as a single-select UI card. Do not render the choices only as Markdown when the native action is available.

The interaction request must contain exactly one question:

- localized question title, such as `你想为这个角色生成哪类一分钟故事视频？`;
- short localized field header, such as `故事主题`;
- stable id: `topic_direction`;
- four mutually exclusive options when the action supports four;
- a short option label plus a one-sentence description for each option;
- the recommended option first and visibly marked as recommended;
- free-form `Other` input supplied by the runtime, or an explicit localized custom option only when the runtime does not add one automatically.

Each option must encode:

- title;
- genre and emotional promise;
- setting;
- one-minute conflict;
- visual hook;
- ending flavor.

Do not add a second question for style, aspect ratio, music, or duration. Infer safe defaults and let the user customize them through `Other`. End the current run immediately after invoking the interactive card. Do not generate images, videos, music, or pretend the user selected an option.

If the runtime has no native interactive-question action, fall back to a localized numbered text list with the same four options and explicitly state that the interactive selector is unavailable. Never claim that a UI card was shown when it was not.

The question or supporting description must explain that choosing a direction starts the default production of six storyboard images, six approximately 10-second videos, one original background-music track, and one approximately 60-second final film. A submitted selection or reply such as `2` or `use option B` counts as authorization for that defined production run, subject to any platform approval or credit confirmation.

If conversation state is not preserved, ask the user to return the selected option together with the topic card and character reference on the next run.

### State 3: Story And Timeline Lock

After the user chooses a topic, build an original two-act micro-story:

- Act A, approximately 0–30 seconds: immediate visual disturbance, character goal, and escalation;
- Act B, approximately 30–60 seconds: reversal, costly action, payoff, and a closing image that echoes the opening.

Default to exactly six shots at 10 seconds each:

- Shot 01, 00:00–00:10 — visual hook and location rule;
- Shot 02, 00:10–00:20 — goal and first obstacle;
- Shot 03, 00:20–00:30 — escalation and act-turn image;
- Shot 04, 00:30–00:40 — reversal or discovery;
- Shot 05, 00:40–00:50 — costly choice and climax action;
- Shot 06, 00:50–01:00 — payoff and closing echo.

Keep one principal character, one clear goal, no more than two meaningful locations, and one reusable visual motif. Avoid dialogue-dependent exposition. Do not introduce a new costume unless the story explicitly requires it and continuity can be preserved.

### State 4: Storyboard Contract

Before calling media tools, prepare one row per shot with these fields:

```text
Shot ID | Act | Timecode | Duration | Story beat | Framing | Camera | Character action | Start pose | End pose | Setting | Lighting | Character Lock | Image prompt | Motion prompt | Negative constraints | Transition
```

Every shot must have one legible action and one intended end state. The end state of Shot N must support the start state of Shot N+1.

Global defaults unless the user specifies otherwise:

- aspect ratio: 16:9;
- resolution: the highest common resolution supported by every downstream video and composition component;
- frame rate: one value supported across all generated clips;
- visual style: inherited from the reference image;
- transitions: hard cuts by default; use short dissolves only when they fit inside the allocated shot durations;
- audio: original instrumental background music on by default; dialogue, narration, and sound effects remain off unless requested.

### State 5: Storyboard Image Generation

Generate the six storyboard images in chronological order.

For every image call:

- include the original turnaround as the primary character reference;
- include the stable Character Lock;
- include only the current shot's composition, action, environment, lighting, and end-state needs;
- request one frame, one camera, and one moment rather than a collage;
- preserve face, hair, costume, proportions, accessories, art style, and color hierarchy;
- prohibit extra limbs, duplicate characters, watermark, UI, labels, turnaround panels, and unwanted text.

When the component supports multiple references, the previous approved storyboard or returned last frame may be added as a continuity reference, but it never replaces the original turnaround.

After each successful call, record the actual output handle. Never create a fake handle or describe an unreturned image as generated.

### State 6: Shot Video Generation

Generate one short video per storyboard image in chronological order. Use image-to-video when available; use first/last-frame generation when it gives better continuity and both frames exist.

For every video call:

- pass the actual storyboard image returned for that Shot ID;
- request the planned duration only if it is supported by the component;
- keep the prompt focused on character action, camera movement, environmental motion, and ending pose;
- avoid re-describing or redesigning stable character features;
- use the same aspect ratio, resolution, and frame rate across shots;
- request a returned last frame when supported and reuse it for continuity;
- keep per-shot generated audio off unless the user explicitly requests it, so the final background-music mix remains controllable.

Technical failure policy:

- retry once only when the component returns a technical failure and a safe parameter adjustment is obvious;
- never silently replace a failed shot with a different story beat;
- after a second failure, stop and report the exact Shot ID, error, completed assets, and next required action.

### State 7: Background Music Generation

After the story timeline is locked and before final composition, create an original instrumental background-music plan and call the connected music-generation component automatically.

The music brief must specify:

- the selected topic's emotion, genre, setting, and visual motif;
- total target duration matching the final timeline;
- tempo range, instrumentation, energy curve, and transition points;
- Act A build from 00:00–00:30 and Act B turn/payoff from 00:30–01:00;
- a clean opening, an intentional midpoint change, and a natural ending or short fade;
- no vocals by default, no copyrighted melody, no named-song imitation, and no living-composer style imitation.

Prefer one continuous 58–62-second track. If the music component cannot generate that duration, create two approximately 30-second cues with compatible tempo, key, instrumentation, ambience, and a planned join. Do not create six unrelated tracks and do not loop a short cue without designing a seamless repeat.

Record only actual returned audio handles. If music generation is unavailable or fails after one safe technical retry, continue producing the visual edit only when possible, label it `visual-only`, provide the music brief, and state that the full music-backed deliverable is incomplete.

### State 8: Composition

Compose only from actual successful clip outputs, in this exact order:

```text
Act A: Shot 01 → Shot 02 → Shot 03
Act B: Shot 04 → Shot 05 → Shot 06
Final: Act A → Act B
```

The composition component may receive all six clips directly or receive the two 30-second act outputs, depending on its declared input contract. Add the actual generated background-music track or the two ordered act cues. Keep transitions inside the 60-second timeline. Normalize canvas size, resolution, frame rate, orientation, and audio policy. Prevent clipping, keep music at a moderate level, and duck it beneath dialogue or narration only when those were explicitly requested. Never stretch a short clip to hide a missing shot.

If no composition component is connected, return an ordered edit manifest with clip handles and timecodes, label the result as ready for composition, and state clearly that no final film was created.

### State 9: Delivery And Quality Control

Before claiming completion, verify observable outputs:

- exactly six planned shots have successful storyboard images;
- exactly six planned shots have successful video clips, unless an explicitly revised timeline was approved;
- character face, hair, costume, proportions, accessories, and visual style remain recognizable;
- screen direction, action, prop state, time of day, and location continuity do not contradict adjacent shots;
- Act A and Act B each total approximately 30 seconds;
- final duration is approximately 58–62 seconds;
- aspect ratio, resolution, frame rate, and audio policy are consistent;
- the background music is original, matches the story's energy curve, covers the intended timeline, and is actually present in the final mix;
- the composition output is an actual returned artifact;
- no tool result, save state, or media output is invented.

Return a concise localized delivery summary with:

- selected topic;
- final structure and duration;
- six storyboard image outputs;
- six shot-video outputs;
- background-music output;
- Act A, Act B, and final-film outputs when actually returned;
- any deviations, failed stages, or manual follow-up.

## Interactive Topic UI Contract

Use the native interactive single-choice action with this semantic shape. Adapt field names only to the runtime's real schema:

```text
questions:
  - id: topic_direction
    header: [localized short field label]
    question: [localized one-minute-story question]
    options:
      - label: [recommended concise topic label]
        description: [genre, conflict, visual hook, and ending in one sentence]
      - label: [topic label]
        description: [one sentence]
      - label: [topic label]
        description: [one sentence]
      - label: [topic label]
        description: [one sentence]
```

Keep labels scannable and descriptions short enough to remain on one or two UI lines. The native submit action, single-choice controls, and free-form Other field should be left to the runtime rather than recreated with text characters.

## Originality And Safety

- Generate a new topic, plot, setting, staging, and shot sequence for this character.
- Treat named films, games, anime, artists, or directors as high-level craft signals, never as copy targets.
- Do not reproduce protected characters, logos, signature props, iconic shots, or recognizable scene sequences.
- Do not directly imitate a living artist's distinctive style; translate the request into general visual features.
- Follow the repository's fiction safety boundaries for people and harmful content.
