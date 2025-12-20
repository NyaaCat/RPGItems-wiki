# Modifiers

Modifiers are dynamic property modifications that can be applied to specific item instances or players. Unlike powers which are defined on the item template, modifiers can change power properties on individual item copies or per-player basis.

## How Modifiers Work

Modifiers target specific power properties and modify their values at runtime. They can be applied to:
- **Hand**: The item currently held in main hand
- **Player**: Stored on the player's persistent data (affects all their items)

Modifiers are processed by priority order, with lower numbers executing first.

## Commands

| Command | Description |
|---------|-------------|
| `/rpgitem modifier add <hand/player> <modifier> [properties]` | Add a modifier |
| `/rpgitem modifier prop <hand/player> <id> [properties]` | View or modify properties |
| `/rpgitem modifier remove <hand/player> <id>` | Remove a modifier |
| `/rpgitem modifier list` | List all available modifiers |

### Target Options

- `hand` - Apply modifier to the item in your main hand
- `<playername>` - Apply modifier to a specific player's persistent data

## Common Properties

All modifiers share these base properties:

| Property | Required | Description |
|----------|----------|-------------|
| `id` | Yes | Unique identifier for this modifier instance |
| `targetItem` | Yes | RPGItem name to target (or empty for all) |
| `targetPower` | Yes | Power namespaced key to target (e.g., `rpgitems:beam`) |
| `targetProperty` | Yes | Property name to modify (e.g., `damage`) |
| `priority` | Yes | Execution order (lower = earlier) |

## Available Modifiers

### PlusModifier

Adds a value to the target property.

```
/rpgitem modifier add hand rpgitems:plusmodifier id:damage-boost targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 value:10
```

| Property | Type | Description |
|----------|------|-------------|
| `value` | double | Value to add |

**Effect**: `result = original + value`

### MulModifier

Multiplies the target property by a value.

```
/rpgitem modifier add hand rpgitems:mulmodifier id:damage-double targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:2 value:2.0
```

| Property | Type | Description |
|----------|------|-------------|
| `value` | double | Multiplier value |

**Effect**: `result = original * value`

### DivModifier

Divides the target property by a value.

```
/rpgitem modifier add hand rpgitems:divmodifier id:damage-half targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:3 value:2.0
```

| Property | Type | Description |
|----------|------|-------------|
| `value` | double | Divisor value |

**Effect**: `result = original / value`

### EvalModifier

Evaluates a mathematical expression to calculate the new value.

```
/rpgitem modifier add hand rpgitems:evalmodifier id:dynamic-damage targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 expression:"value * (1 + durability / 100)"
```

| Property | Type | Description |
|----------|------|-------------|
| `expression` | string | Mathematical expression to evaluate |

**Available Variables in Expression:**

| Variable | Description |
|----------|-------------|
| `value` | The current property value |
| `durability` | Item's current durability |
| `random` | Random number between 0 and 1 |
| `time` | Current server time in ticks |

**Effect**: `result = expression evaluation`

## Examples

### Example 1: Increase Beam Damage by 50%

```
/rpgitem modifier add hand rpgitems:mulmodifier id:beam-boost targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 value:1.5
```

### Example 2: Add Flat Cooldown Reduction

```
/rpgitem modifier add hand rpgitems:plusmodifier id:cooldown-reduce targetItem:mysword targetPower:rpgitems:beam targetProperty:cooldown priority:1 value:-10
```

### Example 3: Dynamic Damage Based on Durability

```
/rpgitem modifier add hand rpgitems:evalmodifier id:durability-scaling targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 expression:"value * (durability / 100)"
```

### Example 4: Apply to Specific Player

```
/rpgitem modifier add Notch rpgitems:mulmodifier id:vip-boost targetItem: targetPower: targetProperty:damage priority:1 value:2.0
```

This gives player "Notch" double damage on all powers for all items.

## Priority and Execution Order

Modifiers are applied in priority order (lowest first). This matters when chaining multiple modifiers:

```
# Priority 1: Add 10 damage first
/rpgitem modifier add hand rpgitems:plusmodifier id:base-boost ... priority:1 value:10

# Priority 2: Then multiply by 2
/rpgitem modifier add hand rpgitems:mulmodifier id:multiply ... priority:2 value:2.0
```

With original damage of 20:
- After priority 1: 20 + 10 = 30
- After priority 2: 30 * 2 = 60

## Notes

- Modifiers applied to `hand` are stored in the item's NBT data
- Modifiers applied to players are stored in player persistent data
- Use empty strings for `targetItem`, `targetPower`, or `targetProperty` to match all
- Modifiers are experimental and may change in future versions
