# Markers

Markers are used to modify static item properties like attribute modifiers and special behaviors.

## Basic Commands

```
/rpgitem marker [operation] [item] [params]
```

Available operations:

- `add` Add a marker
- `remove` Remove a specific marker
- `list` List all markers
- `prop` View or modify marker properties

Example:
```
/rpgitem marker prop myhelmet 0-rpgitems:attributemodifier operation:ADDITION slot:HEAD
```

## Common Marker Properties

All markers share these properties from BaseMarker:

- `markerId` Unique identifier for this marker (default: "")
- `displayName` Custom display name
- `tags` Set of tags for grouping markers

## AttributeModifier

Add Minecraft attribute modifiers to items.

- `amount` Modification amount (default: 2)
- `attribute` Target attribute (default: MAX_HEALTH)
- `name` Friendly modifier name (default: "maxHealth")
- `operation` Mathematical operation (required, default: ADD_NUMBER)
- `slot` Equipment slot(s) where modifier applies
- `namespacedKey` Unique identifier for the modifier

### Available Attributes

See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/Attribute.html

Common attributes:
- `MAX_HEALTH` - Maximum health points
- `ATTACK_DAMAGE` - Melee attack damage
- `ATTACK_SPEED` - Attack cooldown speed
- `ARMOR` - Armor points
- `ARMOR_TOUGHNESS` - Armor toughness
- `MOVEMENT_SPEED` - Movement speed
- `KNOCKBACK_RESISTANCE` - Knockback resistance
- `LUCK` - Luck for loot tables

### Available Operations

See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/AttributeModifier.Operation.html

- `ADD_NUMBER` - Adds flat amount
- `ADD_SCALAR` - Adds percentage of base value
- `MULTIPLY_SCALAR_1` - Multiplies by (1 + amount)

### Available Slots

Uses `EquipmentSlotGroup` for slot targeting:

- `HAND` - Main hand
- `OFF_HAND` - Off hand
- `HEAD` - Helmet slot
- `CHEST` - Chestplate slot
- `LEGS` - Leggings slot
- `FEET` - Boots slot
- `MAINHAND` - Explicit main hand
- `OFFHAND` - Explicit off hand
- `ARMOR` - Any armor slot
- `ANY` - Any equipment slot

Example:
```
/rpgitem marker add myhelmet rpgitems:attributemodifier amount:4 attribute:MAX_HEALTH operation:ADD_NUMBER slot:HEAD
```

## LoreFilter

Filter and preserve specific lore lines matching a pattern.

- `regex` Regular expression pattern to match
- `desc` Display description (required)
- `find` Use regex `.find()` instead of `.match()` (default: false)

Example:
```
/rpgitem marker add myitem rpgitems:lorefilter regex:"^\+[0-9]+ Damage$" desc:"Damage bonus"
```

## Ranged

Mark an item as capable of dealing ranged damage.

- `r` Maximum effective radius in blocks (default: MAX_VALUE)
- `rm` Minimum effective radius in blocks (default: 0)

This marker affects damage calculation for projectile-based powers.

Example:
```
/rpgitem marker add mybow rpgitems:ranged rm:5 r:50
```

## RangedOnly

Mark an item as ONLY dealing ranged damage (no melee).

- `r` Maximum effective radius in blocks (default: MAX_VALUE)
- `rm` Minimum effective radius in blocks (default: 0)

Items with this marker will not deal melee damage on direct hits.

Example:
```
/rpgitem marker add mystaff rpgitems:rangedonly rm:0 r:100
```

## Selector

Advanced entity selection/filtering for AOE powers.

- `id` Unique identifier (required)
- `display` Custom display message
- `type` Entity type filter (comma-separated)
- `count` Maximum number of targets
- `x`, `y`, `z` Reference position (supports tilde notation like `~5`)
- `r` Maximum radius from reference
- `rm` Minimum radius from reference
- `dx`, `dy`, `dz` Maximum distance on each axis
- `score` Scoreboard score filter: `score_name:min,max`
- `tag` Scoreboard tag filter: `MUST_HAVE,!MUST_NOT_HAVE`
- `team` Team filter: `MUST_ON,!MUST_NOT_ON`
- `gameMode` GameMode filter: `MAY_ON,!MUST_NOT_ON`

Example:
```
/rpgitem marker add myaoe rpgitems:selector id:nearby-zombies type:ZOMBIE r:10 count:5
```

Then use in powers:
```
/rpgitem power add myaoe rpgitems:aoedamage selectors:nearby-zombies damage:10
```

## Unbreakable

Mark item as unbreakable - prevents durability loss.

No additional properties beyond common marker properties.

Example:
```
/rpgitem marker add myitem rpgitems:unbreakable
```

## Unique

Mark item as unique - cannot be stacked and generates unique ID per player.

- `enabled` Whether the unique marker is active (default: true)

**Note**: Items with this marker:
- Will be assigned a unique random string when given to players
- Cannot stack even with identical items
- Cannot be used in NPC trades

Example:
```
/rpgitem marker add myitem rpgitems:unique enabled:true
```

## Marker Summary Table

| Marker | Purpose | Key Properties |
|--------|---------|----------------|
| AttributeModifier | Add MC attribute modifiers | amount, attribute, operation, slot |
| LoreFilter | Filter lore lines by regex | regex, desc, find |
| Ranged | Mark as ranged weapon | r, rm |
| RangedOnly | Mark as ranged-only (no melee) | r, rm |
| Selector | Define entity selection rules | id, type, r, rm, count, etc. |
| Unbreakable | Prevent durability loss | (none) |
| Unique | Prevent stacking, unique per player | enabled |
