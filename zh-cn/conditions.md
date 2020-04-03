# 条件列表

条件可以根据多种不同的变量来确定是否激活技能。

条件具有ID，它可以通过技能，触发或是条件本身来进行调用。

你也可以使用多种条件ID并将它们视为AND进行使用(即必须满足所有条件).

可以用于修改条件的基础命令如下:

```
/rpgitem condition [operation] [item] [params]
```

可使用的Operations:

- `add` 增加一个具有详细参数的条件
- `remove` 移除一个条件
- `list` 列出所有条件
- `prop` 列出一个特定条件的消息参数或是使用额外参数来修改条件

示例：

```
/rpgitem condition add set-sword rpgitems:andcondition id:set-armor conditions:arm-helmet,arm-chest,arm-legs,arm-boot
```

其他的常规设置:

- 当`isCritical`设置为`true`时, 若不满足条件将会中止所有技能的运行过程。
- 当`isStatic`设置为`true`时, 条件值将不会在一个单独的运行过程中被改变, 即便后面的技能会使其发生变化，条件也依然是满足的。

## And条件（AndCondition）

在满足所有条件时生效。

## Or条件（OrCondition）

当满足任意一个条件时生效。

## Xor条件（XorCondition）

当所有条件结果的异或值为真时生效。

* 注意: 你需要设置`isStatic`为`false`。

## 物品栏条件（SlotCondition）

当道具放在物品栏中的正确位置时生效。

可用的物品栏位置:

- `ARMOR`
- `HAND`
- `BACKPACK`
- `BELT`
- `INVENTORY`
- `HELMET`
- `CHESTPLATE`
- `LEGGINGS`
- `BOOTS`
- `MAIN_HAND`
- `OFF_HAND`

示例：

```
/rpgitem condition add myhelmet rpgitems:slotcondition id:wear critical:true slots:HELMET
```

## 概率条件（ChanceCondition）

有N%的几率生效。

## 伤害种类条件（DamageTypeCondition）

在伤害种类（damageType）对应时生效。

## 耐久条件（DurabilityCondition）

当耐久在指定范围内时生效。

## 装备条件（EquipmentCondition）

当道具穿戴在正确物品栏位置时生效。

## 计分板条件（ScoreboardCondition）

当计分板的值在指定范围内或具有相应tag时生效。

## 动态条件（EvalCondition）

TBD

## 结果条件（LastResultCondition）

当上一技能正确执行时生效。
