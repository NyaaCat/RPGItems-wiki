# 触发列表

触发用于激活道具的技能。触发器定义了技能何时被激活。

## 通用触发器属性

所有触发器都有一个`priority`属性，用于确定多个触发器同时触发时的执行顺序。

部分触发器支持额外的过滤属性：

- `minDamage` 伤害类触发器的最小伤害阈值（HIT、HIT_GLOBAL、HIT_TAKEN、HURT、DYING）
- `maxDamage` 伤害类触发器的最大伤害阈值
- `minForce` BOW_SHOOT触发器的最小弓力
- `maxForce` BOW_SHOOT触发器的最大弓力

## 基础命令

```
/rpgitem power add [道具名] [技能名] trigger:[触发器名]
```

示例：
```
/rpgitem power add mysword rpgitems:command trigger:RIGHT_CLICK command:"say Hello"
```

多触发器示例：
```
/rpgitem power add myitem rpgitems:sound trigger:RIGHT_CLICK,SNEAK sound:ENTITY_ENDERMAN_TELEPORT
```

## 触发器分类

### 交互触发器

#### RIGHT_CLICK
在点击鼠标右键时触发。

#### LEFT_CLICK
在点击鼠标左键时触发。

#### OFFHAND_CLICK
在副手持有道具时点击触发（按F键）。

#### SNEAK
在开始潜行时触发（按下Shift键）。

#### SNEAKING
在潜行移动时持续触发。

#### SPRINT
在开始奔跑时触发。

#### JUMP
在跳跃时触发。

#### SWIM
在游泳时触发。

### 战斗触发器

#### HIT
在击中实体时触发，仅触发主手中的道具。

**提供的上下文：**
- `target` - 被击中的实体
- `damage` - 造成的伤害值

#### HIT_GLOBAL
在击中实体时触发，会触发除主手外所有在背包中的道具。

**提供的上下文：**
- `target` - 被击中的实体
- `damage` - 造成的伤害值

#### HIT_TAKEN
在你受击时触发，在收到伤害**前**产生计算。

**提供的上下文：**
- `damage` - 即将受到的伤害值
- `damager` - 攻击者实体（如果存在）

#### HURT
在你受击时触发，在收到伤害**后**产生计算。

**提供的上下文：**
- `damage` - 实际受到的伤害值
- `damager` - 攻击者实体（如果存在）

#### DYING
在即将死亡时触发（生命值降至0或以下）。

**提供的上下文：**
- `damage` - 致死伤害值
- `damager` - 攻击者实体（如果存在）

### 弓箭触发器

#### BOW_SHOOT
使用弓射击时触发（指释放弓弦时，且必须在背包中持有箭矢）。

**提供的上下文：**
- `projectile` - 发射的箭实体

### 弹射物触发器

#### PROJECTILE_LAUNCH
在弹射物发射时触发。

**提供的上下文：**
- `projectile` - 发射的弹射物实体

#### PROJECTILE_HIT
在弹射物击中实体或方块时触发。

**提供的上下文：**
- `projectile` - 击中的弹射物
- `target` - 被击中的实体（如果存在）
- `location` - 击中的位置

### 粒子束触发器

#### BEAM_HIT_ENTITY
当粒子束击中实体时触发。

**提供的上下文：**
- `target` - 被击中的实体
- `location` - 击中的位置

#### BEAM_HIT_BLOCK
当粒子束击中方块时触发。

**提供的上下文：**
- `location` - 击中的方块位置
- `block` - 被击中的方块

### 持续触发器

#### TICK
在道具被拿在主手中或穿戴时每刻触发。

**注意：** 每秒触发20次，用于持续效果。

#### TICK_OFFHAND
在道具被拿在副手时每刻触发。

### 物品栏触发器

#### SWAP_TO_MAINHAND
在将道具切换至主手时触发。

#### SWAP_TO_OFFHAND
在将道具切换至副手时触发。

#### PICKUP_OFFHAND
在道具拾起至副手时触发。

### 经验触发器

#### EXP_CHANGE
在获得经验值时触发。

**提供的上下文：**
- `expAmount` - 经验值变化量

### 装备触发器

#### ATTACHMENT
当装备状态发生变化时触发（穿戴/卸下装备）。

### 辅助接口

以下是技能内部使用的接口类型，而非直接使用的触发器：

#### LIVING_ENTITY
提供对生物实体的引用。用于需要目标实体的技能。

#### LOCATION
提供位置信息。用于基于位置的技能。

## 触发器参数

某些触发器可以接受额外参数：

### 通用参数

所有可触发的技能都继承这些属性：

- `triggers` - 触发此技能的触发器列表
- `conditions` - 必须满足的条件ID列表
- `requiredContext` - 需要的上下文变量名

### 触发器组合

你可以在同一个技能上使用多个触发器：

```
/rpgitem power add myitem rpgitems:command trigger:RIGHT_CLICK,SNEAK,SPRINT command:"effect give @p speed 10 1"
```

## 触发器汇总表

| 触发器 | 类型 | 描述 | 提供的上下文 |
|--------|------|------|--------------|
| RIGHT_CLICK | 交互 | 点击鼠标右键 | - |
| LEFT_CLICK | 交互 | 点击鼠标左键 | - |
| OFFHAND_CLICK | 交互 | 副手点击 | - |
| SNEAK | 交互 | 开始潜行 | - |
| SNEAKING | 交互 | 潜行移动中 | - |
| SPRINT | 交互 | 开始奔跑 | - |
| JUMP | 交互 | 跳跃 | - |
| SWIM | 交互 | 游泳 | - |
| HIT | 战斗 | 击中实体（主手） | target, damage |
| HIT_GLOBAL | 战斗 | 击中实体（全部物品栏） | target, damage |
| HIT_TAKEN | 战斗 | 受击（伤害前） | damage, damager |
| HURT | 战斗 | 受击（伤害后） | damage, damager |
| DYING | 战斗 | 即将死亡 | damage, damager |
| BOW_SHOOT | 弓箭 | 弓射击 | projectile |
| PROJECTILE_LAUNCH | 弹射物 | 发射弹射物 | projectile |
| PROJECTILE_HIT | 弹射物 | 弹射物命中 | projectile, target, location |
| BEAM_HIT_ENTITY | 粒子束 | 粒子束击中实体 | target, location |
| BEAM_HIT_BLOCK | 粒子束 | 粒子束击中方块 | location, block |
| TICK | 持续 | 每刻触发（主手/穿戴） | - |
| TICK_OFFHAND | 持续 | 每刻触发（副手） | - |
| SWAP_TO_MAINHAND | 物品栏 | 切换至主手 | - |
| SWAP_TO_OFFHAND | 物品栏 | 切换至副手 | - |
| PICKUP_OFFHAND | 物品栏 | 拾取至副手 | - |
| EXP_CHANGE | 经验 | 经验值变化 | expAmount |
| ATTACHMENT | 装备 | 装备状态变化 | - |
