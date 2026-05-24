---
inclusion: always
---

# Kiro adaptation notes

This file is a Skill written in Anthropic's skill format. When the user triggers it
(see the `description` field below), follow the workflow as guidance, not as a
rigid script. Notes for running it inside Kiro Web:

- Wherever the skill says "use `AskUserQuestion`", just ask the user the question
  directly in chat. Keep the rule of **one question at a time** with sensible defaults
  offered as the first option.
- Render the two final prompts (Stage 1 and Stage 2) inside fenced code blocks so the
  user can copy them in one click.
- Beat timings in Stage 2 must sum exactly to the requested DURATION. If they do
  not, ask the user to fix them before producing the Stage 2 prompt.
- Improvise where it helps the user — these are recommendations, not strict rails.
  If context calls for a different default, propose it and explain why.
- The archive path under `~/Obsidian/...` only applies if the user actually has that
  vault. If not, skip the archive step or offer to save the prompts inside the repo
  instead.

The original skill content begins below this line.

---

---
name: seedance-storyboard-to-video
description: Use this skill whenever the user wants to run the full TWO-STAGE pipeline — turn a single reference image into a multi-frame storyboard sheet (Stage 1, GPT-Image 2) AND then into a cinematic video prompt (Stage 2, Seedance 2.0). Triggers on "storyboard to video", "storyboard pipeline", "image to video", "shot-by-shot video", "cinematic sequence from image", "seedance prompt", "video aus storyboard", "kompletter video workflow", or any request to go from one keyframe to a finished Seedance prompt with a storyboard in between.
---

# Seedance Storyboard → Video Skill

## Purpose

A two-stage pipeline that turns ONE reference image into a finished Seedance 2.0 video prompt:

1. **Stage 1** — Generate a multi-frame storyboard sheet (3×3 = 9 shots or 4×4 = 16 shots) using GPT-Image 2.
2. **Stage 2** — Use the storyboard + original reference as paired anchors to produce a beat-timed Seedance 2.0 prompt with locked camera language, physics, lighting continuity and audio.

The output of this skill is TWO copy-paste-ready prompts — one for each stage. The user runs them in their image/video tool of choice.

## Workflow

### STEP 1 — Collect the reference image

Ask via `AskUserQuestion`:

> Wo liegt das Referenzbild für den Storyboard?

Verify the file exists with the Read tool. Briefly describe what you see to confirm character + setting are clear.

### STEP 2 — Collect Stage 1 metadata (Q&A loop)

ONE QUESTION AT A TIME via `AskUserQuestion`.

| Field | Default option | Notes |
|-------|----------------|-------|
| BOARD_ID | (auto-generate from scene title) | Short unique tag, e.g., "SB-NIGRIN-001" |
| SCENE DESCRIPTION | (must ask) | Plain-language: character + location + emotional arc |
| GRID | 3×3 (9 shots) | Or 4×4 (16 shots) for longer sequences |
| STYLE MODE | Realistic | Or Sketch (with motion-arrow annotations) |
| EMOTIONAL ARC | tension rising | calm-to-chaos · reveal · pursuit · meditation · confrontation |

### STEP 3 — Output Stage 1 prompt

Use the **Storyboard Template** below with all `{{PLACEHOLDERS}}` replaced. Render in a fenced code block. Add the standard instruction:

```
✅ Stage 1 fertig. So gehts weiter:
1. Öffne dein Tool mit GPT-Image 2
2. Hänge das Referenzbild an
3. Paste den Prompt oben rein
4. Generieren → du bekommst das Storyboard-Sheet zurück
5. Speichere die Storyboard-PNG — du brauchst sie für Stage 2
```

### STEP 4 — Confirm continuation

Use `AskUserQuestion`:

> Direkt mit Stage 2 (Seedance Video Prompt) weitermachen?

Options: "Ja, weiter zu Stage 2" / "Nein, später".

If "Nein", offer archive and stop. If "Ja", continue.

### STEP 5 — Collect Stage 2 inputs

The user needs to point to the generated storyboard (Stage 1 output) — they may not have it yet. Allow either:
- "Storyboard ist bereits generiert" → ask for file path
- "Wird später eingehängt" → use placeholder `<STORYBOARD_PNG>` in the prompt

Then collect Stage 2 metadata:

| Field | Default option | Notes |
|-------|----------------|-------|
| DURATION | 10 seconds | Total video length |
| ASPECT_RATIO | 9:16 (vertical) | Or 16:9 / 1:1 |
| STYLE_TAG | photorealistic cinematic | Or "live-action cinematic", "anamorphic film", "documentary handheld" |
| TIMED BEATS | (must ask) | Free-form beat-by-beat brief — timings must sum to DURATION |
| CAMERA LANGUAGE | (suggest defaults) | dolly-in · push-in · pull-back · handheld follow · tracking · orbit · crane · whip pan · lock-off · rack focus |
| AUDIO DIRECTION | (must ask) | ambient · SFX hits · music genre · dialogue/breathing/footsteps |

### STEP 6 — Output Stage 2 prompt

Use the **Seedance Video Template** below with all `{{PLACEHOLDERS}}` replaced. Fenced code block. Add:

```
✅ Stage 2 fertig. So gehts weiter:
1. Öffne Seedance 2.0
2. Hänge BEIDE Bilder an: @image1 = das Storyboard (aus Stage 1), @image2 = das Original-Referenzbild
3. Paste den Prompt oben rein
4. Generieren → dein finales Video
```

### STEP 7 — Optional archive

`~/Obsidian/Hormozi/20 - Konversationen/Character Boards/Storyboard - {{BOARD_ID}}.md` — saves both prompts together so the user can re-run either stage.

---

## Template — Stage 1: Storyboard Sheet (GPT-Image 2)

```text
Generate a single high-resolution storyboard sheet titled "STORYBOARD — {{BOARD_ID}}" using the attached photo as the single source of truth for the character, wardrobe, location, lighting and color grade. Character identity (face, body, wardrobe, hairstyle, skin tone) must remain IDENTICAL across every frame. Do not invent new outfit elements, new locations, or new character traits beyond what is visible in the reference.

Layout: dark near-black background (#0A0A0A), bold yellow "STORYBOARD — {{BOARD_ID}}" header top-left, faint film-grain overlay. Arrange the storyboard as a {{GRID}} grid of {{N}} numbered frames, each frame 16:9, with a clean 2-word uppercase shot label below each frame (e.g., "ESTABLISHING WIDE", "PROFILE TURN", "RACK FOCUS").

Style mode: {{STYLE_MODE}}.
{{STYLE_MODE_DESCRIPTION}}

Scene description for sequencing the {{N}} frames:
{{SCENE_DESCRIPTION}}

Emotional arc across the sequence: {{EMOTIONAL_ARC}}.

Frame sequencing rules:
- Read order is left-to-right, top-to-bottom.
- Each frame is a distinct camera setup that advances the beat — not a copy of the previous frame.
- Coverage should mix wide / medium / close-up / insert / OTS at least once across the sequence.
- Character continuity is absolute: same wardrobe, same lighting era, same physical features in every frame.
- Sketch mode (if selected) uses minimalist line art with optional motion annotations: orange arrows for camera motion, green arrows for character movement, blue arcs for object/energy paths.

Bottom caption (small, English): "Frame-by-frame visual continuity reference for downstream Seedance 2.0 generation."
Bottom-right tags: STYLE · {{STYLE_MODE}} · Cinematic · Continuous

Output: photorealistic if Realistic mode, clean storyboard-sketch if Sketch mode. 8K, fine grain, cinematic color grading derived from the reference image.
```

---

## Template — Stage 2: Seedance Video Prompt

```text
Single continuous shot, {{DURATION}}s, {{ASPECT_RATIO}}, {{STYLE_TAG}}.

@image1 is the storyboard reference — read it in order (left-to-right, top-to-bottom) and follow the beat progression exactly.
@image2 is the character + scene anchor — face, body, wardrobe, lighting era, color grade and location must remain identical to it throughout the shot.

SCENE OVERVIEW:
{{SCENE_OVERVIEW}}

TIMED BEATS (must sum exactly to {{DURATION}}s):
{{TIMED_BEATS}}

CAMERA LANGUAGE:
{{CAMERA_LANGUAGE}}

PHYSICS AND MOTION:
- Character motion follows real human biomechanics — natural weight shifts, footfall contact, controlled momentum.
- Object motion respects mass, inertia and gravity. No floating, no slide-on-rails movement unless explicitly part of the beat.
- Hair, fabric and secondary motion respond to acceleration and ambient air.

LIGHTING CONTINUITY:
- Match the reference image's lighting direction, color temperature and contrast across the full duration.
- Shadows track camera motion realistically — no popping, no flicker, no inconsistent fill.

AUDIO:
{{AUDIO_DIRECTION}}

STYLE:
- Visual tone: {{STYLE_TAG}}.
- Color grade and contrast derived from the reference image — do not stylize away from it.
- Frame composition follows the storyboard reference for every beat.

QUALITY:
- 8K source intent, fine film grain, cinematic depth of field appropriate to the lens implied by the camera language.
- No artifacts, no morphing limbs, no identity drift on the character.
- Final output is broadcast-grade.
```

---

## Important Rules

- **One question at a time** via `AskUserQuestion`. Use sensible defaults as the first option.
- **Always render both prompts in fenced code blocks** so they are easy to copy.
- **Beat timings must sum exactly to DURATION**. If the user gives beats that don't add up, ask them to correct before generating Stage 2.
- **Character identity is sacred** — both templates explicitly forbid inventing wardrobe or location elements.
- **Stage 2 needs both images** — when the user pastes the Stage 2 prompt, they must attach BOTH the storyboard (Stage 1 output) and the original reference. Remind them in the closing instruction.
- **Archive prefix**: `Storyboard - ` so storyboard pipelines group together in the vault.
- **Belongs to the Seedance Reference Board family** — related skills: `seedance-character-board`, `seedance-shot-board`, `seedance-object-board`, `seedance-pose-board`, `seedance-creature-board`.

## Quick Start (Skip-the-Setup Mode)

If the user wants the fast path: skip Stage 1 metadata Q&A, use defaults (3×3, Realistic, "tension rising"), generate Stage 1 immediately. After Stage 1, ask only for DURATION, ASPECT_RATIO and the timed beats — fill the rest with defaults. Useful for iteration speed when the user knows the pipeline.

Trigger: user says "quick storyboard", "fast pipeline", or "schnell durchziehen".
