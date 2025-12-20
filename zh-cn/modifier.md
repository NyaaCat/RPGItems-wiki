# 修改器

修改器是可以应用于特定道具实例或玩家的动态属性修改系统。与定义在道具模板上的技能不同，修改器可以在运行时改变单个道具副本或特定玩家的技能属性。

## 修改器工作原理

修改器针对特定的技能属性并在运行时修改其值。它们可以应用于：
- **Hand（手持物品）**：当前主手持有的道具
- **Player（玩家）**：存储在玩家的持久数据中（影响其所有道具）

修改器按优先级顺序处理，数字越小越先执行。

## 命令

| 命令 | 描述 |
|------|------|
| `/rpgitem modifier add <hand/玩家名> <修改器> [属性]` | 添加修改器 |
| `/rpgitem modifier prop <hand/玩家名> <id> [属性]` | 查看或修改属性 |
| `/rpgitem modifier remove <hand/玩家名> <id>` | 移除修改器 |
| `/rpgitem modifier list` | 列出所有可用的修改器 |

### 目标选项

- `hand` - 将修改器应用于主手中的道具
- `<玩家名>` - 将修改器应用于特定玩家的持久数据

## 通用属性

所有修改器共享这些基础属性：

| 属性 | 必需 | 描述 |
|------|------|------|
| `id` | 是 | 此修改器实例的唯一标识符 |
| `targetItem` | 是 | 目标RPGItem名称（留空表示所有） |
| `targetPower` | 是 | 目标技能的命名空间键（例如 `rpgitems:beam`） |
| `targetProperty` | 是 | 要修改的属性名称（例如 `damage`） |
| `priority` | 是 | 执行顺序（数字越小越先执行） |

## 可用的修改器

### PlusModifier（加法修改器）

为目标属性添加一个值。

```
/rpgitem modifier add hand rpgitems:plusmodifier id:damage-boost targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 value:10
```

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | double | 要添加的值 |

**效果**：`结果 = 原始值 + value`

### MulModifier（乘法修改器）

将目标属性乘以一个值。

```
/rpgitem modifier add hand rpgitems:mulmodifier id:damage-double targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:2 value:2.0
```

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | double | 乘数值 |

**效果**：`结果 = 原始值 * value`

### DivModifier（除法修改器）

将目标属性除以一个值。

```
/rpgitem modifier add hand rpgitems:divmodifier id:damage-half targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:3 value:2.0
```

| 属性 | 类型 | 描述 |
|------|------|------|
| `value` | double | 除数值 |

**效果**：`结果 = 原始值 / value`

### EvalModifier（表达式修改器）

评估数学表达式来计算新值。

```
/rpgitem modifier add hand rpgitems:evalmodifier id:dynamic-damage targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 expression:"value * (1 + durability / 100)"
```

| 属性 | 类型 | 描述 |
|------|------|------|
| `expression` | string | 要评估的数学表达式 |

**表达式中可用的变量：**

| 变量 | 描述 |
|------|------|
| `value` | 当前属性值 |
| `durability` | 道具当前耐久度 |
| `random` | 0到1之间的随机数 |
| `time` | 当前服务器时间（刻） |

**效果**：`结果 = 表达式计算结果`

## 示例

### 示例 1：增加光束伤害50%

```
/rpgitem modifier add hand rpgitems:mulmodifier id:beam-boost targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 value:1.5
```

### 示例 2：添加固定冷却减少

```
/rpgitem modifier add hand rpgitems:plusmodifier id:cooldown-reduce targetItem:mysword targetPower:rpgitems:beam targetProperty:cooldown priority:1 value:-10
```

### 示例 3：基于耐久度的动态伤害

```
/rpgitem modifier add hand rpgitems:evalmodifier id:durability-scaling targetItem:mysword targetPower:rpgitems:beam targetProperty:damage priority:1 expression:"value * (durability / 100)"
```

### 示例 4：应用于特定玩家

```
/rpgitem modifier add Notch rpgitems:mulmodifier id:vip-boost targetItem: targetPower: targetProperty:damage priority:1 value:2.0
```

这将给玩家"Notch"在所有道具的所有技能上双倍伤害。

## 优先级和执行顺序

修改器按优先级顺序应用（数字小的先执行）。当串联多个修改器时这很重要：

```
# 优先级1：首先添加10点伤害
/rpgitem modifier add hand rpgitems:plusmodifier id:base-boost ... priority:1 value:10

# 优先级2：然后乘以2
/rpgitem modifier add hand rpgitems:mulmodifier id:multiply ... priority:2 value:2.0
```

假设原始伤害为20：
- 优先级1执行后：20 + 10 = 30
- 优先级2执行后：30 * 2 = 60

## 注意事项

- 应用于 `hand` 的修改器存储在道具的NBT数据中
- 应用于玩家的修改器存储在玩家持久数据中
- 对于 `targetItem`、`targetPower` 或 `targetProperty` 使用空字符串可匹配所有
- 修改器是实验性功能，未来版本可能会有变化
