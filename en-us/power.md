# Powers

Power is the core feature set that actually implementing an item function.

Powers execute in pitfall order, and only correctly triggered powers will be executed.

There are some general power properties:

- `cooldown` Cooldown ticks before next use
- `cost` Cost to durability for each use
- `condition` Conditions to meet in order to trigger this power
- `trigger` Triggers of this power

Command to modify power on an item:

/rpgitem power [operation] [item] [params]

Available operations:

- `add` Add a power
- `remove` Remove a specific power
- `list` List all powers
- `prop` List details about a specific power, or modify it with additional parameters
- `reorder` Re-order a specific power to a new position. This may affect power execution logic

E.g.,

```
/rpgitem power add mysword rpgitems:beam cost:1 damage:10 ttl:20 triggers:LEFT_CLICK speed:16 particle:CRIT_MAGIC length:24
```

## AOE

Applies effect to targets in an area.

- `amplifier` Effect amplifier
- `duration` Effect duration
- `range` Radius of area
- `selfapplication` Whether the effect applies to yourself
- `name` Effect name, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/potion/PotionEffectType.html

## AOECommand

Let all targets within a range to execute command.

- `type` Target type, can be
  - `entity`
  - `player`
  - `mobs`
- `r` Target range
- `rm` Minimum range
- `facing` Angle in your direction. value from 0 to 180
- `c` Target count
- `mustsee` There must be no block between you and target
- `command` The command to execute
- `permission` The permission to assign for execution of the command

## AOEDamage

Deals damage to a range of targets.

- `range` Maximum target selection range
- `minrange` Minimum target selection range
- `angle` Angle from your direction, value from 0 to 180
- `count` Number of targets can be attacked
- `incluePlayers` (typo) include players when triggering power
- `selfapplication` Apply to yourself when triggering power
- `mustsee` There must be no block between you and target
- `damage` Damage amount to deal, this is independent with item damage property
- `delay` Delay ticks before actual apply damage
- `selectAfterDelay` When delay is not 0, set this to false to select target on use, so that target will be damaged even it's out of range.
- `firingRange`
- `firingLocation` Define location of AOE center, can be `SELF` or `TARGET`
- `castOff` When `firingLocation` is `TARGET` and delay is not 0, set this to true to set center on use before delay

## Airborne

Damage boost when flying.

## Arrow

Deprecated. Use `Projectile`.

## Attachment

Trigger power from other items.

## Attract

Pull or push entity.

- `radius` Radius to select target
- `maxSpeed` Maximum speed to pull/push entity
- `duration` Duration time to pull/push entity
- `attractingTickCost` Cost per tick for using this power
- `attractingEntityTickCost` Cost per tick per entity for using this power
- `attractPlayer` Whether it will pull/push player
- `firingLocation` Can be `SELF` or `TARGET`
- `firingRange`

## Beam

Fire a beam made by particle. This is by far the most complex and powerful power in RPGItems.

- `length` Maximum beam length
- `ttl` Time to live ticks
- `particle` Particle type, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Particle.html
- `mode` Can be `BEAM` (run in 1 tick) or `PROJECTILE` (run in multiple ticks)
- `pierce` Number of targets that can be pierced through
- `ignoreWall` Ignore wall when flying, defaults to `true`
- `damage` Damage deal when contact, this is independent with damage property in item config
- `speed` Beam flying speed, block per second
- `offsetX` particle offsetX
- `offsetY` particle offsetY
- `offsetZ` particle offsetZ
- `particleSpeed` particle speed parameter
- `particleDensity` how many particle spawn dots in one block space
- `cone` Angle from your direction to fire discrete beams
- `homing` Radius for homing
- `homingAngle` Angle from your direction to select target
- `homingRange` Block distance to select target
- `homingMode` Can be
  - `ONE_TARGET` Lock on single target
  - `MULTI_TARGET` Change target after last hit
  - `MOUSE_TRACK` Lock on nearest target of mouse aim
- `homingTarget` Can be `MOBS` / `ENTITY` / `PLAYER`
- `ticksBeforeHoming` Ticks before homing path
- `beamAmount` How many beam will be fired in a single run
- `burstCount` How many beam will be fired in a single trigger
- `burstInterval` Ticks between next burst
- `bounce` How many times the beam will bounce off wall, require `ignoreWall` to `false`
- `hitSelfWhenBounced` Beam may hit yourself when bounce off wall
- `gravity` Gravity of beam, can be negative (trace goes up)
- `extraData` Color of `REDSTONE` particle, in format of `r,g,b,size`. E.g., extraData:\`51,204,255,0.35\` for a cyan slim beam
- `speedBias` Expression to control the flying speed. E.g., `4+x*10*t`
- `behavior` Controls the beam behavior
- `initialRotation`
- `firingLocation` Can be `SELF` or `TARGET`
- `effectOnly` Only act as effect, will not trigger HIT event
- `firingR` Polar coordinates parameter
- `firingTheta` Polar coordinates parameter
- `firingPhi` Polar coordinates parameter
- `firingRange`
- `castOff`

## CancelBowArrow

Cancels bow arrow. Arrow will not be consumed and no arrow is fired. Often used on bows with other powers.

## Charge

Damage boost when sprinting.

- `percentage` Damage increase percentage
- `speedPercentage` Damage increase percentage by speed
- `setBaseDamage` Set to true to use dynamic damage as base, otherwise use damage property in item config
- `cap` Maximum damage cap

## Command

Execute command on use.

- `command` Command to execute. Use \` to quote with space, e.g., `command:\`minecraft:give {player} stone\``
- `display` Message to display in lore
- `permission` Permission to assign just for executing the command

Variables available in command:

- `{player}` the user's name
- `{player.x}` the user's X coordinate
- `{player.y}` the user's Y coordinate
- `{player.z}` the user's Z coordinate
- `{player.yaw}` the user's yaw coordinate
- `{player.pitch}` the user's pitch

## CommandHit

Execute command on hit.

- `command` Command to execute.
- `permission` Permission to assign just for executing the command
- `minDamage` Minimum damage to deal in order to trigger this power

Variables available in command:

- `{entity}` the target's name
- `{entity.uuid}` the target's UUID
- `{entity.x}` the target's X coordinate
- `{entity.y}` the target's Y coordinate
- `{entity.z}` the target's Z coordinate
- `{entity.yaw}` the target's yaw coordinate
- `{entity.pitch}` the target's pitch

## Consume

Consume item on use.

## ConsumeHit

Consume item on hit.

## CriticalHit

Deals critical hit.

- `chance` Chance to trigger the power
- `backstabchance` Chance to backstab
- `factor` Damage multiplier
- `backstabFactor` Damage multiplier when backstab
- `setBaseDamage` Set to true to use dynamic damage as base, otherwise use damage property in item config

## DeathCommand

A chance to kill target and execute command.

## Deflect

Deflect incoming projectile.

## DelayedCommand

Execute command with delay.

## Dummy

Power dummy do nothing but provides a bunch of universal options to help you make item logic execution clean.

- `checkDurabilityBound` Check durability bound before trigger. Once durability out of bound the dummy will not be triggered.
- `costByEnchantment`
- `doEnchReduceCost` Enchantment like durability can reduce cost (chance not to cost)
- `enchCostPercentage` Percentage not to cost for each enchantment level
- `enchantmentType` Enchantment name like `minecraft:unbreaking`
- `costbyDamage` Cost value by damage
- `cooldownKey` When there are multiple dummy power, set a unique key string to make cooldowns independent
- `successResult` Action after this dummy is successfully executed, defaults to `OK` which will continue with next power.
- `costResult` Action after cost. Defaults to `COST` which deals the cost and continue with next power. Set to `ABORT` to break and abort the whole process, `OK` to ignore the cost and continue with next power.
- `cooldownResult` Action after cooldown. Defaults to `COOLDOWN` which break execution of dummy but continue with next power. Set `ABORT` to break and abort the whole process, `OK` to ignore cooldown and continue with next power.
- `showCDwarning` If you wish not to show cooldown warning message, set to `false`.
- `globalCooldown` Set the cooldown to global.

You should already acknowledge that RPGitems execute powers based on trigger, like `RIGHT_CLICK` or `LEFT_CLICK` and activate power one by one in a pitfall. So if you have an item with a list of powers like:

1. PowerProjectile, trigger `LEFT_CLICK`
2. PowerBeam, trigger `RIGHT_CLICK`
3. PowerSound, trigger `RIGHT_CLICK`
4. PowerParticle, trigger `LEFT_CLICK`

When you do left click, only 1 and 4 will be activated. The item firstly shoot a projectile and then play a particle, but in a single tick.

Now if you want to cost 1 durability on each left click use, with 20 ticks cooldown and do not wish to show the duplicated cooldown message, you can add PowerDummy at first place, and set `cooldown:20 cooldownKey:left cooldownResult:ABORT cost:1 showCDWarning:false triggers:LEFT_CLICK`. You should also set PowerProjectile and PowerParticle's `cost` and `cooldown` to 0, indicating you do not need them and only using PowerDummy to control everything.

RPGitem will first check PowerDummy, and if it's in cooldown then abort the whole execution process, which means projectile and particle powers will not be activated. If it's not cooling down and all conditions (if any) are met, cost 1 then continue with power execution (fire a projectile and play particle).

Similarly, if you set `costResult` to `ABORT`, when the item durability less than its lower bound, the power execution will be aborted.

Please note dummy power options is effective only with correct trigger. If you set dummy with `RIGHT_CLICK` trigger, it will not affect any power with triggers other than `RIGHT_CLICK`.

## Economy

Operates economy functions.

## EnchantedHit

Make enchantment boost damage.

- `mode` Can be `ADDITION` or `MULTIPLICATION`
- `amountPerLevel` Boost percentage per enchant level, 1 = 100%
- `enchantmentType` See https://hub.spigotmc.org/javadocs/spigot/org/bukkit/enchantments/Enchantment.html
- `setBaseDamage` Set to true to use dynamic damage as base, otherwise use damage property in item config

## EvalDamage

Set damage based on expression.

## Explosion

Cause explosion on use.

## Fire

Ignite target.

## Fireball

Deprecated. Use `Projectile`.

## Flame

Shoots a bunch of fire.

## Food

Restores saturation level.

## ForceField

Summon a forcefield to trap or block target.

## Glove

Catch and throw entity.

## Gunfu

Projectiles will homing to target.

## Headshot

Damage boost when hit target head.

## Ice

Shoots ice and form an ice block.

## Knockup

Knockup target on hit.

## Lifesteal

Gain health on hit. 

## Lightning

Summon lightning strike.

## Mount

Ride on entity.

## NoImmutableTick

Ignore immutable tick.

## ParticleBarrier

Summon ParticleBarrier to reduce damage taken and give you power.

## Particle

Plays particle on use or hit.

## ParticleTick

Plays particle when holding or wearing.

## PotionHit

Applies potion effect on hit.

## PotionSelf

Applies potion effect to user.

## PotionTick

Applies potion effect when holding or wearing.

## Projectile

Fires projectile with many options.

- `isCone` if the projectile fires in a cone. Defaults to `false`
- `gravity` if the projectile affected by gravity. Defaults to `true`
- `range` cone angle from your direction
- `amount` amount of projectile in a single run
- `speed` projectile flying speed
- `burstCount` how many projectiles will be fired
- `burstInterval` ticks between next burst run
- `setFireballDirection` fireball will not float if set to `true`
- `yield` may cause explosion, defaults to `false`
- `isIncendiary` may cause fire, defaults to `false`
- `projectileType` type of projectile, can be
  - `arrow`
  - `snowball`
  - `fireball`
  - `smallfireball`
  - `llamaspit`
  - `shulkerbullet`
  - `dragonfireball`
  - `trident`
  - `skull`
- `suppressArrow` Cancels arrow when firing from bow, but arrow will still be consumed
- `applyForce` apply bow force for arrow and fireball, affects speed
- `firingLocation` Can be `SELF` or `TARGET`
- `firingR` Polar coordinates parameter
- `firingTheta` Polar coordinates parameter
- `firingPhi` Polar coordinates parameter
- `firingRange`
- `castOff`

## Pumpkin

Force target wear a pumpkin head.

## Rainbow

Shoots colorful wool.

## RealDamage

Deals real damage, ignores armor.

## Repair

Consume material to restore (or cost) durability.

- `durability` Durability restored (or cost) per each repair material
- `display` Message displayed in lore
- `material` Repair material. Can be `HAND` to use item hold in mainhand.
- `isSneak` Require sneak to trigger
- `mode` Can be `DEFAULT`, `ALLOW_OVER` and `ALWAYS`
- `allowBreak` defaults to `true`
- `abortOnSuccess` abort and break execution after successfully triggered (repaired)
- `abortOnFailure` abort and break execution after failed to repair (e.g., no material)
- `customMessage` Message to display in chat
- `amount` Maximum material amount to consume in each trigger
- `showFailMsg` Show repair failed message or not. Default to `true`

## Rescue

A better totem replacement.

## Rumble

Summon a wave to blow target to sky.

## Scoreboard

Operates on scoreboard.

## Shulkerbullet

Shoots shulkerbullet that homing to target.

## Skyhook

Hook to specific block.

## Sound

Plays sound on use or hit.

## Stuck

Cause targets not to move or teleport.

## TNTCannon

Shoots TNT.

## Teleport

Teleport for a distance.

## Throw

Throw entity.

## TippedArrow

Deprecated. Use Projectile and PotionHit

## Torch

Shoots some torches to light up a place.

## Translocator

Shoots a translocator and swap your position.
