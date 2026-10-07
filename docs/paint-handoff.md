# Paint handoff

A fresh session can run this from the commission letter. mflux paints plaque and label decals. It does not draw gauge or dial faces, and it does not build the mesh.

## Checklist

1. fantasy-sneaker commissions the prop: sizes, slots, tier, and quad topology. That letter is the start. Commission sizes are targets.
2. meshytools draws one straight-on design of record. The prompt asks for blank faces and a blank plaque. Re-roll until the picture is clean. Never patch a generated picture. Report the slot centres on that picture, and hand the picture and the slot file to fantasy-sneaker, command-control, and mflux.
3. fantasy-sneaker draws the gauge and dial faces with `tools/dialface.py` and composites them onto the picture, changing no other pixel. Name the plaque wording before the mesh exists. Wording is invented, never a real firm's name. Hand the composite to command-control. The dial law is: numerals evenly on the arc at the major ticks, minor ticks between them, the label centred above a centred hub, the bezel concentric with the face, drawn and not diffused.
4. command-control shows the owner that composite. No create runs before he accepts. The acceptance goes to meshytools and mflux.
5. meshytools builds three views from the clean roll: front straight on, a shallow three-quarter, and a back. Drop any view that distorts the slope. One quad create. A bad create is reported with the retry cost and is not re-run without a letter. Hand over the mesh and the `display_<slot>` anchors with the landed sizes.
6. mflux paints the plaque, and a label when the commission has one, at the landed anchor. The canvas long side is 512, or 1024 when that anchor's long side is over half a metre: 512 by 256, or 1024 by 512, for a landscape plaque. The plate fills the width, and transparent margins make up the canvas aspect. Straight alpha. Write `output/decals/<prop>/<slot>.png` and mail the PNG to fantasy-sneaker, named for `assets/decals/<prop>/<slot>.png`. `/tmp` is scratch and is never the only copy.
7. fantasy-sneaker places the decals, verifies, files them with a SOURCES row, and exports the dressed viewing pair beside the delivered files. meshytools' verify treats that pair as a sidecar. fantasy-sneaker names the folder to command-control.

The dial-console is the reference run. Its plaque reads ASH & VANE / DYNAMO No. 2, canvas 512 by 256, plate 512 by 241 and centred, from `display_plaque` 0.2008 by 0.0946 m. It was mailed in 0080 and filed by fantasy-sneaker at 9ebd636. The instrument-panel plaque is 512 by 256, the plate about 171 px tall and centred, reading ASH & VANE / BOILER No. 4.

The control-plate label is commissioned as a 0.28 by 0.07 strip. Paint it when that design is accepted and the anchor is measured. The landed size governs.

## Texture families

A texture family is one folder: the base, its variations, the contact sheet, and the manifest. meshytools places it at `~/meshyworking/deliverables/fantasy-sneaker/textures/<family>/` and mails it. mflux does not paint the base. A Material Maker base must read as its family and carry that family's structure. meshytools mails the base alone, and command-control's yes comes before its variations. mflux paints v07 to v10 only on an accepted base, as unique-feature variations, writes them under `output/tiles/`, and mails them for meshytools to place. A rejected base is not a source. The four bases lost from `/tmp` are superseded. Do not repaint them. v11 and later wait until command-control says makers may make things again, and a new id never overwrites a filed name.

## Where this stands

2026-10-07. Pull request https://github.com/Therianulf/mflux/pull/1. `feature` includes the merge of origin/main at 013a586. Further commits stay on `feature`. This section is the restart note.

Done: the instrument-panel plaque, and the dial-console plaque (ASH & VANE / DYNAMO No. 2, canvas 512 by 256, plate 512 by 241, mailed in 0080, filed by fantasy-sneaker at 9ebd636).

tileset/1 is accepted. fantasy-sneaker holds it at `docs/TILESETS.md` on feature-proc-walls. A new painted set ships with `<set>.tileset.json`. The eleven already-painted sets do not get one from mflux. fantasy-sneaker's edge table calls those eleven one-class. Files are `<set>_<id>.png`. A set file mflux writes names the maker as the base's tool first, then mflux.

A wall can fill from copper-pipe-cladding, the painted iron-plate-riveted set, and the five combined sets (Material Maker base plus mflux v01 to v10): fieldstone, oak-plank, plaster-lime, slate-roof, thatch. The showcase rooms use those five plus iron-plate-riveted-redo.

The iron redo base, sha256 `ed0cd5687af76dcadae87c6f02097d879af0c3403cb96234ea6ab2a51eea4c6e`, is placed at `textures/iron-plate-riveted/iron-plate-riveted-redo_base.png`. mflux features v07 scorch, v08 rivet patch, v09 crack, and v10 rust drip are placed beside it (mailed in 0096). The deliverables set file maker line is `material-maker base, mflux features`. v01 to v06 wait on Ben's Material Maker go. The rust-noise `iron-plate-riveted-mm` set is not a source. Files under `output/tiles/iron-plate-riveted/` from that rejected set are not the family.

Fantasy-sneaker 0254 kept these pixels and adopted them as features: every brass-panelling variation, boiler-plate v03, v05, v06 and v08, and gasworks-brick v01 and v02. gasworks-brick is base plus v01 to v04 only. Do not add v05 to v10. circuitry-board stays as filed. brass-panelling-mm is not placed: it is grey diamond tread, not the painted brass panels. brick-red is meshytools' and fills from its base alone.

No new paint until command-control says makers may make things again. A later plain replacement ships under a new id, v11 or after, and never over a filed name. Three unsent brass frames under `output/tiles/section3-replacements/` are not a delivery. `output/` is local scratch and is not in the clone. The control-plate label still waits on an accepted design and a measured anchor.
