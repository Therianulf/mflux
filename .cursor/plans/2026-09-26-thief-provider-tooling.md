# Thief texture provider: tooling plan, LoRA training, character consistency

Status: planning. Nothing below is built yet. Written 2026-09-26 for handoff to the next agent session (Claude or Grok).

## Context

This fork is used as an installed CLI tool to generate late-1990s Looking Glass / *Thief: The Dark Project* style game art, not to develop mflux. Other repos (fantasy-sneaker first) commission textures and paintings from this repo's agent over agent-mail. The goal is to make this repo a reliable **provider** for those requests.

### Brief (the original handoff doc is gone; this is the only record)

- Output root: `~/textures/thief/` (paintings in `paintings/`). The 2026-08-12 tileable textures were delivered to `~/Documents/fantasy-sneaker/textures/` and `~/Github/fantasy-sneaker/assets/textures/<material>/`.
- Models: Z-Image Turbo (primary, 9 steps), FLUX.2 Klein (4 steps), Krea 2 (8 steps). Always `-q 8`. Step counts come from `MODEL_INFERENCE_STEPS` in `src/mflux/cli/defaults/defaults.py`, not docs.
- Generate at 1024 (paintings 768x1024), downscale to 256 / 128 later. Colour quantization, dithering and grain are done by the human in Texture Retrofier, not by mflux.
- Prompt template: concrete subject first, then "hand painted late 1990s game texture, Looking Glass Thief The Dark Project style, grainy, muted dark palette, gothic". Keep the abstract part short.
- Naming: `thief_<subject>_<nn>[_variant]_seed_<seed>.png`, always with `--metadata` so a `.metadata.json` sidecar records model, mflux version, seed, steps, size and prompt.
- Model weights live in the standard HF hub cache (`~/.cache/huggingface/hub/`). Z-Image Turbo q8 at 768x1024: ~45 s/image, ~34 GB peak on the M4 Max.

### Three traps that cost rework on 2026-08-12

1. **mflux never overwrites an existing `--output` path.** It writes `name_1.png`, `name_2.png`. Re-rolls silently land in suffixed files while downstream steps keep reading the stale original. Delete the target first or promote the newest suffix.
2. **Judge seams per axis.** Horizontal wrap vs neighbouring columns, vertical wrap vs neighbouring rows. Comparing a top/bottom gap against a column baseline falsely fails every plank- and course-structured texture.
3. **Stacked flatness language produces literally flat images.** "flat even diffuse lighting / uniform coverage / no focal point" together made Z-Image emit blank fields. Demand the structure be visible ("clearly visible mortar joints between every block").

Always eyeball the 3x3 contact sheet; no metric detects a countable repeating feature.

## Part 1: provider tooling (priority order)

1. **`.cursor/skills/thief-textures/` skill.** The prompt template, models and step counts, naming, output dir, the three traps, and the request/delivery contract for agent-mail letters. A request must give: material list, count, tile size, palette anchor, delivery path. A delivery lists: files, contact sheets, provenance record. Bounce incomplete requests instead of guessing.
2. **Move the texture tools into this repo** under `tools/textures/`. They currently sit in `~/Documents/fantasy-sneaker/tools/`, which is not a git repo: `tileify.py` (minimum-error boundary cut, Efros-Freeman; overlap 256 default, 384 for low-detail surfaces), `verify_tiles.py` (per-axis seam audit, ratio <= 1.5 passes), `contactsheet.py` (source beside a 3x3 tiling), `make_tileable.py` (superseded offset-and-inpaint approach). About 360 lines total, numpy + Pillow only, so `uv run` in this repo works.
3. **Provenance emitter + license table.** Read the `.metadata.json` sidecars for a delivery batch and write one provenance file: model repo id, mflux version, seeds, steps, output license by name, attribution text. The license table must be verified against each model card before it is written; do not guess. FLUX.2 Klein variants differ from each other. This closes the open fantasy-sneaker letter `20260819T234202Z-fantasy-sneaker-0064` (unanswered since 2026-08-19), which asks for exactly this.
4. **One-step command.** Generate N seeds, tileify each, per-axis verify, contact sheet, manifest, from a single entrypoint (`uv run python tools/textures/pipeline.py ...`). Five separate steps get skipped; one step gets run.
5. **Housekeeping.** Install `just` (`brew install just`; the rules assume it). Answer the open provenance letter before handoff.

## Part 2: train a Thief style LoRA on existing materials (raise this first next session)

`mflux-train --config train.json` finetunes a LoRA adapter. Training adapters exist for Z-Image, FLUX.2 Klein (base and edit), Krea 2, Ernie Image and FLUX.1. Example configs: `src/mflux/models/common/training/_example/`. Supports `--resume` and `--dry-run`.

Plan:
- Target Z-Image Turbo, the model already producing the paintings.
- Training set: the six nobleman paintings in `~/textures/thief/paintings/` (`thief_painting_noble_01_seed_{11,12,13}.png` framed, `thief_painting_noble_02_noframe_seed_{11,12,13}.png` frameless) plus the August textures listed above.
- Caption each image with one fixed trigger phrase. `--dry-run` first. Then train, and pass the result with `--lora` on every later generation so prompts shrink to the subject.
- Decision for the user: one style LoRA over everything, or separate character (nobleman) and surface (texture) LoRAs.

## Part 3: character consistency

mflux has no character concept and no built-in consistency check. Building blocks it ships:

- **Reference-image editing (strongest, no training):** `mflux-generate-kontext`, `mflux-generate-qwen-edit`, `mflux-generate-flux2-edit`, `mflux-generate-fibo-edit`. Feed the chosen portrait back in as the reference for every later painting of that character.
- **In-context LoRA diptychs:** `mflux-generate-in-context --lora-style identity|portrait|couple` renders a side-by-side pair sharing one identity.
- **Character LoRA:** same trainer as Part 2, on 5-15 portraits of one face.
- **Weak, free:** `--image PATH STRENGTH` img2img at low strength keeps composition; a very specific prompt already locked the same face across seeds 11-13.
- **Redux** conditions on style, not identity; use for palette and brushwork matching only.

Consistency-check idea: `facebook/dinov2-large` is already in the HF cache. Embed a face crop of the reference and of each candidate, cosine similarity as an identity score, threshold tuned by eye once. `mflux-concept` (concept-attention heatmaps) can confirm a prompt element is present. Would live in `tools/textures/`.

## Verification

- Skill: a fresh agent can produce one texture end to end from the skill alone.
- Tools: `uv run python tools/textures/verify_tiles.py` passes on the delivered tiles.
- Provenance: the emitted file answers all three items in the fantasy-sneaker letter.
- LoRA: a short prompt with the trigger phrase and `--lora` produces the Thief look without the long template.
