---
inclusion: always
---

# INVOCATION (Kiro)

**Command to activate this skill:** `/sb-character`

**Strict activation rule:**
- Activate the workflow below ONLY when the user types one of these exact commands
  at the start of a message:
  - `/sb-character`
  - `/sb-character quick`  (skip-the-setup mode — see "Quick Start" section)
- Do NOT auto-activate on loose phrases like "character sheet" or "reference" alone.
- If the user types `/sb-character` with no other context, start at STEP 1.
- If the user is mid-conversation about something else and types the command,
  treat it as a hard reset into this workflow.

# KIRO ADAPTATION NOTES

This file is a Skill written in Anthropic's skill format. When activated, follow
the workflow as **guidance, not as a rigid script**. Notes for Kiro Web:

- Wherever the skill says "use `AskUserQuestion`", just ask the user the question
  directly in chat. **One question at a time** with sensible defaults offered as
  the first option.
- Render the final prompt inside a fenced code block so the user can copy in one
  click.
- Improvise where it helps the user — these are recommendations, not rigid rails.
- Reply in the language the user is using in chat (German, Russian, English…).
- The archive path under `~/Obsidian/...` only applies if the user actually has
  that vault. Otherwise skip the archive step or offer to save inside the repo.

The original skill content begins below this line.

---

---
name: seedance-character-board
description: Use this skill when the user wants a multi-angle CHARACTER REFERENCE SHEET (not a narrative storyboard). The output is a single grid image showing the SAME character from multiple camera angles on a neutral background — front, 3/4, profile, back, top-down, low-angle, close-ups, walk cycle, expression set, texture macro. The point is maximum identity consistency for downstream image/video tools (Seedance, GPT-Image 2, etc.). NO TEXT is rendered on the sheet by default — every pixel goes to character detail. Triggers on "character sheet", "reference sheet", "model sheet", "character turnaround", "multi-angle reference", "character consistency board", "ракурсный лист персонажа", "референс-лист персонажа", "Charakter Referenz".
---

# Seedance Character Board Skill

## Purpose

A single-stage pipeline that turns ONE reference image (or a description) into a
production-grade character reference sheet — the same character rendered from
multiple camera angles on a neutral background, locked for identity continuity.

The output is ONE copy-paste-ready prompt for GPT-Image 2 (or equivalent).

## Pro Defaults (the "good answer" for ~90% of users)

When in doubt, ship these defaults. They are the result of iterating with users
who care about downstream consistency for video pipelines:

| Field | Pro Default | Why |
|-------|-------------|-----|
| **TEXT MODE** | **NO-TEXT (strict)** | No header, labels, captions, tags, watermarks, panel numbers. Text steals pixels that should go to skin/fabric/hair detail. Identity is conveyed by the imagery itself. |
| **ANGLE SET** | **Extended 16 (4×4 grid)** | The 9-pose standard turnaround leaves out hands, walk cycle, expressions and texture macros — all of which are critical anchors for downstream Seedance video. |
| **BACKGROUND** | **Solid neutral light gray `#C8C8C8`** | Industry-standard turnaround background. Doesn't tint skin or wardrobe color readings. No gradient, no cyclorama curve, no environment. |
| **LIGHTING** | **3-point studio, 5500K, CRI 95+** | Soft key front-left ~45°, gentle fill front-right ~45°, subtle rim/hair from back. Calibrated for accurate skin tone. |
| **STYLE MODE** | **Realistic photography** | Photoreal is the only style that preserves identity at the level Seedance needs. |
| **EXPRESSION** | **Neutral on all body angles + close-up. Smile only on Panel 14, Serious only on Panel 15.** | Pure identity anchor on body panels; emotion isolated to dedicated cells. |
| **WARDROBE LOCK** | **As in reference, identical across every panel** | No outfit changes, no accessories appearing/disappearing. |
| **DETAIL PRIORITY** | **Maximum (skin pores, fabric weave, individual hair strands, tack-sharp deep DoF)** | Anti-AI-smoothing, anti-plastic-skin, anti-airbrush. |

## Workflow

### STEP 1 — Collect the reference

Ask the user where the reference image lives. If they don't have one, offer:
- "Wird später eingehängt" → use placeholder `<REFERENCE_IMAGE>` in the prompt
- "Nur Beschreibung, kein Bild" → ask for a detailed character description that
  the model will use as the source of truth instead of an image

Verify the file exists with the Read tool if a path is given. Briefly describe
what you see to confirm character is clear.

### STEP 2 — Collect metadata (Q&A loop, ONE question at a time)

Defaults are pre-loaded from the Pro Defaults table above. ALWAYS offer the
default as option 1. Only deviate if the user explicitly asks.

| Field | Default option | Notes |
|-------|----------------|-------|
| BOARD_ID | (auto-generate from character name; never rendered on sheet) | Internal only, used for archive filename. e.g. "CB-RL-001" |
| ANGLE SET | Extended 16 (4×4) | Or Standard 9 (3×3) · Pro 20 (5×4) · Custom |
| BACKGROUND | Neutral light gray #C8C8C8 | Or pure white · pure black · studio gradient · custom HEX |
| STYLE MODE | Realistic photography | Or 3D render · Concept art · Sketch / line art |
| EXPRESSION SET | Neutral except panels 14 (smile) and 15 (serious) | Or all neutral · all serious · custom |
| WARDROBE LOCK | As in reference | Or "T-pose plain underlayer" for asset-creation use |
| TEXT MODE | NO-TEXT (strict) | Or "labels under panels" if the user explicitly wants annotations |

If the user picks all defaults, you can fast-path through with a single
"all defaults?" confirmation and go straight to STEP 3.

### STEP 3 — Output the prompt

Use the **Character Board Template** below with all `{{PLACEHOLDERS}}` replaced.
Render in a fenced code block. Add the standard instruction:

```
✅ Character Board fertig. So gehts weiter:
1. Öffne dein Tool mit GPT-Image 2 (oder Nano Banana / Midjourney v7 mit Reference)
2. Hänge das Referenzbild an (falls vorhanden)
3. Paste den Prompt oben rein
4. Generieren → du bekommst dein Character Reference Sheet
5. Speichere die PNG — du brauchst sie als Identity Anchor für alle weiteren
   Storyboards, Posen, Shots und Videos
```

### STEP 4 — Optional archive

`~/Obsidian/Hormozi/20 - Konversationen/Character Boards/Character - {{BOARD_ID}}.md`
— saves the prompt so the user can re-run later with a different background or
expression set.

---

## Angle Set Definitions

### Standard 9 (3×3 grid)

1. **FRONT** — full body, eye level, straight-on, character standing relaxed
2. **3/4 RIGHT** — full body, eye level, character rotated 45° to the right
3. **PROFILE RIGHT** — full body, eye level, character rotated 90° to the right
4. **3/4 BACK RIGHT** — full body, eye level, character rotated 135° (back-three-quarter right)
5. **BACK** — full body, eye level, straight back view
6. **3/4 BACK LEFT** — full body, eye level, character rotated 135° to the left
7. **PROFILE LEFT** — full body, eye level, character rotated 90° to the left
8. **3/4 LEFT** — full body, eye level, character rotated 45° to the left
9. **CLOSE-UP FACE** — neutral expression, head and shoulders, front view

### Extended 16 (4×4 grid) — RECOMMENDED DEFAULT

Standard 9 plus:

10. **LOW ANGLE HERO** — full body, camera below waist looking up, character front-facing
11. **HIGH ANGLE / TOP-DOWN** — full body, camera above looking down, character front-facing
12. **HANDS CLOSE-UP** — both hands shown clearly, neutral relaxed pose
13. **WALK CYCLE FRAME** — full body, profile view, mid-stride
14. **EXPRESSION CLOSE-UP — SMILE** — head and shoulders, front view, genuine warm smile
15. **EXPRESSION CLOSE-UP — SERIOUS / DETERMINED** — head and shoulders, front view, focused gaze
16. **TEXTURE MACRO** — extreme close-up on distinctive wardrobe material AND a patch of visible skin (forearm, neck, or collarbone)

### Pro 20 (5×4 grid) — for AAA-level character creation

Extended 16 plus:

17. **REAR 3/4 LOW ANGLE** — back-three-quarter from below, dynamic hero framing
18. **EXPRESSION CLOSE-UP — SUBTLE / THOUGHTFUL** — closed mouth, eyes slightly off-camera
19. **FOOTWEAR / GROUNDING DETAIL** — close-up on shoes/boots and how the character contacts the floor
20. **HAIRLINE / EAR DETAIL** — profile or 3/4 close-up isolating hair root direction, ear shape, sideburn line, jaw curve

---

## Template — Character Reference Sheet (GPT-Image 2)

```text
Generate a single high-resolution character reference sheet using {{REFERENCE}} as the absolute single source of truth for the character (face, body, wardrobe, hairstyle, skin tone, distinguishing features). Character identity must remain ABSOLUTELY IDENTICAL across every panel — same face, same proportions, same wardrobe, same lighting, same skin tone. Do not invent new outfit elements, new hairstyles, scars, tattoos, jewelry, or features that are not visible in the reference.

{{TEXT_MODE_BLOCK}}

LAYOUT:
- {{GRID}} grid of {{N}} uniformly sized panels.
- Each panel is a clean rectangular crop showing the character at the specified angle.
- Thin (2–3 px) neutral separator gutters between panels in the same {{BACKGROUND}} background tone — visually subtle, no borders, no frames.
- Read order is left-to-right, top-to-bottom.

PANEL ORDER (read left-to-right, top-to-bottom):
{{PANEL_LIST}}

EXPRESSIONS (strict):
{{EXPRESSION_BLOCK}}

BACKGROUND:
- Solid neutral {{BACKGROUND_DESCRIPTION}} across every panel — ABSOLUTELY uniform, no gradient, no environment, no props, no studio cyclorama curve.
- Soft natural contact shadow directly under the feet for grounding on full-body panels. No long cast shadows, no environmental shadows on the background itself.
- Floor and background blend seamlessly — no horizon line.

LIGHTING (identical across every panel):
- Neutral 3-point studio lighting calibrated for accurate skin tone reproduction.
- Soft key light from front-left (~45°), gentle fill from front-right (~45°), subtle rim/hair light from back. Color temperature ~5500K. CRI 95+.
- Same lighting setup applied identically to every panel so the character reads as the SAME model under the SAME light from every angle.
- No dramatic shadows, no colored gels, no high-contrast lighting. Goal is even, faithful, reference-grade illumination that reveals true skin tone, fabric color, and hair color.

WARDROBE LOCK (ABSOLUTE):
- {{WARDROBE_LOCK_DESCRIPTION}}
- Nothing appears or disappears between panels. No coats added, no jewelry swapped.
- Wardrobe state must match the reference exactly — do not "clean up" or "stylize" what is shown.

IDENTITY CONTINUITY RULES (ABSOLUTE):
- Face geometry, eye color, eyebrow shape, nose, lips, jawline must be IDENTICAL across all panels.
- Body proportions (height, shoulder width, limb length, body type) must be IDENTICAL across all panels.
- Skin tone, undertone, and skin texture must be IDENTICAL across all panels.
- Hair style, length, color, density, and parting must be IDENTICAL across all panels.
- Do NOT add tattoos, scars, jewelry, makeup, beard variations, or any features that are not visible in the reference.
- Do NOT alter age, weight, posture beyond what is required by the angle.

DETAIL PRIORITY (this is an identity-locked reference, not a stylized illustration):
- Maximum visible skin texture — pores, fine vellus hair, subtle micro-shadows, natural color variation.
- Maximum visible fabric detail — weave, stitching, seam structure, fiber direction, wear patterns.
- Maximum visible hair detail — individual strands, root direction, natural sheen.
- Tack-sharp focus from edge to edge of each panel. Deep depth of field. No bokeh, no atmospheric haze, no soft focus.
- 8K source intent. Fine native grain only.

NEGATIVE DIRECTIVES (DO NOT):
- Do NOT apply AI-style smoothing, plastic skin, beauty filters, airbrush, or skin softening.
- Do NOT use shallow depth of field, lens blur, bokeh, or background blur.
- Do NOT add lens flares, light leaks, vintage filters, color grading effects, or artistic color shifts.
- Do NOT add motion blur, except on the WALK CYCLE panel where minimal natural foot motion blur is acceptable.
- Do NOT generate a stylized illustration — this must read as a photographic reference.
- Do NOT produce a cyclorama / curved studio sweep — background is flat and seamless.
- Do NOT add camera UI, HUD elements, vignettes, or letterboxing.

OUTPUT:
- Photorealistic, neutral studio color grading (sRGB).
- Aspect ratio of overall sheet: {{SHEET_ASPECT}}.
- Each individual panel framed cleanly within the grid cell with consistent margin around the character silhouette.
- Final result is a production-grade, identity-locked, {{TEXT_MODE_TAG}} character turnaround sheet suitable as the master anchor for downstream Seedance 2.0 image-to-video and storyboard generation.
```

---

## Placeholder Fill Logic

When generating the final prompt, fill placeholders as follows:

### `{{REFERENCE}}`
- If user provided an image: `the attached photo`
- If reference will be attached later: `<REFERENCE_IMAGE>`
- If description-only: `the following character description: "{{DESCRIPTION}}"`

### `{{TEXT_MODE_BLOCK}}` — when TEXT_MODE = NO-TEXT (default)

```
NO-TEXT MODE (strict): the rendered sheet must contain ZERO text, ZERO labels, ZERO captions, ZERO numbers, ZERO watermarks, ZERO logos, ZERO tags, ZERO header. No writing of any kind anywhere on the image. Do not place panel labels under the panels. Do not place a title at the top. Every pixel of the canvas is dedicated to the character imagery. Empty space between panels is uniform neutral background only.
```

### `{{TEXT_MODE_BLOCK}}` — when TEXT_MODE = LABELED (only if user opts in)

```
LABEL MODE: render a clean uppercase label below each panel describing the angle (e.g., "FRONT", "3/4 RIGHT", "PROFILE LEFT"). Label typography: thin sans-serif, 1–2% of canvas height, dark gray on the neutral background. No header, no captions, no other text anywhere on the sheet.
```

### `{{GRID}}` and `{{N}}`
- Standard 9 → `3×3`, `9`
- Extended 16 → `4×4`, `16`
- Pro 20 → `5×4`, `20`
- Custom → as specified

### `{{PANEL_LIST}}`
- Inline the numbered list of panels from the chosen Angle Set with each panel's
  detailed framing instruction (full body / head and shoulders / extreme close-up,
  eye-level / low / high, neutral / smile / serious, etc.).

### `{{EXPRESSION_BLOCK}}` — default

```
- Panels 1–13 and 16: NEUTRAL relaxed expression. Mouth closed but not tense. Eyes open, looking forward (or in the natural direction of the angle for profiles/backs). No smile, no frown.
- Panel 14: genuine warm smile, eyes engaged.
- Panel 15: serious, determined, slight brow tension, focused gaze.
```

### `{{BACKGROUND}}` and `{{BACKGROUND_DESCRIPTION}}`
- Default: `#C8C8C8` and `light gray (#C8C8C8)`
- White: `#FFFFFF` and `pure white (#FFFFFF)`
- Black: `#0A0A0A` and `pure black (#0A0A0A)`
- Custom: HEX and human description.

### `{{WARDROBE_LOCK_DESCRIPTION}}` — default

```
Same outfit in every panel. Same garment, same color, same fit, same wrinkles, same accessories.
```

### `{{SHEET_ASPECT}}`
- 3×3 → `square (1:1)` or `slightly wide (4:3)` if aspect of grid demands
- 4×4 → `square (1:1)`
- 5×4 → `slightly wide (5:4)`

### `{{TEXT_MODE_TAG}}`
- NO-TEXT → `text-free`
- LABELED → `minimally labeled`

---

## Important Rules

- **One question at a time.** Use sensible defaults as the first option.
- **Render the final prompt in a fenced code block.**
- **Identity is sacred.** The template forbids inventing wardrobe, features, or
  accessories not present in the reference.
- **Background is neutral by default** — this sheet is for identity, not for
  scene context.
- **Same lighting in every panel** — non-negotiable for downstream consistency.
- **NO-TEXT is the default** — the user has to explicitly ask for labels to
  override.
- **Archive prefix**: `Character - ` so character boards group together in the
  vault.
- **Belongs to the Seedance Reference Board family** — related skills:
  `seedance-storyboard-to-video`, `seedance-shot-board`,
  `seedance-object-board`, `seedance-pose-board`, `seedance-creature-board`.

## Quick Start (Skip-the-Setup Mode)

Trigger: user says `/sb-character quick`, "quick character sheet",
"schnell durchziehen", or "быстрый ракурсный лист".

Behavior: skip metadata Q&A entirely, use Pro Defaults end-to-end (Extended 16,
NO-TEXT, neutral light gray, Realistic, neutral expressions with smile/serious
on panels 14/15, wardrobe as in reference). Ask ONLY for the reference image
path (or accept "later" → placeholder), then immediately produce the final
prompt.
