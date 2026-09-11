# Continuous Character Video Workflow

## Activation

Use this workflow only when both conditions are true:

1. a character turnaround, three-view sheet, or equivalent visual reference is available;
2. the user requests storyboards, animation, a video demo, a short film, or a video of a specified duration.

Treat the reference as the identity anchor. Do not infer a real person's identity, private history, or sensitive traits.

## Required action capabilities

Inspect connected actions before generation. The complete route needs:

- reference-aware image generation;
- image-to-video generation;
- true video extension that accepts the latest full video and returns a longer full video;
- reliable duration metadata for returned videos.

The native topic selector is required before media generation. Music, TTS, sound effects, subtitles, last-frame extraction, preview, and saving are optional unless explicitly requested.

If a required action is missing, stop at the earliest safe point, state what is unavailable, and return only real artifacts already produced. A tool that returns an isolated continuation clip does not satisfy true extension.

## State A: reference intake

Inspect visible identity cues needed for consistency: silhouette, face shape, hair, proportions, wardrobe layers, palette, accessories, and distinctive non-sensitive marks. Build an internal continuity record. Do not invent unseen details as facts; mark necessary guesses as design choices.

## State B: direction card

Apply `interaction-and-topic-ui.md`. Offer four original, visually distinct story directions through exactly one native single-choice card. Stop immediately after the action call. No media action may run during this state.

## State C: duration lock

After selection, parse target duration only from the user's direct prompt. Support natural expressions such as seconds, minutes, mixed minute-second phrases, or clock notation.

If duration is absent or ambiguous, ask one localized technical question and stop. This question is not a topic selector.

Inspect actual action limits:

- allowed initial-video durations;
- extension increments;
- maximum cumulative duration;
- whether exact target duration is representable;
- available duration metadata and tolerance.

Choose the shortest practical initial duration and an ordered extension plan. If the exact target is unreachable, explain the supported nearest outcomes and wait for the user to choose. Never silently round.

## State D: continuity map

Create one compact original story that can unfold as a single continuous film. Record:

- character identity anchors;
- environment and lighting rules;
- the opening action;
- narrative changes assigned to the initial video and each extension;
- prop and spatial continuity;
- emotional progression;
- sound and music curve;
- ending image.

Global time positions belong only in internal planning and progress metadata.

## State E: initial storyboard

Generate one storyboard image that can serve as the first frame of the initial video. Provide the character reference to the image action whenever supported. Require consistency in face, proportions, clothing, palette, and accessories while allowing pose, camera, lighting, and setting to change.

Verify the returned image before using it. If identity drift is material, revise only the storyboard prompt and regenerate that image.

## State F: initial video

Animate the actual storyboard output into one short initial video. The prompt must describe only this action call and use a local timeline beginning at zero.

Example for an eight-second call:

```text
00:00-00:02 establish the pose and environment
00:02-00:06 perform the central action and camera move
00:06-00:08 settle into a continuation-ready final state
```

The prompt must not contain the video's future global position. A ten-second action never receives `00:30-00:40`, even if it later becomes that portion of the final film.

Check the returned artifact, actual duration, identity, motion continuity, and final state before extending.

## State G: sequential extension

Run extensions one at a time. For extension `n`:

1. input the complete video returned by step `n-1`, or the initial video for the first extension;
2. describe only the new action and story development;
3. start the prompt timeline at `00:00`;
4. end the prompt timeline at the duration added by this action call;
5. preserve identity, scene geometry, lighting logic, motion direction, props, and audio policy;
6. wait for the returned full video;
7. verify that its duration increased by the expected amount;
8. use that verified full video as the next input.

Do not launch dependent extensions in parallel. Do not use separately generated clips, montage assembly, or concatenation as a substitute.

Reject an extension result when it is only a tail clip, resets the scene without intent, loses the character identity, or fails the duration increase. Retry only the failed extension with a corrected prompt when safe.

## State H: audio and finish

Follow `audio-and-tts.md`. Generate original instrumental background music by default when connected. Speech is optional unless requested or required by the chosen direction.

Embedding is allowed only through a native audio input on the same video chain or a single-video mux action that does not concatenate video. Otherwise return the music or speech separately with synchronization guidance.

Final acceptance requires:

- returned duration equals the locked target within the declared action tolerance;
- one continuous extension lineage exists from initial video to final video;
- no loops, freezes, speed changes, padding, hidden rounding, or independent-clip concatenation were used;
- character, story, visual style, action, props, and location remain coherent;
- each generation prompt used a local zero-based timeline;
- final video and audio references point to actual action outputs.

Return a localized summary naming the selected direction, requested and verified duration, initial storyboard, initial video, extension lineage, audio status, final artifact, and any unresolved limitation.
