# Item Level System

> This page applies to RPGItems `3.9`.

## 1. Design Goals

Item Level is instance-based progression:

- Different instances of the same RPGItem template can have different levels
- Level integrates with socket validation and condition checks
- Level changes should update behavior and lore immediately

## 2. Core Behavior

- Default level per item instance: `1`
- Persisted in PDC key: `rgi_item_level`
- Managed by admin commands on held item instances

## 3. Runtime Integration (High Level)

Level affects:

1. `itemlevelcondition` evaluation
2. `LevelDescription` lore replacement

When level changes, runtime composite cache uses a new signature and refreshes behavior/display.

## 4. LevelDescription

`LevelDescription` lets you replace lore by level range, based on configured base `description`.

Example:

```yaml
description:
  - "Great Sword"
  - "Damage: &650"
leveldescription:
  "0":
    level: 3
    operation: replaceline
    line: 1
    newlines:
      - "Damage: &6100"
      - "Critical: &c20%"
  "1":
    level: 5
    operation: replacewhole
    line: 0
    newlines:
      - "Great Sword"
      - "Damage: &6200"
      - "Critical: &c25%"
      - "&7This is another lore"
```

Rules:

- `line` is `0-based`
- `replaceline`: replace starting from `line` with `newlines`
- `replacewhole`: ignore `line`, replace full description
- Select the smallest rule where `level >= currentLevel`
- If no rule matches, keep base `description`
- Replacement always uses base configured description, not previous level output

## 5. Relationship with Socketing

Level is part of socket validation:

- Socket is inactive when `itemLevel < socketMinLevel`
- GUI status item reports level-related failures

## 6. Usage Flow

```bash
# 1) Get held instance level
/rpgitem level get <item>

# 2) Set held instance level
/rpgitem level set <item> <level>

# 3) Configure itemlevelcondition / leveldescription
#    then verify behavior and lore updates
```

## 7. Command Reference

> Requires permission `rpgitem.level`.

### 7.1 Get

```bash
/rpgitem level get <item>
```

- Reads level from main-hand item instance
- `<item>` must match held RPGItem

### 7.2 Set

```bash
/rpgitem level set <item> <level>
```

- Writes level to main-hand item instance
- Minimum level is `1`
- Updates item display/runtime logic immediately

## 8. Condition: itemlevelcondition

Condition key: `rpgitems:itemlevelcondition`

Properties:

- `id`: condition ID (required)
- `level`: target level
- `operation`: comparison operator
  - `eq`: equal
  - `gt`: greater than
  - `egt`: greater or equal
  - `lt`: less than
  - `elt`: less or equal
- `isStatic`: default `true`
- `isCritical`: default `false`

Example:

```bash
/rpgitem condition add mysword rpgitems:itemlevelcondition id:need-lv5 level:5 operation:egt
```

Meaning: condition passes only when instance level is `>= 5`.

## 9. Recommended Practice

- Use `itemlevelcondition` for ability gating
- Use `LevelDescription` for visible progression text
- With socketing enabled, define `socketMinLevel` by progression stage
- Keep a consistent level-source policy to avoid live-instance drift
