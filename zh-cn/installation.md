# 安装与配置
!> 本帮助文档基于最新版本的 **RPGItems** 所写。各类命令与系统机制可能会发生变化并且不再适用于旧版本。

对于1.13.2及以上的版本，**RPGItems** 依赖于 **Nyaacore** 与其他库来进行运作。对于1.12.2及以下的版本，它不依赖于任何其他的内容。

在下载完所有必须的jar文件后，请将它们放入plugins文件夹内，并且**重启**服务器。请勿使用reload命令，它不会起任何作用并且会导致你的服务器出错。

你可以在下方寻找对应您服务器版本的 **RPGItems**。

## 1.15.2 - v3.8

- [Vault](https://www.spigotmc.org/resources/vault.34315/)
- [NyaaCore](https://ci.nyaacat.com/job/NyaaCore/362/artifact/build/libs/NyaaCore-mc1.15.2-7.1.362-shadowed.jar)
- [LangUtils](https://ci.nyaacat.com/job/LanguageUtils/25/artifact/build/libs/LangUtils-mc1.15.1-2.3.25.jar)
- [RPGItems](https://ci.nyaacat.com/job/RPGItems-reloaded/job/1.15/78/artifact/build/libs/RPGItems-mc1.15-3.8-78-release.jar)

## 1.14.4

1.14.4下没有稳定的**RPGItems**版本

## 1.13.2 - v3.7

- [Vault](https://www.spigotmc.org/resources/vault.34315/)
- [NyaaCore](https://github.com/NyaaCat/NyaaCore/releases/download/v6.3.329-mc1.13.2/NyaaCore-v6.3.329-mc1.13.2.jar)
- [LangUtils](https://github.com/NyaaCat/LanguageUtils/releases/download/v2.1.17-mc1.13.1/LangUtils-v2.1.17-mc1.13.1.jar)
- [RPGItems](https://github.com/NyaaCat/RPGItems-reloaded/releases/download/v3.7.762-mc1.13.2/rpgitem-reloaded-3.7.762-mc1.13.2.jar)

## 1.11/1.12.2 - v3.5.448

- [RPGItems](https://github.com/NyaaCat/RPGitems-reloaded/releases/download/1.11-v3.5.448/rpgitem-reloaded-mc1.11-v3.5.448.jar)

## 1.10 - 3.5.143

- [RPGItems](https://github.com/NyaaCat/RPGitems-reloaded/releases/download/1.10-v3.5.143/RPGitems-reloaded.jar)

## 1.9 - 3.5.136

- [RPGItems](https://github.com/NyaaCat/RPGitems-reloaded/releases/download/1.9-v3.5.136/RPGitems-reloaded.jar)

## 1.8 - 3.5.264

- [RPGItems](https://github.com/NyaaCat/RPGitems-reloaded/releases/download/1.8-v3.5.264/rpgitem-reloaded-1.8-v.264.jar)


我们同时推荐安装以下插件：

- [EssentialsX](https://www.spigotmc.org/resources/essentialsx.9089)
- [WorldEdit](https://dev.bukkit.org/projects/worldedit)
- [WorldGuard](https://dev.bukkit.org/projects/worldguard)
- [NyaaUtils](https://ci.nyaacat.com/job/NyaaUtils/261/artifact/build/libs/NyaaUtils-mc1.15.1-7.1.261.jar)

# 插件设置

在使用 **RPGItems** 时插件会默认生成一份配置文件。

在大多数情况下您不需要去修改这个文件，但是配置文件中的部分设置可能会对您有用。

这是一份插件的默认配置文件：

```
language: en_US
version: '1.0'
general:
  enabled_languages:
  - en_US
  - zh_CN
  give_perms: false
  item:
    fs_lock: true
    show_loaded: false
    item_stack_uuid: true
command:
  list:
    item_per_page: 9
    power_per_page: 5
support:
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
unused:
  locale_inv: false

```
## config 配置文件说明

- language 插件使用的语言。默认情况下插件使用的语言为英文，如果需要使用中文，请将 `language: en_US` 改为 `language: zh_CN` 。
- general 设置：
  - `enabled_languages` 可使用的语言
  - `give_perms` 使用 `/rpgitem give` 时是否需要额外权限
  - item
    - `fs_lock` 请勿修改此项。是否保护道具配置文件完整性。在 Windows 服务器中锁定道具配置文件。
    - `show_loaded` 是否在启动时显示载入的文件
    - `item_stack_uuid` 是否在给予道具时为道具赋予独特的uuid来防止道具堆叠。此项可能会影响 shopkeepers 等经济插件中的道具交易。
- command 设置
  - list 
    - `item_per_page` 使用 `/rpgitem list` 命令时每页列出的道具量
    - `power_per_page` 使用 `/rpgitem power list` 命令时每页列出的技能数
- support 设置
  - world_guard
    - `enable` 是否启用 Worldguard 插件支持
    - `force_refresh` 是否在玩家使用物品时强制刷新 WorldGuard 保护区。仅当遇到WorldGuard相关问题时启用。
    - `disable_in_no_pvp` 是否在非PVP区域禁用 RPGItems 。
    - `show_warning` 当玩家在非PVP区域使用 RPGItems 时是否发送警告。
- gist 设置
  - `token`
  - `publish` 
- item 设置
  - default 制作道具时的默认生效配置
    - `numeric_bar` 是否默认使用数字显示耐久度。
    - `force_bar` 是否默认显示耐久度。
    - `license` 默认的许可文本。该文本将在物品配置被他人导入时显示。
    - `enchant_mode` 道具附魔模式。
      - `DISALLOW` 禁止附魔
      - `PERMISSION` 需要拥有 `rpgitem.enchant [item_name]` 权限后才可附魔
      - `ALLOW` 无需权限，允许附魔
    - `allow_anvil_enchant` 是否允许通过铁砧附魔。
    
