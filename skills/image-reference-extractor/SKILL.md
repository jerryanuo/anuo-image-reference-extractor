---
name: image-reference-extractor
description: Turn uploaded images into high-quality visual reference assets. Use when the user provides one or more images and wants Codex to automatically identify characters, creatures, props, vehicles, environments, style DNA, or other reusable visual elements, then generate 1:1 reference sheets or image prompts for AI image/video production. Trigger on requests like "处理这个", "把里面的人物拆出来", "做成参考图", "生成角色三视图", "提取这个图里的元素", "拆成资产参考图", or "make reference sheets from this image".
---

# Image Reference Extractor

Transform a user-provided image into a clean reference asset set for downstream image or video generation. The core job is not to crop the source image; it is to recognize useful elements, preserve their visual identity and style, and rebuild them as usable reference sheets.

## Default Behavior

When the user gives an image and says something brief like "处理这个", infer that they want the complete asset extraction workflow:

1. Identify every high-value visual element in the image.
2. Decide which elements deserve standalone reference images.
3. Preserve the source image's style, palette, lighting, texture, and mood.
4. Generate separate 1:1 reference sheets for the selected elements.
5. Save the generated images to a clear output folder when the environment supports file output.

Ask a question only when the image contains too many plausible targets, the requested output type is materially ambiguous, or generation would require unavailable tools.

## Element Detection

Inspect the image and separate elements into these buckets:

- **Characters:** humans, readable individuals, crowd figures with distinct silhouettes, outfits, faces, or roles.
- **Creatures:** animals, spirits, monsters, mascots, robots, living objects, fantasy entities.
- **Props and Objects:** products, weapons, vehicles, furniture, magical objects, devices, symbolic objects.
- **Environments:** rooms, streets, forests, stages, buildings, lighting setups, material worlds.
- **Style DNA:** medium, color system, lighting, grain, brushwork, edge treatment, atmosphere, composition logic.

Do not collapse multiple similar-looking figures too early. If the source image plausibly contains distinct people or creatures, either extract them separately or tell the user which ones were treated as variants.

When gender, age, identity, or object type is unclear, describe visible features instead of guessing. Preserve ambiguity when ambiguity is part of the source.

## Output Types

### Character Reference Sheet

Use this for humans or human-like figures. Generate one square 1:1 image divided into a clean 2x2 grid:

- Top-left: front-facing face close-up.
- Top-right: full-body front view.
- Bottom-left: full-body three-quarter view.
- Bottom-right: full-body side or back three-quarter view.

Each panel must show the same person, with consistent face, hair, body, outfit, proportions, and style. Front-view panels must face forward.

### Creature Reference Sheet

Use the same 2x2 structure as character sheets:

- Top-left: head or face close-up.
- Top-right: full-body front view.
- Bottom-left: full-body three-quarter view.
- Bottom-right: side or back three-quarter view, emphasizing tail, wings, shell, fur, silhouette, or special anatomy.

Keep creature logic simple and consistent. Do not turn gentle or symbolic creatures into generic monsters.

### Prop/Object Reference Sheet

Generate one square 1:1 image divided into a 2x2 grid:

- Top-left: clean hero view.
- Top-right: alternate angle.
- Bottom-left: scale/context view or hand-held/use view.
- Bottom-right: material/detail close-up.

Preserve silhouette, materials, color accents, wear, texture, and any functional details. Avoid adding logos or text unless present and requested.

### Environment Reference Sheet

Generate one square 1:1 image divided into a 2x2 grid:

- Top-left: wide establishing view.
- Top-right: mid-distance usable scene angle.
- Bottom-left: lighting/color/material study.
- Bottom-right: detail patch: ground, wall, foliage, furniture, texture, signage, or atmosphere.

Keep it useful for reconstructing the space, not just pretty.

### Style DNA Card

Use when the user wants the visual language preserved across future images. A style card can be a written summary or a generated 1:1 board containing non-labeled visual studies. Include:

- Medium and rendering method.
- Palette and contrast.
- Lighting and exposure.
- Texture, grain, brushwork, edge quality.
- Mood and pacing.
- Negative constraints.

## Prompt Construction

For each generated reference sheet, build a prompt with this structure:

```text
Use case: stylized-concept
Asset type: square <character|creature|object|environment> reference sheet, 1:1.
Primary request: Create an individual reference image for <target element> from the provided source image. Preserve these identity features: <visible features>.

Layout: one square 1:1 image divided into four equal square panels in a clean 2x2 grid. <panel plan for this element type>.

Style to preserve from the input reference: <medium>, <palette>, <lighting>, <texture>, <edge treatment>, <mood>, <post-processing>.

Important constraints: keep one consistent subject across all four panels; no extra unrelated elements; no text, labels, watermark, signature, or UI; do not change the core design; if source details are hidden, reconstruct conservatively from visible cues.
```

Keep prompts specific. Name the visual traits that make the asset identifiable: hair shape, face geometry, clothing, color blocks, posture, body ratio, glowing edges, material scratches, tail shape, prop silhouette, or environment lighting.

## Style Preservation

Always extract and reuse transferable style cues from the source:

- **Medium:** watercolor, gouache, oil, ink, pixel, 3D, cinematic photo, poster, sketch, collage, etc.
- **Palette:** dominant colors, accent colors, saturation, black level, highlight color.
- **Lighting:** direction, softness, rim light, glow, haze, contrast, exposure.
- **Texture:** grain, paper, brush strokes, noise, blur, halftone, scan artifacts.
- **Shape language:** angular, rounded, blocky, fragile, flattened, painterly, silhouetted.
- **Mood:** childlike, eerie, solemn, comic, intimate, epic, clinical, commercial.

Preserve style without copying protected brands, text, logos, or identifiable IP unless the user explicitly has rights and asks for it.

## Batch Strategy

For multiple distinct assets, make one generation call per asset or prompt. Do not ask one image call to produce several unrelated reference sheets unless the user requests a combined board.

Recommended naming:

```text
<source-slug>-character-01-<visible-trait>.png
<source-slug>-creature-01-<visible-trait>.png
<source-slug>-prop-01-<visible-trait>.png
<source-slug>-environment-01-<visible-trait>.png
```

Save final assets in a dated output folder when possible.

## Quality Check

Before finishing, check:

- Every output is 1:1.
- The subject is consistent across all four panels.
- The source style is recognizable.
- The reference sheet is useful for future generation, not merely decorative.
- Important source elements were not missed.
- Ambiguous elements were handled explicitly, not silently ignored.
- File paths are clear and reusable.

If a result is close but weak, iterate once with a targeted correction such as "more faithful hair shape", "less realistic", "restore the glowing rim", "separate the creature from the background", or "make the front view face forward".

## Response Format

When working directly for the user:

1. State what elements were identified.
2. State what reference sheets were generated.
3. Show or link the final images.
4. Mention any conservative reconstruction caused by occlusion or ambiguity.
5. Provide saved paths when files were written.

Keep analysis short. The user mainly needs usable reference assets.
