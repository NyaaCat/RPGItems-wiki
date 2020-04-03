# 标记列表

标记可以用来更改一些静态的值, 如同属性修改（AttributeModifier）一样。

以下是用来修改标记的命令:

```
/rpgitem marker [operation] [item] [params]
```

可以使用的Operations:

- `add` 增加一个标记
- `remove` 移除一个具体的标记
- `list` 列出所有的标记
- `prop` 列出一个具体标记的内容或是使用附加参数来调整标记

示例,

```
/rpgitem marker prop myhelmet 0-rpgitems:attributemodifier operation:ADDITION slot:HEAD
```

## 道具属性修改（Attribute Modifier）

各类属性:

- `amount` 用于计算的数量
- `attribute` 详见 https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/Attribute.html
- `operation` 详见 https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/AttributeModifier.Operation.html
- `slot` 详见 https://hub.spigotmc.org/javadocs/spigot/org/bukkit/inventory/EquipmentSlot.html

## Lore过滤（Lore Fliter）

TBD

## 远程攻击（Range）

表示道具可以造成远程伤害。

## 仅远程攻击（Range Only）

表示道具可以造成远程伤害且不会造成近战伤害。

## 选择器（Selector）

TBD

## 无法破坏（Unbreakble）

表示道具无法被破坏或是消耗掉。

## 独特性（Unique）

表示道具具有唯一性且无法被堆叠。

注意：使用了此标记的道具在分配给玩家时将会更新一个随机且唯一的String类字符串，因此即使是同一种道具它们也无法进行堆叠，

因此使用了Unique标记的道具也不可用于NPC交易。
