# Advanced Features

This page documents advanced commands and configuration options for power users.

## Template System

Templates allow you to create item blueprints with configurable placeholders.

### Commands

| Command | Description |
|---------|-------------|
| `/rpgitem template create <item> <placeholder1> <placeholder2> ...` | Create template with placeholders |
| `/rpgitem template delete <item>` | Delete template |
| `/rpgitem template apply <item>` | Apply template to all instances |
| `/rpgitem template placeholder add <item> <placeholder>` | Add placeholder |
| `/rpgitem template placeholder remove <item> <placeholder>` | Remove placeholder |
| `/rpgitem template placeholder list <item>` | List placeholders |

### Placeholder Syntax

Placeholders use the format `<powerId:propertyName>`:

```
/rpgitem template create mysword <0-rpgitems:beam:damage> <0-rpgitems:beam:length>
```

This creates a template where damage and length can be customized per instance.

## Item Groups

Organize items into groups for easier management.

| Command | Description |
|---------|-------------|
| `/rpgitem creategroup <name> <item1> <item2> ...` | Create group with items |
| `/rpgitem creategroup <name> /<regex>/` | Create group matching regex |
| `/rpgitem addtogroup <item> <group>` | Add item to group |
| `/rpgitem removefromgroup <item> <group>` | Remove item from group |
| `/rpgitem listgroup <group>` | List items in group |
| `/rpgitem removegroup <group>` | Delete group |

Example:
```
/rpgitem creategroup weapons sword_* dagger_*
/rpgitem creategroup armor /.*_armor$/
```

## Meta Commands

Set item metadata for quality tiers and types.

### Quality System

```
/rpgitem meta quality <item> <quality>
```

Quality prefixes are configured in `config.yml`:
```yaml
item:
  quality:
    trash: "&7[Trash] "
    normal: "&f[Normal] "
    rare: "&9[Rare] "
    epic: "&5[Epic] "
    legendary: "&6[Legendary] "
```

### Type Metadata

```
/rpgitem meta type <item> <type>
```

Sets custom type metadata for the item.

## Modifier System

Apply dynamic modifiers to items in player hands or stored player data.

| Command | Description |
|---------|-------------|
| `/rpgitem modifier add <player/hand> <modifier> [properties]` | Add modifier |
| `/rpgitem modifier prop <player/hand> <id> [properties]` | Modify properties |
| `/rpgitem modifier remove <player/hand> <id>` | Remove modifier |
| `/rpgitem modifier list` | List available modifiers |

## Custom Triggers

Create and manage custom trigger configurations.

| Command | Description |
|---------|-------------|
| `/rpgitem trigger add <item> <power> <name> [properties]` | Add custom trigger |
| `/rpgitem trigger prop <item> <name> [properties]` | Modify trigger |
| `/rpgitem trigger remove <item> <name>` | Remove trigger |
| `/rpgitem trigger list` | List triggers |

## Power Commands

Manage powers attached to items.

| Command | Description |
|---------|-------------|
| `/rpgitem power add <item> <power> [properties]` | Add power to item |
| `/rpgitem power prop <item> <index> [properties]` | View/modify power properties |
| `/rpgitem power remove <item> <index>` | Remove power from item |
| `/rpgitem power reorder <item> <from> <to>` | Reorder power position |
| `/rpgitem power list` | List available powers |

## Condition Commands

Manage conditions attached to items.

| Command | Description |
|---------|-------------|
| `/rpgitem condition add <item> <condition> [properties]` | Add condition |
| `/rpgitem condition prop <item> <index> [properties]` | View/modify condition |
| `/rpgitem condition remove <item> <index>` | Remove condition |
| `/rpgitem condition list` | List available conditions |

## Marker Commands

Manage markers attached to items.

| Command | Description |
|---------|-------------|
| `/rpgitem marker add <item> <marker> [properties]` | Add marker |
| `/rpgitem marker prop <item> <index> [properties]` | View/modify marker |
| `/rpgitem marker remove <item> <index>` | Remove marker |
| `/rpgitem marker list` | List available markers |

## Import/Export

Share items via GitHub Gist.

### Export

```
/rpgitem export <items> [GIST] [token:<token>] [publish:<true/false>] [description:<text>]
```

- Without `GIST`: Exports to file
- With `GIST`: Uploads to GitHub Gist (requires token)

### Import

```
/rpgitem import GIST <gist_id> [token:<token>]
```

### Configuration

```yaml
gist:
  token: "your_github_token"
  publish: false
```

## GUI Browser

```
/rpgitem gui [page:<page>] [displayFilter:<text>] [loreFilter:<text>] [nameFilter:<text>]
```

Opens an inventory GUI for browsing and managing items with optional filters.

## Item Modes

### Update Mode

Controls how items update when held:

```
/rpgitem updatemode <item> <mode>
```

| Mode | Description |
|------|-------------|
| `FULL_UPDATE` | Update all properties |
| `DISPLAY_ONLY` | Only update display name |
| `LORE_ONLY` | Only update lore |
| `ENCHANT_ONLY` | Only update enchantments |
| `NO_DISPLAY` | Skip display update |
| `NO_LORE` | Skip lore update |
| `NO_ENCHANT` | Skip enchantment update |
| `NO_UPDATE` | No automatic updates |

### Damage Mode

Controls damage calculation:

```
/rpgitem damagemode <item> <mode>
```

| Mode | Description |
|------|-------------|
| `FIXED` | Use fixed damage value |
| `FIXED_WITHOUT_EFFECT` | Fixed damage, no effects |
| `FIXED_RESPECT_VANILLA` | Fixed damage with vanilla modifiers |
| `FIXED_WITHOUT_EFFECT_RESPECT_VANILLA` | Fixed damage without effects, with vanilla modifiers |
| `VANILLA` | Standard Minecraft damage |
| `ADDITIONAL` | Add to vanilla damage |
| `ADDITIONAL_RESPECT_VANILLA` | Additional with vanilla modifiers |
| `MULTIPLY` | Multiply vanilla damage |

### Attribute Mode

```
/rpgitem attributemode <item> <mode>
```

| Mode | Description |
|------|-------------|
| `FULL_UPDATE` | Fully update attributes |
| `PARTIAL_UPDATE` | Partially update attributes |

### Enchant Mode

```
/rpgitem enchantmode <item> <mode>
```

| Mode | Description |
|------|-------------|
| `DISALLOW` | Cannot be enchanted |
| `PERMISSION` | Requires permission to enchant |
| `ALLOW` | Can be enchanted normally |

## Durability Bar Formats

```
/rpgitem durability <item> barformat <format>
```

| Format | Description |
|--------|-------------|
| `DEFAULT` | Standard durability bar |
| `NUMERIC` | Numeric display |
| `NUMERIC_MINUS_ONE` | Numeric minus one |
| `NUMERIC_BIN` | Binary display |
| `NUMERIC_BIN_MINUS_ONE` | Binary minus one |
| `NUMERIC_HEX` | Hexadecimal display |
| `NUMERIC_HEX_MINUS_ONE` | Hexadecimal minus one |

## Paper API Features (1.20+)

### Custom Model Data

Extended custom model data with multiple types:

```
/rpgitem customModel <item> [floats:<values>] [strings:<values>] [flags:<values>] [colors:<values>]
```

- `floats:` Semicolon-separated float values
- `strings:` Semicolon-separated strings (escape with `\;`)
- `flags:` Semicolon-separated boolean values
- `colors:` Semicolon-separated RGB triplets (r,g,b)

Example:
```
/rpgitem customModel mysword floats:1.5;2.0 strings:effect1;effect2 flags:true;false colors:255,0,0;0,255,0
```

### Item Model

Set custom item model:

```
/rpgitem itemmodel <item> <namespace:key>
/rpgitem itemmodel <item>  # Remove model
```

## Debugging

| Command | Description |
|---------|-------------|
| `/rpgitem debug` | Debug item in hand (Paper: uses `paper dumpitem`) |
| `/rpgitem dump <item>` | Dump item YAML configuration |
| `/rpgitem loadfile <path>` | Load item from file path |

## Item Management

| Command | Description |
|---------|-------------|
| `/rpgitem backupitem <item>` | Create backup (unlocks for editing) |
| `/rpgitem cleanbackup` | Clean all backup files |
| `/rpgitem reloaditem <item>` | Reload single item from disk |

## Additional Properties

### Damage Type

```
/rpgitem damageType <item> [type]
```

Types: `melee`, `ranged`, `magic`, `summon`

### Armor Expression

Custom armor calculation expressions:

```
/rpgitem armorExpression <item> <expression>
/rpgitem playerArmorExpression <item> <expression>
```

### Item Permissions

```
/rpgitem canUse <item> <true/false>
/rpgitem canPlace <item> <true/false>
```

### Item Metadata

```
/rpgitem author <item> [@player]
/rpgitem note <item> [text]
/rpgitem license <item> [license]
```

## Wiki Generation

Generate markdown documentation for all powers, conditions, and markers:

```
/rpgitem gen-wiki [locale]
```

Outputs to the plugin data folder.
