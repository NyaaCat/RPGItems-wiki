# Powers

Power is the core feature set that actually implementing an item function.

Powers execute in pitfall order, and only correctly triggered powers will be executed.

There are some general power properties:

- `cooldown` Cooldown ticks before next use
- `cost` Cost to durability for each use
- `conditions` Conditions to meet in order to trigger this power
- `triggers` Triggers of this power
- `displayName` Display name for this power
- `selectors` Entity selectors for filtering targets
- `powerId` Unique identifier for this power
- `powerTags` Tags for grouping powers
- `showCooldownWarning` Whether to show cooldown warning message (default: true)
- `requiredContext` Context required for this power to activate
- `requireHurtByEntity` When trigger is `HURT` or `HIT_TAKEN`, set to `true` to only trigger on entity damage (not fire, poison, etc.). Default: `true`
- `setBaseDamage` For damage-modifying powers, set to `true` to use dynamic damage as base for subsequent powers

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

Applies potion effect to targets in an area.

- `amplifier` Effect amplifier level (default: 1)
- `duration` Effect duration in ticks (default: 15)
- `range` Radius of area in blocks (default: 5)
- `selfapplication` Whether the effect applies to yourself (default: true)
- `type` Effect type, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/potion/PotionEffectType.html
- `display` Custom display text
- `count` Maximum number of targets, -1 for unlimited (default: -1)
- `angle` Cone angle from your direction, 0 to 180 (default: 180)
- `target` Target filter: `ALL`, `MOBS`, or `PLAYERS` (default: ALL)

## AOECommand

Let all targets within a range execute a command.

- `type` Target type: `entity`, `player`, or `mobs` (default: entity)
- `r` Maximum target range in blocks (default: 10)
- `rm` Minimum range in blocks (default: 0)
- `facing` Angle from your direction, value from 0 to 180 (default: 30)
- `c` Maximum target count (default: 100)
- `mustsee` Require line of sight to target (default: false)
- `command` The command to execute
- `permission` Permission to assign for execution of the command
- `selfapplication` Include yourself in targets (default: false)

## AOEDamage

Deals damage to targets in an area.

- `range` Maximum target selection range in blocks (default: 10)
- `minrange` Minimum target selection range (default: 0)
- `angle` Cone angle from your direction, 0 to 180 (default: 180)
- `count` Maximum number of targets (default: 100)
- `includePlayers` Include players when triggering power (default: false)
- `selfapplication` Apply to yourself when triggering (default: false)
- `mustsee` Require line of sight to target (default: false)
- `damage` Damage amount, independent of item damage property (default: 0)
- `delay` Delay in ticks before applying damage (default: 0)
- `selectAfterDelay` When delay > 0, set to `false` to select targets on use (default: false)
- `suppressMelee` Suppress melee hit trigger (default: false)
- `firingLocation` AOE center location: `SELF` or `TARGET` (default: SELF)
- `firingRange` Maximum distance for TARGET firing location (default: 64)
- `castOff` When `firingLocation` is `TARGET` and delay > 0, set to `true` to set center before delay (default: false)

## Airborne

Damage boost when flying or in the air.

- `percentage` Damage increase percentage (default: 50)
- `cap` Maximum damage cap (default: 300.0)
- `setBaseDamage` Use dynamic damage as base for subsequent powers (default: false)

## Arrow

Fires an arrow on use. This is the basic arrow power - for more advanced projectiles, use `Projectile`.

- `cooldown` Cooldown in ticks (default: 0)
- `cost` Durability cost (default: 0)
- `setShooter` Whether to set the player as arrow shooter (default: false)

## Attachments

Trigger powers from other RPG items in inventory.

- `allowInvSlots` Allowed inventory slots, -1 to disable
- `allowItems` Allowed RPGItem names
- `limit` Maximum number of attachments

## Attract

Pull or push entities toward/away from a location.

- `radius` Selection radius in blocks (default: 5)
- `maxSpeed` Maximum pull/push speed (default: 0.4)
- `duration` Duration in ticks (default: 5)
- `attractingTickCost` Durability cost per tick (default: 0)
- `attractingEntityTickCost` Durability cost per entity per tick (default: 0)
- `attractPlayer` Whether to affect players (default: false)
- `firingLocation` Effect center: `SELF` or `TARGET` (default: SELF)
- `firingRange` Maximum distance for TARGET (default: 64)

## Beam

Fire a beam made of particles. This is the most complex and powerful power in RPGItems.

### Basic Properties
- `length` Maximum beam length in blocks (default: 10)
- `ttl` Time to live in ticks (default: 100)
- `particle` Particle type, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Particle.html (default: LAVA)
- `mode` Beam mode: `BEAM` (instant) or `PROJECTILE` (travels over time) (default: BEAM)
- `damage` Damage dealt on contact (default: 20)
- `speed` Beam travel speed in blocks per second (default: 20)

### Piercing and Collision
- `pierce` Number of entities the beam can pierce through (default: 0)
- `ignoreWall` Pass through solid blocks (default: false)
- `bounce` Number of times beam bounces off walls, requires `ignoreWall:false` (default: 0)
- `hitSelfWhenBounced` Beam can hit you after bouncing (default: false)

### Particle Settings
- `offsetX` Particle X offset (default: 0)
- `offsetY` Particle Y offset (default: 0)
- `offsetZ` Particle Z offset (default: 0)
- `particleSpeed` Particle speed parameter (default: 0)
- `particleDensity` Particles per block (default: 2)
- `extraData` Color for DUST particle: `r,g,b,size`. E.g., `extraData:51,204,255,0.35` for cyan

### Spread and Burst
- `cone` Angle for spread fire (default: 0)
- `beamAmount` Beams fired per burst (default: 1)
- `burstCount` Number of bursts per trigger (default: 1)
- `burstInterval` Ticks between bursts (default: 10)

### Homing
- `homing` Homing radius, 0 to disable (default: 0)
- `homingAngle` Maximum angle to acquire target (default: 30)
- `homingRange` Maximum range to acquire target (default: 50)
- `homingMode` Homing behavior:
  - `ONE_TARGET` Lock onto single target
  - `MULTI_TARGET` Switch targets after each hit
  - `MOUSE_TRACK` Follow cursor aim
- `homingTarget` What to home on: `MOBS`, `ENTITY`, or `PLAYER` (default: MOBS)
- `ticksBeforeHoming` Delay before homing activates (default: 0)

### Physics
- `gravity` Beam gravity, can be negative for upward curve (default: 0)
- `speedBias` Expression to control speed. Variables: `x` (distance), `t` (time). E.g., `4+x*10*t`

### Advanced
- `behavior` List of behavior modifiers. Available behaviors:
  - `PLAIN` - Standard straight beam
  - `DNA` - Double helix spiral pattern
  - `CIRCLE` - Circular rotating pattern
  - `LEGACY_HOMING` - Legacy homing algorithm
  - `RAINBOW_COLOR` - Color-cycling rainbow effect
  - `CONED` - Cone-shaped spread pattern
  - `FLAT` - Flat horizontal beam
  - `UNIFORMED` - Uniform particle distribution
  - `CAST_LOCATION_ROTATED` - Rotates around cast point
- `behaviorParam` JSON parameters for behaviors (default: "{}")
- `initialRotation` Initial rotation angle (default: 0)
- `effectOnly` Only visual, won't trigger HIT events (default: false)
- `suppressMelee` Suppress melee damage on hit (default: false)

### Firing Location
- `firingLocation` Launch point: `SELF` or `TARGET` (default: SELF)
- `firingRange` Maximum range for TARGET location (default: 64)
- `firingR` Polar coordinate R offset
- `firingTheta` Polar coordinate theta offset
- `firingPhi` Polar coordinate phi offset
- `castOff` Lock target location before delay (default: false)

## CancelBowArrow

Cancels bow arrow. Arrow will not be consumed and no arrow is fired. Often used on bows with other powers.

- `cancelArrow` Whether to cancel the arrow (default: true)

## Charge

Damage boost when sprinting.

- `percentage` Base damage increase percentage (default: 30)
- `speedPercentage` Additional percentage based on speed (default: 20)
- `cap` Maximum damage cap (default: 300)
- `setBaseDamage` Use dynamic damage as base (default: false)

## Command

Execute command on use.

- `command` Command to execute. Use backticks for spaces: `` command:`minecraft:give {player} stone` ``
- `display` Message shown in item lore (default: "Runs command")
- `permission` Permission granted temporarily for command execution. Special values:
  - `console` Execute as console
  - `*` Execute with OP permissions
- `cooldown` Cooldown in ticks (default: 0)
- `cost` Durability cost (default: 0)

Variables available in command:

- `{player}` Player name
- `{player.x}` Player X coordinate
- `{player.y}` Player Y coordinate
- `{player.z}` Player Z coordinate
- `{player.yaw}` Player yaw rotation
- `{player.pitch}` Player pitch rotation
- `{yaw}` Player yaw (alternative format)
- `{pitch}` Player pitch (alternative format)

If PlaceholderAPI is installed, placeholders are resolved before execution.

## CommandHit

Execute command on hit.

- `command` Command to execute
- `permission` Permission for command execution
- `minDamage` Minimum damage required to trigger (default: 0)

Inherits all variables from Command, plus:

- `{entity}` Target entity name
- `{entity.uuid}` Target UUID
- `{entity.x}` Target X coordinate
- `{entity.y}` Target Y coordinate
- `{entity.z}` Target Z coordinate
- `{entity.yaw}` Target yaw rotation
- `{entity.pitch}` Target pitch rotation
- `{damage}` Damage dealt

## Consume

Consume item on use.

## ConsumeHit

Consume item on hit.

## CriticalHit

Deals critical hit with configurable chance and multipliers.

- `chance` Critical hit chance percentage (default: 20)
- `backstabChance` Backstab critical chance percentage (default: 0)
- `factor` Critical damage multiplier (default: 1.5)
- `backstabFactor` Backstab damage multiplier (default: 1.5)
- `setBaseDamage` Use dynamic damage as base (default: false)

## Dash

Dash in a specified direction. Applies velocity to the player.

- `direction` Dash direction (default: FORWARD):
  - `FORWARD` - Direction player is facing
  - `BACKWARD` - Opposite to facing direction
  - `LEFT` - Left relative to facing
  - `RIGHT` - Right relative to facing
  - `UP` - Straight up
  - `DOWN` - Straight down
  - `RANDOM` - Random 3D direction
  - `RANDOM_HORIZONTAL` - Random horizontal direction
  - `RANDOM_VERTICAL` - Random vertical direction
  - `NORTH` - World north (-Z)
  - `SOUTH` - World south (+Z)
  - `EAST` - World east (+X)
  - `WEST` - World west (-X)
- `speed` Dash speed multiplier (default: 1)

## DeathCommand

A chance to instantly kill target and execute command.

- `chance` Kill probability as 1/N (default: 20)
- `command` Command to execute on kill
- `desc` Description text for item lore
- `count` Number of times to execute command (default: 1)
- `permission` Permission for command execution

## Deflect

Deflect incoming projectiles.

- `chance` Passive deflect chance percentage (default: 50)
- `cooldownpassive` Passive deflect cooldown in ticks (default: 0)
- `duration` Active deflect duration in ticks (default: 50)
- `facing` Maximum deflect angle from your direction, 0 to 180 (default: 30)
- `deflectCost` Durability cost per deflection (default: 0)

## DelayedCommand

Execute command after a delay.

- `delay` Delay in ticks before execution (default: 20)
- `command` Command to execute
- `display` Display text for item lore
- `permission` Permission for command execution
- `cmdInPlace` Execute at original location (default: false)

## Dummy

Power dummy does nothing but provides control options for power execution flow.

- `checkDurabilityBound` Check durability bounds before trigger (default: true)
- `costByEnchantment` Use enchantment for cost calculation (default: false)
- `doEnchReduceCost` Enchantment can reduce cost (default: false)
- `enchCostPercentage` Cost reduction per enchantment level (default: 6)
- `enchantmentType` Enchantment name (default: "unbreaking")
- `costByDamage` Cost based on damage dealt (default: false)
- `cooldownKey` Unique key for independent cooldowns (default: "dummy")
- `successResult` Action after success: `OK`, `ABORT` (default: OK)
- `costResult` Action after cost: `COST`, `ABORT`, `OK` (default: COST)
- `cooldownResult` Action on cooldown: `COOLDOWN`, `ABORT`, `OK` (default: COOLDOWN)
- `showCDWarning` Show cooldown warning message (default: true)
- `globalCooldown` Apply cooldown globally to all players (default: false)
- `display` Display text

**Use PowerDummy to disable cooldown messages, unify and control cooldown / cost.**

Powers execute based on triggers like `RIGHT_CLICK` or `LEFT_CLICK`, activating powers one by one in order. For example:

1. PowerProjectile, trigger `LEFT_CLICK`
2. PowerBeam, trigger `RIGHT_CLICK`
3. PowerSound, trigger `RIGHT_CLICK`
4. PowerParticle, trigger `LEFT_CLICK`

Left click only activates 1 and 4 in a single tick.

To cost 1 durability per left click with 20 tick cooldown and no message:
1. Add PowerDummy first: `cooldown:20 cooldownKey:left cooldownResult:ABORT cost:1 showCDWarning:false triggers:LEFT_CLICK`
2. Set other powers' `cost` and `cooldown` to 0

## Economy

Operates economy functions (requires Vault).

- `coolDown` Cooldown in ticks (default: 0)
- `amountToPlayer` Amount to give (positive) or take (negative)
- `showFailMessage` Show error message on failure
- `abortOnFailure` Abort power chain on failure (default: true)

## EnchantedHit

Make enchantment boost damage.

- `mode` Calculation mode: `ADDITION` or `MULTIPLICATION` (default: ADDITION)
- `amountPerLevel` Damage increase per enchant level, 1 = 100% (default: 1)
- `enchantmentType` Enchantment type, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/enchantments/Enchantment.html (default: POWER)
- `setBaseDamage` Use dynamic damage as base (default: false)
- `display` Display text

## EvalDamage

Calculate damage using mathematical expressions. Uses the EvalEx library.

- `expression` Expression for entity damage calculation (required)
- `playerExpression` Separate expression when target is a player
- `setBaseDamage` Use result as base damage (default: false)
- `display` Display text

### Available Variables

- `damage` - Base damage value
- `finalDamage` - Final calculated damage
- `attackCooldown` - Player attack cooldown (0.0 to 1.0)
- `isDamageByProjectile` - 1 if projectile damage, 0 otherwise
- `damagerTicksLived` - How long the damager has existed (ticks)
- `distance` - Distance between player and target
- `playerYaw`, `playerPitch` - Player rotation
- `playerX`, `playerY`, `playerZ` - Player coordinates
- `entityType` - Target entity type name (e.g., "ZOMBIE")
- `entityYaw`, `entityPitch` - Entity rotation
- `entityX`, `entityY`, `entityZ` - Entity coordinates
- `entityLastDamage` - Entity's last damage taken
- `cause` - Damage cause enum name (e.g., "ENTITY_ATTACK")

### Available Functions

- `scoreBoard(name)` - Get player's scoreboard score
- `context(key)` - Get value from power context
- `now()` - Current timestamp in milliseconds

### PlaceholderAPI Support

If PlaceholderAPI is installed, placeholders in expressions are resolved before evaluation.
Use `target:` prefix in playerExpression for target player placeholders.

Example:
```
/rpgitem power add mysword rpgitems:evaldamage expression:"damage * (1 + distance / 10)"
```

## Explosion

Cause explosion on use.

- `distance` Maximum distance for explosion (default: 20)
- `chance` Trigger chance percentage (default: 20)
- `explosionPower` Explosion power/radius (default: 4.0)

## Fire

Create a trail of fire.

- `distance` Fire trail length in blocks (default: 15)
- `burnduration` Burn duration in ticks (default: 40)

## Fireball

Deprecated. Use `Projectile`.

## Flame

Ignite hit targets.

- `burntime` Burn duration in ticks (default: 20)

## Food

Restores hunger/saturation.

- `foodpoints` Food points to restore (required)

## ForceField

Summon a force field barrier.

- `radius` Barrier radius in blocks (default: 5)
- `height` Barrier height in blocks (default: 30)
- `base` Barrier base offset, can be negative (default: -15)
- `ttl` Time to live in ticks (required, default: 100)
- `wallMaterial` Material for walls (default: WHITE_WOOL)
- `barrierMaterial` Material for barrier blocks (default: BARRIER)

## Glove

Catch and throw entities.

- `maxDistance` Maximum catch distance (default: 5)
- `maxTicks` Maximum hold time in ticks (default: 200)
- `throwSpeed` Throw velocity (default: 0.0)

## GunFu

Make projectiles home toward targets.

- `distance` Target detection range (default: 20)
- `viewAngle` Maximum angle to detect targets (default: 30)
- `initVelFactor` Initial velocity factor (default: 0.5)
- `velFactor` Velocity adjustment per tick (default: 0.05)
- `forceFactor` Homing force multiplier (default: 1.5)
- `maxTicks` Maximum homing duration (default: 200)
- `delay` Delay before homing starts (default: 0)

## Headshot

Damage boost when hitting target's head.

- `factor` Damage multiplier (default: 1.5)
- `particleEnemy` Show particles on hit enemy (default: true)
- `soundSelf` Play sound for shooter (default: true)
- `soundEnemy` Play sound for target (default: false)
- `setBaseDamage` Use dynamic damage as base (default: false)

## Ice

Shoots ice and creates ice blocks.

## Knockup

Launch target into the air on hit.

- `chance` Trigger probability as 1/N (default: 20)
- `knockUpPower` Launch power (default: 2)

## LifeSteal

Heal based on damage dealt.

- `chance` Trigger probability as 1/N (default: 20)
- `factor` Heal multiplier (default: 1)

## Lightning

Summon lightning strike.

- `chance` Strike probability as 1/N (default: 20)

## Mending

Enhanced mending behavior.

- `repairFactor` Experience to durability ratio (default: 2)
- `requireEnchantment` Require mending enchantment (default: false)

## Mount

Ride on entities.

- `maxDistance` Maximum mount distance (default: 5)
- `maxTicks` Maximum ride duration (default: 200)

## MythicSkillCast

Cast MythicMobs skills (requires MythicMobs plugin).

- `skill` Skill name to cast
- `suppressArrow` Cancel arrow when cast (default: false)
- `applyForce` Force value (default: "NaN")

## NoImmutableTick

Reduce or remove damage immunity ticks.

- `immuneTime` Immunity time in ticks (default: 1)

## ParticleBarrier

Convert incoming damage to particle barrier energy.

- `barrierHealth` Barrier health pool (default: 40)
- `energyPerBarrier` Energy gained per barrier (default: 40)
- `energyDecay` Energy decay per second (default: 1.5)
- `energyPerLevel` Energy needed per effect level (default: 10)
- `projected` Whether barrier is projected (default: false)
- `effect` Potion effect granted by barrier (default: STRENGTH)

## Particle

Play particles on use or hit.

- `particle` Particle type
- `effect` Effect type (legacy)
- `particleCount` Number of particles (default: 1)
- `offsetX` X spread (default: 0)
- `offsetY` Y spread (default: 0)
- `offsetZ` Z spread (default: 0)
- `extra` Extra data/speed (default: 1)
- `force` Force render for all players (default: false)
- `material` Material for block particles
- `dustColor` Color for dust particles as integer (default: 0)
- `dustSize` Size for dust particles (default: 0)
- `playLocation` Where to spawn: `SELF`, `HIT_LOCATION`, `TARGET`, `ENTITY` (default: HIT_LOCATION)
- `delay` Delay in ticks (default: 0)
- `firingRange` Range for TARGET location (default: 20)

## ParticleTick

Continuously play particles while holding or wearing.

All parameters from Particle, plus:
- `interval` Spawn interval in ticks (default: 15)

## PotionHit

Apply potion effect on hit.

- `type` Potion effect type (required)
- `duration` Effect duration in ticks (default: 20)
- `amplifier` Effect level (default: 1)
- `chance` Trigger probability as 1/N (default: 20)
- `summingUp` Stack amplifier with other PotionHit powers (default: false)

## PotionSelf

Apply potion effect to yourself.

- `type` Potion effect type
- `duration` Effect duration in ticks
- `amplifier` Effect level
- `clear` Clear all effects instead (default: false)

## PotionTick

Continuously apply potion effect while holding or wearing.

- `type` Potion effect type
- `amplifier` Effect level
- `interval` Application interval in ticks

## Projectile

Fire projectiles with extensive options.

### Basic Settings
- `projectileType` Projectile type (required):
  - `arrow`, `snowball`, `fireball`, `smallfireball`
  - `llamaspit`, `shulkerbullet`, `dragonfireball`
  - `trident`, `skull`, `egg`
- `speed` Projectile speed (default: 1)
- `gravity` Affected by gravity (default: true)

### Spread and Burst
- `isCone` Fire in a cone pattern (default: false)
- `range` Cone angle (default: 15)
- `amount` Projectiles per burst (default: 5)
- `burstCount` Number of bursts (default: 1)
- `burstInterval` Ticks between bursts (default: 1)

### Projectile Behavior
- `setFireballDirection` Fireball follows aim (default: false)
- `yield` Explosion radius (default: 0)
- `isIncendiary` Causes fire (default: false)
- `pierceLevel` Pierce through entities (default: 0)
- `suppressArrow` Cancel arrow from bow (default: false)
- `applyForce` Apply bow draw force (default: false)

### Egg Settings
- `eggShouldHatch` Egg can spawn chicken (default: true)
- `eggHatchEntity` Entity spawned (default: "CHICKEN")
- `eggHatchNumber` Number spawned, -1 for random (default: -1)

### Fireball Settings
- `fireballItem` Item displayed in fireball (default: "FIRE_CHARGE")

### Firing Location
- `firingLocation` Launch from: `SELF` or `TARGET` (default: SELF)
- `firingRange` Range for TARGET location (default: 64)
- `firingR` Polar coordinate R offset
- `firingTheta` Polar coordinate theta offset
- `firingPhi` Polar coordinate phi offset
- `initialRotation` Initial rotation angle (default: 0)
- `castOff` Lock target before delay (default: false)

## Pumpkin

Force target to wear a pumpkin head.

- `chance` Trigger probability as 1/N (default: 20)
- `drop` Pumpkin drop chance (default: 0)

## Rainbow

Shoots colorful wool blocks.

- `count` Number of wool blocks (default: varies)
- `isFire` Add fire (default: false)

## RealDamage

Deal damage that bypasses armor.

- `realDamage` Amount of true damage to deal
- `minDamage` Minimum hit damage required to trigger (default: 0)

## Repair

Consume materials to repair or damage durability.

- `durability` Durability change per material (positive = repair)
- `material` Repair material, or `HAND` for held item
- `display` Message in item lore
- `isSneak` Require sneaking to trigger
- `mode` Mode: `DEFAULT`, `ALLOW_OVER`, `ALWAYS` (default: DEFAULT)
- `allowBreak` Allow item to break (default: true)
- `abortOnSuccess` Stop chain after successful repair (default: false)
- `abortOnFailure` Stop chain on failure (default: false)
- `customMessage` Chat message on repair
- `amount` Maximum materials consumed per use
- `showFailMsg` Show failure message (default: true)

## Rescue

Prevent death and teleport to safety.

- `healthTrigger` Health threshold to trigger rescue (default: 4)
- `damageTrigger` Damage threshold to trigger rescue (default: 1024)
- `useBed` Teleport to bed spawn (default: true)
- `inPlace` Rescue in place instead of teleport (default: false)

## Rumble

Send a shockwave through the ground.

- `power` Knockup power (default: 2)
- `distance` Wave travel distance (default: 15)
- `damage` Damage dealt (default: 0)

## Scoreboard

Modify player scoreboard values and tags.

- `objective` Scoreboard objective name
- `scoreOperation` Operation: `NO_OP`, `ADD_SCORE`, `SET_SCORE`, `RESET_SCORE` (default: NO_OP)
- `value` Score value to set/add (default: 0)
- `tag` Tags to add/remove: `ADD,!REMOVE` format
- `team` Teams to join/leave: `JOIN,!LEAVE` format
- `delay` Delay in ticks (default: 20)
- `reverseTagAfterDelay` Undo tag changes after delay (default: false)
- `abortOnSuccess` Stop chain after success (default: false)

## ShulkerBullet

Shoots homing shulker bullets.

- `range` Homing range (default: varies)

## SkyHook

Hook onto specific blocks and travel along them.

- `railMaterial` Block material to hook (default: GLASS)
- `hookDistance` Maximum hook distance (default: 10)
- `hookingTickCost` Durability cost per tick while hooked (default: 0)

## Sound

Play sound on use or hit.

- `sound` Sound name (Minecraft sound ID)
- `volume` Sound volume (default: 1.0)
- `pitch` Sound pitch (default: 1.0)
- `delay` Delay in ticks (default: 0)
- `playLocation` Where to play: `SELF`, `HIT_LOCATION`, `TARGET` (default: HIT_LOCATION)
- `firingRange` Range for TARGET location (default: 20)
- `display` Display text (default: "Plays sound")

## Stuck

Prevent targets from moving or teleporting.

- `chance` Trigger probability as 1/N (default: 3)
- `duration` Stuck duration in ticks (default: 100)
- `range` AOE range in blocks (default: 10)
- `facing` Cone angle (default: 30)
- `costAoe` Durability cost for AOE use (default: 0)
- `costPerEntity` Durability cost per entity (default: 0)

## TNTCannon

Shoots primed TNT.

## Teleport

Teleport in the direction you're looking.

- `distance` Maximum teleport distance (default: 5)
- `targetMode` How to find destination:
  - `DEFAULT` Block iteration
  - `RAY_TRACING` Ray trace to surface
  - `RAY_TRACING_EXACT` Precise ray trace
  - `RAY_TRACING_SWEEP` Ray trace with player collision
  - `RAY_TRACING_EXACT_SWEEP` Precise with collision
- `particle` Particle effect (default: "PORTAL")
- `sound` Sound effect (default: "entity.enderman.teleport")

## Throw

Spawn and throw entities.

- `entityName` Entity type to throw (required)
- `speed` Throw velocity (required)
- `display` Display text for lore (required)
- `entityData` Additional entity NBT data
- `isPersistent` Save entity to world (default: false)

## TippedArrow

Deprecated. Use Projectile and PotionHit together.

## Torch

Throws torches to light up areas.

## Translocator

Throw a beacon and teleport to it.

- `speed` Beacon throw speed
- `setupCost` Durability cost to throw beacon
- `tpCost` Durability cost to teleport

## Undead

Special effects for undead targets.
