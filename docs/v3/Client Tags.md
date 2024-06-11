---
layout: default
title: Client Tags
nav_order: 1
---

# Client Tags
The client tag system is very similar to vanilla's datapack tags.

`/assets/mypack/tags/block/test_tag.json`
```json
{
    "values": [
        "minecraft:grass_block",
        "#minecraft:logs"
    ]
}
```
These are loaded first before other client resources, allowing them to be used in other assets. For Trimmed, this is so that tags can be used in the open permuted atlas source.

### Full example
```json
{
    "replace": true,
    "values": [
        "minecraft:oak_log",
        {
            "id": "minecraft:netherite_block",
            "required": false
        }
    ]
}
```

### Definition
- `replace`: If this element is `true` it will replace all elements from lower packs with the elements defined in this tag. This element is **optional**. Default value is `false`.
- `values`: An array of elements associated with this type of tag. If it's an unchecked tag. Each element can be one of the following:
    - Either an element or a tag, ex. `minecraft:diamond_sword` or `#minecraft:arrows`
    - An object containing the following:
        - `id`: Either an element or a tag
        - `required`: If this element is `true`, then resource loading may log a warning or fail. Default value is `false`.

### Explanation
The unchecked client tags were created so that Trimmed could keep track of all pattern textures needed for permutation in the final armor and item trim textures. Because it's tag-like, these can be added upon by other mods or resource packs. Client tags also support registry tags, for example `assets/modid/tags/item/fiery_swords.json`. An important concept is that client tags and datapack tags are **entirely** separate; they do not merge by default in-game. Client tags are loaded before all other reloadable client resources.

## Modder API
For modders, the api for retrieving client tag info is available from `TrimmedClientTagApi`.

### Example

```java
void printItemTextures() {
    TrimmedClientTagApi.getInstance().getTag(ClientTags.TRIM_PATTERN_TEXTURES).getSet()
            .forEach(LOGGER::info);
}
```