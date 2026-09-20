# Storyboard Production Workflow

Use this route when the user supplies a screenplay, its creative direction and final text are confirmed, and the user selects `制作分镜表` or an equivalent request as the next step.

Do not treat a draft as final merely because it is long. If the script or creative direction is still unresolved, return to the story workflow. Once this route starts, keep the script version fixed until the user explicitly revises it.

## Required capabilities

The complete route requires connected actions for reference-aware image generation, 20–30 second TTS or voice audition generation, dialogue audio generation, and reference-and-audio-aware video generation. Use only real connected action contracts. Missing required capability stops the workflow at the earliest safe point without fabricating an asset.

## State record

Maintain a production ledger:

```yaml
screenplay_version: integer
characters:
  character_id:
    profile_version: integer
    asset_version: integer
    asset_status: pending | confirmed | revise
    voice_version: integer
    voice_status: pending | confirmed | revise
scenes:
  scene_id:
    script_version: integer
    storyboard_version: integer
    dialogue_versions: map of character_id to voice_version
    video_asset_versions: map of character_id to asset_version
    video_status: pending | confirmed | revise | stale
```

Never use a pending, revised, or stale dependency for downstream generation.

## Stage 1: screenplay and character extraction

Parse only the final confirmed screenplay. Identify every principal character who materially affects the plot or appears in generated scenes. Create a stable `character_id` and a character bible containing:

- name or functional label;
- apparent age range;
- facial and body features needed for continuity;
- hair, wardrobe layers, footwear, accessories, and palette;
- identity, role, occupation, or social position stated by the script;
- personality, habitual behavior, emotional baseline, and relationship pressure;
- voice requirements inferred from age, identity, temperament, and dramatic function.

Separate explicit facts from production choices. Do not infer sensitive traits or a real person's identity. Ask only when a missing detail would materially change design or casting.

If the screenplay has no characters, record `characterless: true`, skip Stages 2–4, and continue to scene parsing. Narration, if present, may use a neutral narrator without creating a character asset or audition unless the user requests narrator casting.

## Stage 2: 16:9 character asset sheets

Generate exactly one 16:9 asset image for each character. This sheet ratio is fixed and independent of the final scene-video ratio.

Each sheet must show the same character in four coordinated views:

1. front facial close-up;
2. front full-body view;
3. side full-body view;
4. back full-body view.

Use a clean four-panel layout and a neutral production background. Keep facial structure, features, hair, wardrobe, body proportions, accessories, materials, palette, age presentation, and visual style identical across views. Avoid action poses, perspective distortion, cropped feet, extra limbs, conflicting costume details, branded logos, and generated descriptive text inside the image.

Generate one separate sheet per character rather than combining several characters on one sheet. Record the actual returned artifact as that character's `asset_version`.

After all character sheets are generated, display them with their concise character settings and stop. Ask the user to confirm or request changes for every character. Do not start auditions until every `asset_status` is `confirmed`.

If a sheet is revised, regenerate only that character's sheet, increment `asset_version`, and return to the same confirmation checkpoint.

## Stage 3: 20–30 second voice auditions

Begin only after all character assets are confirmed.

Generate one separate 20–30 second audition for each character. Use the story language and a neutral non-impersonation voice suitable for the confirmed age range, personality, identity, social role, and emotional baseline. Do not clone or imitate a real person, celebrity, actor, singer, or protected character voice.

Write original audition text that demonstrates enough range to judge casting. Include several shifts such as neutral introduction, conversational warmth or restraint, tension, urgency or determination, and a softer or reflective ending. The audition is a casting sample, not screenplay dialogue, and must not reveal private inferred traits.

Record the actual artifact and settings as `voice_version`. Show every audition with the character name and voice profile, then stop. Require explicit confirmation or revision for each character. Do not generate any scene dialogue or scene video while any `voice_status` is not `confirmed`.

When the user requests a voice change, regenerate only that character's audition, increment `voice_version`, and return to voice confirmation.

## Stage 4: voice lock

Create a voice casting map only when every character is confirmed:

```text
character_id → confirmed voice handle + voice_version + language + delivery profile
```

Never substitute another character's voice or a newer unconfirmed voice. The map is mandatory for scene dialogue.

## Stage 5: scene-one breakdown and storyboard table

Parse scene one from the confirmed screenplay. Create a scene record containing location, time, environment, participating characters, dramatic purpose, entry state, exit state, actions, props, dialogue order, emotion, pauses, and continuity links.

Create a storyboard table with one row per shot:

```text
shot id | estimated duration | framing | camera and movement | characters and asset versions | action | dialogue/audio cue | environment/props | transition
```

Use the project's smart scene-video ratio from `aspect-ratio-routing.md`; the 16:9 character sheet does not force the scene-video ratio. Preserve local zero-based timing for any video action.

## Stage 6: scene dialogue audio

Generate scene-one dialogue in exact screenplay order. For every line:

- use the speaking character's confirmed voice handle and `voice_version`;
- retain the confirmed wording unless a required safety recovery needs a meaning-preserving revision;
- reproduce intended emotion, pace, emphasis, pause, interruption, and overlap when supported;
- keep stable line and chunk identifiers;
- preserve successful chunks when one line fails.

Assemble or mix dialogue only through a connected action that supports ordered timing. Apply `audio-and-tts.md` for text-risk rejection, `audio-copyright-recovery.md` for output-side audio copyright rejection, and `audio-duration-preflight.md` before passing audio to video. When individual lines are below the video model's minimum, prefer a verified timed scene mix; otherwise extend only the audio container with supported natural silence or room tone without adding words. Verify speaker identity, line order, actual duration, and synchronization before video generation.

## Stage 7: scene video

Generate scene one only after its dialogue is verified. Supply the confirmed character asset versions, dialogue audio, scene description, shot plan, environment, props, and continuity record to the connected video action.

Keep character appearance, confirmed voices, environment, actions, emotional progression, and plot meaning consistent. Use actual returned artifacts. Apply smart ratio routing, local `00:00` timing, and existing media-policy recovery rules.

Scene one becomes the first video checkpoint. For later scenes, preserve one continuous film by extending the latest confirmed complete-video checkpoint when true extension supports the required transition. Do not concatenate independent scene clips into a claimed final film. If the user explicitly requests separate scene previews, label them as previews and do not treat them as a continuous final video.

## Stage 8: per-scene confirmation

After scene one returns, display the actual result and a concise dependency summary, set its status to `pending`, and stop. Do not process scene two until the user confirms scene one.

After confirmation, set scene one to `confirmed`, parse the next scene, generate its dialogue and video with the same dependency checks, display it, and stop again. Repeat one scene at a time until the screenplay ends.

## Change propagation

When the user changes a character profile, appearance, or voice:

- increment the changed version;
- keep the new version pending until explicitly confirmed;
- appearance changes mark every scene video containing that character as `stale`;
- voice changes mark that character's affected dialogue audio and every dependent scene video as `stale`;
- script changes re-parse affected characters and scenes, then invalidate their downstream artifacts;
- regenerate only stale dependencies, in order, using the latest confirmed versions;
- if a changed scene sits inside a continuous extension chain, regenerate that scene checkpoint and every later checkpoint that depends on it.

Never mix versions within one scene. Before any generation, compare the scene's recorded asset and voice versions with the current confirmed ledger.

## Completion

The route completes only after every required character asset, voice, scene dialogue, and scene video is confirmed. Return a localized production summary listing the final screenplay version, character asset and voice versions, confirmed scene checkpoints, actual artifacts, smart video ratio, and any unresolved limitation.
