# External Integrations

RPGItems supports integration with several popular plugins to extend functionality.

## MythicMobs

MythicMobs integration allows RPGItems to cast MythicMobs skills and be used as MythicMobs drops.

### Configuration

Enable in `config.yml`:
```yaml
support:
  MythicMobs:
    enable: true
```

### MythicSkillCast Power

Cast MythicMobs skills from RPGItems.

- `skill` MythicMobs skill name to cast (required)
- `cooldown` Cooldown in ticks (default: 20)
- `cost` Durability cost (default: 0)
- `suppressArrow` Cancel arrow when triggered by BOW_SHOOT (default: false)

Example:
```
/rpgitem power add mystaff rpgitems:mythicskillcast skill:Fireball cooldown:40 triggers:RIGHT_CLICK
```

### Using RPGItems as MythicMobs Drops

Add RPGItems to MythicMobs drop tables using the `rpg.` prefix:

```yaml
Drops:
  - rpg.mysword 1 0.5
```

Format: `rpg.<itemname> <amount> <chance>`

## PlaceholderAPI

PlaceholderAPI integration provides placeholders for use in other plugins and enables placeholder support in RPGItems expressions.

### Requirements

- PlaceholderAPI 2.10.2 or later

### Configuration

Enable in `config.yml`:
```yaml
support:
  PlaceholderAPI:
    enable: true
```

### Available Placeholders

Use these placeholders in other plugins:

| Placeholder | Description |
|-------------|-------------|
| `%rpgitems_items%` | Total number of RPGItems defined |
| `%rpgitems_item_display_<name>%` | Display name of the specified item |
| `%rpgitems_item_lores_<name>%` | Lore text of the specified item |

### Using Placeholders in RPGItems

PlaceholderAPI placeholders can be used in:

- **Command power**: Placeholders in command strings are resolved
- **EvalDamage power**: Placeholders in expressions are resolved
- **PlaceholderAPICondition**: Compare placeholder values

Example with Command power:
```
/rpgitem power add myitem rpgitems:command command:`tellraw {player} {"text":"Your level is %player_level%"}`
```

### PlaceholderAPICondition

Compare PlaceholderAPI values to control power execution.

- `placeholder` PlaceholderAPI placeholder (required)
- `operator` Comparison operator (required)
- `value` Value to compare against (required)

**Available Operators:**

| Operator | Description |
|----------|-------------|
| `==` | Equal (case-sensitive) |
| `!=` | Not equal (case-sensitive) |
| `===` | Equal (case-insensitive) |
| `!==` | Not equal (case-insensitive) |
| `contains` | Contains substring |
| `!contains` | Does not contain |
| `startwith` | Starts with |
| `!startwith` | Does not start with |
| `endwith` | Ends with |
| `!endwith` | Does not end with |
| `matches` | Matches regex |
| `!matches` | Does not match regex |
| `>=` | Greater than or equal (numeric) |
| `>` | Greater than (numeric) |
| `<` | Less than (numeric) |
| `<=` | Less than or equal (numeric) |

Example:
```
/rpgitem condition add myitem rpgitems:placeholderapicondition id:high-level placeholder:%player_level% operator:>= value:10
```

## WorldGuard

WorldGuard integration enables region-based control of RPGItems powers.

### Requirements

- WorldGuard 7.0.0-beta2 or later

### Configuration

Enable in `config.yml`:
```yaml
support:
  world_guard:
    enable: true
    force_refresh: false
    disable_in_no_pvp: false
    show_warning: true
```

| Option | Description |
|--------|-------------|
| `enable` | Enable WorldGuard integration |
| `force_refresh` | Refresh player region data every power use |
| `disable_in_no_pvp` | Disable all powers in no-PvP regions |
| `show_warning` | Show warning messages when powers are blocked |

### Region Flags

Configure RPGItems behavior per-region using WorldGuard region flags in `worldguard_region.yml`:

```yaml
regions:
  my_region:
    disabled_items:
      - mysword
      - "*"  # Disable all items
    enabled_items:
      - myhelmet  # Override to allow specific item
    disabled_powers:
      - "mysword:0-rpgitems:beam"
    enabled_powers: []
    warning_message: "&cRPGItems powers are disabled here!"
```

### Commands

| Command | Description |
|---------|-------------|
| `/rpgitem worldguard` | Toggle WorldGuard integration |
| `/rpgitem wgforcerefresh` | Toggle force refresh mode |
| `/rpgitem wgignore <item>` | Make item ignore WorldGuard restrictions |

## Residence

Residence integration enables flag-based control of RPGItems in residences.

### Configuration

Enable in `config.yml`:
```yaml
support:
  Residence:
    enable: true
```

### Custom Flag

RPGItems registers a custom flag `enable-rpgitems-power`:

- `true` - Allow RPGItems powers in this residence
- `false` - Block RPGItems powers in this residence

Set the flag using Residence commands:
```
/res set <residence> enable-rpgitems-power true
```

## Vault / Economy

Economy integration through Vault allows items to interact with server economy.

### Economy Power

Deposit or withdraw money when power activates.

- `amountToPlayer` Amount to give (positive) or take (negative)
- `coolDown` Cooldown in ticks (default: 0)
- `showFailMessage` Show error message on failure (default: true)
- `abortOnFailure` Abort power chain on failure (default: true)

Example - Cost 100 money to use:
```
/rpgitem power add myitem rpgitems:economy amountToPlayer:-100 abortOnFailure:true triggers:RIGHT_CLICK
```

Example - Reward 50 money on hit:
```
/rpgitem power add mysword rpgitems:economy amountToPlayer:50 triggers:HIT
```
