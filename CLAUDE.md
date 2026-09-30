# mflux

Hard rules:

- A finished render is written under `output/tiles/` in the checkout. Mail each finished base as soon as it lands. `/tmp` is scratch and is never the only copy.
- A texture family is one folder: the base, its variations, the contact sheet, and the manifest. In the meshy deliverables tree that folder is `~/meshyworking/deliverables/<project>/<category>/<family>/`. meshytools owns that move.

Tooling:

- Project rules: `.cursor/rules/RULE.md`
- Agent notes: `Agents.md`
- Recipes: `justfile`, entry point `just`
