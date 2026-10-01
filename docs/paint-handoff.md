# Paint handoff

mflux paints plaque and label decals. fantasy-sneaker draws gauge and dial faces with `tools/dialface.py` and composites them into the design. meshytools builds the mesh. command-control shows the owner the picture before any credit moves.

## Sequence

1. fantasy-sneaker commissions the prop: sizes, slots, tier, and quad topology for hard-surface props.
2. meshytools draws one design of record with the display faces blank and reports the slot centres. Wording is invented. It is never a real firm's name.
3. fantasy-sneaker draws the gauge and dial faces by script and composites them into that design. The dial law is: numerals evenly on the arc at the major ticks, minor ticks between them, the label centred above a centred hub, the bezel concentric with the face, drawn and not diffused.
4. The owner accepts or corrects that picture. No create runs before he accepts.
5. meshytools regenerates the blank views (`no text, no lettering, blank plaques, blank dial faces`), checks them against the slots, and runs one quad create. A bad create is reported with the retry cost. It is not re-run without a letter.
6. mflux paints the plaque and label decals from the accepted design, at the measured anchor size.
7. fantasy-sneaker places those decals on the anchors, verifies, files them, and names the deliverables folder to command-control.

## What mflux paints

Wait until the design is the accepted record and meshytools has reported the measured anchor for that slot.

- A plaque uses the contract's fixed canvas, 512 by 256. The plate fills the slot's width, and transparent margins make up that aspect. The instrument-panel plaque is that canvas, the plate about 171 px tall and centred, reading ASH & VANE / BOILER No. 4.
- The control-plate label is the 0.28 by 0.07 strip centred on a 512 by 256 canvas, about 128 px tall, with transparent margin above and below.
- Gauge and dial faces are not painted here.

Write the file at `output/decals/<prop>/<slot>.png`. Mail it to fantasy-sneaker when it lands, named for `assets/decals/<prop>/<slot>.png`. `/tmp` is scratch and is never the only copy.

## Texture families

A texture family is one folder: the base, its variations, the contact sheet, and the manifest. meshytools places it at `~/meshyworking/deliverables/fantasy-sneaker/textures/<family>/` and mails it. mflux does not paint the base. When a steampunk Material Maker base lands, mflux paints v07 to v10 as unique-feature variations on that base, writes them under `output/tiles/`, and mails them for meshytools to place. The four bases lost from `/tmp` are superseded. Do not repaint them. v11 and later wait on the owner's word.
