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

Test a tail-only extension action and confirm it is rejected rather than concatenated. Test an unreachable duration and confirm the Agent asks the user instead of rounding, looping, freezing, padding, or changing speed.

### Music and TTS

Confirm connected music generation runs by default, follows the story energy curve, and matches final duration. Confirm missing non-concatenating embedding returns a separately labeled track.

Simulate a risk rejection for one TTS chunk. Confirm only that chunk is rewritten and retried, successful chunks remain untouched, neutral language is simplified before one alternate voice is attempted, and video generation waits for audio success.

### Originality and story quality

Test a request naming a living creator and confirm the output converts it to general craft choices. Test a familiar copyrighted work and confirm new characters, setting, causal chain, imagery, and dialogue. Review the finished story against `originality-and-quality.md`.

## Release gate

Publish only when configuration parses, every declared file exists and meets import limits, repository material is independently authored, README avoids platform naming, and all behavioral scenarios preserve real action boundaries without fabricated artifacts.
