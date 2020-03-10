# Basic commands

Here is a list of basic commands to manage and customize items.

## Create

```
/rpgitem create myitem
```

## Delete

```
/rpgitem delete myitem
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

