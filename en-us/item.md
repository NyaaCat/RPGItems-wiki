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

You can toggle the status to use custom model (pre-1.13.2):

```
/rpgitem customitemmodel mysword
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
/rpgitem description insert 1 `&cRed inserted description`
```

Remove lore

```
/rpgitem description remove 0
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

## Item flags

```
/rpgitem additemflag mysword HIDE_ATTRIBUTES
```

```
/rpgitems removeitemflag mysword HIDE_ENCHANTS
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
- `ADDITIONAL` RPGItems damage + vanilla damage (including enchants and attribute modifiers)
- `VANILLA` Vanilla damage only

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
/rpgitem durability mysword barformat [FORMAT]
```

Available formats:

- `DEFAULT` use default format in configuration
- `NUMERIC` use numeric format
- `NUMERIC_MINUS_ONE` use `numeric - 1` format
- `NUMERIC_HEX` use hexadecimal
- `NUMERIC_HEX_MINUS_ONE` use `hexadecimal - 1` format
- `DEFAULT8` use Octadecimal format

```
/rpgitem durability mysword togglebar
```

Toggle display/hide durability bar

## Permission

```
/rpgitem permission mysword rpgitem.use.mysword
```

