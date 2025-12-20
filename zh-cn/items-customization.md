# 制作道具

## 道具展示名设置（Display Name）

```
/rpgitem display mysword `&aHero Sword`
```

* 如果道具展示名中包含空格，你可以使用反引号 `` ` ``将文本括起来
* 如果需要在道具展示名中使用样式代码，请使用`&`符号。

## 道具模型设置（Item Model）

```
/rpgitem item mysword [material] <option>
```

* `option` 应当是一个由十六进制色值转换为十进制值的数值。

示例,

```
/rpgitem item mysword diamond_sword
```

将道具模型设置为钻石剑。

```
/rpgitem item myhelmet leather_helmet 6737151
```

将道具模型设置为颜色为#66CCFF的皮革帽子。

你也可以使用自定义模型数据(1.15.2+):

```
/rpgitem customModel mysword 1234567
```

## 道具描述设置（Lore）

添加一行描述

```
/rpgitem description [item] add `&9Blue description line`
```

设置某一行描述

```
/rpgitem description [item] set 1 `&aGreen description line`
```

* 注意：描述的行数从0开始。

插入一行描述

```
/rpgitem description [item] insert 1 `&cRed inserted description`
```

移除一行描述

```
/rpgitem description [item] remove 0
```

默认情况下，道具属性与技能将会作为lore显示，如果你不希望它们显示出来, 你可以使用以下命令来开启或关闭它们的显示状态：

```
/rpgitem togglepowerlore mysword
```

```
/rpgitem togglearmorlore mysword
```

## 道具附魔设置（Enchantment）

你可以使用以下命令来控制道具是否允许被附魔：

```
/rpgitem enchantmode mysword ALLOW
```

```
/rpgitem enchantmode mysword DISALLOW
```

如果想让道具默认就拥有附魔，你需要在主手拿着一个有附魔的任意道具，并使用以下命令：

```
/rpgitem enchantment mysword clone
```

如果要清除道具的默认附魔，请使用以下命令：

```
/rpgitem enchantment mysword clear
```

添加指定附魔：

```
/rpgitem enchantment mysword add sharpness 5
```

移除指定附魔：

```
/rpgitem enchantment mysword remove sharpness
```

## 道具设置（Flags）

```
/rpgitem additemflag mysword HIDE_ATTRIBUTES
```

```
/rpgitem removeitemflag mysword HIDE_ENCHANTS
```

## 道具属性更新（Attribute Update）

默认的情况下道具的属性修改不会应用给已经分发出去的道具（`PARTIAL_UPDATE`）。如果你想让已经存在的道具也进行更新，可以使用 `FULL_UPDATE` 

```
/rpgitem attributemode mysword FULL_UPDATE
```

## 道具伤害设置（Damage）

```
/rpgitem damage mysword 10
```

每次击中造成10点伤害。

```
/rpgitem damagemode mysword FIXED
```

以下是可以设置的模式:

- `FIXED` 使用RPGItems设置的伤害值
- `FIXED_WITHOUT_EFFECT` 固定伤害，忽略效果
- `FIXED_RESPECT_VANILLA` 固定伤害，应用原版修正
- `FIXED_WITHOUT_EFFECT_RESPECT_VANILLA` 固定伤害无效果，应用原版修正
- `VANILLA` 仅使用原版物品的伤害值
- `ADDITIONAL` 使用RPGItems设置的伤害值 + 原版物品伤害值 (包含物品的附魔与属性修改)
- `ADDITIONAL_RESPECT_VANILLA` 附加伤害，应用原版修正
- `MULTIPLY` 乘以原版伤害值

```
/rpgitem damageType mysword melee
```

伤害种类可以使用条件中的任意String。

## 道具护甲设置（Armour）

```
/rpgitem armour myhelmet 20
```

以上命令仅减少20%的伤害。

穿戴多个防护道具的伤害计算为 `damage = damage * helmet.armor * chestplate.armor * leggings.armor * boots.armor`

```
/rpgitem armorExpression myhelmet `min(damage,max(40,damage-20))`
```

护甲伤害将会使用表达式计算并且略过其他的处理 (比如附魔等...)

你也可以设置玩家特定的护甲表达式：

```
/rpgitem playerArmorExpression myhelmet <expression>
```

## 道具耐久系统（Durability）

```
/rpgitem durability mysword 1001
```

将道具耐久设置为1001。

```
/rpgitem durability mysword bound 2 1001
```

设置道具耐久的下界与上界分别为2与1001。

```
/rpgitem durability mysword default 500
```

设置新生成道具的默认耐久值为500。

```
/rpgitem durability mysword barformat [FORMAT]
```

可以使用的格式:

- `DEFAULT` 使用设置文件中默认的设置
- `NUMERIC` 使用十进制数值显示
- `NUMERIC_MINUS_ONE` 使用 `十进制数值 - 1` 格式显示
- `NUMERIC_BIN` 使用二进制显示
- `NUMERIC_BIN_MINUS_ONE` 使用 `二进制值 - 1` 格式显示
- `NUMERIC_HEX` 使用十六进制值显示
- `NUMERIC_HEX_MINUS_ONE` 使用 `十六进制值 - 1` 格式显示

```
/rpgitem durability mysword togglebar
```

控制道具耐久条的开启与关闭

## 道具权限设置（Permission）

```
/rpgitem permission mysword rpgitem.use.mysword true
```

权限命令需要三个参数：道具名、权限字符串和启用状态（true/false）。
