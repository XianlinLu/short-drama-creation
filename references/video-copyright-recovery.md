# Video Copyright Policy Recovery

Use this protocol when a video action reports an output-side copyright restriction, including error signatures such as:

- `OutputVideoSensitiveContentDetected.PolicyViolation`;
- error code `23007` paired with a copyright-restriction message;
- a `BALLMVideo` `EulerError` whose nested result contains either signature above;
- an equivalent action response stating that the generated video may be too similar to protected material.

This is a policy rejection, not a normal transport failure. Do not blindly repeat the same request.

## Non-evasion boundary

The goal is compliant original re-expression. Never try to defeat the detector by misspelling names, translating protected names, encoding instructions, hiding references in metadata, removing watermarks from third-party material, switching models or providers solely to obtain a rejected result, or resubmitting the same inputs until one passes.

Do not claim to know which protected work caused an output-side rejection. Identify only the risk signals visible in the inputs and prompt.

## Preserve the checkpoint

Before changing anything, record internally:

- whether the failure occurred during the initial video or extension number `n`;
- the last verified complete video, if one exists;
- the current storyboard or source image;
- the failed video prompt and requested call duration;
- selected topic, character continuity anchors, and story function of the failed beat;
- returned error code, log id, request id, and raw action message.

Keep every verified image, audio result, and video. Never restart the entire workflow merely because one video call was rejected.

## Input and prompt audit

Check for risk signals:

- franchise, film, television, game, comic, studio, creator, actor, or character names;
- instructions to reproduce an exact scene, shot, trailer, transformation, costume, pose, or camera sequence;
- distinctive emblems, logos, text, branded products, signature props, or recognizable uniforms;
- requests for a specific living creator's style or a specific performer's likeness;
- screenshots, posters, watermarked frames, celebrity photos, or recognizable third-party characters used as references;
- wording such as “identical to,” “exactly like,” “same as,” or “recreate.”

If the reference appears to be a recognizable third-party character, celebrity, branded asset, film frame, poster, or watermarked image—or its origin is too uncertain to support a safe retry—stop automatic video retry. Ask for an original, unbranded character reference. If the user needs new creative alternatives, present four original directions through the native topic selector and stop.

If the reference is an original user asset and contains no recognizable protected identity, preserve its core character identity. Do not alter face, body proportions, or essential costume merely to satisfy a retry unless the user approves a redesign.

## Recovery attempt 1: prompt-only originalization

Rewrite only the failed video prompt:

1. remove titles, franchise names, character names, studio names, artist or director names, actor names, brands, and named style comparisons;
2. replace them with functional visual language describing mood, materials, motion, lens behavior, lighting, and story action;
3. remove requests for exact recreation and write a new staging and camera path;
4. exclude logos, readable brand text, watermarks, signature props, and iconic compositions;
5. retain the selected story's meaning, character intent, emotional direction, duration, continuity, and zero-based local timeline;
6. retry only the failed initial-video or extension action once.

Do not change successful upstream results during this attempt.

## Recovery attempt 2: replace the failed visual expression

Use this attempt only if attempt 1 receives the same class of policy rejection and the inputs remain suitable for generation.

Preserve the narrative function but create a clearly different visual realization. Change at least three relevant dimensions:

- environment or architecture;
- camera position, lens behavior, and movement;
- blocking, gesture, or action choreography;
- props and visual symbols;
- color palette, weather, or lighting pattern;
- composition and depth layout;
- non-essential wardrobe detail when it is not a locked identity anchor.

For an initial-video failure, create one revised original storyboard for the same story function, then retry the initial-video action once from that revised storyboard.

For an extension failure, keep the last verified complete video as the extension input. Replace only the next failed beat with new staging and action while preserving the established character and continuity. The extension prompt still begins at local `00:00`; earlier footage is not regenerated.

This is the final automatic retry.

## Stop condition

After two recovery attempts, stop the video branch if the policy rejection persists. Return the last verified complete video, if any, and a localized explanation containing the failed stage, actual error code, log id or request id, and which compliant rewrites were attempted.

Offer safe next steps without repeating rejected content:

- upload an original unbranded reference;
- remove protected names, logos, or exact-scene requirements;
- select a substantially different original visual direction;
- continue from the last verified video after the user supplies a revised direction.

If several replacement directions are offered, use the mandatory native single-choice card. Do not provide a text list of directions.

## Successful recovery

When a retry succeeds, verify the returned artifact and duration, update the extension checkpoint, and resume the normal sequential workflow from that actual result. Do not restart earlier successful stages. Include the recovery in the final delivery summary without claiming that any particular copyrighted work was detected.
