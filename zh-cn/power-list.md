# 技能列表

技能是 RPGItems 物品的核心组成部分。通过技能之间的组合，可以设计出各式不同的道具。本页面将详细介绍各个技能的效果和参数用法。

使用 `/rpgitem power [item] [power] <parameter:value> ...` 可以为物品添加技能。技能的参数可以在添加时一口气设置完成，也可以在之后使用 `/rpgitem set [item] [power] [No.] [parameter] [value]` 进行设置。无论是何种方法，RPGItems 插件均提供便捷的命令补全。部分技能的参数较多，可能无法在聊天框完整输入。这时请使用命令方块执行命令。

## 通用参数

以下参数存在于大部分的技能中，并且具有通用的含义。这里将这些参数单独列出，在具体技能中的含义如非发生变化，不再详细说明。

* cooldown 技能的冷却时间，以 tick 为单位。处于冷却时间内时无法使用技能。默认为 0 。
* cost 技能的消耗。每次使用技能将会消耗设定的耐久值。更为具体的消耗规则见 [耐久使用方法](advanced.md#nai-jiu-de-te-shu-shi-yong) 。默认为 0 。
* trigger 技能的触发。触发技能的动作或事件。支持的值详见 [触发详解](advanced.md#chu-fa-xiang-jie) 。大部分主动技能默认为 RIGHT\_CLICK ，命中技能默认为 HIT ，持续效果技能默认为 TICK 。
* conditions 在非条件类技能中表示技能的条件。仅当条件为真时技能会被触发。如需为技能指定条件，首先使用 [条件类技能](power-list.md#tiao-jian) 定义条件，并将该技能的 condition 参数设置为条件中定义的 id 参数。进一步的使用方法详见 [条件妙用](advanced.md#qiao-yong-tiao-jian) 。
* displayName 技能的备注信息，不作实际用途。
* requiredContext 限制技能执行的一个值。大多数情况下不需要设置。
* requireHurtByEntity 当触发设置为 HURT 或 HIT\_TAKEN 时，将此值设置为 true 可以使仅来自于生物的有源伤害能够触发技能（不包括中毒、凋零、火焰、摔落伤害等）。默认为 true 。
* selectors 技能选择器，使技能仅对部分实体生效。如需为技能指定选择器，首先使用 selector 技能定义一个选择器，并将该技能的 selectors 参数设置为 selector 技能中定义的 id 。
* setBaseDamage 设置为 true 时，后续的伤害加成技能将基于此伤害值计算，武器的最终伤害取叠加值。 设置为 false 时，后续的伤害加成技能将基于原伤害值计算，武器的最终伤害取最高值。 默认为 false。

## 攻击技能

RPGItems 最为核心的远程攻击技能。

### projectile

#### 技能描述

向准心方向发射弹射物攻击目标，造成武器伤害。支持多种弹射物类型，并且能设置飞行速度、散射角、连射速度、连射数等参数，是一大基础技能。

#### 参数

* amount 单次发射的弹射物数量。仅当 isCone 设置的为 true 时生效。
* applyForce 当触发设置为 BOW\_SHOOT 时，拉弓的蓄力程度是否影响飞行速度。
* burstCount 连射数量。
* burstInterval 连射间隔，以 tick 为单位。
* gravity 弹射物是否受到重力影响
* isCone 是否为弹射物添加散射角。添加散射角后可以设置单次发射数量 amount 。
* isIncendiary 弹射物是否点燃目标。
* projectileType 弹射物种类。支持的值有：
  * arrow 箭；
  * dragonfireball 龙息火球，无法造成命中伤害；
  * fireball 恶魂火球（会爆炸）；
  * llamaspit 羊驼口水；
  * shulkerbullet 潜影贝子弹；
  * skull 凋零骷髅头颅；
  * smallfireball 烈焰人火球，对抗火敌人无效；
  * snowball 雪球；
  * trident 三叉戟。
* range 散射角数值的一半，单位为度。
* setFireballDirection 当弹射物为恶魂火球时，可以设置火球的加速度方向。
* speed 弹射物的飞行速度。
* supressArrow 当触发为 BOW\_SHOOT 时，是否移除原本射出的箭矢。
* yield 当设置为恶魂火球时的爆炸范围。其单位并非方块距离，且爆炸无衰减，无视障碍物。

### aoedamage

#### 技能描述

对扇形范围内的敌人直接造成伤害。可以设定伤害值、视角范围、最大攻击敌人数量、伤害延迟等参数。当最大数量设置为 1 时，仅攻击离准心最近的目标。

#### 参数

* angle 技能作用角度的一半，单位为度。设置为 180 时为全方向作用。
* count 技能作用的最多目标个数
* damage 技能伤害。此伤害独立于武器伤害。
* delay 从技能发动到目标受到伤害的延迟，单位为 tick。
* includePlayers 是否能攻击到其他玩家。
* minrange 技能作用的最近距离。此距离内的目标不会被攻击。
* mustsee 是否要求玩家与目标之间没有障碍物。
* name 显示在技能信息栏的内容。
* range 技能作用的最远距离。
* selfapplication 技能是否会作用于自身。如果作用于自身，则会造成无视护甲的真实伤害。
* supressMelee 设置为 true 时会使得所造成的伤害不会触发任何伤害加成和特殊效果，即不会导致 HIT 触发。

### beam

#### 技能描述

向准心方向发射直线粒子光束，对命中的敌人造成伤害。粒子光束具有多种攻击模式，包括是否穿透敌人、是否穿透方块、是否反弹、是否自动追踪敌人，并可以自定义粒子种类、伤害值、光束速度、连射参数等。合理使用会使其成为一个兼具美观和实用性的技能。

注意，此技能目前处于不稳定状态。参数值设置过于夸张时，会导致严重的服务器卡顿。技能触发过于频繁时，还会导致客户端的网络阻塞问题。

#### 参数

* amount 与粒子数量相关的一个值。大部分情况下，将其设置得较大时才会发挥作用，否则与设置至 1 无异。
* beamAmount 单次发射的光束数量，仅当 cone 设置至 true 时有效。
* bounce 光束遇到墙壁时的最多反弹次数。默认为 0。
* burstCount 连射数量。
* burstInterval 连射间隔，单位为 tick 。
* cone 光束是否散射。
* coneRange 光束散射角的一半。
* damage 技能伤害，该伤害独立于武器伤害。
* extraData 粒子颜色的额外参数。格式为 `R,G,B,Size` 。R G B 均为 0-255 的数值。
* gravity 光束的重力参数。数值越大，受重力影响越大。
* hitSelfWhenBounced 是否在光束反弹时能够对自身造成伤害。
* homing 光束是否自动追踪目标。开启后光束会自动向离准心最近的目标修正路线。
* homingAngle 光束追踪时每 tick 修正的角度。根据其余参数的不同，合适值从 0.01 至 1 不等，需要根据实际效果调整。
* homingRange 光束追踪目标的最大距离。
* homingTarget 光束追踪的目标类型。支持：
  * MOBS 非玩家生物，默认值；
  * PLAYERS 玩家；
  * ALL 全部。
* homingTargetMode 追踪多目标时的模式。仅当 pierce 设置为 true 时生效。支持的值有：
  * ONE\_TARGET 始终追踪一个目标，直至其死亡后追踪下一个目标；
  * MULTI\_TARGET 在命中目标后切换下一个目标为当前离准心最近的目标，可以是同一个目标。
* ignoreWall 光束是否穿墙。
* length 光束的长度。
* mode 光束模式。支持：
  * BEAM 瞬发光束。
  * PROJECTILE 具有一定移动速度的光束。
* movementTicks 光束飞行完成全长所需的时间，单位为 tick 。
* offsetX 粒子的 X 方向偏移，同时影响光束命中判定区沿 X 方向的大小。
* offsetY 粒子的  Y 方向偏移，同时影响光束命中判定区沿 Y 方向的大小。
* offsetZ 粒子的 Z 方向偏移，同时影响光束命中判定区沿 Z 方向的大小。
* particle 粒子种类。支持的种类详见补全。
* pierce 光束在命中后是否穿透
* spawnsPerBlock 单位长度生成粒子位置的个数，决定粒子密度。
* speed 粒子的速度参数，不是光束速度。
* stepsBeforeHoming 在开始追踪目标前的一段延迟。
* supressMelee 设置为 true 时会使得所造成的伤害不会触发任何伤害加成和特殊效果，即不会导致 HIT 触发。

### shulkerbullet

#### 技能描述

发射潜影贝导弹，命中后造成武器伤害。与 projectile 技能不同，潜影贝导弹的飞行方式与原版相同，会缓慢飞行，并自动追踪离准心最近的目标。

#### 参数

* range 追踪目标的最大距离。

## 伤害加成

这些技能会为攻击提供额外的伤害。

### criticalhit

#### 技能描述

在攻击命中时，有一定的几率造成暴击，使本次伤害乘以一个倍率。

同时，在攻击背对自己的目标时，有一定的几率造成背刺暴击，同样使当次伤害乘以一个倍率。两个倍率乘算叠加。

#### 参数

* backstabChance 攻击背对的目标时发动被刺的几率。默认为 20%。
* backstabFactor 被刺发动时的伤害倍率，默认为 1.5。
* chance 暴击概率，默认为 20%。
* factor 暴击倍率，默认为 1.5%。

### enchantedhit

#### 技能描述

在攻击命中时，如果武器上具有某个特定的附魔，会按照附魔等级增加本次伤害。

#### 参数

* amountPerLevel 每一等级的附魔提供的增加量。默认为 1.0。
* display 显示在技能信息栏的内容。
* enchantmentType 影响伤害量的附魔类型，默认为 ARROW\_DAMAGE。
* mode 加成模式，可以为：
  * ADDITION 每一级附魔添加固定值；
  * MULTIPLICATION 每一级附魔乘以固定倍率。

### evaldamage

#### 技能描述

在攻击命中时，将按照一个自定义公式重新计算伤害。

在被攻击时，将按照一个自定义公式重新计算受到的伤害。

#### 参数

* display
* expression
* triggers

### airborne

#### 技能描述

在攻击命中时，如果玩家处于滑翔状态，则按比例增加当次伤害。

#### 参数

* cap
* percentage

### charge

#### 技能描述

在攻击命中时，如果玩家处于冲刺状态，则按比例增加当次伤害。

同时，如果玩家的速度快于正常速度（例如玩家拥有速度状态效果），玩家的速度越快，造成的伤害也越多。

#### 参数

* cap
* percentage
* speedPercentage

### headshot

#### 技能描述

使用 projectile 技能或者弓箭射击命中头部后，当次伤害将乘以一个倍率。光束命中不支持爆头。

#### 参数

* factor
* particleEnemy
* soundEnemy
* soundSelf

### realdamage

#### 技能描述

在攻击命中时，在普通伤害之外，再额外造成一部分真实伤害。

#### 参数

* minDamage
* realDamage

## 命中效果

这些技能会为攻击提供额外的效果。

### lifesteal

#### 技能描述

攻击命中时，根据伤害值按比例回复生命值。

#### 参数

* chance
* factor

### flame

#### 技能描述

攻击命中时点燃敌人。

#### 参数

* burntime

### knockup

#### 技能描述

攻击命中时将敌人击飞至空中。

#### 参数

* chance
* power

### explosion

#### 技能描述

使用 projectile 技能或弓命中目标或方块时造成爆炸（不会破坏方块）。

#### 参数

* chance
* distance
* power

### lightning

#### 技能描述

攻击命中时雷击目标。

#### 参数

* chance

## 位移

便于玩家移动的技能。

### teleport

#### 技能描述

向视角方向传送一定距离，但无法穿过实体方块。

#### 参数

* distance
* targetMode

### skyhook

#### 技能描述

类似抓钩一般的技能，可以将玩家拉向瞄准的方块，

#### 参数

* railMaterial
* hookDistace
* hookTickCost

### translocator

#### 技能描述

类似《守望先锋》中 “ 黑影 ” 的传送信标。将物品从主手切换至副手时掷出信标，从副手切换回主手时传送至信标位置，并进入技能 CD 。信标在空中飞行时即可传送，并可被破坏。

注意，信标位于已卸载的区块时，传送会失效。

#### 参数

* setupCost
* speed
* tpCost

## 控场

对目标进行控制的技能。

### attract

#### 技能描述

吸引或排斥附近的生物。

#### 参数

* radius
* attractingEntityTickCost
* attractingTickCost
* attractPlayer
* duration
* maxSpeed

### stuck

#### 技能描述

攻击命中时使目标在一定时间内无法移动或传送。

也可以主动施放，使附近范围内的敌人一定时间内无法移动或传送。

#### 参数

* chance
* costAoe
* costPerEntity
* duration
* facing
* range

### rumble

#### 技能描述

需在地面上使用。向前发出冲击波，使冲击波遇到的第一个目标被击飞至空中。

#### 参数

* power
* damage
* distance

### glove

#### 技能描述

将目标举起，并可以从视角方向扔出。

#### 参数

* maxDistance
* maxTicks
* throwSpeed

### mount

#### 技能描述

类似骑马，骑乘任意指向的目标。

#### 参数

* maxDistance
* maxTicks

## 防护

保护类技能。

### deflect

#### 技能描述

手持物品时反弹弹射物，并将其由视角方向重新射出。

#### 参数

* chance
* cooldown
* cooldownpassive
* cost
* deflectCost
* duration
* facing

### forcefield

#### 技能描述

在身边生成不可穿过的屏障，持续一段时间。

#### 参数

* base
* height
* radius
* ttl

### particlebarrier

#### 技能描述

类似《守望先锋》中 “ 查莉娅 ” 的粒子屏障技能。使自身获得一个持续一段时间的屏障。屏障能够吸收一定的伤害，并将伤害转化为状态效果。在不使用技能时，状态效果将不断衰减。屏障可以被投射至其他的玩家。

#### 参数

* barrierHealth
* cooldown
* effect
* energyDecay
* energyPerBarrier
* energyPerLevel
* projected

## 辅助

起到辅助作用的技能。

### throw

#### 技能描述

向视角方向投掷实体。可以投掷任何实体，并自定义实体的 NBT 标签。

#### 参数

* display
* entityName
* speed
* entityData
* isPersistent

### repair

#### 技能描述

消耗背包中的某一种材料，修复物品耐久值。

#### 参数

* display
* material
* abortOnFalure
* abortOnSuccess
* allowBreak
* amount
* customMessage
* durability
* isSneak
* mode
* showFailMsg

### rescue

#### 技能描述

防止玩家受到的致命伤害，获得一段无敌时间，并进入冷却时间。可以设置使玩家留在原地或者传送回床边。

#### 参数

* damageTrigger
* healthTrigger
* inPlace
* useBed

### economy

#### 技能描述

失去或者获得一定数量的金钱。需要 Essentials 插件支持。

#### 参数

* abortOnFailure
* amountToPlayer
* showFailMessage

## 药水效果

与状态效果相关的技能。

### aoe

#### 技能描述

对范围内的目标施放药水效果。

#### 参数

* duration
* range
* type
* amplifier
* name
* selfapplication

### potionhit

#### 技能描述

攻击命中后，使目标获得药水效果。

#### 参数

* amplifier
* duration
* chance
* type

### potionself

#### 技能描述

自身获得药水效果。

#### 参数

* amplifier
* duration
* clear
* type

### potiontick

#### 技能描述

手持或者穿戴时获得持续的药水效果。

#### 参数

* amplifier
* clear
* duration
* effect
* interval

## 执行命令

与执行命令相关的技能。

### aoecommand

#### 技能描述

对范围内的所有敌人分别执行命令。

#### 参数

* command
* display
* r
* rm
* c
* facing
* mustsee
* permission
* selfapplication
* type

### command

#### 技能描述

执行命令。

#### 参数

* display
* command
* permission

### commandhit

#### 技能描述

攻击命中后执行命令。

#### 参数

* display
* command
* minDamage
* permission

### delayedcommand

#### 技能描述

延迟一定的时间后执行命令。

#### 参数

* delay
* display
* command
* permission

### scoreboard

#### 技能描述

为玩家增减tag，修改玩家的队伍。

可以被 command 技能代替，但使用更便捷。

#### 参数

* abortOnSuccess
* tag
* team
* scoreOperation
* objective
* value
* reverseTagAfterDelay
* delay

### deathcommand

#### 技能描述

几率秒杀目标，并在杀死后执行命令。

#### 参数

* chance
* command
* count
* desc

## 破坏方块

可以破坏方块的技能。

### tntcannon

#### 技能描述

发射点燃的 TNT 。未开启爆炸保护时会破坏方块。

#### 参数

该技能没有额外参数。

## 音画特效

对于攻击没有过多的作用，但可以用来制作特效。

### sound

#### 技能描述

在玩家位置播放指定声音。触发设置为 HIT 时在命中的目标位置播放。

#### 参数

* display
* pitch
* sound
* volume

### particle

#### 技能描述

在玩家位置播放指定粒子效果。触发设置为 HIT 时在命中的目标位置播放。

#### 参数

* dustColor
* dustSize
* effect
* extra
* force
* material
* offsetX
* offsetY
* offsetZ
* particle
* particleCount

### particletick

#### 技能描述

手持或穿戴时在玩家位置持续播放粒子效果

#### 参数

* dustColor
* dustSize
* effect
* extra
* force
* interval
* material
* offsetX
* offsetY
* offsetZ
* particle
* particleCount

### ice

#### 技能描述

发射大块冰块，并在落地后成为方块，一段时间后冰块会自动消失。

#### 参数

该技能没有额外参数。

### fire

#### 技能描述

发射大量火焰方块，并点燃前方的地面。火焰在一段时间后自动熄灭。

#### 参数

* burnduration
* distance

### rainbow

#### 技能描述

发射大量彩色羊毛，并在落地后变为方块。一段时间后羊毛自动消失。

羊毛也可以用火焰代替。

#### 参数

* count
* isFire

### torch

#### 技能描述

发射火把并插在落地点。一段时间后火把会变为掉落物。

#### 参数

该技能没有额外参数。

### pumpkin

#### 技能描述

攻击目标时几率为其戴上南瓜头。南瓜头有概率掉落。

#### 参数

* chance
* drop

### lorefilter

#### 技能描述

没有实际作用。使物品描述中特定的行数不会在更新时被更改。

#### 参数

* regex
* desc
* find

## 流程控制类

这些技能均没有实际作用，但能够辅助其它技能的执行，并控制执行流程。

### dummy

#### 技能描述

没有任何作用，但会为技能提供很好的流程控制。详见 [进阶技巧](advanced.md#shi-yong-dummy-ji-neng-tong-yi-guan-li) 。

#### 参数

* checkDurabilityBound
* cooldownKey
* cooldownResult
* costByDamage
* costByEnchantment
* costResult
* display
* doEnchReduceCost
* enchantmentType
* enchCostPercentage
* showCDWarning
* successResult

### attachments

#### 技能描述

自身没有作用，但能够触发携带的其它物品的 ATTACHMENT 触发。

#### 参数

* allowedInvSlots
* allowedItems
* allowedSlots
* limit

### cancelbowarrow

#### 技能描述

在弓射出后移除射出的箭矢。可以配合其它时候 BOW\_SHOOT 触发的技能使用。

#### 参数

* cancelArrow

### consume

#### 技能描述

使用后消耗一个堆叠的物品。

#### 参数

该技能没有额外参数。

### consumehit

#### 技能描述

攻击命中后消耗一个堆叠的物品。

#### 参数

该技能没有额外参数。

### selector

#### 技能描述

定义一个实体选择器，以供其它技能的 selector 参数使用。

#### 参数

* count
* display
* dx
* dy
* dz
* id
* r
* rm
* score
* tag
* team
* type
* x
* y
* z

## 物品属性

这些技能会为物品添加额外的属性。

### gunfu

#### 技能描述

使 projectile 技能射出的弹射物和弓射出的箭矢能够自动追踪敌人。

#### 参数

* delay
* distance
* forceFactor
* initVelFactor
* maxTicks
* velFactor
* viewAngle

### attributemodifier

#### 技能描述

为物品添加 attribute 属性。

#### 参数

* amount
* attribute
* name
* operation
* slot
* uuidLeast
* uuidMost

### noimmutabletick

#### 技能描述

使物品的攻击能够无视 Minecraft 原版的 10 tick 受伤间隔，或者重新规定使用该物品攻击的受伤间隔。

#### 参数

* immuneTime

### ranged

#### 技能描述

限制物品的远程攻击范围。

添加 ranged 或者 rangedonly 技能会引入一个插件特性，并导致部分技能失效，解决方法详见 [伤害与攻击方式管理](advanced.md#shang-hai-yu-gong-ji-fang-shi-guan-li) 。

#### 参数

* r
* rm

### rangedonly

#### 技能描述

使物品仅能通过远程方式攻击，并限制物品的远程攻击范围。

添加 ranged 或者 rangedonly 技能会引入一个插件特性，并导致部分技能失效，解决方法详见 [伤害与攻击方式管理](advanced.md#shang-hai-yu-gong-ji-fang-shi-guan-li) 。

#### 参数

* r
* rm

### unbreakable

#### 技能描述

使物品变得不可破坏。添加该技能后，尝试将物品的耐久消耗至 1 以下的技能会被取消。

#### 参数

该技能没有额外参数。

## 条件

这里的技能定义一系列条件，以供几乎所有技能的 condition 参数使用。这些条件会在被 condition 参数指定的技能触发时进行判断，并决定是否继续执行技能。

条件类技能拥有这些通用参数：

* id
* isCritical
* isStatic

### chancecondition

该条件有一定的概率为真。

#### 技能描述

#### 参数

* chancePercentage

### evalcondition

#### 技能描述

根据所给的表达式计算该条件的真值。

#### 参数

* expression

### durabilitycondition

#### 技能描述

当物品的耐久值在某个范围时为真。

#### 参数

* durabilityMax
* durabilityMin

### equipmentcondition

#### 技能描述

在玩家穿戴或手持指定物品时为真。物品可以为原版物品、带标签的原版物品、 RPGItems 插件物品、RPGItems 物品组。

#### 参数

* itemStack
* matchAllSlot
* material
* rpgitem
* slots

### lastresultcondition

#### 技能描述

该触发的上一个技能执行成功时为真。

#### 参数

* failOnNoResult
* okResults

### scoreboardcondition

#### 技能描述

玩家的某个记分板项的值处于某个范围时为真；玩家拥有某个标签时为真；玩家位于某一队伍时为真。

以上三个条件均指定时，会进行与运算。

#### 参数

* score
* tag
* team

### andcondition

#### 技能描述

与条件：对指定的条件依次进行与计算，并得到最后的结果。

如果为一个技能的 condition 参数指定了多个条件，在触发这个技能时也会对这些条件依次进行与运算。该条件技能在条件复用时会提供一定的便利。

#### 参数

* contitions

### orcondition

#### 技能描述

或条件：对指定的条件依次进行或计算，并得到最后的结果。

#### 参数

* conditions

### xorcondition

#### 技能描述

异或条件：由初始值开始对指定的条件依次进行异或计算，并得到最后的结果。

将初始值设置为 true ，并仅指定一个条件，即可使用异或条件实现非条件。

#### 参数

* conditions
* init

## 弃用

这里的技能拥有更好更灵活的替代技能，不再需要使用。

### arrow

#### 技能描述

发射箭矢。可以使用 projectile 技能代替。

#### 参数

该技能没有额外参数。

### color

#### 技能描述

为彩色方块染色。未完成。

#### 参数

* clay
* glass
* wool

### fireball

#### 技能描述

发射火球。可以使用 projectile 技能代替。

#### 参数

### food

#### 技能描述

回复饥饿值，并消耗物品。可以使用 potionself 和 consume 技能代替。

#### 参数

该技能没有额外参数。

### tippedarrow

#### 技能描述

发射药水箭。可以使用 projectile 和 potionhit 技能代替。

#### 参数

* duration
* type
* amplifier

