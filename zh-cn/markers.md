# 标记列表

标记可以用来修改一些静态的物品属性，如属性修改器（AttributeModifier）和特殊行为。

## 基础命令

```
/rpgitem marker [operation] [item] [params]
```

可使用的Operations:

- `add` 增加一个标记
- `remove` 移除一个具体的标记
- `list` 列出所有的标记
- `prop` 列出一个具体标记的内容或使用附加参数来调整标记

示例：
```
/rpgitem marker prop myhelmet 0-rpgitems:attributemodifier operation:ADDITION slot:HEAD
```

## 通用标记属性

所有标记共享以下属性（来自BaseMarker）：

- `markerId` 此标记的唯一标识符（默认：""）
- `displayName` 自定义显示名称
- `tags` 用于分组标记的标签集合

## 属性修改器（AttributeModifier）

为道具添加Minecraft属性修改器。

**属性：**
- `amount` 修改数量（默认：2）
- `attribute` 目标属性（默认：MAX_HEALTH）
- `name` 修改器友好名称（默认："maxHealth"）
- `operation` 数学运算方式（必需，默认：ADD_NUMBER）
- `slot` 修改器生效的装备槽位
- `namespacedKey` 修改器的唯一标识符

### 可用属性

详见 https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/Attribute.html

常用属性：
- `MAX_HEALTH` - 最大生命值
- `ATTACK_DAMAGE` - 近战攻击伤害
- `ATTACK_SPEED` - 攻击冷却速度
- `ARMOR` - 护甲值
- `ARMOR_TOUGHNESS` - 护甲韧性
- `MOVEMENT_SPEED` - 移动速度
- `KNOCKBACK_RESISTANCE` - 击退抗性
- `LUCK` - 战利品表幸运值

### 可用运算方式

详见 https://hub.spigotmc.org/javadocs/spigot/org/bukkit/attribute/AttributeModifier.Operation.html

- `ADD_NUMBER` - 添加固定数值
- `ADD_SCALAR` - 添加基础值的百分比
- `MULTIPLY_SCALAR_1` - 乘以(1 + amount)

### 可用槽位

使用`EquipmentSlotGroup`进行槽位定位：

- `HAND` - 主手
- `OFF_HAND` - 副手
- `HEAD` - 头盔槽位
- `CHEST` - 胸甲槽位
- `LEGS` - 护腿槽位
- `FEET` - 靴子槽位
- `MAINHAND` - 明确指定主手
- `OFFHAND` - 明确指定副手
- `ARMOR` - 任意护甲槽位
- `ANY` - 任意装备槽位

**示例：**
```
/rpgitem marker add myhelmet rpgitems:attributemodifier amount:4 attribute:MAX_HEALTH operation:ADD_NUMBER slot:HEAD
```

## Lore过滤器（LoreFilter）

过滤并保留匹配指定模式的特定lore行。

**属性：**
- `regex` 要匹配的正则表达式模式
- `desc` 显示描述（必需）
- `find` 使用正则`.find()`而非`.match()`（默认：false）

**示例：**
```
/rpgitem marker add myitem rpgitems:lorefilter regex:"^\\+[0-9]+ Damage$" desc:"伤害加成"
```

## 远程攻击（Ranged）

标记道具可以造成远程伤害。

**属性：**
- `r` 最大有效半径，单位为方块（默认：MAX_VALUE）
- `rm` 最小有效半径，单位为方块（默认：0）

此标记影响基于弹射物技能的伤害计算。

**示例：**
```
/rpgitem marker add mybow rpgitems:ranged rm:5 r:50
```

## 仅远程攻击（RangedOnly）

标记道具**仅**造成远程伤害（无近战伤害）。

**属性：**
- `r` 最大有效半径，单位为方块（默认：MAX_VALUE）
- `rm` 最小有效半径，单位为方块（默认：0）

带有此标记的道具在直接命中时不会造成近战伤害。

**示例：**
```
/rpgitem marker add mystaff rpgitems:rangedonly rm:0 r:100
```

## 选择器（Selector）

用于范围技能的高级实体选择/过滤。

**属性：**
- `id` 唯一标识符（必需）
- `display` 自定义显示消息
- `type` 实体类型过滤器（逗号分隔）
- `count` 最大目标数量
- `x`, `y`, `z` 参考位置（支持波浪号表示法，如`~5`）
- `r` 距参考点的最大半径
- `rm` 距参考点的最小半径
- `dx`, `dy`, `dz` 各轴最大距离
- `score` 计分板分数过滤器：`分数名:最小值,最大值`
- `tag` 计分板标签过滤器：`必须有的标签,!不能有的标签`
- `team` 队伍过滤器：`必须在的队伍,!不能在的队伍`
- `gameMode` 游戏模式过滤器：`可以是的模式,!不能是的模式`

**示例：**
```
/rpgitem marker add myaoe rpgitems:selector id:nearby-zombies type:ZOMBIE r:10 count:5
```

然后在技能中使用：
```
/rpgitem power add myaoe rpgitems:aoedamage selectors:nearby-zombies damage:10
```

## 无法破坏（Unbreakable）

标记道具为无法破坏 - 防止耐久度损耗。

除通用标记属性外无额外属性。

**示例：**
```
/rpgitem marker add myitem rpgitems:unbreakable
```

## 独特性（Unique）

标记道具为独特 - 无法堆叠且每个玩家生成唯一ID。

**属性：**
- `enabled` 是否启用独特标记（默认：true）

**注意：** 带有此标记的道具：
- 分配给玩家时将被赋予唯一的随机字符串
- 即使是相同的道具也无法堆叠
- 不能用于NPC交易

**示例：**
```
/rpgitem marker add myitem rpgitems:unique enabled:true
```

## 标记汇总表

| 标记 | 用途 | 关键属性 |
|------|------|----------|
| AttributeModifier | 添加MC属性修改器 | amount, attribute, operation, slot |
| LoreFilter | 按正则过滤lore行 | regex, desc, find |
| Ranged | 标记为远程武器 | r, rm |
| RangedOnly | 标记为仅远程（无近战） | r, rm |
| Selector | 定义实体选择规则 | id, type, r, rm, count等 |
| Unbreakable | 防止耐久度损耗 | （无） |
| Unique | 防止堆叠，每玩家唯一 | enabled |
