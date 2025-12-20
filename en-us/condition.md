# Conditions

Conditions determine whether a power can be activated based on various criteria.

Conditions have an ID that can be referenced in powers, triggers, and other conditions.

Multiple condition IDs can be specified and are treated as AND operation (all conditions must be met).

## General Condition Properties

All conditions share these common properties:

- `id` Unique identifier for this condition (required for most conditions)
- `isStatic` When `true`, condition is evaluated once and cached. When `false`, evaluated each time (default varies)
- `isCritical` When `true`, failing this condition aborts the entire power execution chain (default: false)
- `displayText` Optional display text for the condition
- `tags` Set of tags for grouping conditions

## Basic Commands

```
/rpgitem condition [operation] [item] [params]
```

Available operations:

- `add` Add a condition with parameters
- `remove` Remove a condition
- `list` List all conditions
- `prop` View or modify condition properties

Example:

```
/rpgitem condition add set-sword rpgitems:andcondition id:set-armor conditions:arm-helmet,arm-chest,arm-legs,arm-boot
```

## Logic Conditions

### AndCondition

Returns true only if ALL referenced conditions pass.

- `id` Unique identifier (required)
- `conditions` Comma-separated list of condition IDs to check
- `isStatic` Cache result (default: true)
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:andcondition id:all-armor conditions:has-helmet,has-chest,has-legs,has-boots
```

### OrCondition

Returns true if ANY referenced condition passes.

- `id` Unique identifier (required)
- `conditions` Comma-separated list of condition IDs to check
- `isStatic` Cache result (default: true)
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:orcondition id:any-armor conditions:has-helmet,has-chest
```

### XorCondition

Returns true when an ODD number of conditions pass (exclusive OR).

- `id` Unique identifier (required)
- `conditions` Comma-separated list of condition IDs to check
- `init` Initial XOR value (default: false)
- `isStatic` Cache result (default: true)
- `isCritical` Abort on failure (default: false)

**Note**: You typically need to set `isStatic` to `false` for dynamic behavior.

## Probability Conditions

### ChanceCondition

Returns true based on random percentage chance.

- `id` Unique identifier (required)
- `chancePercentage` Success percentage from 0 to 100 (default: 0)
- `isStatic` Always false for runtime evaluation
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:chancecondition id:fifty-fifty chancePercentage:50
```

## Equipment Conditions

### SlotCondition

Returns true when item is in the correct inventory slot.

- `id` Unique identifier (required)
- `slots` Slot types to check (can specify multiple)
- `critical` Abort on failure (default: false)

Available slot values:

**Armor Slots:**
- `ARMOR` / `ANY_ARMOR` - Any single armor piece matches
- `ALL_ARMOR` - All 4 armor pieces must match
- `HELMET` - Head slot only
- `CHESTPLATE` - Chest slot only
- `LEGGINGS` - Legs slot only
- `BOOTS` - Feet slot only

**Hand Slots:**
- `HAND` / `BOTH_HAND` - Both hands must have item
- `ANY_HAND` - Either hand matches
- `MAIN_HAND` - Main hand only
- `OFF_HAND` - Off hand only

**Inventory Slots:**
- `BACKPACK` - Slots 10-35 (main inventory)
- `BELT` / `HOTBAR` - Slots 0-8 (hotbar)
- `INVENTORY` - Backpack OR hotbar

Example:
```
/rpgitem condition add myhelmet rpgitems:slotcondition id:wear critical:true slots:HELMET
```

### EquipmentCondition

Returns true when specific equipment is worn.

- `id` Unique identifier (required)
- `slots` Equipment slots to check
- `material` Bukkit material to match
- `itemStack` Specific ItemStack to match
- `rpgitem` RPGItem name or UID to match
- `matchAllSlot` If true, ALL slots must match; if false, ANY slot matches (default: false)
- `requireEmpty` If true, slots must be empty (default: false)
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:equipmentcondition id:has-diamond-helm slots:HELMET material:DIAMOND_HELMET
```

## Combat Conditions

### DamageTypeCondition

Returns true when damage type matches.

- `id` Unique identifier (required)
- `damageType` Type to match: `melee`, `ranged`, `magic`, or `summon`
- `isStatic` Runtime evaluation (default: false)
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:damagetype id:ranged-only damageType:ranged
```

### DurabilityCondition

Returns true when item durability is within range.

- `id` Unique identifier (required)
- `durabilityMin` Minimum durability (default: MIN_VALUE)
- `durabilityMax` Maximum durability (default: MAX_VALUE)
- `isStatic` Runtime evaluation (default: false)
- `isCritical` Abort on failure (default: false)

Example:
```
/rpgitem condition add myitem rpgitems:durabilitycondition id:low-durability durabilityMax:100
```

## Expression Conditions

### EvalCondition

Evaluate mathematical/logical expressions using EvalEx library.

- `id` Unique identifier (required)
- `expression` Expression to evaluate (required)
- `isStatic` Runtime evaluation (default: false)
- `isCritical` Abort on failure (default: false)

Available variables in expression:
- `playerYaw` - Player head yaw rotation
- `playerPitch` - Player head pitch rotation
- `playerX`, `playerY`, `playerZ` - Player coordinates
- `playerLastDamage` - Last damage taken

Available functions:
- `scoreBoard(name)` - Get player's scoreboard score for objective
- `context(key)` - Get value from power context
- `now()` - Current timestamp in milliseconds

Expression returns true if result equals 1 (BigDecimal.ONE).

Example:
```
/rpgitem condition add myitem rpgitems:evalcondition id:low-damage expression:"playerLastDamage < 5"
```

## Scoreboard Conditions

### ScoreboardCondition

Check player scoreboard scores, tags, and team membership.

- `id` Unique identifier (required)
- `score` Score selector: `objective_name:min,max another:min,max`
- `tag` Tag selector: `MUST_HAVE,!MUST_NOT_HAVE` (empty string = no tags, `!` = any tags)
- `team` Team selector: `MUST_ON,!MUST_NOT_ON` (empty string = no team, `!` = any team)
- `isStatic` Runtime evaluation (default: false)
- `isCritical` Abort on failure (default: false)

All three selectors are AND-ed together when present.

Example:
```
/rpgitem condition add myitem rpgitems:scoreboardcondition id:has-kills score:kills:10,100 tag:warrior
```

## External Integration Conditions

### PlaceholderAPICondition

Compare PlaceholderAPI values (requires PlaceholderAPI plugin).

- `id` Unique identifier (required)
- `placeholder` PlaceholderAPI placeholder syntax (required)
- `operator` Comparison operator (required)
- `value` Expected value to compare (required)
- `isStatic` Runtime evaluation (default: false)
- `isCritical` Abort on failure (default: false)

Available operators:

**String Comparison:**
- `==` Equal
- `!=` Not equal
- `===` Equal (case-insensitive)
- `!==` Not equal (case-insensitive)

**String Matching:**
- `contains` / `contain` - Contains substring
- `!contains` / `!contain` - Does not contain
- `startwith` - Starts with
- `!startwith` - Does not start with
- `endwith` - Ends with
- `!endwith` - Does not end with

**Regex:**
- `matches` / `match` - Matches regex
- `!matches` / `!match` - Does not match regex

**Numeric:**
- `>=` Greater than or equal
- `>` Greater than
- `<` Less than
- `<=` Less than or equal

Example:
```
/rpgitem condition add myitem rpgitems:placeholdercondition id:high-level placeholder:%player_level% operator:>= value:10
```

## Chain Conditions

### LastResultCondition

Check the result of the previously executed power.

- `id` Unique identifier (required)
- `okResults` Set of acceptable result types (default: OK)
- `failOnNoResult` Fail when no previous result exists (default: true)
- `isCritical` Abort on failure (default: false)

Available result types: `OK`, `FAIL`, `ABORT`, `COOLDOWN`, `COST`, `NOOP`

Example:
```
/rpgitem condition add myitem rpgitems:lastresultcondition id:prev-ok okResults:OK
```

## Condition Summary Table

| Condition | Type | Evaluation | Key Properties |
|-----------|------|------------|----------------|
| AndCondition | Logic | Static/Dynamic | conditions |
| OrCondition | Logic | Static/Dynamic | conditions |
| XorCondition | Logic | Static/Dynamic | conditions, init |
| ChanceCondition | Random | Runtime | chancePercentage |
| SlotCondition | Equipment | Static | slots |
| EquipmentCondition | Equipment | Static | slots, material, rpgitem |
| DamageTypeCondition | Combat | Runtime | damageType |
| DurabilityCondition | Item | Runtime | durabilityMin, durabilityMax |
| EvalCondition | Expression | Runtime | expression |
| ScoreboardCondition | Scoreboard | Runtime | score, tag, team |
| PlaceholderAPICondition | External | Runtime | placeholder, operator, value |
| LastResultCondition | Chain | Runtime | okResults |
