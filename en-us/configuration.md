# Configuration

This page refers to the RPGItems configuration file (`config.yml`).

In most cases, there is no need to touch this file but it provides global options that may be useful.

## Default Configuration

```yaml
language: en_US
version: '1.2'
general:
  enabled_languages:
  - en_US
  - zh_CN
  give_perms: false
  item:
    fs_lock: false
    show_loaded: false
    item_stack_uuid: true
    show_cooldown_warning_to_actionbar: false
command:
  list:
    item_per_page: 9
    power_per_page: 5
support:
  MythicMobs:
    enable: true
  PlaceholderAPI:
    enable: true
  Residence:
    enable: true
  world_guard:
    enable: true
    force_refresh: false
    disable_in_no_pvp: true
    show_warning: true
gist:
  token: ''
  publish: true
item:
  defaults:
    numeric_bar: false
    force_bar: false
    license: All Right Reserved
    enchant_mode: DISALLOW
    allow_anvil_enchant: true
    note: ''
    author: ''
  quality:
    trash: '&7'
    normal: '&f'
    rare: '&b'
    epic: '&3'
    legendary: '&e'
unused:
  locale_inv: false
```

## Configuration Options

### General Settings

| Option | Default | Description |
|--------|---------|-------------|
| `language` | en_US | Default language for messages |
| `general.enabled_languages` | [en_US, zh_CN] | List of enabled language files |
| `general.give_perms` | false | Require permission `rpgitem.give.<itemname>` for give command |
| `general.item.fs_lock` | false | Enable filesystem locking for item files |
| `general.item.show_loaded` | false | Show loaded items on startup |
| `general.item.item_stack_uuid` | true | Assign unique UUIDs to item stacks (improves performance, prevents stacking) |
| `general.item.show_cooldown_warning_to_actionbar` | false | Show cooldown warnings in action bar instead of chat |

### Command Settings

| Option | Default | Description |
|--------|---------|-------------|
| `command.list.item_per_page` | 9 | Items shown per page in list command |
| `command.list.power_per_page` | 5 | Powers shown per page in power list |

### Plugin Integration

| Option | Default | Description |
|--------|---------|-------------|
| `support.MythicMobs.enable` | true | Enable MythicMobs integration |
| `support.PlaceholderAPI.enable` | true | Enable PlaceholderAPI integration |
| `support.Residence.enable` | true | Enable Residence integration |
| `support.world_guard.enable` | true | Enable WorldGuard integration |
| `support.world_guard.force_refresh` | false | Force refresh region data on every power use |
| `support.world_guard.disable_in_no_pvp` | true | Disable powers in no-PvP regions |
| `support.world_guard.show_warning` | true | Show warning when power is blocked by region |

### Gist Settings

| Option | Default | Description |
|--------|---------|-------------|
| `gist.token` | '' | GitHub personal access token for Gist import/export |
| `gist.publish` | true | Create public gists by default |

### Item Defaults

| Option | Default | Description |
|--------|---------|-------------|
| `item.defaults.numeric_bar` | false | Use numeric durability display by default |
| `item.defaults.force_bar` | false | Force show durability bar |
| `item.defaults.license` | "All Right Reserved" | Default license for new items |
| `item.defaults.enchant_mode` | DISALLOW | Default enchant mode (DISALLOW, PERMISSION, ALLOW) |
| `item.defaults.allow_anvil_enchant` | true | Allow enchanting in anvil by default |
| `item.defaults.note` | '' | Default note for new items |
| `item.defaults.author` | '' | Default author for new items |

### Quality Prefixes

The `item.quality` section defines color prefixes for item quality tiers used with the `meta quality` command:

```yaml
item:
  quality:
    trash: '&7'      # Gray
    normal: '&f'     # White
    rare: '&b'       # Aqua
    epic: '&3'       # Dark Aqua
    legendary: '&e'  # Yellow
```

You can add custom quality tiers by adding new entries to this section.
