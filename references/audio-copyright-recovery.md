# Audio Copyright Policy Recovery

Use this protocol when an audio-producing or audio-embedding action returns `OutputAudioSensitiveContentDetected.PolicyViolation`, a copyright-restriction message, or an equivalent output-side audio policy result.

This is different from a TTS text risk audit that identifies a rejected dialogue chunk. Route the error here unless the action explicitly reports a text-risk chunk identifier.

## Non-evasion boundary

Recover through new original audio expression. Never attempt to make rejected audio pass by pitch shifting, time stretching, speeding up, slowing down, reversing, adding noise, changing codecs, slicing the same recording, hiding a song or performer name, translating or misspelling protected names, repeatedly submitting identical inputs, or switching providers solely to evade the policy.

Do not claim to know which recording, composition, performance, or voice caused an output-side rejection. Report only observable risk signals.

## Preserve the checkpoint

Record internally:

- failed stage: music generation, TTS, sound effect, audio embedding, or mux;
- last verified complete video and its duration;
- every successful TTS chunk, sound effect, and music result;
- failed prompt, spoken text, voice settings, reference audio, and target duration;
- selected story direction and intended audio function;
- returned log id, request id, policy code, and raw action message.

Never regenerate a successful video because an audio step failed. Never discard successful audio chunks that are unrelated to the rejection.

## Audit the failed input

Check for:

- song titles, artist, composer, band, soundtrack, franchise, studio, performer, celebrity, or fictional-character voice names;
- instructions such as “same melody,” “sound exactly like,” “cover,” “remix,” “clone,” “impersonate,” or “recreate”;
- quoted or adapted song lyrics;
- a reference recording, extracted film or game audio, branded jingle, signature alert, or recognizable sample;
- requests for a celebrity, actor, singer, public figure, or protected character voice;
- a user-uploaded track whose origin is unclear.

If the input includes a recognizable third-party recording, melody reference, branded sound, celebrity or character voice, extracted soundtrack, or unclear reference audio, stop automatic retry with that input. Ask for user-created audio or continue without the reference. Do not process the same audio to disguise it.

## Recovery attempt 1: remove similarity anchors

Retry only the failed audio step once.

For background music:

- remove song, artist, composer, soundtrack, studio, and franchise names;
- remove melody-copying, cover, remix, and soundalike instructions;
- request an original instrumental cue using only story mood, tempo range, meter, instrumentation family, energy curve, transition points, and ending behavior;
- do not reuse reference audio, lyrics, a named melody, or a recognizable motif.

For TTS:

- remove celebrity, actor, singer, public-figure, fictional-character, or named-performer voice requests;
- replace them with neutral voice properties such as pace, register, texture, age range, clarity, and emotional energy;
- if the spoken text contains protected lyrics or a recognizable quotation, rewrite only that passage while preserving its narrative meaning;
- preserve every successful chunk and retry only the failed one.

For sound effects:

- replace named brand sounds, signature alerts, or cinematic sound references with a functional physical description;
- generate a new effect rather than transforming the rejected audio.

For mux or audio embedding:

- preserve the complete video;
- remove the rejected audio input;
- substitute only a newly generated original track or verified neutral speech result.

## Recovery attempt 2: new audio design

If attempt 1 receives the same policy class, make one final original generation attempt.

For music, change at least four dimensions: tempo band, meter or groove, melodic contour, harmonic movement, instrumentation, sound palette, section structure, and ending cadence. Preserve only duration, story function, dialogue space, and emotional arc.

For TTS, use one different neutral non-impersonation voice and a newly rendered performance. Keep the approved text, character intent, pace, and timing. Do not use voice conversion or post-processing to imitate the rejected target.

For sound effects, replace the synthesis concept and source materials while preserving only the physical event and timing.

For embedding, use the new original audio with the same complete video and verify that video duration is unchanged.

This is the final automatic retry.

## Stop condition and delivery

After two recovery attempts, stop the affected audio branch if the policy rejection persists. Preserve and deliver the last verified video. When narration is essential to an unfinished downstream step, stop that dependent step; when only music fails, the verified video may be delivered without music if clearly labeled.

Return a localized explanation containing the failed audio stage, actual log id or request id, policy code, preserved artifacts, and the compliant recovery attempts. Offer safe next steps: use no reference audio, choose a neutral synthetic voice, request a fully original instrumental cue, provide user-created audio, or deliver the verified video without the rejected track.

If several new creative audio directions are offered, use the mandatory native single-choice card instead of a text list.

## Successful recovery

Verify the returned audio artifact, duration, voice assignment, dialogue order, and synchronization. If audio is embedded, confirm the video duration and continuous lineage did not change. Resume only the dependent downstream step; do not restart the whole workflow.
