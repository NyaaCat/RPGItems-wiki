# Triggers

Triggers are used to activate powers on an item. Each power can have one or more triggers assigned.

## General Trigger Properties

All triggers have a `priority` property that determines execution order when multiple triggers fire simultaneously.

Some triggers support additional filtering properties:

- `minDamage` Minimum damage threshold for damage-based triggers (HIT, HIT_GLOBAL, HIT_TAKEN, HURT, DYING)
- `maxDamage` Maximum damage threshold for damage-based triggers
- `minForce` Minimum bow force for BOW_SHOOT trigger
- `maxForce` Maximum bow force for BOW_SHOOT trigger

## Available Triggers

### Input Triggers

#### LEFT_CLICK
Triggered when left-clicking with the item in main hand.
- **Event**: PlayerInteractEvent
- **Power Interface**: PowerLeftClick

#### RIGHT_CLICK
Triggered when right-clicking with the item in main hand.
- **Event**: PlayerInteractEvent
- **Power Interface**: PowerRightClick

#### OFFHAND_CLICK
Triggered when clicking with the item in off hand.
- **Event**: PlayerInteractEvent
- **Power Interface**: PowerOffhandClick

#### SNEAK
Triggered once when the player starts sneaking.
- **Event**: PlayerToggleSneakEvent
- **Power Interface**: PowerSneak

#### SNEAKING
Triggered continuously every tick while the player is sneaking.
- **Event**: Generic tick event
- **Power Interface**: PowerSneaking
- **Note**: Uses custom data with LivingEntity and damage value

#### SPRINT
Triggered when the player starts sprinting.
- **Event**: PlayerToggleSprintEvent
- **Power Interface**: PowerSprint

#### JUMP
Triggered when the player jumps.
- **Event**: PlayerJumpEvent (Paper API)
- **Power Interface**: PowerJump

#### SWIM
Triggered when the player starts swimming.
- **Event**: EntityToggleSwimEvent
- **Power Interface**: PowerSwim

#### CONSUME
Triggered when consuming the item (food, potions).
- **Event**: PlayerItemConsumeEvent
- **Power Interface**: PowerConsume

### Combat Triggers

#### HIT
Triggered when hitting an entity with the item in main hand.
- **Event**: EntityDamageByEntityEvent
- **Power Interface**: PowerHit
- **Return Type**: Optional<Double> (modified damage)
- **Properties**: `minDamage`, `maxDamage` for filtering

#### HIT_GLOBAL
Triggered when hitting an entity, activates for ALL items in inventory except main hand.
- **Event**: EntityDamageByEntityEvent
- **Power Interface**: PowerHit
- **Return Type**: Optional<Double> (modified damage)
- **Properties**: `minDamage`, `maxDamage` for filtering

#### HIT_TAKEN
Triggered when taking damage. Calculated before damage is applied.
- **Event**: EntityDamageEvent
- **Power Interface**: PowerHitTaken
- **Return Type**: Optional<Double> (modified damage)
- **Properties**: `minDamage`, `maxDamage` for filtering
- **Note**: Can modify or cancel incoming damage

#### HURT
Triggered when taking damage. Calculated after damage is applied.
- **Event**: EntityDamageEvent
- **Power Interface**: PowerHurt
- **Properties**: `minDamage`, `maxDamage` for filtering

#### DYING
Triggered when taking potentially fatal damage.
- **Event**: EntityDamageEvent
- **Power Interface**: PowerHitTaken
- **Return Type**: Optional<Void>
- **Properties**: `minDamage`, `maxDamage` for filtering
- **Note**: Used for rescue/death prevention powers

### Ranged Combat Triggers

#### BOW_SHOOT
Triggered when shooting a bow (releasing the bowstring).
- **Event**: EntityShootBowEvent
- **Power Interface**: PowerBowShoot
- **Return Type**: Optional<Float> (bow force)
- **Properties**: `minForce`, `maxForce` for filtering
- **Note**: Requires arrow in inventory

#### PROJECTILE_HIT
Triggered when a projectile hits a target (entity or block).
- **Event**: ProjectileHitEvent
- **Power Interface**: PowerProjectileHit

#### PROJECTILE_LAUNCH
Triggered when a projectile is launched.
- **Event**: ProjectileLaunchEvent
- **Power Interface**: PowerProjectileLaunch

### Beam Triggers

#### BEAM_HIT_ENTITY
Triggered when a beam power hits an entity.
- **Event**: BeamHitEntityEvent (custom)
- **Power Interface**: PowerBeamHit
- **Return Type**: Optional<Double> (damage)

#### BEAM_HIT_BLOCK
Triggered when a beam power hits a block.
- **Event**: BeamHitBlockEvent (custom)
- **Power Interface**: PowerBeamHit

#### BEAM_END
Triggered when a beam power ends.
- **Event**: BeamEndEvent (custom)
- **Power Interface**: PowerBeamHit
- **Return Type**: Optional<Double> (final damage)

### Passive/Tick Triggers

#### TICK
Triggered every server tick while holding or wearing the item.
- **Event**: Server tick
- **Power Interface**: PowerTick
- **Note**: Activates for main hand and armor slots

#### TICK_INVENTORY
Triggered every server tick while item is anywhere in inventory.
- **Event**: Server tick
- **Power Interface**: PowerTick
- **Note**: Includes all inventory slots

#### TICK_OFFHAND
Triggered every server tick while holding item in off hand.
- **Event**: Server tick
- **Power Interface**: PowerTick

### Inventory Triggers

#### SWAP_TO_MAINHAND
Triggered when swapping item from off hand to main hand.
- **Event**: PlayerSwapHandItemsEvent
- **Power Interface**: PowerOffhandItem
- **Return Type**: Boolean

#### SWAP_TO_OFFHAND
Triggered when swapping item from main hand to off hand.
- **Event**: PlayerSwapHandItemsEvent
- **Power Interface**: PowerMainhandItem
- **Return Type**: Boolean

#### PLACE_OFF_HAND
Triggered when placing item in off hand via inventory.
- **Event**: InventoryClickEvent
- **Power Interface**: PowerMainhandItem
- **Return Type**: Boolean

#### PICKUP_OFF_HAND
Triggered when picking up item from off hand via inventory.
- **Event**: InventoryClickEvent
- **Power Interface**: PowerOffhandItem
- **Return Type**: Boolean

### Status Triggers

#### MENDING
Triggered when item is repaired via mending enchantment.
- **Event**: PlayerItemMendEvent
- **Power Interface**: PowerMending

#### EXP_CHANGE
Triggered when player's experience changes.
- **Event**: PlayerExpChangeEvent
- **Power Interface**: PowerExpChange

#### POTION_EFFECT
Triggered when a potion effect is applied.
- **Event**: EntityPotionEffectEvent
- **Power Interface**: PowerPotionEffect

### Special Triggers

#### ATTACHMENT
Triggered by attachment powers from other items.
- **Event**: Generic
- **Power Interface**: PowerAttachment
- **Note**: Uses original ItemStack and Event as context

#### LOCATION
Triggered with location context (used by certain powers).
- **Event**: Generic
- **Power Interface**: PowerLocation
- **Note**: Receives Location object as parameter

#### LIVINGENTITY
Triggered with living entity context (used by certain powers).
- **Event**: Generic
- **Power Interface**: PowerLivingEntity
- **Note**: Receives LivingEntity and damage value as parameters

## Usage Examples

### Single Trigger
```
/rpgitem power add mysword rpgitems:command triggers:RIGHT_CLICK command:`say Hello`
```

### Multiple Triggers
```
/rpgitem power add mysword rpgitems:particle triggers:HIT,RIGHT_CLICK particle:FLAME
```

### Damage-Filtered Trigger
```
/rpgitem power add mysword rpgitems:command triggers:HIT minDamage:5 command:`say Big hit!`
```

## Trigger Return Values

Some triggers can modify event outcomes:

| Trigger | Return Type | Effect |
|---------|-------------|--------|
| HIT | Double | Modifies dealt damage |
| HIT_GLOBAL | Double | Modifies dealt damage |
| HIT_TAKEN | Double | Modifies received damage |
| BOW_SHOOT | Float | Modifies bow force |
| SWAP_TO_* | Boolean | Can cancel swap |
| PLACE/PICKUP_OFF_HAND | Boolean | Can cancel action |
