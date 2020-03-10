# Markers

Markers are used to change something static, like item attribute modifiers.

Command to modify marker on an item:

```
/rpgitem marker [operation] [item] [params]
```

Available operations:

- `add` Add a marker
- `remove` Remove a specific marker
- `list` List all markers
- `prop` List details about a specific marker, or modify it with additional parameters

E.g.,

```
/rpgitem marker prop myhelmet 0-rpgitems:attributemodifier operation:ADDITION slot:HEAD
```

## Attribute Modifier

Attributes:

- `amount` Amount to calculate
- `attribute` See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/Attribute.html
- `operation` See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/AttributeModifier.Operation.html
- `slot` See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/inventory/EquipmentSlot.html

## Lore Filter

TBD

## Ranged

Indicates the item deals ranged damage.

## Ranged Only

Indicates the item deals ranged damage and will not deal melee damage.

## Selector

TBD

## Unbreakable

Indicates the item will not break or consumed.

## Unique

Indicates the item is unique and cannot be stacked.

Note: Items with this marker will be updated with a unique random string when distributed to other players, so that it won't stack even with same type of item.

As a result, item with unique marker cannot be added to NPC trades.
