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
- **Defaults are MY professional recommendation under the user's stated use
  case** — they are NOT pre-loaded from previous conversations. Always re-derive
  the recommendation from this file's "Use-Case to Pro Preset" matrix.
- Reply in the language the user is using in chat (Russian, German, English).
- Improvise where it helps the user — these are recommendations, not rigid rails.

The original skill content begins below this line.

---

---
name: seedance-character-board
description: Use this skill when the user wants a multi-angle CHARACTER REFERENCE SHEET (not a narrative storyboard). The output is a single grid image showing the SAME character from multiple camera angles on a neutral background, with absolute forensic-grade facial biometric continuity across every panel — same face topology, anatomy, iris color, ear/nose/lip/jaw geometry whether shot at full distance or extreme close-up. The point is identity that would pass a forensic facial comparison side-by-side. NO TEXT is rendered on the sheet — every pixel goes to character detail. Triggers on "character sheet", "reference sheet", "model sheet", "character turnaround", "multi-angle reference", "character consistency board", "forensic identity sheet", "ракурсный лист персонажа", "референс-лист персонажа", "Charakter Referenz".
---

# Seedance Character Board Skill

## Purpose

A single-stage pipeline that turns ONE reference image (or a description) into a
production-grade character reference sheet — the same character rendered from
multiple camera angles on a neutral background, locked for **forensic-grade
identity continuity**.

The output is ONE copy-paste-ready prompt for GPT-Image 2 (or equivalent).

---

## CONSTANTS (never variable, never asked)

These are baked into every prompt this skill produces. Do not negotiate them
with the user, do not offer them as options, do not skip them.

### CONSTANT 1 — NO-TEXT MODE (strict)
The rendered sheet contains ZERO text of any kind: no header, no titles, no
panel labels, no captions, no watermarks, no logos, no panel numbers, no tags.
Every pixel of canvas is dedicated to character imagery. Empty space between
panels is uniform neutral background only.

### CONSTANT 2 — FORENSIC-GRADE FACIAL BIOMETRIC LOCK
The character's face must remain absolutely identical across every panel at
every camera distance — wide, full-body, three-quarter, close-up, extreme
macro. The standard is: a trained forensic facial comparison expert examining
any two panels side by side must conclude they are the same individual.

This means **all** of the following biometric markers stay identical from
panel to panel:

- **Eye region:** interocular distance, eye shape and tilt (palpebral fissure
  inclination), upper/lower lid geometry, iris color (exact tone), pupil
  baseline size, sclera tone, eyelash density and direction.
- **Eyebrow region:** arch shape, density, root direction, tail length, hair
  color, position relative to brow ridge.
- **Nose:** dorsal profile, bridge angle, length, tip rotation and projection,
  alar shape and width, nostril topology, columella curve. The 3D nose mesh
  must be the same nose seen from frontal, three-quarter, profile and
  worm's-eye angles.
- **Mouth:** vermilion border geometry, philtrum length and ridge depth,
  cupid's bow shape, mouth corner position and angle, lip volume distribution
  upper-vs-lower, resting muscle tone.
- **Jawline and chin:** gonion angle (mandibular angle), mandible width and
  curve, chin projection and shape (square / pointed / cleft), submental
  contour.
- **Cheek / zygomatic structure:** cheekbone height and prominence, malar fat
  distribution, infraorbital hollow depth, buccal hollow.
- **Forehead and brow ridge:** hairline shape and density, frontal bone
  curvature, supraorbital ridge prominence, glabella geometry.
- **Ears (whenever visible):** helix curl, lobe shape and attachment
  (free-vs-attached), antihelix fold, scapha shape, conchal bowl depth, tragus
  shape, ear height relative to eyes and nose tip.
- **Skin:** pore density and pattern, mole/freckle/blemish positions (if
  present in the reference), undertone, sebaceous activity, micro-texture.
- **Hair:** style, length, color, density, root direction, parting, hairline
  edge.

This applies at every camera distance:
- In wide / full-body panels the face must remain the SAME face — do not let
  it drift, simplify, average toward an "AI ideal", or "stylize down" because
  it occupies fewer pixels.
- In close-ups the same biometric structure must read at higher detail — same
  nose mesh, same lip vermilion, same eye geometry, same ears.
- Never produce a "different-but-similar" face.

The reference image (or detailed description) is the absolute biometric ground
truth. Do not interpolate, beautify, harmonize toward symmetry, or shift
toward an idealized average face.

### CONSTANT 3 — LENS DISCIPLINE
- Full-body and three-quarter panels: 50mm full-frame equivalent — neutral
  perspective.
- Head-and-shoulders close-ups: 85–105mm equivalent — eliminates wide-angle
  distortion of facial features.
- Macro detail panels (eyes, nose, mouth, ears, hands, texture): 100mm macro
  equivalent.
- ABSOLUTELY NO wide-angle (<35mm) lens distortion on the character. Faces
  must not be stretched, pinched, or fish-eyed.

### CONSTANT 4 — IDENTICAL LIGHTING
The same light setup is rendered in every panel so the character reads as the
same person under the same light from every angle. Variations in light cannot
be used to compensate for identity drift.

---

## Variables (asked one question at a time, with MY recommendation as option 1)

These are derived from the user's stated **use case** (see "Use-Case to Pro
Preset" matrix below). Always offer my professional recommendation as option
1, with reasoning. Do NOT pre-load the user's previous choices as defaults.

| # | Variable | Examples |
|---|----------|----------|
| 1 | REFERENCE | attached photo · placeholder for later · description-only |
| 2 | USE CASE | identity anchor for video · casting / actor lookbook · 3D / game-ready asset · comic / story bible · forensic / biometric document |
| 3 | ANGLE SET | Forensic 6 · Casting 9 · Identity Lock 12 · Director's 12 · Extended 16 · Pro 20 · Custom |
| 4 | BACKGROUND | light gray #C8C8C8 · pure white · pure black · custom HEX |
| 5 | LIGHTING STYLE | editorial neutral studio (5500K, CRI 95+) · natural diffused · soft north-window · custom |
| 6 | EXPRESSION SET | all neutral · neutral + smile + serious · full emotion range · custom |
| 7 | WARDROBE LOCK | as in reference · stripped to plain underlayer · custom outfit |

BOARD_ID is auto-generated silently from the use case + a short slug
(e.g., `CB-VIDEO-001`). It is NEVER rendered on the sheet (CONSTANT 1).

---

## Use-Case to Pro Preset matrix (MY recommendation engine)

When the user names their use case, derive recommendations from this matrix.
Always SHOW the user the recommendation and ask them to confirm or override.
Never silently apply.

### A. Identity anchor for downstream image-to-video (Seedance / Runway / Kling)

| Variable | My recommendation | Reason |
|----------|-------------------|--------|
| ANGLE SET | **Identity Lock 12** | Six body angles + six biometric close-ups (face neutral, eyes macro, nose/philtrum macro, ears L/R, jaw+chin macro, hairline). The close-ups give downstream video models the per-feature anchor they need to lock identity through motion. |
| BACKGROUND | Light gray #C8C8C8 | No tint contamination on skin/wardrobe color. |
| LIGHTING | Editorial neutral 3-point studio, 5500K, CRI 95+ | Same flat reference light Seedance can read consistently. |
| EXPRESSION | All neutral | Pure identity anchor — no muscle activation distorting biometric markers. |
| WARDROBE | As in reference | Identical across panels. |

### B. Casting / actor lookbook

| Variable | My recommendation | Reason |
|----------|-------------------|--------|
| ANGLE SET | **Casting 9** | Full body front + profile L/R + back, three-quarter L/R, headshot neutral + smile + serious. The standard PCR (commercial casting standard). |
| BACKGROUND | Light gray #C8C8C8 (industry standard) | |
| LIGHTING | Editorial neutral 3-point studio | |
| EXPRESSION | Neutral + smile + serious | Standard casting deliverable. |
| WARDROBE | As in reference | |

### C. 3D / game-ready character asset turnaround

| Variable | My recommendation | Reason |
|----------|-------------------|--------|
| ANGLE SET | **Pro 20** (5x4) | Standard 9 + low/high angles + walk cycle + hands + ears + texture macros + footwear/grounding + hairline. Maximum coverage for sculptors. |
| BACKGROUND | Pure white #FFFFFF | Cutout-ready for asset pipelines. |
| LIGHTING | Flat editorial neutral, slightly stronger fill | Reduces baked shadows that interfere with retopology reference. |
| EXPRESSION | All neutral | Sculptors don't want emotional bias in geometry. |
| WARDROBE | As in reference (or stripped to plain underlayer if user explicitly wants the body geometry isolated) | |

### D. Comic / book / story bible character reference

| Variable | My recommendation | Reason |
|----------|-------------------|--------|
| ANGLE SET | **Director's 12** | Story-relevant: full body front + 3/4 L/R + profile L/R + back + walk cycle + low hero + headshots (neutral + 2 character expressions) + hands. |
| BACKGROUND | Light gray #C8C8C8 | |
| LIGHTING | Editorial neutral 3-point studio | Pure reference; the comic itself adds drama. |
| EXPRESSION | Neutral + 2 character-defining (e.g., smile + serious, or smile + intense) | |
| WARDROBE | As in reference | |

### E. Forensic / biometric document

| Variable | My recommendation | Reason |
|----------|-------------------|--------|
| ANGLE SET | **Forensic 6** | Frontal + profile L + profile R + 3/4 L + 3/4 R + top crown — minimum biometric coverage as in police booking standards, plus macro eyes, macro nose+philtrum, macro ears L+R as add-on. |
| BACKGROUND | Light gray #C8C8C8 | |
| LIGHTING | Editorial neutral 3-point studio, 5500K, CRI 95+ | Forensic-grade neutrality. |
| EXPRESSION | All neutral, mouth closed, eyes open, looking straight ahead | Booking standard. |
| WARDROBE | As in reference (or per case requirement) | |

---

## Workflow

### STEP 1 — Collect the reference

Ask in chat:

> Где лежит референс персонажа? (или подключим позже / опишем словами)

Options:
1. Path to image file in workspace
2. "Wird später eingehängt / подключим позже" → use placeholder `<REFERENCE_IMAGE>`
3. "Только описание" → ask for a detailed character description (face, body,
   wardrobe, hair, skin tone, distinguishing features). The description
   becomes the biometric ground truth.

If a path is given, read the file and briefly describe what you see. Confirm
character is clear.

### STEP 2 — Use case (THE most important question)

Ask in chat:

> Для чего тебе этот лист? (определяет мои рекомендации по сетке, фону,
> свету и эмоциям)

Offer:
1. Identity anchor для downstream видео (Seedance / Runway / Kling)
2. Casting / actor lookbook
3. 3D / game-ready character asset
4. Comic / story bible character reference
5. Forensic / biometric document
6. Other → ask for details

The answer here determines which row of the Use-Case to Pro Preset matrix to
apply for steps 3–7.

### STEP 3 — ANGLE SET

Show MY recommendation as option 1 (from the matrix), with one-line reasoning.
Then list the other angle sets as alternatives.

> Под твою задачу я рекомендую **{{RECOMMENDED_SET}}** — {{REASON}}.
>
> Хочешь так, или сменить?
> 1. ✅ {{RECOMMENDED_SET}}
> 2. Forensic 6 (3x2)
> 3. Casting 9 (3x3)
> 4. Identity Lock 12 (4x3)
> 5. Director's 12 (4x3)
> 6. Extended 16 (4x4)
> 7. Pro 20 (5x4)
> 8. Custom — назови сам

### STEP 4 — BACKGROUND

Show MY recommendation, then alternatives.

> Под твою задачу — **{{RECOMMENDED_BG}}**, потому что {{REASON}}.
> 1. ✅ {{RECOMMENDED_BG}}
> 2. Light gray #C8C8C8
> 3. Pure white #FFFFFF
> 4. Pure black #0A0A0A
> 5. Custom HEX

### STEP 5 — LIGHTING STYLE

Show MY recommendation, then alternatives.

> 1. ✅ Editorial neutral 3-point studio (5500K, CRI 95+) — мой стандартный выбор для identity anchor
> 2. Natural diffused (north-facing window) — мягче, чуть менее техничный
> 3. Soft chiaroscuro — для художественных проектов, НЕ рекомендую если нужен биометрический референс
> 4. Custom — опиши

### STEP 6 — EXPRESSION SET

Show MY recommendation, then alternatives.

> Под твою задачу — **{{RECOMMENDED_EXPR}}**.
>
> 1. ✅ {{RECOMMENDED_EXPR}}
> 2. Все нейтральные (чистый identity anchor)
> 3. Нейтральные + smile + serious (классика casting)
> 4. Полный набор (neutral · smile · serious · surprise · contemplative · intense)
> 5. Custom

### STEP 7 — WARDROBE LOCK

> 1. ✅ Идентично референсу (по умолчанию)
> 2. Раздеть до плотного базового слоя (для asset creation)
> 3. Custom — опиши

### STEP 8 — Output the prompt

Use the **Character Board Template** below with all `{{PLACEHOLDERS}}`
replaced. Render in a fenced code block. Add the standard instruction:

```
✅ Character Board fertig. So gehts weiter:
1. Открой инструмент с GPT-Image 2 (или Nano Banana / Midjourney v7 с Reference)
2. Прикрепи референсное изображение (если оно у тебя есть)
3. Вставь промпт выше
4. Сгенерируй → получишь свой Character Reference Sheet
5. Сохрани PNG — это identity anchor для всех будущих сторибордов, поз, шотов и видео
```

### STEP 9 — Optional archive

`~/Obsidian/Hormozi/20 - Konversationen/Character Boards/Character - {{BOARD_ID}}.md`
— saves the prompt so the user can re-run later with different background or
expression set.

---

## Angle Set Catalog

### Forensic 6 (3x2 grid)

For biometric documentation. Police booking standard plus a top crown view.

1. **FRONTAL** — full body, eye level, straight-on, character standing relaxed, arms at sides, neutral.
2. **PROFILE LEFT** — full body, eye level, 90° rotation left, head straight (not turned to camera).
3. **PROFILE RIGHT** — full body, eye level, 90° rotation right.
4. **3/4 LEFT** — full body, eye level, 45° rotation left.
5. **3/4 RIGHT** — full body, eye level, 45° rotation right.
6. **TOP / CROWN** — head and shoulders, camera directly above looking down on the crown of the head, hairline and ear position visible.

Optional macro add-ons (recommended for forensic use case): eyes macro, nose+philtrum macro, ear L macro, ear R macro.

### Casting 9 (3x3 grid)

Commercial casting standard.

1. FRONTAL full body
2. PROFILE LEFT full body
3. PROFILE RIGHT full body
4. 3/4 LEFT full body
5. 3/4 RIGHT full body
6. BACK full body
7. HEADSHOT — neutral
8. HEADSHOT — genuine warm smile
9. HEADSHOT — serious / determined

### Identity Lock 12 (4x3 grid) — recommended for video anchor

Six body angles + six biometric close-ups designed to give downstream video
models the per-feature anchor they need to keep identity locked through
motion.

1. FRONTAL full body
2. 3/4 RIGHT full body
3. PROFILE RIGHT full body
4. BACK full body
5. PROFILE LEFT full body
6. 3/4 LEFT full body
7. HEADSHOT NEUTRAL — head and shoulders, frontal
8. EYES MACRO — extreme close-up on both eyes, brow to upper cheek, iris detail visible
9. NOSE + PHILTRUM MACRO — nose tip to upper lip, three-quarter angle preferred
10. EAR LEFT MACRO — left ear in profile, full helix and lobe visible
11. EAR RIGHT MACRO — right ear in profile, full helix and lobe visible
12. JAW + CHIN MACRO — chin and jawline, three-quarter low angle, neck and submental contour visible

### Director's 12 (4x3 grid)

Story-relevant for comics, illustrated books, character bibles.

1. FRONTAL full body
2. 3/4 RIGHT full body
3. PROFILE RIGHT full body
4. BACK full body
5. PROFILE LEFT full body
6. 3/4 LEFT full body
7. WALK CYCLE FRAME — profile, mid-stride
8. LOW ANGLE HERO — full body, camera below waist looking up
9. HEADSHOT — neutral
10. HEADSHOT — character expression A (e.g., smile)
11. HEADSHOT — character expression B (e.g., intense / serious)
12. HANDS CLOSE-UP — both hands, neutral relaxed pose

### Extended 16 (4x4 grid)

Generalist's choice. Wide coverage but no use-case specialization.

Standard 9 + low angle hero + top-down + hands close-up + walk cycle frame +
expression smile + expression serious + texture macro.

### Pro 20 (5x4 grid)

For 3D / game asset creation. Maximum coverage for sculptors and texture artists.

Extended 16 plus:
17. REAR 3/4 LOW ANGLE — back-three-quarter from below.
18. EXPRESSION CLOSE-UP — subtle / thoughtful (closed mouth, eyes off camera).
19. FOOTWEAR / GROUNDING DETAIL — close-up on shoes/boots and floor contact.
20. HAIRLINE / EAR DETAIL — profile or 3/4 close-up isolating hair root direction, ear shape, sideburn line, jaw curve.

---

## Template — Character Reference Sheet (GPT-Image 2)

```text
Generate a single high-resolution character reference sheet using {{REFERENCE}} as the absolute single source of truth for the character (face topology, anatomy, body geometry, wardrobe, hairstyle, skin tone, distinguishing features). Character identity must remain ABSOLUTELY IDENTICAL across every panel — same face, same biometric markers, same proportions, same wardrobe, same lighting, same skin tone. Do not invent new outfit elements, new hairstyles, scars, tattoos, jewelry, or any features that are not visible in the reference.

NO-TEXT MODE (strict): the rendered sheet must contain ZERO text, ZERO labels, ZERO captions, ZERO numbers, ZERO watermarks, ZERO logos, ZERO tags, ZERO header. No writing of any kind anywhere on the image. Do not place panel labels under the panels. Do not place a title at the top. Every pixel of the canvas is dedicated to the character imagery. Empty space between panels is uniform neutral background only.

LAYOUT:
- {{GRID}} grid of {{N}} uniformly sized panels.
- Each panel is a clean rectangular crop showing the character at the specified angle.
- Thin (2–3 px) neutral separator gutters between panels in the same {{BACKGROUND}} background tone — visually subtle, no borders, no frames.
- Read order is left-to-right, top-to-bottom.

PANEL ORDER (read left-to-right, top-to-bottom):
{{PANEL_LIST}}

EXPRESSIONS (strict):
{{EXPRESSION_BLOCK}}

FORENSIC-GRADE FACIAL BIOMETRIC LOCK (NON-NEGOTIABLE — applies to every panel at every camera distance):
The face must read as the SAME individual to a trained forensic facial comparison expert examining any two panels side by side. The following biometric markers must be IDENTICAL across all panels:
- Eye region: interocular distance, eye shape and tilt (palpebral fissure inclination), upper/lower lid geometry, exact iris color, pupil baseline size, sclera tone, eyelash density and direction.
- Eyebrow region: arch shape, density, root direction, tail length, color, position relative to brow ridge.
- Nose: dorsal profile, bridge angle, length, tip rotation and projection, alar shape and width, nostril topology, columella curve. The 3D nose mesh is the same nose seen from frontal, three-quarter, profile and worm's-eye angles.
- Mouth: vermilion border geometry, philtrum length and ridge depth, cupid's bow shape, mouth corner position and angle, lip volume distribution upper-vs-lower, resting muscle tone.
- Jawline and chin: gonion angle (mandibular angle), mandible width and curve, chin projection and shape, submental contour.
- Cheek/zygomatic structure: cheekbone height and prominence, malar fat distribution, infraorbital hollow depth, buccal hollow.
- Forehead and brow ridge: hairline shape and density, frontal bone curvature, supraorbital ridge prominence, glabella geometry.
- Ears (whenever visible): helix curl, lobe shape and attachment, antihelix fold, scapha shape, conchal bowl depth, tragus shape, ear height relative to eyes and nose tip.
- Skin: pore density and pattern, mole/freckle/blemish positions if present in reference, undertone, sebaceous activity, micro-texture.
- Hair: style, length, color, density, root direction, parting, hairline edge.

This applies at EVERY camera distance:
- In wide / full-body panels the face must remain the SAME face — do not let it drift, simplify, average toward an "AI ideal", or stylize down because it occupies fewer pixels.
- In close-ups the same biometric structure must read at higher detail — same nose mesh, same lip vermilion, same eye geometry, same ears.
- Never produce a "different-but-similar" face. The reference image is the absolute biometric ground truth — do not interpolate, beautify, harmonize toward symmetry, or shift toward an idealized average.

LENS DISCIPLINE:
- Full-body and three-quarter panels: 50mm full-frame equivalent, neutral perspective.
- Head-and-shoulders close-ups: 85–105mm equivalent — eliminates wide-angle distortion of facial features.
- Macro detail panels (eyes, nose, mouth, ears, hands, texture): 100mm macro equivalent.
- ABSOLUTELY NO wide-angle (<35mm) lens distortion on the character. Faces must not be stretched, pinched, or fish-eyed at any angle.

BACKGROUND:
- Solid neutral {{BACKGROUND_DESCRIPTION}} across every panel — ABSOLUTELY uniform, no gradient, no environment, no props, no studio cyclorama curve.
- Soft natural contact shadow directly under the feet for grounding on full-body panels. No long cast shadows, no environmental shadows on the background itself.
- Floor and background blend seamlessly — no horizon line.

LIGHTING (identical across every panel):
{{LIGHTING_BLOCK}}

WARDROBE LOCK (ABSOLUTE):
- {{WARDROBE_LOCK_DESCRIPTION}}
- Nothing appears or disappears between panels. No coats added, no jewelry swapped.
- Wardrobe state must match the reference exactly — do not "clean up", repair, iron out wrinkles, or stylize what is shown.

BODY CONTINUITY (ABSOLUTE):
- Body proportions (height, shoulder width, torso length, limb length, body type) must be IDENTICAL across all panels.
- Posture is relaxed and natural, weight balanced unless a specific panel calls for an action pose (walk cycle).
- Do NOT alter age, weight, or build between panels.

DETAIL PRIORITY (this is an identity-locked reference, not a stylized illustration):
- Maximum visible skin texture — pores, fine vellus hair, subtle micro-shadows, natural color variation, sebaceous sheen if present in reference.
- Maximum visible fabric detail — weave, stitching, seam structure, fiber direction, wear patterns.
- Maximum visible hair detail — individual strands, root direction, natural sheen.
- Tack-sharp focus from edge to edge of each panel. Deep depth of field. No bokeh, no atmospheric haze, no soft focus.
- 8K source intent. Fine native grain only.

NEGATIVE DIRECTIVES (DO NOT):
- Do NOT apply AI-style smoothing, plastic skin, beauty filters, airbrush, or skin softening.
- Do NOT use shallow depth of field, lens blur, bokeh, or background blur.
- Do NOT add lens flares, light leaks, vintage filters, color grading effects, or artistic color shifts.
- Do NOT add motion blur, except minimal natural foot-motion blur on a WALK CYCLE panel if present.
- Do NOT generate a stylized illustration — this must read as a photographic reference.
- Do NOT produce a cyclorama / curved studio sweep — background is flat and seamless.
- Do NOT add camera UI, HUD elements, vignettes, or letterboxing.
- Do NOT average the character's face toward symmetry or toward an "idealized" face.
- Do NOT change facial geometry, ear shape, or nose shape between panels even in pursuit of a "better composition".

OUTPUT:
- Photorealistic, neutral studio color grading (sRGB).
- Aspect ratio of overall sheet: {{SHEET_ASPECT}}.
- Each individual panel framed cleanly within the grid cell with consistent margin around the character silhouette.
- Final result is a production-grade, identity-locked, text-free character reference sheet that would pass forensic facial comparison side-by-side across every panel — suitable as the master anchor for downstream Seedance 2.0 image-to-video and storyboard generation.
```

---

## Placeholder Fill Logic

When generating the final prompt, fill placeholders as follows:

### `{{REFERENCE}}`
- If user provided an image: `the attached photo`
- If reference will be attached later: `<REFERENCE_IMAGE>`
- If description-only: `the following character description: "{{DESCRIPTION}}"`

### `{{GRID}}` and `{{N}}`
- Forensic 6 → `3x2`, `6`
- Casting 9 → `3x3`, `9`
- Identity Lock 12 → `4x3`, `12`
- Director's 12 → `4x3`, `12`
- Extended 16 → `4x4`, `16`
- Pro 20 → `5x4`, `20`
- Custom → as specified

### `{{PANEL_LIST}}`
Inline the numbered list of panels from the chosen Angle Set with each panel's
detailed framing instruction (full body / head and shoulders / extreme
close-up, eye-level / low / high, neutral / smile / serious, etc.).

### `{{EXPRESSION_BLOCK}}`
- All neutral: `Every panel: NEUTRAL relaxed expression. Mouth closed but not tense. Eyes open, looking forward (or in the natural direction of the angle for profiles/backs). No smile, no frown.`
- Neutral + smile + serious (Casting 9, etc.): `Body and full-figure panels: NEUTRAL relaxed expression. Headshot panels designated for emotion: one with a genuine warm smile (eyes engaged), one with a serious / determined expression (slight brow tension, focused gaze).`
- Custom: as described.

### `{{BACKGROUND}}` and `{{BACKGROUND_DESCRIPTION}}`
- `#C8C8C8` and `light gray (#C8C8C8)`
- `#FFFFFF` and `pure white (#FFFFFF)`
- `#0A0A0A` and `pure black (#0A0A0A)`
- Custom: HEX and human description.

### `{{LIGHTING_BLOCK}}`
- Editorial neutral 3-point studio:
  ```
  - Neutral 3-point studio lighting calibrated for accurate skin tone reproduction.
  - Soft key light from front-left (~45°), gentle fill from front-right (~45°), subtle rim/hair light from back. Color temperature 5500K. CRI 95+.
  - Same lighting setup applied identically to every panel so the character reads as the SAME model under the SAME light from every angle.
  - No dramatic shadows, no colored gels, no high-contrast lighting.
  ```
- Natural diffused (north-window):
  ```
  - Natural soft daylight from a single large diffused source (north-facing window equivalent), color temperature ~5800K, soft white reflector fill on the opposite side.
  - Same lighting setup applied identically across every panel so the character reads as the same person under the same light from every angle.
  - Soft shadows only, no harsh contrast.
  ```
- Custom: insert as described.

### `{{WARDROBE_LOCK_DESCRIPTION}}`
- As in reference: `Same outfit in every panel. Same garment, same color, same fit, same wrinkles, same accessories.`
- Stripped to underlayer: `Plain neutral base layer (fitted T-shirt or tank, plain dark trousers, no accessories) so body geometry is fully readable.`
- Custom: as described.

### `{{SHEET_ASPECT}}`
- 3x2 → `landscape (3:2)`
- 3x3 → `square (1:1)`
- 4x3 → `landscape (4:3)`
- 4x4 → `square (1:1)`
- 5x4 → `landscape (5:4)`

---

## Important Rules

- **Constants are not variables.** NO-TEXT, FORENSIC FACIAL BIOMETRIC LOCK,
  LENS DISCIPLINE, and IDENTICAL LIGHTING are baked into every prompt and
  are NEVER asked or offered as toggles.
- **Defaults = MY recommendation under the user's use case.** Pull from the
  Use-Case to Pro Preset matrix every time. Do not pre-load whatever the user
  said in the previous session.
- **One question at a time** with my recommendation as option 1, briefly
  reasoned. The user picks "1" to accept or names a different option.
- **Render the final prompt in a fenced code block.**
- **Identity is sacred.** The template forbids inventing wardrobe, features,
  or accessories not present in the reference, and forbids averaging the
  face toward symmetry or "AI ideal".
- **Background is neutral by default** — this sheet is for identity, not for
  scene context.
- **Same lighting in every panel** — non-negotiable for downstream consistency.
- **Archive prefix**: `Character - ` so character boards group together in
  the vault.
- **Belongs to the Seedance Reference Board family** — related skills:
  `seedance-storyboard-to-video`, `seedance-shot-board`,
  `seedance-object-board`, `seedance-pose-board`, `seedance-creature-board`.

## Quick Start (Skip-the-Setup Mode)

Trigger: user says `/sb-character quick`, "quick character sheet",
"schnell durchziehen", or "быстрый ракурсный лист".

Behavior: ask only TWO questions:
1. Reference (image / placeholder / description)
2. Use case (one of the 5 in the matrix)

Then immediately apply the Pro Preset for that use case end-to-end and produce
the final prompt. No further Q&A.
