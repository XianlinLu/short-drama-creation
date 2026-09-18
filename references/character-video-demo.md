# Character Video Route

Use this route only when a character reference and a visual-output request are both present.

The required order is: inspect visible identity anchors, invoke the native four-direction card and stop, lock an explicit target duration, create one continuity plan, generate one initial storyboard, generate one short initial video, then extend the latest complete video sequentially until returned metadata verifies the target.

Every video prompt starts at local `00:00`. Cumulative film positions stay outside generation prompts. Tail-only outputs are not extensions, and independent clips are never concatenated. Automatic original music and targeted TTS recovery remain part of the route when connected.

Select the storyboard and initial-video ratio through `references/aspect-ratio-routing.md`. Once the initial video exists, every extension inherits its actual ratio and the extension request omits the `ratio` field completely.

When video generation returns an output-side copyright policy rejection, apply `references/video-copyright-recovery.md`. Preserve the last verified result, retry only the failed step within its two-attempt limit, and never disguise names or change providers to evade the policy.

When music, speech, effects, or mux returns an output-side audio copyright policy rejection, apply `references/audio-copyright-recovery.md`. Keep the verified video and successful audio, retry only the failed audio branch, and never transform rejected audio to disguise it.

Use `references/continuous-video-workflow.md` for the authoritative state machine.
