# Powers

Power is the core feature set that actually implementing an item function.

Powers execute in pitfall order, and only correctly triggered powers will be executed.

## AOE

Applies effect to targets in an area.

- `cooldown` Cooldown in ticks
- `amplifier` Effect amplifier
- `duration` Effect duration
- `range` Radius of area
- `selfapplication` Whether the effect applies to yourself
- `name` Effect name, see https://hub.spigotmc.org/javadocs/spigot/org/bukkit/potion/PotionEffectType.html
- `cost` Cost to durability for each use

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

## Airborne

## Arrow

## Attachment

## Attract

## Beam

## CancelBowArrow

## Charge

## Command

## CommandHit

## Consume

## ConsumeHit

## CriticalHit

## DeathCommand

## Deflect

## DelayedCommand

## Dummy

## Economy

## EnchantedHit

## EvalDamage

## Explosion

## Fire

## Fireball

## Flame

## Food

## ForceField

## Glove

## Gunfu

## Headshot

## Ice

## Knockup

## Lifesteal

## Lightning

## Mount

## NoImmutableTick

## ParticleBarrier

## Particle

## ParticleTick

## PotionHit

## PotionSelf

## PotionTick

## Projectile

## Pumpkin

## Rainbow

## RealDamage

## Repair

## Rescue

## Rumble

## Scoreboard

## Shulkerbullet

## Skyhook

## Sound

## Stuck

## TNTCannon

## Teleport

## Throw

## TippedArrow

## Torch

## Translocator
