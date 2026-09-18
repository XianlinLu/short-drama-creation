# Smart Aspect Ratio Routing

Use this protocol for storyboard, initial-video, and video-extension aspect ratio handling.

## Core rule

Choose or request an aspect ratio only before the initial storyboard and initial video. After the initial video is returned, lock its actual width-to-height ratio from metadata. Every true video-extension call must inherit that ratio from its input video.

For an extension action, omit the `ratio` parameter entirely. Do not send a numeric ratio, string ratio, `auto`, `null`, an empty string, the locked ratio, or a copied default. “Omit” means the field is absent from the submitted request object.

The extension prompt should say to preserve the existing frame composition when useful, but it should not request a new output ratio.

## Initial ratio selection

Inspect ratios supported by the connected storyboard and initial-video actions. Resolve `planned_ratio` in this order:

1. **Explicit user choice:** use the requested ratio when both initial actions support it. If unsupported, present supported alternatives and wait; do not silently substitute.
2. **Explicit destination:** use a vertical ratio for a named vertical short-video destination, a landscape ratio for cinematic or widescreen delivery, and square only for an explicitly square feed or asset.
3. **Composition:** prefer `9:16` for a single full-body character, portrait-centered staging, or mobile-first short drama; prefer `16:9` for multi-character blocking, wide action, architecture, or environment-led storytelling.
4. **Fallback:** choose the connected actions' shared default. When both `9:16` and `16:9` are equally supported and no other signal exists, use `9:16` for this short-drama workflow.

Do not copy the aspect ratio of a wide three-view character sheet merely because it is the uploaded reference. The sheet supplies identity; the intended final composition determines the output ratio.

If the actions use other supported tokens or dimensions, map the chosen orientation to the closest supported ratio without changing orientation. Record the selected value and its source: `explicit`, `destination`, `composition`, or `fallback`.

## Ratio lock

Generate the initial storyboard and initial video with the same planned orientation when their schemas support ratio control. After the initial video succeeds:

1. read actual width and height from returned metadata;
2. reduce them to an actual ratio or store the provider's returned ratio token;
3. set `locked_ratio` to that actual output, even if it differs slightly from the plan;
4. use `locked_ratio` only for validation and delivery metadata, never as an extension request parameter.

If the user asks to change ratio after the initial video exists, explain that true extension preserves the input video's ratio. A different ratio requires a new initial-video branch; do not crop, pad, stretch, or re-encode the current extension chain unless the user explicitly requests a separate post-production workflow and a connected action supports it.

## Known error recovery

Trigger this recovery when an extension returns:

- `InvalidParameter.TaskTypeConstraint`;
- `param: ratio`;
- a message stating that video-extension output ratio follows the selected input video;
- or an equivalent bad-request response caused by specifying ratio for an extension task.

Preserve the latest verified complete video and the failed extension prompt. Then:

1. remove the `ratio` field from the failed extension request object;
2. keep the same input video, added duration, local `00:00` timeline, story action, continuity constraints, and other valid parameters;
3. retry that failed extension once;
4. verify the returned duration and confirm its actual ratio matches the input video within metadata tolerance;
5. continue the sequential extension chain only from the verified result.

Do not restart the storyboard, initial video, or earlier extensions. Do not repair this error by cropping, letterboxing, padding, stretching, or transcoding.

If the retry still reports a ratio constraint, inspect the real action mapping for a wrapper or default that automatically reinserts `ratio`. Use an extension-specific configuration that leaves the field absent. If the connected action cannot omit it, stop and report a connector configuration problem with the actual log id, request id, action name, and rejected parameter.

## Verification

Before each extension call, assert internally:

- task type is video extension;
- input is the latest verified complete video;
- request object has no `ratio` key;
- prompt requests continuity rather than a new canvas shape;
- duration and local timeline are valid.

At delivery, report the actual final ratio from metadata and note whether it came from explicit choice, destination, composition, or fallback planning.
