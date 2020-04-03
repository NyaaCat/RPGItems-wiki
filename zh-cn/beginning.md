# 入门教程

RPGItems 插件拥有高度的可自定义性，使用它可以创建出各式各样的武器、防具、道具等等。也正因如此，RPGItems 插件需要一定的时间来熟悉它的使用方式。

本教程以制作一把简易的枪支类武器开始，带你逐步熟悉一个道具从创建到使用的过程。

## 创建一个道具

- 使用 `rpgitem create gun` 来创建一个新的道具，道具的ID为 `gun` 。

## 调整道具的外观

- 使用 `rpgitem item gun STONE_HOE` 将道具的材质更换为石锄头。

- 使用 ```rpgitem display gun `&d手枪 凛` ``` 将道具的展示名设置为 `手枪 凛`（粉色）。
  - 在展示名或描述中包含空格时，需要使用反引号`` ` ``将文本括起来
  - 在展示名或描述中需要使用样式代码时，请使用 `&` 符号

## 为道具添加技能

既然是一把远程武器，我们可以选用粒子束 `beam` ，弹射物 `projectile` 两个技能的其中一种。

`beam` 的参数设置偏为复杂，作为入门教程，我们这里选用 `projectile` 。

- 使用以下指令添加一个弹射物技能：`/rpgitem power add gun projectile cooldown:0 isCone:false amount:1 burstCount:1 burstInterval:0 cost:1 projectileType:arrow speed:5 triggers:LEFT_CLICK`
  - `cooldown:0` 表示技能冷却时间为0
  - `isCone:false` 表示弹射物发射不会发生离散
  - `amount:1` 表示每一波发射弹射物数量为1
  - `burstCount:1` 表示每次触发会发射1波弹射物
  - `burstInterval:0` 表示两波之间发射时间间距为0
  - `cost:1` 表示每次触发会消耗1耐久
  - `projectileType:arrow` 表示选用的弹射物种类为箭矢
  - `speed:5` 表示弹射物发射出去后每秒飞行5个方块距离
  - `triggers:LEFT_CLICK` 表示点击鼠标左键触发技能
  
 在显示 `技能 projectile 已添加至 0 号位` 时，表示技能已经成功应用在了道具上。
 
 关于更多的技能设置内容详见 [技能列表](powers.md)
 
 ## 为道具添加描述文本
 
 在完成上述步骤后我们已经拥有了一把点击左键发射箭矢的枪支类武器，但我们发现，在物品描述中会显示技能的信息，这时候我们希望将它进行隐藏并添加我们需要的描述。
 
 - 使用 `rpgitem togglepowerlore gun`来关闭道具的技能描述显示。
 
 - 使用 ```rpgitem description add gun `&7实用的防身武器` ``` 给道具增加一行文本为`实用的防身武器`的描述。
 
 ## 为道具设置耐久值
 
很多我们可能并不希望一个道具在玩家手中是可以无损耗使用的，因此我们可以设置道具的耐久度。
 
需要注意的是，RPGItems的道具使用了自己的**独立耐久度系统**，道具的耐久无法通过原版的任何方式来进行修复（例如经验修补附魔，铁砧，砂轮合成，背包合成等）。

- 使用 `rpgitem durability 7` 将道具的最大耐久与默认耐久设置为 7 。

- 使用 `rpgitem durability togglebar` 将道具的耐久度显示在描述中。

- 使用 `rpgitem durability NUMERIC` 将道具的耐久度显示格式更改为数字模式。

## 为道具设置伤害

至此，我们的道具已经完成了大部分的制作工序，我们还需要为它设置伤害值。

- 使用 `rpgitem damage 10` 将道具的伤害设置为10点。

在完成此步骤后，我们的道具就算是制作完成了，尽管它有些简易，但已经是一把可以正常使用并且能造成一定伤害的枪支类武器。

对于效果更为复杂的道具，可能会在将来写入新的教程！
