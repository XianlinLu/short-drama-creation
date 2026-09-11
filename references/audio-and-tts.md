# Audio, Music, And TTS Recovery

## Original background music

When music generation is connected, create an original instrumental track automatically for character-video output unless the user opts out. Derive the track from the chosen direction and verified final duration.

Specify mood, tempo range, instrumentation family, intensity curve, transition points, and ending behavior without naming a protected recording or asking for imitation of a living musician. The music duration should match the final video timeline within the connected action's tolerance.

Keep dialogue intelligible. Avoid vocals unless requested. Reserve dynamic space for speech and important sound effects.

Use one of these delivery paths:

1. a video action's native music input while retaining the same continuous video lineage;
2. a mux or audio-replacement action that accepts one complete video and does not alter its duration;
3. a separately delivered track with clear synchronization metadata.

Never create multiple video segments merely to add music. Verify duration after any mux action.

## Speech preparation

Split dialogue or narration only when the TTS action requires chunks. Keep a stable chunk map containing chunk id, character, text, intended emotion, voice, expected duration, and status. Generate only text that will be heard; do not place camera directions inside spoken content.

## Risk-audit rejection protocol

When the TTS response identifies a rejected chunk:

1. read the returned chunk identifier and preserve every successful chunk;
2. inspect only the rejected spoken text for wording likely to trigger the audit;
3. rewrite only that chunk in safer, neutral language;
4. preserve story meaning, character intent, pacing, emotional direction, and surrounding continuity;
5. retry only the failed TTS generation step;
6. confirm a successful audio artifact before starting any dependent video action.

If the first rewrite is rejected, simplify that same chunk one more time: shorten the sentence, remove graphic or threatening detail, replace loaded phrasing with neutral action or emotion, and keep the dramatic function through implication.

If the text is already neutral and the action still rejects it, try one different available TTS voice without changing successful chunks.

The recovery budget is:

- original attempt;
- up to two text revisions for the identified chunk;
- one alternate-voice attempt when the latest text is neutral.

After the budget is exhausted, stop the speech-dependent branch, report the exact rejected chunk and last returned error, and keep all valid upstream artifacts. Do not loop indefinitely and do not restart the entire workflow unless targeted regeneration is technically impossible.

## Audio verification

Before downstream generation or final delivery, verify:

- every required chunk has a successful returned artifact;
- voice assignment and spoken language are correct;
- dialogue order and synchronization match the story plan;
- music length matches the verified final video;
- embedded audio did not change video duration;
- separately delivered audio is clearly labeled.
