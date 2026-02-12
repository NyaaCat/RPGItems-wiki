# 条件列表

条件可以根据多种不同的变量来确定是否激活技能。

条件具有ID，它可以通过技能、触发或条件本身来进行调用。

你也可以使用多种条件ID并将它们视为AND进行使用（即必须满足所有条件）。

## 基础命令

```
/rpgitem condition [operation] [item] [params]
```

可使用的Operations:

- `add` 增加一个具有详细参数的条件
- `remove` 移除一个条件
- `list` 列出所有条件
- `prop` 列出一个特定条件的消息参数或使用额外参数来修改条件

示例：
```
/rpgitem condition add set-sword rpgitems:andcondition id:set-armor conditions:arm-helmet,arm-chest,arm-legs,arm-boot
```

## 通用条件属性

所有条件共享以下属性（来自BaseCondition）：

- `id` 条件的唯一标识符（大多数条件必需）
- `isStatic` 当设置为`true`时，条件值将被缓存，不会在单次执行过程中重新计算（默认值因条件而异）
- `isCritical` 当设置为`true`时，若不满足条件将中止所有后续技能的执行（默认：false）
- `displayText` 条件的可选显示文本
- `tags` 用于分组条件的标签集合

## 逻辑条件

### And条件（AndCondition）

在满足**所有**引用条件时生效。

**属性：**
- `id` 唯一标识符（必需）
- `conditions` 要检查的条件ID列表，逗号分隔
- `isStatic` 缓存结果（默认：true）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:andcondition id:all-armor conditions:has-helmet,has-chest,has-legs,has-boots
```

### Or条件（OrCondition）

在满足**任意一个**引用条件时生效。

**属性：**
- `id` 唯一标识符（必需）
- `conditions` 要检查的条件ID列表，逗号分隔
- `isStatic` 缓存结果（默认：true）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:orcondition id:any-armor conditions:has-helmet,has-chest
```

### Xor条件（XorCondition）

当满足条件的数量为**奇数**时生效（异或运算）。

**属性：**
- `id` 唯一标识符（必需）
- `conditions` 要检查的条件ID列表，逗号分隔
- `init` 初始异或值（默认：false）
- `isStatic` 缓存结果（默认：true）
- `isCritical` 失败时中止（默认：false）

**注意：** 通常需要将`isStatic`设置为`false`以实现动态行为。

## 概率条件

### 概率条件（ChanceCondition）

基于随机百分比几率生效。

**属性：**
- `id` 唯一标识符（必需）
- `chancePercentage` 成功百分比，0到100（默认：0）
- `isStatic` 始终为false以进行运行时评估
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:chancecondition id:fifty-fifty chancePercentage:50
```

## 装备条件

### 物品栏条件（SlotCondition）

当道具在正确的物品栏位置时生效。

**属性：**
- `id` 唯一标识符（必需）
- `slots` 要检查的槽位类型（可指定多个）
- `critical` 失败时中止（默认：false）

**可用槽位值：**

**护甲槽位：**
- `ARMOR` / `ANY_ARMOR` - 任意单个护甲位置匹配
- `ALL_ARMOR` - 所有4个护甲位置都必须匹配
- `HELMET` - 仅头盔槽位
- `CHESTPLATE` - 仅胸甲槽位
- `LEGGINGS` - 仅护腿槽位
- `BOOTS` - 仅靴子槽位

**手持槽位：**
- `HAND` / `BOTH_HAND` - 双手都必须持有道具
- `ANY_HAND` - 任意一只手匹配
- `MAIN_HAND` - 仅主手
- `OFF_HAND` - 仅副手

**物品栏槽位：**
- `BACKPACK` - 槽位10-35（主背包）
- `BELT` / `HOTBAR` - 槽位0-8（快捷栏）
- `INVENTORY` - 背包或快捷栏

**示例：**
```
/rpgitem condition add myhelmet rpgitems:slotcondition id:wear critical:true slots:HELMET
```

### 装备条件（EquipmentCondition）

当穿戴特定装备时生效。

**属性：**
- `id` 唯一标识符（必需）
- `slots` 要检查的装备槽位
- `material` 要匹配的Bukkit材质
- `itemStack` 要匹配的特定ItemStack
- `rpgitem` 要匹配的RPGItem名称或UID
- `matchAllSlot` 如果为true，所有槽位都必须匹配；如果为false，任意槽位匹配即可（默认：false）
- `requireEmpty` 如果为true，槽位必须为空（默认：false）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:equipmentcondition id:has-diamond-helm slots:HELMET material:DIAMOND_HELMET
```

## 战斗条件

### 伤害类型条件（DamageTypeCondition）

当伤害类型匹配时生效。

**属性：**
- `id` 唯一标识符（必需）
- `damageType` 要匹配的类型：`melee`（近战）、`ranged`（远程）、`magic`（魔法）或`summon`（召唤）
- `isStatic` 运行时评估（默认：false）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:damagetype id:ranged-only damageType:ranged
```

### 耐久条件（DurabilityCondition）

当道具耐久度在指定范围内时生效。

**属性：**
- `id` 唯一标识符（必需）
- `durabilityMin` 最小耐久度（默认：MIN_VALUE）
- `durabilityMax` 最大耐久度（默认：MAX_VALUE）
- `isStatic` 运行时评估（默认：false）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:durabilitycondition id:low-durability durabilityMax:100
```

### 物品等级条件（ItemLevelCondition）

> 本节内容适用于 RPGItems `3.9` 版本。

当道具实例等级满足比较条件时生效。

**属性：**
- `id` 唯一标识符（必需）
- `level` 目标等级（默认：1）
- `operation` 比较操作：`eq`、`gt`、`egt`、`lt`、`elt`（默认：`eq`）
- `isStatic` 是否静态缓存（默认：true）
- `isCritical` 失败时中止（默认：false）

**示例：**
```
/rpgitem condition add myitem rpgitems:itemlevelcondition id:need-lv5 level:5 operation:egt
```

## 表达式条件

### 动态条件（EvalCondition）

使用EvalEx库计算数学/逻辑表达式。

**属性：**
- `id` 唯一标识符（必需）
- `expression` 要计算的表达式（必需）
- `isStatic` 运行时评估（默认：false）
- `isCritical` 失败时中止（默认：false）

**表达式中可用的变量：**
- `playerYaw` - 玩家头部偏航旋转
- `playerPitch` - 玩家头部俯仰旋转
- `playerX`、`playerY`、`playerZ` - 玩家坐标
- `playerLastDamage` - 最后受到的伤害

**可用函数：**
- `scoreBoard(name)` - 获取计分板分数
- `context(key)` - 获取上下文值
- `now()` - 当前时间戳

表达式结果等于1时返回true。

**示例：**
```
/rpgitem condition add myitem rpgitems:evalcondition id:low-damage expression:"playerLastDamage < 5"
```

## 计分板条件

### 计分板条件（ScoreboardCondition）

检查玩家的计分板分数、标签和队伍成员身份。

**属性：**
- `id` 唯一标识符（必需）
- `score` 分数选择器：`目标名:最小值,最大值 另一个目标:最小值,最大值`
- `tag` 标签选择器：`必须有的标签,!不能有的标签`（空字符串=无标签，`!`=任意标签）
- `team` 队伍选择器：`必须在的队伍,!不能在的队伍`（空字符串=无队伍，`!`=任意队伍）
- `isStatic` 运行时评估（默认：false）
- `isCritical` 失败时中止（默认：false）

当存在多个选择器时，它们之间进行AND运算。

**示例：**
```
/rpgitem condition add myitem rpgitems:scoreboardcondition id:has-kills score:kills:10,100 tag:warrior
```

## 外部集成条件

### PlaceholderAPI条件（PlaceholderAPICondition）

比较PlaceholderAPI的值（需要PlaceholderAPI插件）。

**属性：**
- `id` 唯一标识符（必需）
- `placeholder` PlaceholderAPI占位符语法（必需）
- `operator` 比较运算符（必需）
- `value` 要比较的预期值（必需）
- `isStatic` 运行时评估（默认：false）
- `isCritical` 失败时中止（默认：false）

**可用运算符：**

**字符串比较：**
- `==` 相等
- `!=` 不相等
- `===` 相等（不区分大小写）
- `!==` 不相等（不区分大小写）

**字符串匹配：**
- `contains` / `contain` - 包含子字符串
- `!contains` / `!contain` - 不包含
- `startwith` - 以...开头
- `!startwith` - 不以...开头
- `endwith` - 以...结尾
- `!endwith` - 不以...结尾

**正则表达式：**
- `matches` / `match` - 匹配正则
- `!matches` / `!match` - 不匹配正则

**数值比较：**
- `>=` 大于等于
- `>` 大于
- `<` 小于
- `<=` 小于等于

**示例：**
```
/rpgitem condition add myitem rpgitems:placeholdercondition id:high-level placeholder:%player_level% operator:>= value:10
```

## 链式条件

### 结果条件（LastResultCondition）

检查上一个执行的技能的结果。

**属性：**
- `id` 唯一标识符（必需）
- `okResults` 可接受的结果类型集合（默认：OK）
- `failOnNoResult` 当没有上一个结果时失败（默认：true）
- `isCritical` 失败时中止（默认：false）

**可用结果类型：** `OK`、`FAIL`、`ABORT`、`COOLDOWN`、`COST`、`NOOP`

**示例：**
```
/rpgitem condition add myitem rpgitems:lastresultcondition id:prev-ok okResults:OK
```

## 条件汇总表

| 条件 | 类型 | 评估时机 | 关键属性 |
|------|------|----------|----------|
| AndCondition | 逻辑 | 静态/动态 | conditions |
| OrCondition | 逻辑 | 静态/动态 | conditions |
| XorCondition | 逻辑 | 静态/动态 | conditions, init |
| ChanceCondition | 概率 | 运行时 | chancePercentage |
| SlotCondition | 装备 | 静态 | slots |
| EquipmentCondition | 装备 | 静态 | slots, material, rpgitem |
| DamageTypeCondition | 战斗 | 运行时 | damageType |
| DurabilityCondition | 物品 | 运行时 | durabilityMin, durabilityMax |
| ItemLevelCondition | 物品 | 静态/运行时 | level, operation |
| EvalCondition | 表达式 | 运行时 | expression |
| ScoreboardCondition | 计分板 | 运行时 | score, tag, team |
| PlaceholderAPICondition | 外部 | 运行时 | placeholder, operator, value |
| LastResultCondition | 链式 | 运行时 | okResults |
