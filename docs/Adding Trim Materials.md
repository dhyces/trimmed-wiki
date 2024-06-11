---
layout: default
title: Adding Trim Materials
nav_order: 5
---

# Adding a Trim Material
For a finished example, see [SeaweedMaterial].

## Quick Start

1. Create a trim material json in `data/<datapack-name>/trim_material/`. **It is highly recommended to include your pack id in the asset name to avoid clashing with other mods. The example trim material pack uses this convention with the asset name "seaweedmaterial_kelp"**. The "item_model_index" does not matter and is not use with Trimmed, feel free to just set it to 0.
2. Add the item associated with your trim material to the `trim_materials` item tag, located in `data/minecraft/tags/items/trim_materials.json`.
3. Create the texture palette for your material and place it in `assets/<resourcepack-name>/textures/trims/color_palettes/`.
4. Create the lang entry for your trim material. Make sure the key matches what you have in your trim material json.
5. Add your trim material to the client-map `assets/trimmed/trimmed/maps/trimmed/texture/material_suffixes.json`.

![Example 2]

[SeaweedMaterial]: https://github.com/dhyces/trimmed/tree/1.20/Packs/SeaweedMaterial

[Example 2]: /trimmed-wiki/assets/images/example_2.png "Examples of custom armor trim models being used in game without overriding the model files"