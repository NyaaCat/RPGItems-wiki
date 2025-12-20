# 技能列表

技能是RPGItems中制作道具的核心机制，你可以使用不同的技能来制作出各种各样的道具。

技能是按照顺序执行的，并且只在有对应触发时才会执行。

部分时候技能参数命令字节会超出 Minecraft 聊天栏限制，此时可以使用命令方块完成命令的输入

以下是技能的通用参数:

- `cooldown` 在下次可以使用前的冷却时间（单位：Ticks）
- `cost` 每次使用时消耗的耐久值
- `conditions` 用来触发技能的条件
- `triggers` 技能的触发
- `displayName` 技能的显示名称
- `selectors` 技能选择器，使技能仅对部分实体生效
- `powerId` 技能的唯一标识符
- `powerTags` 技能标签，用于分组
- `showCooldownWarning` 是否显示冷却警告消息（默认：true）
- `requiredContext` 技能激活所需的上下文
- `requireHurtByEntity` 当触发设置为 `HURT` 或 `HIT_TAKEN` 时，将此值设置为 `true` 可以使仅来自于生物的有源伤害能够触发技能（不包括中毒、凋零、火焰、摔落伤害等）。默认为 `true`
- `setBaseDamage` 设置为 `true` 时，后续的伤害加成技能将基于此伤害值计算。默认为 `false`

以下是用来修改一个道具技能的命令:

/rpgitem power [operation] [item] [params]

可使用的Operations:

- `add` 增加一个技能
- `remove` 移除一个指定技能
- `list` 列出所有技能
- `prop` 列出一个指定技能的详细参数或是使用额外参数修改技能
- `reorder` 调整一个技能到新的顺序位，这个功能会影响到技能的执行逻辑

示例：

```
/rpgitem power add mysword rpgitems:beam cost:1 damage:10 ttl:20 triggers:LEFT_CLICK speed:16 particle:CRIT_MAGIC length:24
```

## 范围效果（AOE）

对范围内的目标施加药水效果。

- `amplifier` 效果倍率（默认：1）
- `duration` 效果持续时间，单位Ticks（默认：15）
- `range` 范围半径，单位方块（默认：5）
- `selfapplication` 效果是否会应用给自己（默认：true）
- `type` 效果类型，详见：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/potion/PotionEffectType.html
- `display` 自定义显示文本
- `count` 最大目标数量，-1为无限制（默认：-1）
- `angle` 从自身方向的角度范围，0至180（默认：180）
- `target` 目标过滤：`ALL`、`MOBS`或`PLAYERS`（默认：ALL）

## 范围命令（AOECommand）

对范围内的目标执行命令。

- `type` 目标种类：`entity`、`player`或`mobs`（默认：entity）
- `r` 最大目标范围，单位方块（默认：10）
- `rm` 最小范围，单位方块（默认：0）
- `facing` 角度范围，可以由0至180度（默认：30）
- `c` 目标数量（默认：100）
- `mustsee` 在你与目标之间是否不得有方块阻拦（默认：false）
- `command` 执行的命令
- `permission` 执行命令所需要的权限节点
- `selfapplication` 是否包含自身（默认：false）

## 范围伤害（AOEDamage）

对范围内的目标造成伤害。

- `range` 目标范围，单位方块（默认：10）
- `minrange` 最小范围（默认：0）
- `angle` 角度范围，可以由0至180度（默认：180）
- `count` 会被攻击的目标数量（默认：100）
- `includePlayers` 是否包含玩家（默认：false）
- `selfapplication` 是否包含自身（默认：false）
- `mustsee` 在你与目标之间是否不得有方块阻拦（默认：false）
- `damage` 造成的伤害量，独立于道具属性的伤害（默认：0）
- `delay` 造成伤害前的延时，单位Ticks（默认：0）
- `selectAfterDelay` 当延时不为0时，将此参数设置为`false`可以使得目标脱离范围时仍然受到伤害（默认：false）
- `suppressMelee` 抑制近战伤害触发（默认：false）
- `firingLocation` 技能生效中心位置：`SELF`或`TARGET`（默认：SELF）
- `firingRange` 技能生效中心最远距离（默认：64）
- `castOff` 当技能生效位置是`TARGET`且延时不为0时，设置此参数为`true`可以在延时前设定范围中心（默认：false）

## 空中处决（Airborne）

在飞行或空中时增加伤害。

- `percentage` 伤害增加的百分比量（默认：50）
- `cap` 可造成伤害的最大值（默认：300.0）
- `setBaseDamage` 使用动态伤害作为后续技能的基础（默认：false）

## 箭矢（Arrow）

发射一支箭矢。这是基础的箭矢技能 - 如需更高级的弹射物，请使用`Projectile`。

- `cooldown` 冷却时间，单位Ticks（默认：0）
- `cost` 耐久消耗（默认：0）
- `setShooter` 是否将玩家设置为箭矢的射手（默认：false）

## 配件（Attachments）

用于触发背包中其他RPG道具的技能。

- `allowInvSlots` 可用的背包格位置，-1为禁用
- `allowItems` 可用的RPGItem名称
- `limit` 配件数量限制

## 吸引（Attract）

吸引或推开实体。

- `radius` 范围半径，单位方块（默认：5）
- `maxSpeed` 吸引/推开的最大速度（默认：0.4）
- `duration` 持续时间，单位Ticks（默认：5）
- `attractingTickCost` 使用技能时每Tick所消耗的耐久（默认：0）
- `attractingEntityTickCost` 使用时每个实体每Tick所消耗的耐久（默认：0）
- `attractPlayer` 是否会吸引玩家（默认：false）
- `firingLocation` 技能生效中心位置：`SELF`或`TARGET`（默认：SELF）
- `firingRange` 技能生效中心最远距离（默认：64）

## 粒子束（Beam）

发射由粒子组成的光束。这是RPGItems中最为复杂也是最为强大的技能。

### 基础属性
- `length` 粒子束长度，单位方块（默认：10）
- `ttl` 粒子存在时间，单位Ticks（默认：100）
- `particle` 粒子种类，详见：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Particle.html（默认：LAVA）
- `mode` 模式：`BEAM`（1Tick内运行）或`PROJECTILE`（在多Ticks内运行）（默认：BEAM）
- `damage` 粒子束的伤害量（默认：20）
- `speed` 粒子束飞行速度，单位方块/秒（默认：20）

### 穿透与碰撞
- `pierce` 可以穿透的目标数量（默认：0）
- `ignoreWall` 粒子束飞行途中是否无视完整方块（默认：false）
- `bounce` 粒子束会在完整方块上弹射的次数，需要将`ignoreWall`设置为`false`（默认：0）
- `hitSelfWhenBounced` 粒子束弹射后是否能击中自己（默认：false）

### 粒子设置
- `offsetX` 粒子生成X轴偏移（默认：0）
- `offsetY` 粒子生成Y轴偏移（默认：0）
- `offsetZ` 粒子生成Z轴偏移（默认：0）
- `particleSpeed` 粒子速度参数（默认：0）
- `particleDensity` 在一个方块空间内生成的粒子量（默认：2）
- `extraData` 红石粒子`DUST`的颜色参数，格式为`r,g,b,size`。示例：`extraData:51,204,255,0.35`为青色

### 散射与连发
- `cone` 从自身发射粒子束的离散角度（默认：0）
- `beamAmount` 每波发射的粒子束数量（默认：1）
- `burstCount` 单次触发时发射的粒子束波数（默认：1）
- `burstInterval` 下一波发射前的等待时间，单位Ticks（默认：10）

### 追踪
- `homing` 追踪半径，0为禁用（默认：0）
- `homingAngle` 从自身到目标之间的追踪角度（默认：30）
- `homingRange` 从自身到目标之间的追踪方块距离（默认：50）
- `homingMode` 追踪模式：
  - `ONE_TARGET` 追踪单一目标
  - `MULTI_TARGET` 在上一次击中后更换目标
  - `MOUSE_TRACK` 追踪距离光标指向位置最近的目标
- `homingTarget` 追踪目标类型：`MOBS`、`ENTITY`或`PLAYER`（默认：MOBS）
- `ticksBeforeHoming` 追踪前的等待时间，单位Ticks（默认：0）

### 物理
- `gravity` 粒子束重力，可以设置为负数（粒子束可以向上弯曲）（默认：0）
- `speedBias` 使用表达式来控制粒子束速度。变量：`x`（距离）、`t`（时间）。示例：`4+x*10*t`

### 高级
- `behavior` 行为修改器列表。可用行为：
  - `PLAIN` - 标准直线粒子束
  - `DNA` - 双螺旋图案
  - `CIRCLE` - 圆形旋转图案
  - `LEGACY_HOMING` - 旧版追踪算法
  - `RAINBOW_COLOR` - 彩虹颜色循环效果
  - `CONED` - 锥形扩散图案
  - `FLAT` - 平面水平粒子束
  - `UNIFORMED` - 均匀粒子分布
  - `CAST_LOCATION_ROTATED` - 围绕施法点旋转
- `behaviorParam` 行为的JSON参数（默认："{}"）
- `initialRotation` 初始旋转角度（默认：0）
- `effectOnly` 仅作为视觉效果，不会触发任何HIT事件（默认：false）
- `suppressMelee` 抑制击中时的近战伤害（默认：false）

### 发射位置
- `firingLocation` 粒子束发射位置：`SELF`或`TARGET`（默认：SELF）
- `firingRange` 粒子束发射位置的最远距离（默认：64）
- `firingR` 极坐标R参数
- `firingTheta` 极坐标θ参数
- `firingPhi` 极坐标ρ参数
- `castOff` 在延迟前锁定目标位置（默认：false）

## 取消箭矢（CancelBowArrow）

取消弓的箭矢，不会造成任何箭矢的消耗也不会发射任何箭矢。常用于使用了其他技能的弓道具。

- `cancelArrow` 是否取消箭矢发射（默认：true）

## 勇气绝杀（Charge）

奔跑时增加伤害。

- `percentage` 基础伤害增加的百分比量（默认：30）
- `speedPercentage` 根据速度增加伤害增加的百分比量（默认：20）
- `cap` 最大伤害上限（默认：300）
- `setBaseDamage` 使用动态伤害作为基础（默认：false）

## 命令（Command）

使用时执行命令。

- `command` 执行的命令。当命令中包含空格时请使用反引号。示例：`` command:`minecraft:give {player} stone` ``
- `display` 在道具描述中显示的展示名（默认："Runs command"）
- `permission` 执行命令所需要的临时权限节点。特殊值：
  - `console` 以控制台身份执行
  - `*` 以OP权限执行
- `cooldown` 冷却时间，单位Ticks（默认：0）
- `cost` 耐久消耗（默认：0）

可在命令中使用的变量：

- `{player}` 玩家名
- `{player.x}` 玩家的X轴坐标
- `{player.y}` 玩家的Y轴坐标
- `{player.z}` 玩家的Z轴坐标
- `{player.yaw}` 玩家的yaw坐标
- `{player.pitch}` 玩家的pitch
- `{yaw}` 玩家yaw（简写格式）
- `{pitch}` 玩家pitch（简写格式）

如果安装了PlaceholderAPI，命令中的占位符将在执行前被解析。

## 击中命令（CommandHit）

在击中时执行命令。

- `command` 执行的命令
- `permission` 执行命令需要的权限节点
- `minDamage` 触发技能所需要的最低伤害（默认：0）

继承Command的所有变量，另外：

- `{entity}` 目标名
- `{entity.uuid}` 目标的UUID
- `{entity.x}` 目标的X轴坐标
- `{entity.y}` 目标的Y轴坐标
- `{entity.z}` 目标的Z轴坐标
- `{entity.yaw}` 目标的yaw坐标
- `{entity.pitch}` 目标的pitch
- `{damage}` 造成的伤害

## 消耗（Consume）

使用时消耗道具。

## 击中消耗（ConsumeHit）

击中时消耗道具。

## 暴击伤害（CriticalHit）

造成暴击伤害。

- `chance` 暴击概率百分比（默认：20）
- `backstabChance` 背刺概率百分比（默认：0）
- `factor` 暴击伤害倍率（默认：1.5）
- `backstabFactor` 背刺伤害倍率（默认：1.5）
- `setBaseDamage` 使用动态伤害作为基础（默认：false）

## 冲刺（Dash）

向指定方向冲刺，给玩家施加速度。

- `direction` 冲刺方向（默认：FORWARD）：
  - `FORWARD` - 玩家面朝方向
  - `BACKWARD` - 面朝方向的反向
  - `LEFT` - 相对面朝方向的左侧
  - `RIGHT` - 相对面朝方向的右侧
  - `UP` - 垂直向上
  - `DOWN` - 垂直向下
  - `RANDOM` - 随机三维方向
  - `RANDOM_HORIZONTAL` - 随机水平方向
  - `RANDOM_VERTICAL` - 随机垂直方向
  - `NORTH` - 世界北方（-Z）
  - `SOUTH` - 世界南方（+Z）
  - `EAST` - 世界东方（+X）
  - `WEST` - 世界西方（-X）
- `speed` 冲刺速度倍率（默认：1）

## 即死命令（DeathCommand）

以一定概率即死目标并执行命令。

- `chance` 执行概率，1/N（默认：20）
- `command` 执行的命令
- `desc` 道具描述中的说明文字
- `count` 命令执行次数（默认：1）
- `permission` 执行命令所需要的权限节点

## 弹反（Deflect）

将飞来的弹射物弹反回去。

- `chance` 被动弹反的百分比概率（默认：50）
- `cooldownpassive` 被动弹反的冷却时间，单位Ticks（默认：0）
- `duration` 主动弹反持续时间，单位Ticks（默认：50）
- `facing` 最大有效角度，从0至180度（默认：30）
- `deflectCost` 每次弹反的耐久消耗（默认：0）

## 延时命令（DelayedCommand）

在延时后执行命令。

- `delay` 延时时间，单位Ticks（默认：20）
- `command` 执行的命令
- `display` 显示在道具描述中的展示名
- `permission` 执行命令需要的权限节点
- `cmdInPlace` 在原位置执行命令（默认：false）

## 空技能（Dummy）

空技能虽然不会做任何事情但是它提供了许多可以帮助您控制技能运行逻辑的通用选项。

- `checkDurabilityBound` 触发前检查物品的耐久上下界（默认：true）
- `costByEnchantment` 依赖附魔来计算耐久消耗（默认：false）
- `doEnchReduceCost` 附魔可以降低道具耐久消耗（默认：false）
- `enchCostPercentage` 每一附魔等级可以减少耐久消耗的几率（默认：6）
- `enchantmentType` 附魔名字（默认："unbreaking"）
- `costByDamage` 根据伤害消耗耐久值（默认：false）
- `cooldownKey` 当有多个空技能时，设定一个唯一的字符串来让冷却时间互相独立（默认："dummy"）
- `successResult` 在空技能成功执行后的动作：`OK`、`ABORT`（默认：OK）
- `costResult` 在耐久消耗后执行的动作：`COST`、`ABORT`、`OK`（默认：COST）
- `cooldownResult` 冷却后执行的动作：`COOLDOWN`、`ABORT`、`OK`（默认：COOLDOWN）
- `showCDWarning` 是否显示冷却信息（默认：true）
- `globalCooldown` 设置全体玩家共享冷却时间（默认：false）
- `display` 显示文本

**使用空技能来禁用冷却信息的显示并统一控制技能冷却时间与耐久消耗**

技能是需要触发执行的，比如`RIGHT_CLICK`或是`LEFT_CLICK`然后根据顺序来一个一个的执行。如果你的道具设置的技能如同以下这样：

1. PowerProjectile, trigger `LEFT_CLICK`
2. PowerBeam, trigger `RIGHT_CLICK`
3. PowerSound, trigger `RIGHT_CLICK`
4. PowerParticle, trigger `LEFT_CLICK`

当你点击鼠标左键，只有1和4会被触发执行，并且是在同一个Tick中执行的。

如果你想要在每次点击鼠标左键时会消耗1耐久，冷却时间20Ticks并且不希望显示冷却消息：
1. 在第一位添加空技能：`cooldown:20 cooldownKey:left cooldownResult:ABORT cost:1 showCDWarning:false triggers:LEFT_CLICK`
2. 设置其他技能的`cost`和`cooldown`为0

## 经济（Economy）

消耗或给予使用者金钱（需要Vault插件）。

- `coolDown` 冷却时间，单位Ticks（默认：0）
- `amountToPlayer` 金额，正数为给予，负数为消耗
- `showFailMessage` 是否显示失败信息
- `abortOnFailure` 是否在触发失败时中止（默认：true）

## 附魔攻击（EnchantedHit）

让附魔来增强伤害量。

- `mode` 计算模式：`ADDITION`（加法）或`MULTIPLICATION`（乘法）（默认：ADDITION）
- `amountPerLevel` 每一附魔等级增加百分比，1 = 100%（默认：1）
- `enchantmentType` 附魔类型，详见：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/enchantments/Enchantment.html（默认：POWER）
- `setBaseDamage` 使用动态伤害作为基础（默认：false）
- `display` 显示文本

## 动态伤害（EvalDamage）

使用数学表达式来计算伤害。使用EvalEx库进行表达式求值。

- `expression` 用于计算实体伤害的表达式（必需）
- `playerExpression` 当目标为玩家时使用的单独表达式
- `setBaseDamage` 将结果作为基础伤害（默认：false）
- `display` 显示文本

### 可用变量

- `damage` - 基础伤害值
- `finalDamage` - 最终计算伤害
- `attackCooldown` - 玩家攻击冷却（0.0到1.0）
- `isDamageByProjectile` - 如果是弹射物伤害为1，否则为0
- `damagerTicksLived` - 攻击者存在的时间（Ticks）
- `distance` - 玩家与目标之间的距离
- `playerYaw`、`playerPitch` - 玩家旋转角度
- `playerX`、`playerY`、`playerZ` - 玩家坐标
- `entityType` - 目标实体类型名称（如"ZOMBIE"）
- `entityYaw`、`entityPitch` - 实体旋转角度
- `entityX`、`entityY`、`entityZ` - 实体坐标
- `entityLastDamage` - 实体上次受到的伤害
- `cause` - 伤害原因枚举名称（如"ENTITY_ATTACK"）

### 可用函数

- `scoreBoard(name)` - 获取玩家的计分板分数
- `context(key)` - 从技能上下文获取值
- `now()` - 当前时间戳（毫秒）

### PlaceholderAPI支持

如果安装了PlaceholderAPI，表达式中的占位符会在求值前被解析。
在playerExpression中使用`target:`前缀来获取目标玩家的占位符。

示例：
```
/rpgitem power add mysword rpgitems:evaldamage expression:"damage * (1 + distance / 10)"
```

## 爆炸（Explosion）

在使用时产生一个爆炸。

- `distance` 发生爆炸的最远距离（默认：20）
- `chance` 发生爆炸的百分比几率（默认：20）
- `explosionPower` 爆炸强度/半径（默认：4.0）

## 火焰（Fire）

创建一条火焰轨迹。

- `distance` 火焰轨迹长度，单位方块（默认：15）
- `burnduration` 燃烧持续时间，单位Ticks（默认：40）

## 火焰弹（Fireball）

已弃用。建议使用弹射物技能`Projectile`。

## 火焰附加（Flame）

点燃被击中的目标。

- `burntime` 燃烧持续时间，单位Ticks（默认：20）

## 食物（Food）

回复饥饿值。

- `foodpoints` 回复的饥饿值量（必填）

## 力场护盾（ForceField）

生成一个护盾来保护自己。

- `radius` 护盾半径，单位方块（默认：5）
- `height` 护盾高度，单位方块（默认：30）
- `base` 护盾底部偏移，可为负数（默认：-15）
- `ttl` 护盾持续时间，单位Ticks（必填，默认：100）
- `wallMaterial` 墙壁材质（默认：WHITE_WOOL）
- `barrierMaterial` 屏障材质（默认：BARRIER）

## 鬼手（Glove）

抓住实体并将它扔出去。

- `maxDistance` 最大抓取距离（默认：5）
- `maxTicks` 最大抓取时间，单位Ticks（默认：200）
- `throwSpeed` 投掷速度（默认：0.0）

## 枪斗术（GunFu）

使弹射物追踪目标。

- `distance` 最大索敌距离（默认：20）
- `viewAngle` 最大索敌角度（默认：30）
- `initVelFactor` 初速度倍率（默认：0.5）
- `velFactor` 每Tick速度调整倍率（默认：0.05）
- `forceFactor` 追踪力度倍率（默认：1.5）
- `maxTicks` 最大追踪时间（默认：200）
- `delay` 追踪开始前的延时（默认：0）

## 爆头（Headshot）

弹射物击中目标头部造成额外伤害。

- `factor` 伤害倍率（默认：1.5）
- `particleEnemy` 是否在命中敌人时显示粒子（默认：true）
- `soundSelf` 是否为射手播放音效（默认：true）
- `soundEnemy` 是否为目标播放音效（默认：false）
- `setBaseDamage` 使用动态伤害作为基础（默认：false）

## 冰冻（Ice）

发射冰块并形成一个冰牢。

## 击飞（Knockup）

将目标击飞。

- `chance` 击飞概率，1/N（默认：20）
- `knockUpPower` 击飞力度（默认：2）

## 生命窃取（LifeSteal）

根据击中伤害吸取生命值。

- `chance` 吸取概率，1/N（默认：20）
- `factor` 吸取倍率（默认：1）

## 唤雷（Lightning）

生成闪电造成雷击。

- `chance` 雷击概率，1/N（默认：20）

## 修复（Mending）

增强的经验修复行为。

- `repairFactor` 经验到耐久的转换比率（默认：2）
- `requireEnchantment` 是否需要经验修补附魔（默认：false）

## 乘骑（Mount）

可以骑在生物上。

- `maxDistance` 生物最远距离（默认：5）
- `maxTicks` 最大乘骑时间，单位Ticks（默认：200）

## MythicMobs技能施放（MythicSkillCast）

施放MythicMobs技能（需要MythicMobs插件）。

- `skill` 要施放的技能名称
- `suppressArrow` 施放时取消箭矢（默认：false）
- `applyForce` 力度值（默认："NaN"）

## 取消无敌时间（NoImmutableTick）

减少或移除伤害无敌时间。

- `immuneTime` 无敌时间，单位Ticks（默认：1）

## 粒子屏障（ParticleBarrier）

将受到的攻击转化为粒子屏障能量。

- `barrierHealth` 屏障生命值池（默认：40）
- `energyPerBarrier` 每个屏障获得的能量（默认：40）
- `energyDecay` 每秒能量衰减（默认：1.5）
- `energyPerLevel` 每级效果所需能量（默认：10）
- `projected` 是否为投射屏障（默认：false）
- `effect` 屏障提供的药水效果（默认：STRENGTH）

## 粒子效果（Particle）

在使用或击中时生成粒子。

- `particle` 粒子类型
- `effect` 效果类型（旧版）
- `particleCount` 粒子数量（默认：1）
- `offsetX` X轴扩散（默认：0）
- `offsetY` Y轴扩散（默认：0）
- `offsetZ` Z轴扩散（默认：0）
- `extra` 额外数据/速度（默认：1）
- `force` 强制为所有玩家渲染（默认：false）
- `material` 方块粒子的材质
- `dustColor` 尘埃粒子颜色，整数值（默认：0）
- `dustSize` 尘埃粒子大小（默认：0）
- `playLocation` 生成位置：`SELF`、`HIT_LOCATION`、`TARGET`、`ENTITY`（默认：HIT_LOCATION）
- `delay` 延时，单位Ticks（默认：0）
- `firingRange` TARGET位置的范围（默认：20）

## 佩戴粒子效果（ParticleTick）

在持有或穿戴时持续生成粒子。

所有粒子效果（Particle）的参数，另加：
- `interval` 生成间隔，单位Ticks（默认：15）

## 效果命中（PotionHit）

在击中时施加药水效果。

- `type` 药水效果类型（必填）
- `duration` 效果持续时间，单位Ticks（默认：20）
- `amplifier` 效果等级（默认：1）
- `chance` 触发概率，1/N（默认：20）
- `summingUp` 与其他PotionHit技能叠加等级（默认：false）

## 自身状态效果（PotionSelf）

对自己施加药水效果。

- `type` 药水效果类型
- `duration` 效果持续时间，单位Ticks
- `amplifier` 效果等级
- `clear` 清除所有效果（默认：false）

## 佩戴状态效果（PotionTick）

在持有或穿戴时持续施加药水效果。

- `type` 药水效果类型
- `amplifier` 效果等级
- `interval` 施加间隔，单位Ticks

## 弹射物（Projectile）

发射弹射物，有多种选项。

### 基础设置
- `projectileType` 弹射物类型（必填）：
  - `arrow`箭矢、`snowball`雪球、`fireball`火焰弹、`smallfireball`小火焰球
  - `llamaspit`羊驼口水、`shulkerbullet`潜影弹、`dragonfireball`龙息弹
  - `trident`三叉戟、`skull`凋零头颅、`egg`鸡蛋
- `speed` 弹射物飞行速度（默认：1）
- `gravity` 弹射物是否受重力影响（默认：true）

### 散射与连发
- `isCone` 弹射物是否离散发射（默认：false）
- `range` 发射的离散角度（默认：15）
- `amount` 每一波发射的弹射物数量（默认：5）
- `burstCount` 发射波数（默认：1）
- `burstInterval` 下一波发射前的间隔时间，单位Ticks（默认：1）

### 弹射物行为
- `setFireballDirection` 火焰弹是否跟随瞄准方向（默认：false）
- `yield` 爆炸半径（默认：0）
- `isIncendiary` 是否会点燃（默认：false）
- `pierceLevel` 穿透实体等级（默认：0）
- `suppressArrow` 取消弓箭发射（默认：false）
- `applyForce` 是否应用弓的拉伸力度（默认：false）

### 鸡蛋设置
- `eggShouldHatch` 鸡蛋是否能孵化（默认：true）
- `eggHatchEntity` 孵化的实体类型（默认："CHICKEN"）
- `eggHatchNumber` 孵化数量，-1为随机（默认：-1）

### 火焰弹设置
- `fireballItem` 火焰弹显示的物品（默认："FIRE_CHARGE"）

### 发射位置
- `firingLocation` 发射位置：`SELF`或`TARGET`（默认：SELF）
- `firingRange` TARGET位置的范围（默认：64）
- `firingR` 极坐标R参数
- `firingTheta` 极坐标θ参数
- `firingPhi` 极坐标ρ参数
- `initialRotation` 初始旋转角度（默认：0）
- `castOff` 在延迟前锁定目标（默认：false）

## 南瓜头（Pumpkin）

攻击僵尸或者骷髅时给他们戴上南瓜头。

- `chance` 触发几率，1/N（默认：20）
- `drop` 南瓜头掉落几率（默认：0）

## 彩虹（Rainbow）

随机发射各种颜色的羊毛。

- `count` 发射数量
- `isFire` 是否发射火焰（默认：false）

## 真实伤害（RealDamage）

造成无视护甲的真实伤害。

- `realDamage` 真实伤害量
- `minDamage` 触发的最低伤害（默认：0）

## 修理（Repair）

消耗材料来回复或消耗道具耐久。

- `durability` 每一个修复材料回复（或消耗）的耐久量
- `material` 修复材料，可使用`HAND`来使用主手物品
- `display` 在道具描述中展示的信息
- `isSneak` 是否在潜行状态时才能触发
- `mode` 模式：`DEFAULT`、`ALLOW_OVER`、`ALWAYS`（默认：DEFAULT）
- `allowBreak` 是否允许道具损坏（默认：true）
- `abortOnSuccess` 是否在修理成功后中止后续技能（默认：false）
- `abortOnFailure` 是否在修理失败后中止后续技能（默认：false）
- `customMessage` 修理时在聊天栏展示的信息
- `amount` 每次触发时消耗的最大材料量
- `showFailMsg` 是否显示修理失败后的信息（默认：true）

## 拯救（Rescue）

受到过量伤害或者血量即将低于指定值时拯救玩家。

- `healthTrigger` 生命触发值（默认：4）
- `damageTrigger` 伤害触发值（默认：1024）
- `useBed` 是否传送到床上（默认：true）
- `inPlace` 是否原地拯救（默认：false）

## 裂地冲击（Rumble）

在地面上生成一个冲击波将目标击飞。

- `power` 击飞力度（默认：2）
- `distance` 冲击波最大距离（默认：15）
- `damage` 冲击波伤害（默认：0）

## 计分板（Scoreboard）

修改玩家的计分板标签或数值。

- `objective` 计分项名称
- `scoreOperation` 分数操作：`NO_OP`、`ADD_SCORE`、`SET_SCORE`、`RESET_SCORE`（默认：NO_OP）
- `value` 分数值（默认：0）
- `tag` 添加/移除的标签：`ADD,!REMOVE`格式
- `team` 加入/离开的队伍：`JOIN,!LEAVE`格式
- `delay` 延时，单位Ticks（默认：20）
- `reverseTagAfterDelay` 是否在延时后撤销标签变更（默认：false）
- `abortOnSuccess` 是否在成功后中止技能链（默认：false）

## 潜影弹（ShulkerBullet）

发射追踪目标的潜影弹。

- `range` 追踪范围

## 钩爪（SkyHook）

发射钩爪用于钩住指定方块。

- `railMaterial` 允许被钩住的材质（默认：GLASS）
- `hookDistance` 钩爪最大距离（默认：10）
- `hookingTickCost` 钩住时每Tick耐久消耗（默认：0）

## 声音（Sound）

在使用或击中实体时播放声音。

- `sound` 声音名称（Minecraft声音ID）
- `volume` 音量（默认：1.0）
- `pitch` 音高（默认：1.0）
- `delay` 延时，单位Ticks（默认：0）
- `playLocation` 播放位置：`SELF`、`HIT_LOCATION`、`TARGET`（默认：HIT_LOCATION）
- `firingRange` TARGET位置的范围（默认：20）
- `display` 显示文本（默认："Plays sound"）

## 定身（Stuck）

使目标无法移动或传送。

- `chance` 定身概率，1/N（默认：3）
- `duration` 持续时间，单位Ticks（默认：100）
- `range` 范围距离，单位方块（默认：10）
- `facing` 角度范围（默认：30）
- `costAoe` 范围定身耐久消耗（默认：0）
- `costPerEntity` 每一个实体的耐久消耗（默认：0）

## TNT大炮（TNTCannon）

发射点燃的TNT。

## 传送（Teleport）

传送到你所看方向的一段距离。

- `distance` 传送距离（默认：5）
- `targetMode` 寻找传送点的模式：
  - `DEFAULT` 方块迭代
  - `RAY_TRACING` 射线追踪到表面
  - `RAY_TRACING_EXACT` 精确射线追踪
  - `RAY_TRACING_SWEEP` 射线追踪带玩家碰撞
  - `RAY_TRACING_EXACT_SWEEP` 精确带碰撞
- `particle` 粒子效果（默认："PORTAL"）
- `sound` 音效（默认："entity.enderman.teleport"）

## 投掷（Throw）

生成并投掷实体。

- `entityName` 实体类型（必填）
- `speed` 投掷速度（必填）
- `display` 道具描述中的展示名（必填）
- `entityData` 额外实体NBT数据
- `isPersistent` 实体是否写入世界存档（默认：false）

## 药箭（TippedArrow）

已弃用。请使用弹射物`Projectile`与命中效果`PotionHit`技能。

## 火把（Torch）

投掷一些火把来照亮一片区域。

## 传送信标（Translocator）

投掷一个传送信标并传送到信标所在地。

- `speed` 信标投掷速度
- `setupCost` 设立信标的耐久消耗
- `tpCost` 传送耐久消耗

## 亡灵效果（Undead）

对亡灵目标的特殊效果。
