# Conditions

Conditions determine whether a power can be activated or not based on many different variables.

Conditions have ID, to be invoked in powers, triggers and condition themselves.

Multiple condition IDs are also acceptable, and treated as AND operation (all conditions must be met).

Basic command to modify conditions on an item:

```
/rpgitem condition [operation] [item] [params]
```

Available operations:

- `add` Add a condition with detailed parameters
- `remove` Remove a condition
- `list` List all conditions
- `prop` List detailed parameters of a specific condition, or modify it with additional parameters

E.g.,

```
/rpgitem condition add set-sword rpgitems:andcondition id:set-armor conditions:arm-helmet,arm-chest,arm-legs,arm-boot
```

Other general attributes:

- `isCritical` When `true`, and if such condition is not met, abort the whole power execution process.
- `isStatic` When `true`, condition value will not change in a single execution process, even following powers may change it, it's still considered met.

## AndCondition

Return true only if all conditions are met.

## OrCondition

Return true when one of the conditions is met.

## XorCondition

Return true when an odd number of conditions are met.

* Note: You need to set `isStatic` to `false`.

## SlotCondition

Return true when the item is in correct inventory slot.

## ChanceCondition

Return true in the chance of N percent.

## DamageTypeCondition

Return true when damageType is matched.

## DurabilityCondition

Return true when item durability is within range.

## EquipmentCondition

Return true when item is equipped in correct slot.

## ScoreboardCondition

Return true when scoreboard value is within range or have corresponding tag.

## EvalCondition

TBD

## LastResultCondition

Return true when last power is correctly executed.
