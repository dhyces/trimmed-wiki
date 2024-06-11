---
layout: default
title: Client Maps
nav_order: 2
---

# Client Maps
Maps are a new kind of element included with Trimmed. These are simply key-value pairs, matching a resource location to another object. Maps aquire the same override behavior as tags, where they can be fully replaced or simply appended to.

[/assets/trimmed/trimmed/maps/trimmed/texture/material_suffixes.json]
```json
{
  {
  "__comment": "This is a map which represents the actual texture locations and their corresponding generated texture suffix. It is highly recommended that mods prefix their suffixes with the modid, ie \"modid_adamantite\"",
  "values": {
    "minecraft:trims/color_palettes/quartz": "quartz",
    "minecraft:trims/color_palettes/iron": "iron",
    "minecraft:trims/color_palettes/gold": "gold",
    "minecraft:trims/color_palettes/diamond": "diamond",
    "minecraft:trims/color_palettes/netherite": "netherite",
    "minecraft:trims/color_palettes/redstone": "redstone",
    "minecraft:trims/color_palettes/copper": "copper",
    "minecraft:trims/color_palettes/emerald": "emerald",
    "minecraft:trims/color_palettes/lapis": "lapis",
    "minecraft:trims/color_palettes/amethyst": "amethyst"
  },
  "__comment_2": "Use the darker map for any textures that are specialized for only certain cases",
  "append": [
    "trimmed:darker_material_suffixes"
  ]
}
}
```

### Full example
```json
{
    "replace": true,
    "values": {
        "minecraft:oak_log": "minecraft:netherite_block",
        "minecraft:netherite_block": {
            "value": "anothermod:a_block",
            "required": false
        }
    },
    "append": [
        "modid:separate_block_map"
    ]
}
```

### Definition
- `replace`: If this element is `true` it will replace all entries from lower packs with the elements defined in this map. This element is **optional**. Default value is `false`.
- `values`: A mapping of IDs to some text. The IDs may be verified if this is a registry map. An entry value may also be an object containing the following fields:
    - `value`: The value that the key maps to
    - `required`: If this is `true`, then resource loading may log a warning or fail. Default value is `false`.
- `append`: An array of map locations. This allows maps to combine other maps within them so they can be separated into smaller parts that may be used by themselves elsewhere. This element is **optional**. Default value is an empty array.

## Modder API
The API for getting client maps is obtained through `TrimmedClientMapApi`. There are two map types, `SimpleMapType` and `AdvancedMapType`, the latter supporting a specific type of map for optimization or for desired behavior. 

```java
void loadJsonTextureData() {
    MapHolder<ResourceLocation, ResourceLocation> mapHolder = TrimmedClientMapApi.getInstance().getSimpleMap(ClientMapKeys.TRIM_OVERLAYS);
    mapHolder.getMap().forEach((key, value) -> {
        JsonObject obj = loadResource(key);
        if (obj == null && mapHolder.isRequired(key)) {
            LOGGER.error("Element " + key + " does not exist and is required!");
            return;
        }
        doSomething(obj, value);
    });
}
```

Some ideas where this could be used is for specifying specific elements on the client to receive special rendering effects or implementing a trim system for other game objects.

[/assets/trimmed/trimmed/maps/trimmed/texture/material_suffixes.json]: https://github.com/dhyces/trimmed/blob/1.20/Common/src/main/resources/assets/trimmed/maps/unchecked/vanilla_trim_material_permutations.json