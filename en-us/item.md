# Item Customization

## Display name

```
/rpgitem display mysword `&aHero Sword`
```

* You can use \`\` to quote a name containing spaces.
* You can use `&` symbol to indicate a formatting code.

## Item model

```
/rpgitem item mysword [material] <option>
```

* `option` can be a decimal value converted from hexadecimal color value.

E.g.,

```
/rpgitem item mysword diamond_sword
```

Change the item model to diamond sword.

```
/rpgitem item myhelmet leather_helmet 6737151
```

Change the item model to leather helmet with color #66CCFF.

You can also use customModelData (1.15.2+):

```
/rpgitem customModel mysword 1234567
```

## Lore

Add lore

```
/rpgitem description [item] add `&9Blue description line`
```

Set lore

```
/rpgitem description [item] set 1 `&aGreen description line`
```

* Note: lore starts from line 0.

Insert lore

```
/rpgitem description [item] insert 1 `&cRed inserted description`
```

Remove lore

```
/rpgitem description [item] remove 0
```

By default, armor and power description will be added as lore. If you wish not to use this feature, toggle it using:

```
/rpgitem togglepowerlore mysword
```

```
/rpgitem togglearmorlore mysword
```

## Enchantment

Set enchant mode to allow, disallow or with permission:

```
/rpgitem enchantmode mysword ALLOW
```

```
/rpgitem enchantmode mysword DISALLOW
```

To add enchantment to an item as default, hold a random item and make enchants on it, then use

```
/rpgitem enchantment mysword clone
```

To clear default enchantments on an item, use

```
/rpgitem enchantment mysword clear
```

To add a specific enchantment:

```
/rpgitem enchantment mysword add sharpness 5
```

To remove a specific enchantment:

```
/rpgitem enchantment mysword remove sharpness
```

## Item flags

```
/rpgitem additemflag mysword HIDE_ATTRIBUTES
```

```
/rpgitem removeitemflag mysword HIDE_ENCHANTS
```

## Attribute Update

By default attribute modifiers won't be updated for existing items (`PARTIAL_UPDATE`). To change this, you need to instruct the item to do `FULL_UPDATE`.

```
/rpgitem attributemode mysword FULL_UPDATE
```

## Damage system

```
/rpgitem damage mysword 10
```

Deals 10 damage each hit.

```
/rpgitem damagemode mysword FIXED
```

Available modes:

- `FIXED` RPGItems fixed damage
- `FIXED_WITHOUT_EFFECT` Fixed damage, ignoring effects
- `FIXED_RESPECT_VANILLA` Fixed damage with vanilla modifiers
- `FIXED_WITHOUT_EFFECT_RESPECT_VANILLA` Fixed damage without effects, with vanilla modifiers
- `VANILLA` Vanilla damage only
- `ADDITIONAL` RPGItems damage + vanilla damage (including enchants and attribute modifiers)
- `ADDITIONAL_RESPECT_VANILLA` Additional damage with vanilla modifiers
- `MULTIPLY` Multiply vanilla damage by RPGItems damage factor

```
/rpgitem damageType mysword melee
```

DamageType can be any string which can be used in conditions.

## Armor

```
/rpgitem armour myhelmet 20
```

Will simply reduce 20% damage.

Multiple armor pieces calculate as `damage = damage * helmet.armor * chestplate.armor * leggings.armor * boots.armor`

```
/rpgitem armorExpression myhelmet `min(damage,max(40,damage-20))`
```

Damage will be calculated as expression and then pass to next handler (enchants, etc.)

You can also set a player-specific armor expression:

```
/rpgitem playerArmorExpression myhelmet <expression>
```

## Durability system

```
/rpgitem durability mysword 1001
```

Sets durability to 1001.

```
/rpgitem durability mysword bound 2 1001
```

Sets lower and upper bound to 2 and 1001.

```
/rpgitem durability mysword default 500
```

Sets the default durability for newly spawned items to 500.

```
/rpgitem durability mysword barformat [FORMAT]
```

Available formats:

- `DEFAULT` use default format in configuration
- `NUMERIC` use numeric format
- `NUMERIC_MINUS_ONE` use `numeric - 1` format
- `NUMERIC_BIN` use binary format
- `NUMERIC_BIN_MINUS_ONE` use `binary - 1` format
- `NUMERIC_HEX` use hexadecimal
- `NUMERIC_HEX_MINUS_ONE` use `hexadecimal - 1` format

```
/rpgitem durability mysword togglebar
```

Toggle display/hide durability bar

Use-based costs:

```
/rpgitem cost [item] [operation] [value]
```

`operation` could be

- `toggle` - switch from percentage cost (based on damage, etc) or fixed value
- `hit` - cost durability when get hit
- `hitting` - cost durability when hitting with the item
- `breaking` - cost durability when breaking blocks with the item

For example, define an axe to cost 1 durability when breaking blocks, use

```
/rpgitem cost my_axe breaking 1
```

Leave `[value]` empty will return the current cost information.

## Permission

```
/rpgitem permission mysword rpgitem.use.mysword true
```

The permission command takes three arguments: item, permission string, and enabled (true/false).

## Socketing System

> This section applies to RPGItems `3.9`.

Socketing treats one RPGItem as a container and merges multiple RPGItems as sockets into a single item instance.

Runtime order is: `container -> socket1 -> socket2 -> ...`.

- `condition`: shared and evaluated as one set
- `marker`: processed sequentially
- `power`: triggered sequentially by trigger order

### Recommended: Create Socket Items by Commands (Hot Update)

Example: set `ruby_gem` as a socket item, and `great_sword` as a container:

```bash
/rpgitem socket ruby_gem socketitem true
/rpgitem socket ruby_gem tags set GEM
/rpgitem socket ruby_gem weight 3
/rpgitem socket ruby_gem minlevel 2
/rpgitem socket ruby_gem lore clear
/rpgitem socket ruby_gem lore add &7[Gem] +10 Damage

/rpgitem socket great_sword container true
/rpgitem socket great_sword accepttags set GEM
/rpgitem socket great_sword maxweight 10
/rpgitem socket great_sword insertline 1
```

Notes:

- All commands are hot-updated immediately; no manual yml editing required
- Use `/rpgitem socket <item> info` to inspect current socket config
- Then use `/rpgitem socket` to open GUI and edit instance sockets

### Container fields

```yaml
socketAcceptTags:
  - GEM
  - RUNE
socketMaxWeight: 10
socketInsertLine: 1
```

- `socketAcceptTags`: accepted socket tags (match any)
- `socketMaxWeight`: max total socket weight
- `socketInsertLine`: lore insertion line for socket descriptions (`0-based`)

### Socket item fields

```yaml
socketTags:
  - GEM
socketWeight: 3
socketMinLevel: 2
socketingDescription:
  - "&7[Gem] +10 Damage"
```

- `socketTags`: tags provided by this socket item
- `socketWeight`: weight cost
- `socketMinLevel`: minimum container level
- `socketingDescription`: lore lines inserted after socketing

### Matching rules

- Empty `socketAcceptTags`: accepts no sockets (default)
- Empty `socketTags`: matches no tags
- Special tag `ANY`: wildcard for any tag
- Socket is invalid if tag mismatch, level too low, or weight overflow

### Lore behavior

- Instance lore only inserts `socketingDescription`
- Newly created items default to hidden armor lore and power lore

## Item Level and LevelDescription

> This section applies to RPGItems `3.9`.

Every item **instance** starts at level `1`. Level and sockets are persisted in per-instance PDC data.

### LevelDescription example

```yaml
description:
  - "Great Sword"
  - "Damage: &650"
leveldescription:
  "0":
    level: 3
    operation: replaceline
    line: 1
    newlines:
      - "Damage: &6100"
      - "Critical: &c20%"
  "1":
    level: 5
    operation: replacewhole
    line: 0
    newlines:
      - "Great Sword"
      - "Damage: &6200"
      - "Critical: &c25%"
      - "&7This is another lore"
```

Rules:

- `line` is `0-based`
- `replaceline`: replace starting from `line` with `newlines`
- `replacewhole`: ignore `line`, replace the whole description
- Select the smallest rule where `level >= currentLevel` (fallback to original description if none)
- Replacement is always based on configured base `description`, not previous level output
