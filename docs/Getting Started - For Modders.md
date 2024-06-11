## Custom armor materials
Mojang uses an internal `Map<ArmorMaterials, String>` to map armor materials to an alternate asset name so that, for example, a diamond trim used on diamond armor can use a different, darker asset. With Trimmed, you use a client-map located in `maps/unchecked/armor_material_overrides/` with the file name of the trim material. So if you add a trim material `mymod:ender`, you would create the map `mymod:maps/unchecked/armor_material_overrides/ender.json`. In this map you put key-value entries of the 'armor material name' -> 'overridden asset name'. Here's an [example](https://github.com/dhyces/trimmed/blob/1.20/Forge/TestMod/src/generated/resources/assets/trimmed_testmod/maps/unchecked/armor_material_overrides/adamantium.json) from the test mod.

## `ItemOverrideDataProvider`
There is a data provider offered in the API for datagenning any item model overrides, along with a few helper methods for the included override providers. Thank you for using this mod!

## Quick Start for Creating an `ItemOverrideProvider`
To create your own `ItemOverrideProvider` you need to write a provider and its codec and register a provider type. Pretty simple!

1. Create a class implementing `ItemOverrideProvider`
2. Write a codec for your provider
3. Register an `ItemOverrideProviderType` using `TrimmedAPI` somewhere. **Ensure it gets class loaded**
4. Return your type in `ItemOverrideProvider#getType`
5. Write your implementation, returning model ids in `ItemOverrideProvider#getModelsToBake` if needed

Here's a longer form tutorial, using the provider from the test mod:

[Finished provider](https://github.com/dhyces/trimmed/blob/1.19/TestMod/src/main/java/dhyces/testmod/client/providers/BlockStateItemOverrideProvider.java)

### Create an `ItemOverrideProvider`
First, you want to create a new class that implements `ItemOverrideProvider` and then write the accompanying codec for any data you want to handle. For this case, we don't need any data, we're going to dynamically handle block state representation via an NBT tag on the item, so we'll just use a unit codec here. If you do need to utilize some kind of parsed data and are unfamiliar with codecs, see [the nbt provider](https://github.com/dhyces/trimmed/blob/1.19/src/main/java/dhyces/trimmed/api/client/override/provider/providers/NbtItemOverrideProvider.java) or [the armor trim provider](https://github.com/dhyces/trimmed/blob/1.19/src/main/java/dhyces/trimmed/api/client/override/provider/providers/TrimItemOverrideProvider.java) for some examples of how codecs are written, or visit https://forge.gemwire.uk/wiki/Codecs for a nice writeup.

`/dhyces/testmod/client/providers/BlockStateItemOverrideProvider.java`
```java
public class BlockStateItemOverrideProvider implements ItemOverrideProvider {
    public static final Supplier<BlockStateItemOverrideProvider> LAZY_INSTANCE = Suppliers.memoize(BlockStateItemOverrideProvider::new);
    public static final Codec<BlockStateItemOverrideProvider> CODEC = Codec.unit(LAZY_INSTANCE);

    public BlockStateItemOverrideProvider() {}
. . .
```
Along with the regular codecs provided by DFU and Minecraft itself, there is also a `ModelIdentifier` codec provided by Trimmed in the `CodecUtil` class. This allows users to specify specific custom inventory models potentially provided from other mods or specific block state models. A json example would be this test from the test mod:

`TestMod/src/main/generated/assets/minecraft/models/item/overrides/grass_block.json`
```json
{
  "values": [
    {
      "type": "trimmed:nbt",
      "model": "minecraft:grass_block#snowy=true",
      "nbt": {
        "BlockStateTag": {
          "snowy": "true"
        }
      }
    }
  ]
}
```
This is using the nbt provider type and the model is the grass block with the snowy block state property set to true.

### Create an `ItemOverrideProviderType`
Now that we have a codec for the basic data we would handle (in the case of the example, none), we need to create a type to pass back in the `ItemOverrideProvider#getType` method. I'm going to create a new class called `MyProviderTypes` to handle this type and any others I might make. Here, we specify a static final field to store this type and use the Trimmed API to register it to the override registry. A type is simply a storage mechanism for the codec of our override type, which allows us to refer to it when parsing overrides. Here, I also have an empty init method, which I call in the main test mod class to class load the type. **Ensure that the type is classloaded, otherwise an error which prevents resources from loading will occur when attempting to parse your type.**

`/dhyces/testmod/client/providers/MyProviderTypes.java`
```java
public final class MyProviderTypes {
    public static final ItemOverrideProviderType<BlockStateItemOverrideProvider> BLOCK_STATE = TrimmedApi.INSTANCE.registerItemOverrideType(new Identifier(TrimmedTest.MODID, "block_state"), () -> BlockStateItemOverrideProvider.CODEC);

    public static void init() {}
}
```

Now that the type is made, we can return it in our item override class' `getType` method.

`/dhyces/testmod/client/providers/BlockStateItemOverrideProvider.java`
```java
. . .
    public ItemOverrideProviderType<?> getType() {
        return MyProviderTypes.BLOCK_STATE;
    }
. . .
```

Now we can focus on the fun part! Doing our checks in `ItemOverrideProvider#getModel` and returning a new model id if they pass. If you add new models in your overrides or need to ensure that parsed model ids are loaded, override `ItemOverrideProvider#getModelsToBake` and return a stream of the model ids.

Now that the code is all done, you just need a json to add an override for an item. This will go in the `assets/<namespace>/models/item/overrides` directory of whatever item you want to override. The name of the file will correspond to the item ingame. Here, we will create an override for `minecraft:bamboo_stairs`, so we can show the different block states if the item has the block states tag.

`/TestMod/src/main/generated/assets/minecraft/models/item/overrides/bamboo_stairs.json`
```json
{
  "values": [
    {
      "type": "testmod:block_state"
    }
  ]
}
```
This is a very simple example, since it's dynamic and doesn't need any other data. For other examples, see [the generated overrides for armor trims](https://github.com/dhyces/trimmed/tree/1.19/TestMod/src/main/generated/assets/minecraft/models/item/overrides).

Now we have our overrides showing in game with only two classes!

![Example 1](https://github.com/dhyces/trimmed/wiki/resources/example_1.png "Examples of models being replaced in game without overriding the model files")