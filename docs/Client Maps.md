# Client Maps
Maps are a new kind of element included with Trimmed. These are simply key-value pairs, matching a resource location to another object. Maps aquire the same override behavior as tags, where they can be fully replaced or simply appended to.

[/assets/trimmed/maps/unchecked/vanilla_trim_material_permutations.json](https://github.com/dhyces/trimmed/blob/1.20/Common/src/main/resources/assets/trimmed/maps/unchecked/vanilla_trim_material_permutations.json)
```json
{
  "__comment": "Map of palette texture location to suffix for generated textures",
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
    "minecraft:trims/color_palettes/amethyst": "amethyst",
    "minecraft:trims/color_palettes/iron_darker": "iron_darker",
    "minecraft:trims/color_palettes/gold_darker": "gold_darker",
    "minecraft:trims/color_palettes/diamond_darker": "diamond_darker",
    "minecraft:trims/color_palettes/netherite_darker": "netherite_darker"
  }
}
```

### Full example
```json
{
    "replace": true,
    "pairs": {
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
- `pairs`: A mapping of IDs to some text. The IDs may be verified if this is a registry map. An entry value may also be an object containing the following fields:
    - `value`: The value that the key maps to
    - `required`: If this is `true`, then resource loading may log a warning or fail. Default value is `false`.
- `append`: An array of map locations. This allows maps to combine other maps within them so they can be separated into smaller parts that may be used by themselves elsewhere. This element is **optional**. Default value is an empty array.

## Modder API
If you are utilizing an unchecked client map and using the unstable api methods, ensure that you handle cases in which an entry may be optional. The experimental `TrimmedClientMapApi#mapStream` method is helpful for this, otherwise `LimitedMap` which can be obtained through `TrimmedClientMapApi#map` has an `isRequired` method. I will restate this again, this is experimental API and is subject to large changes in major version updates.

```java
void loadJsonTextureData() {
    TrimmedClientMapApi.INSTANCE.mapStream(TEXTURE_MAPPING_KEY).forEach(entry -> {
            JsonObject obj = loadResource(entry.key());
            if (obj == null && entry.isRequired()) {
                LOGGER.error("Element " + entry.key() + " does not exist and is required!");
                return;
            }
            doSomething(obj, entry.value());
        });
}
```

Some ideas where this could be used is for specifying specific elements on the client to receive special rendering effects or implementing a trim system for other game objects. Map values can also be queried from `TrimmedClientMapApi#getUncheckedClientValue`. As with the prior aspects, this method name is subject to change in major versions.

```java
TrimmedClientMapApi.INSTANCE.getRegistryClientValue(ITEM_ICON_MAP, Items.DIAMOND_SWORD);
```