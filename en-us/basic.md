# Basic commands

Here is a list of basic commands to manage and customize items.

## Create

```
/rpgitem create myitem
```

## Remove

```
/rpgitem remove myitem
```

## List

```
/rpgitem list [page] <param>
```

Additional parameters are available as filter.

* `display` - the item display name
* `name` - the item name in RPGItems database

E.g.,

```
/rpgitem list display:Hero
```

Will show a list of items that display name matching "Hero".

## Give

```
/rpgitem give [item] <target> <amount>
```

E.g., give two `mysword` item to player `Notch`.

```
/rpgitem give mysword Notch 2
```

You may also give one to yourself

```
/rpgitem give mysword
```

## Load from file

```
/rpgitem loadfile mysword-item.yml
```

Items are stored in `items/` directory under RPGItems plugin directory.

## Reload item from file

```
/rpgitem reloaditem mysword
```

## Reload plugin

```
/rpgitem reload
```

## Import and Export

```
/rpgitem import [GIST|URL] [value]
```

```
/rpgitem export mysword
```

* Note: You need Gist API key to use this feature.

## Clone

```
/rpgitem clone mysword mynewsword
```

## Dump

```
/rpgitem dump mysword
```

Will print raw item configuration content in YAML format for further inspection.

## Save All

```
/rpgitem save-all
```

Saves all items to disk.

## Version

```
/rpgitem version
```

Displays the current RPGItems plugin version.

## Give Permissions Toggle

```
/rpgitem giveperms
```

Toggles whether the give command requires permission (`rpgitem.give.<itemname>`).

## Item Info

```
/rpgitem info
```

Display detailed information about the RPGItem in your main hand. Shows item properties and whether the item is a model.

Requires permission: `rpgitem.info`

## Convert to Model

```
/rpgitem tomodel
```

Converts the RPGItem in your main hand to a model (static copy without power functionality).

Requires permission: `rpgitem.tomodel`

## Socketing and Item Level

> This section applies to RPGItems `3.9`.

### Open Socket GUI

```
/rpgitem socket
```

- Permission: `rpgitem.socket` (admin)
- Opens a 3-row chest GUI for editing sockets on an item instance
- Taking back the center item closes the GUI immediately

### Get/Set Instance Level

```
/rpgitem level get <item>
/rpgitem level set <item> <level>
```

- Permission: `rpgitem.level` (admin)
- Works on the **item instance** in main hand
- `<item>` must match the held RPGItem
- Minimum level is `1`
