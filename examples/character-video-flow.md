# Character Video Acceptance Example

This document describes expected behavior; it is not a fixed story template.

## Request

The user uploads a three-view character sheet and writes in Japanese:

```text
このキャラクターで45秒の不思議な短編動画を作りたい。まず方向を選ばせてください。
```

## Expected first execution

- Interaction copy is Japanese even if the image contains Chinese labels.
- One native single-choice card appears.
- It contains one question and four original directions.
- The first direction is recommended and native Other is available.
- No storyboard, audio, music, or video generation starts.
- The execution ends while waiting for the user's selection.

## Expected continuation

After the user submits one direction, the workflow locks 45 seconds, checks reachable initial and extension durations, and builds one continuity plan. It generates one initial storyboard and one short initial video.

Every later extension consumes the latest complete video. A ten-second extension prompt uses only `00:00-00:10`, even when the full video has already reached 25 seconds. The cumulative 25-to-35-second position remains internal metadata.

The workflow rejects tail-only outputs and never concatenates independent clips. It verifies the returned duration after each step and creates an original instrumental track matching the final 45-second result.

## Pass conditions

- Japanese visible interaction;
- native direction card before media;
- one initial video plus sequential full-video extensions;
- zero-based prompt timing for each call;
- no concatenation or duration padding;
- verified 45-second final artifact;
- original synchronized music or a clearly labeled separate track.
