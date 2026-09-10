# Character Turnaround To Extended Short-Drama Video

Use this mode only when the user supplies a character turnaround or three-view reference image and asks for a storyboard, video demo, short film, or duration-controlled character video. Ordinary fiction requests remain in Fiction Mode.

## Core Promise

The Agent creates one continuous video lineage instead of generating independent clips and concatenating them:

```text
character reference → interactive topic choice → target-duration lock → continuity bible → seed storyboard → short seed video → extend the latest video repeatedly → duration-verified final video
```

The user's explicit target duration is authoritative. Each extension must use the previous successful video as its source and return a longer continuous video that already contains the earlier material. Never create several standalone clips and splice, concatenate, or reorder them to claim the final result.

## Required Connected Capabilities

Map connected tools by capability and declared parameters, not by invented names.

Required for the complete visual demo:

- a native interactive single-choice user-input action for the topic card;
- a media or image input that exposes the character turnaround;
- a reference-aware image generation component for the seed frame;
- an image-to-video component for the initial short video;
- a true video-extension or video-continuation component that accepts an existing video and returns a longer continuous video;
- duration metadata or another reliable way to verify each returned video's cumulative duration.

Optional:

- original music generation;
- native video-audio conditioning or a single-video audio muxer that does not concatenate or retime video;
- speech, sound-effect, subtitle, preview, save, last-frame, or enhancement components.

A component that returns only a new tail clip is not a video-extension capability for this workflow because using it would require concatenation. If true extension is unavailable, stop after the seed video, return the actual seed output, identify the missing capability, and do not fall back to clip composition.

Before generation, calculate the required call count from the component's supported seed durations, extension increments, maximum cumulative duration, and the user's target. Keep automated extension attempts bounded. If more than 12 extension calls are required, stop before media generation and ask the user to shorten the duration or explicitly approve the larger execution budget.

## Mode And Language Routing

1. Apply `language-routing.md` before every visible output.
2. Use the interaction language for topic cards, duration questions, progress, plans, errors, and delivery notes.
3. Treat the reference image as content, never as a language signal.
4. If a media model requires prompts in a fixed language, translate only the internal tool prompt.
5. Keep private chain-of-thought private; show concise decisions and observable progress only.

## State Machine

Do not skip or merge states that require user input.

### State 1: Character Intake

Confirm that the attached image is usable as a turnaround or multi-view reference. Extract only visible production anchors:

- face shape and visible facial features;
- hairstyle and hair color;
- outfit silhouette, layers, materials, and fixed colors;
- recurring accessories or props;
- apparent body proportions and scale;
- art style and render treatment.

Do not infer identity, ethnicity, health, religion, sexuality, personality, or other sensitive traits from appearance. When views conflict, use the front view for face and outfit hierarchy, the side view for silhouette, and the back view for rear construction. Record uncertainty rather than inventing hidden details.

Create a compact internal Character Lock. Reuse the original reference and this same lock for the seed storyboard and every extension call that accepts image references or prompt context.

### State 2: Topic Direction — Mandatory Stop

Before any image, video, or music generation, ask the user to choose one topic direction with the runtime's native single-choice action. Use exactly one localized question with stable id `topic_direction`, a short header, four mutually exclusive original options, a recommended first option, concise descriptions, and the runtime's free-form Other path.

Each option encodes a title, genre and emotional promise, setting, target-duration conflict, visual hook, and ending flavor. The card must explain that selection starts one short seed video followed by continuous extension to the requested duration. Do not promise six clips, two 30-second acts, or video concatenation.

Do not add a second UI question in the same call. End the run after showing the topic card. If the runtime lacks native interactive input, return equivalent localized numbered options and label the fallback honestly.

If conversation state is not preserved, ask the user to return the selection, topic card, target duration, and original character reference on the next run.

### State 3: Target Duration Lock

Read duration only from the user's direct request or later selection message. Accept clear formats such as `45 seconds`, `60秒`, `1分30秒`, or `00:45`. Normalize the value internally to positive seconds while preserving the user's displayed format.

If no duration is present, ask one concise localized duration question and stop before media generation. Do not silently default to one minute.

Inspect the connected components before planning:

- supported initial video durations;
- supported extension increments or target-duration parameters;
- whether an extension output is cumulative or tail-only;
- maximum cumulative duration;
- aspect ratio, resolution, frame-rate, and audio limits;
- returned duration metadata and tolerance.

Choose the shortest supported seed duration that is narratively usable and leaves an exactly reachable remainder. Build a sequential extension schedule whose final cumulative duration equals the requested duration within the component's declared frame or duration tolerance.

If the target is shorter than the minimum seed duration, exceeds maximum cumulative duration, or cannot be reached from supported increments, stop before media generation. Show the nearest supported durations and ask the user to choose; never silently round, overshoot, trim, slow, loop, or splice.

### State 4: Story And Continuation Map

Design one original visual micro-story scaled to the locked duration. Use proportional beats rather than a fixed six-shot timeline:

- 0–15%: immediate visual hook and setting rule;
- 15–40%: character goal, obstacle, and escalation;
- 40–65%: discovery or reversal;
- 65–85%: costly choice and climax action;
- 85–100%: payoff and closing echo.

Keep one principal character, one clear goal, no more than two meaningful locations, one recurring visual motif, and one stable costume unless change is essential. Avoid dialogue-dependent exposition.

Create a continuation map with one row for the seed and one for each planned extension:

```text
Stage ID | Input duration | Added duration | Cumulative duration | Story beat | Camera | Action | Start state | End state | Setting | Lighting | Character Lock | Continuation prompt | Negative constraints
```

Every stage must begin from the previous returned video's real end state. Use the same aspect ratio, resolution, frame rate, character lock, visual style, and audio policy throughout.

### State 5: Seed Storyboard And Optional Audio

Generate one seed storyboard image, not six unrelated scene images. Pass the original turnaround and Character Lock. Request one frame, one camera, and one moment. Preserve the character's face, hair, costume, proportions, fixed accessories, style, and color hierarchy. Prohibit extra limbs, duplicate subjects, watermarks, interface elements, labels, turnaround panels, and unwanted text.

If dialogue or narration is requested, finish TTS before video generation. Apply the TTS Risk-Audit Recovery rule in `SKILL.md`: isolate the rejected chunk, rewrite only that spoken text, preserve successful chunks and media, and confirm the replacement audio before continuing.

For background music, automatically create one original instrumental brief matching the target duration and story energy curve. Prefer either:

1. native soundtrack or audio conditioning supported by the seed/extension component; or
2. a single-video audio mux after visual extension, only when it adds audio to the one final extended video without concatenating, trimming, retiming, or replacing its visuals.

If neither route exists, generate the music as a separate synchronized deliverable and label the video `music-not-embedded`. Do not call multi-clip composition merely to add music. Never copy a melody, imitate a named song, or imitate a living composer's distinctive style.

### State 6: Short Seed Video

Generate one short initial video from the actual seed storyboard. Request exactly the planned supported seed duration. Focus the prompt on the opening action, camera motion, environment, and an end state that can continue naturally.

Record the actual returned handle and cumulative duration. Do not proceed if the returned duration is missing or outside declared tolerance. A technical retry is allowed once only when a safe parameter correction is obvious; otherwise stop with the actual error and completed outputs.

### State 7: Sequential Video Extension

Extend strictly in sequence. For extension `N`:

1. pass the actual full video returned by extension `N-1`, or the seed video for the first extension;
2. request only the planned supported added duration or cumulative target;
3. provide the next continuation-map beat and the previous real end-state anchors;
4. reuse Character Lock, original reference, style, aspect ratio, resolution, frame rate, and audio policy when accepted;
5. wait for success and verify the returned cumulative duration before starting the next extension;
6. replace the working video handle with this newly returned longer video.

Never run extensions in parallel. Never feed the original seed to every extension. Never accept a tail-only result as the new final video. Never concatenate tail clips, generate independent replacement scenes, loop frames, change playback speed, or use a composition component to reach the requested duration.

If an extension fails, retry that extension once only when a safe parameter correction is clear. Preserve the latest successful cumulative video. After a second failure, stop and report the failed stage, last verified duration, target duration, actual error, and next action. Do not restart the seed or earlier successful extensions.

### State 8: Duration And Delivery Gate

Claim completion only after verifying:

- the final handle descends from the seed through one continuous extension chain;
- every extension consumed the immediately previous successful full video;
- no standalone clips were concatenated or reordered;
- returned metadata matches the user's target duration within declared tolerance;
- face, hair, costume, proportions, accessories, visual style, and story continuity remain recognizable;
- aspect ratio, resolution, frame rate, and audio policy stayed consistent;
- background music or requested speech is embedded only through a supported non-concatenating route, or is clearly delivered separately;
- the final output is an actual returned artifact and no media state is invented.

Return a concise localized summary containing the selected topic, requested and verified duration, seed storyboard, seed video, extension chain with cumulative durations, final video handle, audio status, and any deviation or required follow-up.

## Interactive Topic UI Contract

Use the native interactive single-choice action with this semantic shape, adapting field names only to the runtime's real schema:

```text
questions:
  - id: topic_direction
    header: [localized short field label]
    question: [localized story-direction question]
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

Keep labels scannable and descriptions short. Leave native radio controls, submit action, and free-form Other input to the runtime.

## Originality And Safety

- Generate a new topic, plot, setting, staging, and visual progression.
- Treat named films, games, anime, artists, or directors as high-level craft signals, never copy targets.
- Do not reproduce protected characters, logos, signature props, iconic shots, or recognizable scene sequences.
- Do not directly imitate a living artist's distinctive style; translate the request into general visual features.
- Follow the repository's fiction safety boundaries for people and harmful content.
