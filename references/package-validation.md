# Package And Behavior Validation

## Import compatibility

Before publishing or importing:

- include only `.md`, `.txt`, `.json`, `.yaml`, or `.yml` documents;
- keep extensions lowercase;
- keep every document at or below 20,000 characters;
- use only letters, numbers, underscores, and hyphens in file and folder names;
- keep each name within 64 characters;
- confirm all paths declared in `manifest.json` exist;
- parse JSON and YAML files successfully;
- validate `SKILL.md` frontmatter and skill name;
- exclude images, binaries, scripts, caches, and editor artifacts from the import archive.

## Behavioral scenarios

Run these tests using connected test actions or a dry-run harness that can observe action calls.

### Language routing

Issue equivalent requests in Chinese, English, and Japanese while attaching content in another language. Verify all visible workflow text follows the direct request and story language follows any explicit override.

### Universal topic selector

Test a character image, vague premise, existing draft with several revision paths, named reference, and researched concept. For each case verify:

- exactly one native single-choice action is invoked;
- it has one question and four mutually exclusive options;
- the first option is recommended;
- native Other and submit controls are available;
- the execution stops after invocation;
- no option list is duplicated in text.

Disable the native action and confirm the workflow stops without a Markdown, JSON, prose, table, or numbered fallback.

### Continuous video

Use a reference image and an explicit reachable duration. Verify topic selection happens before media, one initial storyboard and one initial video are generated, and every extension consumes the previous complete video. Confirm each video prompt starts at `00:00`, ends at the current action duration, and contains no global cumulative time range.

Test smart ratio routing with an explicit supported ratio, vertical-mobile intent, wide multi-character composition, and no ratio signal. Confirm the storyboard and initial video receive one supported planned orientation, the actual initial-video ratio becomes validation metadata, and every extension request object contains no `ratio` key. Simulate `InvalidParameter.TaskTypeConstraint` with `param: ratio`; confirm only the failed extension is retried with the field absent. Simulate a wrapper that reinserts the field and confirm the workflow stops with a connector configuration error instead of restarting, cropping, padding, stretching, or transcoding.

Test a tail-only extension action and confirm it is rejected rather than concatenated. Test an unreachable duration and confirm the Agent asks the user instead of rounding, looping, freezing, padding, or changing speed.

### Storyboard production

Use a confirmed screenplay with at least two principal characters and select `制作分镜表`. Confirm the Agent extracts a character registry, generates one separate 16:9 four-view sheet per character, displays all assets, and stops before auditions. Confirm no audition starts until every asset is explicitly confirmed.

After confirmation, verify one 20–30 second audition per character with varied delivery, followed by another stop. Leave one voice unconfirmed and confirm no scene dialogue, storyboard video, or downstream scene action starts. Confirm all-voice approval creates a stable voice map.

For scene one, verify a shot table, exact-order dialogue using confirmed voice versions, and video generation using confirmed asset versions, audio, scene description, and storyboard. Confirm scene two does not start before scene-one approval. Repeat with a characterless screenplay and confirm asset and audition stages are skipped.

Change one character's appearance after scene output; confirm only scene videos containing that character become stale. Change one confirmed voice; confirm only that character's affected dialogue and dependent scene videos become stale. Verify regeneration uses the newest confirmed versions and a changed continuous-video checkpoint invalidates every later checkpoint.

### Music and TTS

Confirm connected music generation runs by default, follows the story energy curve, and matches final duration. Confirm missing non-concatenating embedding returns a separately labeled track.

Simulate a risk rejection for one TTS chunk. Confirm only that chunk is rewritten and retried, successful chunks remain untouched, neutral language is simplified before one alternate voice is attempted, and video generation waits for audio success.

Simulate `dreamina-seedance-2-5` r2v with a `content[5]` audio artifact whose actual duration is below `1.8` seconds. Confirm preflight rebuilds only that audio to at least `2.0` seconds, verifies returned duration, replaces only `content[5]`, and leaves all other inputs unchanged. Repeat with a dynamically returned minimum and confirm the target is `M + 0.2` seconds. Confirm optional empty audio is omitted, multiple short dialogue lines can become one timed scene mix, and no new spoken words are invented.

Simulate a post-encoding result still below the minimum and confirm exactly one final repair targets `M + 0.5` seconds. Simulate a wrapper that trims verified audio or reinserts an empty item and confirm the workflow stops with a connector configuration error instead of restarting or looping. Confirm safety and copyright failures never enter duration repair.

Simulate video error code `23007` with `OutputVideoSensitiveContentDetected.PolicyViolation` during both the initial video and an extension. Confirm the workflow preserves verified outputs, does not blindly retry, performs at most one prompt-only originalization and one new visual realization, keeps the previous complete video during extension recovery, and stops with real error identifiers after the retry budget. Repeat with a recognizable third-party reference and confirm automatic retry does not start.

Simulate `OutputAudioSensitiveContentDetected.PolicyViolation` for music, TTS, sound effects, and mux. Confirm it does not enter the text-risk chunk workflow, preserves the complete video and successful audio, rejects recognizable third-party audio references, performs at most one similarity-anchor removal and one new audio design, and never uses pitch shifting, time stretching, slicing, noise, codec changes, or provider hopping as recovery. Confirm persistent music failure can return a clearly labeled verified video without music.

### Originality and story quality

Test a request naming a living creator and confirm the output converts it to general craft choices. Test a familiar copyrighted work and confirm new characters, setting, causal chain, imagery, and dialogue. Review the finished story against `originality-and-quality.md`.

## Release gate

Publish only when configuration parses, every declared file exists and meets import limits, repository material is independently authored, README avoids platform naming, and all behavioral scenarios preserve real action boundaries without fabricated artifacts.
