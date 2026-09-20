# Audio Minimum-Duration Preflight

Use this protocol before every video or reference-to-video action that receives audio. It handles input-duration validation only; safety and copyright rejections still use their dedicated recovery rules.

## Resolve the minimum

Read the connected action schema or returned error for the model- and task-specific minimum. When no newer action value is available, use this known constraint:

```text
model: dreamina-seedance-2-5
task: r2v
minimum audio duration: 1.8 seconds
safe target: 2.0 seconds
```

For any declared minimum `M`, set the first repair target to at least `M + 0.2 seconds`. Validate the actual decoded or returned duration of the final encoded artifact, not the requested duration, script estimate, filename, or UI label.

## Preflight every audio input

Before constructing the video request:

1. inventory every audio-bearing `content[n]` item and its role: dialogue, narration, music, sound effect, or mixed scene track;
2. read each artifact's actual duration from trustworthy action or media metadata;
3. omit an optional empty audio item instead of sending a zero-length placeholder;
4. require every submitted audio item to meet the active model/task minimum;
5. prefer one verified mixed scene track when the video action expects one track and individual dialogue clips would be shorter than the minimum;
6. start the video action only after every required audio item passes.

For short dialogue, preserve the exact approved words. Reach the minimum through supported delivery pacing, natural silence, room tone, or a correctly timed scene mix; do not invent dialogue or alter story meaning. For music or sound effects, sustain or regenerate an original tail that preserves function. Keep intended line start/end timing in the scene timing map so a longer container does not shift synchronization.

Voice auditions remain 20–30 seconds, but their returned duration must still be verified. Never claim that a requested duration was produced when metadata says otherwise.

## Targeted recovery from `content[n]`

Use this branch when a video request returns `InvalidParameter` and states that `content[n]` audio must be at least `M` seconds:

1. parse the exact content index, minimum, model, task, request id, and log id from the error;
2. map `content[n]` to the exact audio artifact used in that request;
3. preserve the video input, prompt, character assets, other audio, and every successful upstream result;
4. regenerate or remix only the failing audio artifact to an actual duration of at least `M + 0.2 seconds`;
5. verify the new artifact's decoded duration, replace only `content[n]`, and retry only the failed video call once;
6. if the replacement is still shorter after encoding, make one final repair to at least `M + 0.5 seconds`, verify it, and retry that same video call once more.

Stop after the final repair if the call still fails. Report the real identifiers, required minimum, measured durations, and failing index. If a wrapper trims valid audio or silently inserts an empty item, report a connector configuration problem rather than looping or restarting the workflow.

Do not use padding or regeneration to disguise unsafe or copyright-rejected audio. Route those errors to `audio-and-tts.md` or `audio-copyright-recovery.md` as appropriate.
