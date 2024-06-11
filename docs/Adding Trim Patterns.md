# Adding a Trim Pattern
For a finished example, see https://github.com/dhyces/trimmed/tree/1.20/Packs/ChunkedPattern

## Quick Start

1. Create a trim pattern json in `data/<datapack-name>/trim_pattern/` [Ref](https://github.com/dhyces/trimmed/blob/dev/1.20.5/example_packs/ChunkedPattern/data/chunkedpattern/trim_pattern/chunked.json)
2. Add the item associated with your trim pattern to the `trim_templates` item tag in `data/minecraft/tags/items/trim_templates.json` [Ref](https://github.com/dhyces/trimmed/blob/dev/1.20.5/example_packs/ChunkedPattern/data/minecraft/tags/items/trim_templates.json)
3. Create a `minecraft:smithing_trim` recipe in `data/<datapack-name>/recipes/` [Ref](https://github.com/dhyces/trimmed/blob/dev/1.20.5/example_packs/ChunkedPattern/data/chunkedpattern/recipes/chunked_grass_block_smithing_trim.json)
4. **Optionally** you may also create an advancement with the recipe so it will show your new recipe in a toast [Ref](https://github.com/dhyces/trimmed/blob/dev/1.20.5/example_packs/ChunkedPattern/data/chunkedpattern/advancements/recipes/combat/chunked_grass_block_smithing_trim.json)
5. Add two pattern textures in `assets/<resourcepack-name>/textures/trims/models/armor/` [Ref](https://github.com/dhyces/trimmed/tree/dev/1.20.5/example_packs/ChunkedPattern/assets/chunkedpattern/textures/trims/models/armor)
6. Add your trim pattern's texture locations to the client-tag `assets/trimmed/tags/unchecked/trim_pattern_textures.json` [Ref](https://github.com/dhyces/trimmed/blob/dev/1.20.5/example_packs/ChunkedPattern/assets/trimmed/tags/unchecked/trim_pattern_textures.json)

![Example 3](https://github.com/dhyces/trimmed/wiki/resources/example_3.png "Example of custom armor trim patterns being used in game without overriding each other")