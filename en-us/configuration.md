# Configuration

This page refers to the RPGItems configuration file.

In most cases, there is no need to touch this file but it provides a few global options that may usable.

Here is a default configuration file.

```
language: en_US
version: '1.0'
general:
  enabled_languages:
  - en_US
  - zh_CN
  give_perms: false
  item:
    fs_lock: false
    show_loaded: false
command:
  list:
    item_per_page: 9
    power_per_page: 5
support:
  world_guard:
    enable: false
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
    enchant_mode: ALLOW
    allow_anvil_enchant: false
unused:
  locale_inv: false
```
