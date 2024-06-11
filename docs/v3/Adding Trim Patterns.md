---
layout: default
title: Adding Trim Patterns
nav_order: 6
---

# Adding a Trim Pattern
For a finished example, see [ChunkedPattern]

## Quick Start
v3 {: .label .label-green }

1. Create a trim pattern json in `data/<datapack-name>/trim_pattern/`.
2. Add the item associated with your trim pattern to the `trim_templates` item tag in `data/minecraft/tags/items/trim_templates.json`.
3. Create a `minecraft:smithing_trim` recipe in `data/<datapack-name>/recipes/`.
4. **Optionally** you may also create an advancement with the recipe so it will show your new recipe in a toast.
5. Add two pattern textures in `assets/<resourcepack-name>/textures/trims/models/armor/`.
6. Add your trim pattern's texture locations to the client-tag `assets/trimmed/trimmed/tags/trimmed/texture/trim_armor_patterns.json`.

![Example 3]

[ChunkedPattern]: https://github.com/dhyces/trimmed/tree/1.20/Packs/ChunkedPattern

[Example 3]: /trimmed-wiki/assets/images/example_3.png "Example of custom armor trim patterns being used in game without overriding each other"