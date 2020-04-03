# 技能列表

技能是RPGItems中制作道具的核心机制，你可以使用不同的技能来制作出各种各样的道具。

技能是按照顺序执行的，并且只在有对应触发时才会执行。

以下是部分技能的通用参数:

- `cooldown` 在下次可以使用前的冷却时间（单位：Ticks）。
- `cost` 每次使用时消耗的耐久值。
- `condition` 用来触发技能的条件。
- `trigger` 技能的触发。
- `displayName` 技能的备注信息。
- `requiredContext` 限制技能执行的一个值。
- `requireHurtByEntity` 当触发设置为`HURT`或`HIT_TAKEN`时，将此值设置为`true`可以使仅来自于生物的有源伤害能够触发技能（不包括中毒、凋零、火焰、摔落伤害等）。默认为`true` 
- `selectors` 技能选择器，使技能仅对部分实体生效。如需为技能指定选择器，首先使用`selector`技能定义一个选择器，并将该技能的 selectors 参数设置为 selector 技能中定义的 id 。
- `setBaseDamage` 设置为 true 时，后续的伤害加成技能将基于此伤害值计算，武器的最终伤害取叠加值。 设置为 false 时，后续的伤害加成技能将基于原伤害值计算，武器的最终伤害取最高值。 默认为 false。


以下是用来修改一个道具技能的命令:

/rpgitem power [operation] [item] [params]

可使用的Operations:

- `add` 增加一个技能
- `remove` 移除一个指定技能
- `list` 列出所有技能
- `prop` 列出一个指定技能的详细参数或是使用额外参数修改技能。
- `reorder` 调整一个技能到新的顺序位，这个功能会影响到技能的执行逻辑。

示例：

```
/rpgitem power add mysword rpgitems:beam cost:1 damage:10 ttl:20 triggers:LEFT_CLICK speed:16 particle:CRIT_MAGIC length:24
```

## 范围效果（AOE）

对范围内的目标释放药水效果。

- `amplifier` 效果倍率
- `duration` 效果持续时间
- `range` 范围半径
- `selfapplication` 效果是否会应用给自己
- `name` 效果名字，详见：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/potion/PotionEffectType.html

## 范围命令（AOECommand）

对范围内的目标执行命令

- `type` 目标种类，可以是：
  - `entity` 实体
  - `player` 玩家
  - `mobs` 怪物
- `r` 目标范围
- `rm` 最小范围
- `facing` 角度范围，可以由0至180度
- `c` 目标数量
- `mustsee` 在你与目标之间是否不得有方块阻拦
- `command` 执行的命令
- `permission` 指令命令所需要的权限节点

## 范围伤害（AOEDamage）

对范围内的目标造成伤害。

- `range` 目标范围
- `minrange` 最小范围
- `angle` 角度范围, 可以由0至180度
- `count` 会被攻击的目标数量
- `incluePlayers` (typo) 是否包含玩家
- `selfapplication` 是否包含自身
- `mustsee` 在你与目标之间是否不得有方块阻拦
- `damage` 造成的伤害量，独立于于道具属性的伤害
- `delay` 造成伤害前的延时（单位：Ticks）
- `selectAfterDelay` 当延时（delay）不为0时，将此参数设置为false可以使得目标脱离范围时仍然受到伤害
- `firingRange` 
- `firingLocation` 确定技能生效位置, 可以是自身`SELF`或是目标`TARGET`
- `castOff` 当技能生效位置`firingLocation`是目标`TARGET`且延时（delay）不为0时,设置此参数为`true`可以在延时前设定范围中心

## 空中处决（Airborne）

在飞行时增加伤害

- `cap` 可造成伤害的最大值
- `percentage` 飞行时造成伤害的增加百分比量

## 箭矢（Arrow）

发射箭矢，不推荐使用。请使用弹射物 `Projectile` 。

## 配件（Attachment）

用于触发其他道具的ATTACHMENT触发。

- `allowInvSlots` 可用的背包格位置，设置为-1为禁用
- `allowItems` 可用的RPGItem
- `limit` 配件数量

## 吸引（Attract）

吸引范围内的目标

- `radius` 范围半径
- `maxSpeed` 吸引实体的最大速度
- `duration` 吸引实体的持续时间
- `attractingTickCost` 使用技能时每Tick所消耗的耐久
- `attractingEntityTickCost` 使用时每个实体每Tick所消耗的耐久
- `attractPlayer` 是否会吸引玩家
- `firingLocation` 技能生效位置, 可以是自身`SELF`或是目标`TARGET`
- `firingRange`

## 粒子束（Beam）

发射由粒子组成的光束. 这是到目前为止RPGItems中最为复杂也是最为强大的技能。

- `length` 粒子束长度
- `ttl` 粒子存在时间（单位Ticks）
- `particle` 粒子种类：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Particle.html
- `mode` 可以设置为`BEAM`(1Tick内运行)或是`PROJECTILE`(在多Ticks内运行)
- `pierce` 可以穿透的目标数量
- `ignoreWall` 粒子束飞行途中是否无视完整方块,默认为`true`
- `damage` 粒子束的伤害量,此参数独立于道具参数的伤害设置
- `speed` 粒子束飞行速度（单位：方块/s）
- `offsetX` 粒子生成X轴范围
- `offsetY` 粒子生成Y轴范围
- `offsetZ` 粒子生成Z轴范围
- `particleSpeed` 粒子速度参数
- `particleDensity` 在一个方块空间内生成的粒子量
- `cone` 从自身发射粒子束的离散角度
- `homing` 追踪半径
- `homingAngle` 从自身到目标之间的追踪角度
- `homingRange` 从自身到目标之间的追踪方块距离
- `homingMode` 追踪模式，可以是以下类型：
  - `ONE_TARGET` 追踪单一目标
  - `MULTI_TARGET` 在上一次击中后更换目标
  - `MOUSE_TRACK` 追踪距离光标指向位置最近的目标
- `homingTarget` 追踪目标类型，可以是`MOBS` / `ENTITY` / `PLAYER`
- `ticksBeforeHoming` 追踪前的等待时间（单位：Ticks）
- `beamAmount` 单次运行时发射的粒子束数量
- `burstCount` 单次触发时发射的粒子束波数
- `burstInterval` 下一波发射前的等待时间（单位：Ticks）
- `bounce` 粒子束会在完整方块上弹射的次数,需要将`ignoreWall`设置为`false`
- `hitSelfWhenBounced` 粒子束弹射后是否能击中自己
- `gravity` 粒子束重力，可以设置为负数（粒子束可以逆而上天）
- `extraData` 红石粒子`REDSTONE`的颜色参数,格式为`r,g,b,size`。示例：extraData:\`51,204,255,0.35\` 青色粒子束。
- `speedBias` 使用表达式来控制粒子束速度。示例：`4+x*10*t` 其中x为粒子束已飞行路程，t为飞行时间。
- `behavior` 控制粒子束行为
- `initialRotation`
- `firingLocation` 发射位置，可以是自己`SELF`或是目标`TARGET`
- `effectOnly` 仅作为视觉效果，不会触发任何HIT事件
- `firingR` 极坐标参数
- `firingTheta` 极坐标参数
- `firingPhi` 极坐标参数
- `firingRange`
- `castOff`

## 取消箭矢（CancelBowArrow）

取消弓的箭矢，不会造成任何箭矢的消耗也不会发射任何箭矢。常用于使用了其他技能的弓道具。

- `cancelArrow` 是否取消箭矢发射

## 勇气绝杀（Charge）

奔跑时增加伤害。

- `percentage` 伤害增加的百分比量
- `speedPercentage` 根据速度增加伤害增加的百分比量。
- `setBaseDamage` 是否将动态伤害设为基础伤害（伤害增益可叠加）, 不然将会使用道具属性中的伤害量
- `cap` 最大伤害上限

## 命令（Command）

使用时执行命令。

- `command` 执行的命令. 当命令中包含空格时请使用\`符号。示例：command:\`minecraft:give {player} stone\`
- `display` 在道具描述中显示的展示名
- `permission` 执行命令所需要的命令节点

可在命令中使用的变量：

- `{player}` 玩家名
- `{player.x}` 玩家的X轴坐标
- `{player.y}` 玩家的Y轴坐标
- `{player.z}` 玩家的Z轴坐标
- `{player.yaw}` 玩家的yaw坐标
- `{player.pitch}` 玩家的pitch

## 击中命令（CommandHit）

在击中时执行命令。

- `command` 执行的命令
- `permission` 执行命令需要的命令节点
- `minDamage` 触发技能所需要的最低伤害

命令中可以使用的变量：

- `{entity}` 目标名
- `{entity.uuid}` 目标的UUID
- `{entity.x}` 目标的X轴坐标
- `{entity.y}` 目标的Y轴坐标
- `{entity.z}` 目标的Z轴坐标
- `{entity.yaw}` 目标的yaw坐标
- `{entity.pitch}` 目标的pitch

## 消耗（Consume）

使用时消耗道具。

## 击中消耗（ConsumeHit）

击中时消耗道具。

## 暴击伤害（CriticalHit）

造成暴击伤害。

- `chance` 暴击概率，N%
- `backstabchance` 背刺概率，N%
- `factor` 暴击伤害倍率
- `backstabFactor` 背刺伤害倍率

## 即死命令（DeathCommand）

以一定概率即死目标并执行命令。

- `chance` 执行概率，1/N
- `command` 执行的命令
- `des` 技能提示的文字
- `permission` 执行命令所需要的命令节点
- `count`

## 弹反（Deflect）

将飞来的弹射物弹反回去。

- `chance` 被动弹反的百分比概率
- `cooldownPassive` 被动弹反的冷却时间，单位Ticks
- `duration` 弹反持续时间，单位Ticks
- `facing` 最大有效角度，从0至180度
- `deflectCost` 每次弹反的耐久消耗

## 延时命令（DelayedCommand）

在延时后执行命令。

- `delay` 延时时间，单位Ticks
- `display` 显示在道具描述中的展示名
- `command` 执行的命令
- `permission` 执行命令需要的命令节点
- `cmdInPlace` 

## 空（Dummy）

空技能虽然不会做任何事情但是它提供了许多可以帮助您理清技能运行逻辑的通用选项。

- `checkDurabilityBound` 触发前检查物品的耐久上下界，一旦耐久超出上下界空技能将不会被触发。
- `costByEnchantment`
- `doEnchReduceCost` 类似于耐久附魔 ，可以降低道具耐久消耗(几率性不消耗耐久)
- `enchCostPercentage` 每一附魔等级可以减少耐久消耗的几率
- `enchantmentType` 附魔名字，可以像`minecraft:unbreaking`
- `costbyDamage` 根据伤害消耗耐久值
- `cooldownKey` 当有多个空技能时,设定一个唯一的string字符串来让冷却时间互相独立
- `successResult` 在空技能成功执行后的动作，默认值为`OK`，用于继续执行下一个技能
- `costResult` 在耐久消耗后执行的动作。默认设置为`COST`，在消耗耐久后继续下一个技能；`ABORT`，中断并中止整个技能进程；`OK`，无视耐久消耗并继续下一个技能。
- `cooldownResult` 冷却后执行的动作。默认设置为`COOLDOWN`，中断空技能并继续下一个技能；`ABORT`中断并中止整个技能进程；`OK`，无视冷却时间并继续下一个技能。
- `showCDwarning` 如果你不希望显示任何冷却信息，将它设置为`false`
- `globalCooldown` 设置全体的冷却时间

**使用空技能来禁用冷却信息的显示并统一控制技能冷却时间与耐久消耗**

你应当知道了RPGItems中的道具技能是需要触发执行的, 比如`RIGHT_CLICK`或是`LEFT_CLICK`然后根据顺序来一个一个的执行。所以如果你的道具设置的技能如同以下这样：

1. PowerProjectile, trigger `LEFT_CLICK`
2. PowerBeam, trigger `RIGHT_CLICK`
3. PowerSound, trigger `RIGHT_CLICK`
4. PowerParticle, trigger `LEFT_CLICK`

当你点击了鼠标左键，只有1和4会被触发执行，并且是在同一个Tick中执行的。

如果你想要在每次点击鼠标左键时会消耗1耐久，冷却时间20Ticks并且不希望显示冷却消息提示, 那么你可以把一个空技能放置在技能顺位的第一位并设置空技能为所示参数：`cooldown:20 cooldownKey:left cooldownResult:ABORT cost:1 showCDWarning:false triggers:LEFT_CLICK`。同时你也需要设置PowerProjectile与PowerParticle的耐久消耗`cost`与冷却时间`cooldown`为0，因为你不再需要它们，只需要用空技能来控制所有的技能。

点击鼠标左键后，RPGItems会首先检查空技能，如果它还在冷却时间内将会中止后续技能的执行，也就是说PowerProjectile与PowerParticle不会被执行。当它不在冷却时间内且满足所有条件（如果有的话）时，就会消耗1耐久并执行后续技能（发射弹射物与生成粒子）。

同样，如果你将 `costResult`设置为`ABORT`，当道具耐久低于耐久下界时，技能也会被中止。

请注意空技能的使用仅适用于对应的触发，如果你将空技能的触发设置为`RIGHT_CLICK`，它的执行不会影响到其他任何触发为`RIGHT_CLICK`之外的技能。

## 经济（Economy）

消耗或给予使用者金钱。

- `abortOnFailure` 是否在触发失败时中止
- `amountToPlayer` 消耗或给予使用者的金额，负数为消耗，正数为给予
- `showFailMassege` 是否显示失败信息

## 附魔攻击（EnchantedHit）

让附魔来增强伤害量。

- `mode` 可以是增加量`ADDITION`或是倍率增加`MULTIPLICATION`
- `amountPerLevel` 每一附魔等级增加百分比，1 = 100%
- `enchantmentType` 详见：https://hub.spigotmc.org/javadocs/spigot/org/bukkit/enchantments/Enchantment.html

## 动态伤害（EvalDamage）

使用表达式来计算伤害。

- `expression` 用于计算伤害的表达式

## 爆炸（Explosion）

在使用区产生一个爆炸。

- `chance` 发生爆炸的百分比几率
- `distance` 发生爆炸的最远距离
- `explosionPower` 爆炸强度

## 火焰（Fire）

发射火焰来点燃目标。

- `burnduration` 燃烧持续时间
- `distance` 可点燃的最远距离

## 火焰弹（Fireball）

发射火焰弹，不推荐使用.建议使用弹射物技能`Projectile`。

## 火焰附加（Flame）

点燃被击中的目标。

- `burntime` 点燃持续时间

## 食物（Food）

回复饥饿值。（此技能使用会必然消耗一个道具）

- `foodpoint` 回复的饥饿值量

## 力场护盾（ForceField）

生成一个护盾来保护自己。

- `base` 护盾的地基，必填
- `cooldown` 护盾的冷却时间，必填
- `height` 护盾的高度，必填
- `radius` 护盾的半径，必填
- `ttl` 护盾持续时间

## 鬼手（Glove）

抓住实体并将它扔出去。

- `maxDistance` 最大抓取距离
- `maxTicks` 最大抓取时间
- `throwSpeed` 抓取结束后的投掷速度

## 枪斗术（Gunfu）

使弹射物追踪目标。

- `delay` 索敌延时
- `distance` 最大索敌距离
- `forceFactor` 力度倍率
- `initVelFactor` 初速度倍率
- `maxTicks` 最大追踪时间
- `VelFactor` 每Tick速度改变倍率
- `viewAngle` 最大索敌角度

## 爆头（Headshot）

弹射物击中目标头部造成额外伤害。

- `factor` 伤害倍率
- `soundEnemy` 是否播放被爆头粒子音效
- `soundSelf` 是否播放爆头粒子音效
- `particle` 是否显示爆头粒子

## 冰冻（Ice）

发射冰块并形成一个冰牢。

## 击飞（Knockup）

将目标击飞。

- `chance` 击飞概率，1/N
- `knockUpPower` 击飞力度

## 生命窃取（Lifesteal）

根据击中伤害吸取生命值

- `chance` 吸取百分比几率
- `factor` 吸取倍率

## 唤雷（Lightning）

生成闪电造成雷击。

- `chance` 雷击概率，1/N

## 乘骑（Mount）

可以骑在生物上。

- `maxDistance` 生物最远距离
- `maxTicks` 最大乘骑时间

## 取消无敌时间（NoImmutableTick）

无视无敌时间。

- `immuTime` 无敌时间，单位Ticks

## 粒子屏障（ParticleBarrier）

将受到的攻击转化为粒子效果。

- `barrierHealth` 屏障生命值
- `effect` 屏障效果
- `energyDecay` 能量每秒的衰减
- `energyPerBarrier` 屏障能提供的能量
- `energyPerLevel` 每级力量效果所需要的能量
- `projected` 是否为投射屏障

## 粒子效果（Particle）

在使用或击中时生成粒子。

- `delay` 延时时间
- `dustColor` 尘埃粒子颜色
- `dustSize` 尘埃粒子大小
- `effect` 粒子效果
- `extra` REDSTONE粒子颜色参数
- `material` FALLING_DUST粒子颜色对应的原物品
- `playLocation` 生成位置
  - `HIT_LOCATION` 击中位置
  - `SELF` 自身
  - `TARGET` 目标
- `force` 是否设置粒子为随时可见
- `particle` 粒子类型
- `particleCount` 粒子数量
- `offsetX` 粒子生成X轴偏移
- `offsetY` 粒子生成Y轴偏移
- `offsetZ` 粒子生成Z轴偏移

## 佩戴粒子效果（ParticleTick）

在持有或穿戴时生成粒子。

见**粒子效果（Particle）**

## 效果命中（PotionHit）

在击中时赋予状态效果。

- `amplifier` 效果倍率
- `duration` 效果持续时间，单位Ticks
- `chance` 效果发生概率，1/N
- `type` 效果类型

## 自身状态效果（PotionSelf）

赋予使用者状态效果。

- `clear` 是否清除所有效果

其他参数见**效果命中（PotionHit）**

## 佩戴状态效果（PotionTick）

在持有或穿戴时赋予状态效果。

- `interval` 生效间隔时间，单位Tick
- `effect` 效果类型

其他参数见**效果命中（PotionHit）

## 弹射物（Projectile）

发射弹射物，弹射物的参数有多种选项。

- `isCone` 弹射物是否离散发射，默认设置为`false`
- `gravity` 弹射物是否受重力影响，默认设置为`true`
- `range` 发射的离散角度
- `amount` 每一波发射多少弹射物
- `speed` 弹射物飞行速度
- `burstCount` 发射多少波数
- `burstInterval` 下一波发射前的间隔时间
- `setFireballDirection` 火焰弹的轨迹是否会浮动，默认设置为`true`
- `yield` 爆炸物的爆炸半径
- `isIncendiary` 是否会点燃，默认设置为`false`
- `projectileType` 弹射物类型，可以是下列内容：
  - `arrow`
  - `snowball`
  - `fireball`
  - `smallfireball`
  - `llamaspit`
  - `shulkerbullet`
  - `dragonfireball`
  - `trident`
  - `skull`
- `suppressArrow` 取消箭矢的发射但仍然消耗箭矢
- `applyForce` 是否应用弓的拉伸力度到箭矢与火焰弹上，会影响到飞行速度
- `firingLocation` 发射位置，可以是自身`SELF`或是目标`TARGET`
- `firingR` 极坐标参数
- `firingTheta` 极坐标参数
- `firingPhi` 极坐标参数
- `firingRange`
- `castOff`

## 南瓜头（Pumpkin）

攻击僵尸或者骷髅时给他们戴上南瓜头。

- `chance` 触发几率，百分比
- `drop` 南瓜头掉落几率

## 彩虹（Rainbow）

发射各种颜色的羊毛。

- `count` 发射数量
- `isFire` 是否发射火焰

## 真实伤害（RealDamage）

造成无视护甲的真实伤害。

- `minDamage` 触发的最低伤害
- `realDamage` 造成的真实伤害

## 修理（Repair）

消耗材料来回复或是消耗道具耐久。

- `durability` 每一个修复材料会回复或是消耗的耐久量
- `display` 在道具lore中展示的信息。
- `material` 用与修复的材料。你可以使用`HAND`来将主手中持有的物品作为修复材料。
- `isSneak` 是否在潜行状态时才能触发。
- `mode` 可以是默认`DEFAULT`，允许超出上限`ALLOW_OVER`或是总是可以修复`ALWAYS`。
- `allowBreak` 是否会把道具修爆，默认设置为`true`
- `abortOnSuccess` 是否在修理成功后中止后续技能
- `abortOnFailure` 是否在修理失败后中止后续技能(比如没有材料了)
- `customMessage` 在聊天栏中展示的信息
- `amount` 每次触发时会消耗的最大材料量
- `showFailMsg` 是否显示修理失败后的信息。默认设置为`true`

## 拯救（Rescue）

受到过量伤害或者血量即将低于指定值时拯救玩家，比不死图腾更好！

- `damageTrigger` 伤害量触发值
- `healthTrigger` 生命触发值
- `inPlace` 是否原地拯救
- `useBed` 是否回到床上

## 裂地冲击（Rumble）

在地面上生成一个冲击波将目标击飞。

- `power` 击飞力度
- `distance` 冲击波最大距离
- `damage` 冲击波伤害

## 计分板（Scoreboard）

修改玩家的计分板标签或数值。

## 潜影弹（Shulkerbullet）

发射追踪目标的潜影弹。

- `range` 追踪范围

## 钩爪（Skyhook）

发射钩爪用于钩住指定方块。

- `railMaterial` 允许被钩住的材质，必填
- `hookDistance` 钩爪距离
- `hookingTickCost` 钩住时每Tick耐久消耗

## 声音（Sound）

在使用或击中实体时播放声音。

- `delay` 延时时间，单位Ticks
- `pitch` 音高
- `playLocation` 播放中心
  - `HIT_LOCATION` 击中位置
  - `SELF` 自身
  - `TARGET` 目标
- `sound` 声音类型
- `volume` 音量

## 定身（Stuck）

使目标无法移动或是传送。

- `chance` 定身概率，1/N
- `costAOE` 范围定身耐久消耗
- `costPerEntity` 每一个实体的耐久消耗
- `duration` 持续时间
- `facing` 角度范围
- `range` 距离范围

## TNT大炮（TNTCannon）

发送TNT。

## 传送（Teleport）

传送一段距离。

- `distance` 传送距离
- `targetMode` 寻找传送点的模式
  - `DEFAULT`
  - `RAY_TRACING`
  - `RAY_TRACING_EXACT`
  - `RAY_TRACING_EXACT_SWEEP`
  - `RAY_TRACING_SWEEP`

## 投掷（Throw）

投掷实体。

- `entityName` 实体名，必填
- `speed` 投掷速度，必填
- `display` 技能在道具描述中的展示名，必填
- `cooldown` 冷却时间，必填
- `entityData` 实体数据
- `isPersistent` 实体是否写入世界存档

## 药箭（TippedArrow）

发射带有药水效果的箭矢，不推荐使用。请使用弹射物`Projectile`与命中效果`PotionHit`技能。

## 火把（Torch）

投掷一些火把来照亮一片区域。

## 传送信标（Translocator）

投掷一个传送信标并传送到信标所在地。

- `speed` 
- `setupCost` 设立信标的耐久消耗
- `tpCost` 传送耐久消耗

