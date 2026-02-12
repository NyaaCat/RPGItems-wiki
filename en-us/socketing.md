# Socketing System

> This page applies to RPGItems `3.9`.

## 1. Design Goals

Socketing turns an RPGItem from a fixed template into a composable instance:

- One item instance can hold multiple socket items
- Composition is instance-scoped, not global-template scoped
- Players can edit sockets through GUI
- Admins can hot-update socket configuration via commands

## 2. Core Concepts

- `Container Item`: the host item that accepts sockets
- `Socket Item`: RPGItem that contributes extra powers/markers/conditions
- `Socket Slot`: one socket position on an item instance
- `Active Socket`: a socket that passes validation rules

### 2.1 Exclusive Roles

An RPGItem cannot be both container and socket item at the same time.

Conflicting operations are rejected. You must disable the conflicting role first, for example:

```bash
/rpgitem socket myitem container false
/rpgitem socket myitem socketitem true
```

## 3. Data and Config Model

### 3.1 Container fields

```yaml
socketAcceptTags:
  - GEM
socketMaxWeight: 10
socketInsertLine: 1
```

- `socketAcceptTags`: accepted socket tags (match any)
- `socketMaxWeight`: max total socket weight
- `socketInsertLine`: lore insertion line (`0-based`)

### 3.2 Socket item fields

```yaml
socketTags:
  - GEM
socketWeight: 3
socketMinLevel: 2
socketingDescription:
  - "&7[Gem] +10 Damage"
```

- `socketTags`: tags provided by this socket item
- `socketWeight`: weight cost
- `socketMinLevel`: minimum container item level
- `socketingDescription`: lore lines inserted after socketing

### 3.3 Tag matching rules

- Empty `socketAcceptTags`: accepts no sockets (default)
- Empty `socketTags`: matches no tags
- Special tag `ANY`: wildcard for any tag

A socket is inactive when tag mismatch, level too low, or total weight overflow happens.

### 3.4 Instance persistence (PDC)

Socketing and level are stored in item instance PDC:

- `rgi_sockets`: ordered socket RPGItem IDs
- `rgi_item_level`: item instance level
- `rgi_instance_cache_key`: runtime cache key

> Socket data stores RPGItem IDs only, not full ItemStack snapshots.

### 3.5 3.9 Limitation: no per-socket-item level support

- `rgi_item_level` applies to the container item instance only
- `rgi_sockets` stores socket RPGItem IDs only, without per-socket instance level
- Therefore in 3.9, a socket item's own level is not persisted/evaluated after socketing
- `itemlevelcondition` / `LevelDescription` in socket runtime context use container instance level

## 4. Runtime Logic (High Level)

When an instance has level/socket data, RPGItems builds a runtime composite item.

Composition order: `container -> socket1 -> socket2 -> ...`

Execution model:

- `condition`: shared and evaluated together
- `marker`: processed in order
- `power`: triggered in order per trigger

If a socket ID becomes invalid (deleted item, etc.), it is kept in data but disabled at runtime (no crash).

## 5. Performance and Caching

To avoid rebuilding on every trigger:

- Runtime composite cache is keyed by `instance_cache_key` + signature
- Cache is cleared on reload/config change paths
- Inventory update is queued per player and processed with per-tick budget

## 6. GUI Usage

Open command:

```bash
/rpgitems socketing
```

Admin alias:

```bash
/rpgitem socketing
```

Permissions:

- User: `rpgitems.socketing`
- Admin alias: `rpgitem.socketing`

GUI shape (3 rows):

- Left 3x3 area, only center is the container slot
- Right area is socket slots
- Last column is one status item (lore summarizes all errors/status)

## 7. Quick Start

```bash
# 1) Configure socket item
/rpgitem socket ruby_gem socketitem true
/rpgitem socket ruby_gem tags set GEM
/rpgitem socket ruby_gem weight 3
/rpgitem socket ruby_gem minlevel 2
/rpgitem socket ruby_gem lore clear
/rpgitem socket ruby_gem lore add &7[Gem] +10 Damage

# 2) Configure container
/rpgitem socket great_sword container true
/rpgitem socket great_sword accepttags set GEM
/rpgitem socket great_sword maxweight 10
/rpgitem socket great_sword insertline 1

# 3) Inspect config
/rpgitem socket ruby_gem info
/rpgitem socket great_sword info

# 4) Open GUI for instance socketing
/rpgitems socketing
```

## 8. Command Reference (Admin Config)

> Requires `rpgitem.socket`.

### 8.1 Inspect config

```bash
/rpgitem socket <item> info
```

### 8.2 Role switches

```bash
/rpgitem socket <item> socketitem <true|false>
/rpgitem socket <item> container <true|false>
```

- `socketitem true`: enables socket item role (auto-fills `ANY` tag when empty)
- `container true`: enables container role (auto-fills `ANY` accept tag when empty)
- conflicting role enable is rejected

### 8.3 Container tags

```bash
/rpgitem socket <item> accepttags list
/rpgitem socket <item> accepttags set GEM,RUNE
/rpgitem socket <item> accepttags add GEM
/rpgitem socket <item> accepttags remove GEM
/rpgitem socket <item> accepttags clear
```

### 8.4 Socket item tags

```bash
/rpgitem socket <item> tags list
/rpgitem socket <item> tags set GEM,RUNE
/rpgitem socket <item> tags add GEM
/rpgitem socket <item> tags remove GEM
/rpgitem socket <item> tags clear
```

### 8.5 Numeric settings

```bash
/rpgitem socket <item> maxweight <value>
/rpgitem socket <item> insertline <line>
/rpgitem socket <item> weight <value>
/rpgitem socket <item> minlevel <value>
```

### 8.6 Socket lore lines

```bash
/rpgitem socket <item> lore list
/rpgitem socket <item> lore add <text>
/rpgitem socket <item> lore insert <line> <text>
/rpgitem socket <item> lore set <line> <text>
/rpgitem socket <item> lore remove <line>
/rpgitem socket <item> lore clear
```

All line indexes are `0-based`.

## 9. Troubleshooting

If a socket does not activate, check in this order:

- tag matching
- current instance level vs `socketMinLevel`
- total weight vs `socketMaxWeight`
- socket item ID validity
