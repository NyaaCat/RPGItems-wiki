# 安装与配置

## 插件安装

**RPGItems 3.7** 基于游戏版本 **1.13.2** 开发。你可以在 [这里](https://github.com/NyaaCat/RPGItems-reloaded/releases) 获取最新版本的插件。

在安装前，你需要安装以下前置插件：

* Vault
* [LanguageUtils](https://github.com/NyaaCat/LanguageUtils/releases)
* [NyaaCore](https://github.com/NyaaCat/NyaaCore/releases)

我们同时推荐安装以下插件：

* EssentialsX
* WorldEdit / FAWE
* WorldGuard
* [NyaaUtils](https://github.com/NyaaCat/NyaaUtils/releases)

## 插件配置

首次启动时，插件将会自动生成默认配置文件。一般情况下，不需要修改任何配置即可正常使用插件。

`config.yml` 的默认配置如下：

```text
language: en_US
version: '1.0'
general:
  pid_compat: false
  item_compat: true
  give_perms: false
  item:
    fs_lock: true
    show_loaded: false
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
unused:
  locale_inv: false
```

* language：插件的显示语言。 目前支持 `zh_CN` 与 `en_US` 。
* item：
  * defaults：
    * numeric\_bar：是否默认使用数字指示耐久。
    * force\_bar：是否默认显示数字耐久。
    * license：默认的许可文本。该文本将在物品配置被他人导入时显示。
    * enchant\_mode：默认的物品附魔模式。
      * DISALLOW：物品不允许附魔。
      * PERMISSION：需要拥有 `rpgitem.enchant.[item_name]` 权限后才可附魔。
      * ALLOW：不需要权限，允许附魔。
* support：
  * world\_guard
    * enable：开启 WorldGuard 插件支持。
    * force\_refresh：是否在玩家使用物品时强制刷新 WorldGuard 保护区。仅当遇到WorldGuard相关问题时启用。
    * disable\_in\_no\_pvp：在禁止 pvp 的区域禁用 RPGItems。
    * show\_warning：在玩家在禁止使用的区域使用物品是是否发送警告。
* general：
  * spu\_endpoint：[详见此处](https://github.com/NyaaCat/RPGItems-reloaded/wiki/RPGItems-3.6-for-1.13.2-Upgrade-Guide#item-auto-update-from-112-to-113)
  * pid\_compat：不要修改。是否保留由 3.5 至 3.6 升级时的物品 ID，在 3.7 版本无作用。
  * item\_compat：在 3.7 版本中是否尝试解析 3.5-3.6 版本的物品。
  * give\_perms：使用 `/rpgitem give` 命令时是否需要额外权限。
  * item
    * fs\_lock：不要修改。保护物品配置文件完整性。在 Windows 服务器中锁定物品配置文件。
    * show\_loaded：是否在启动时显示载入的文件。
* command：
  * list：
    * power\_per\_page：使用 `/rpgitem listpower` 时每页显示的技能数
    * item\_per\_page：使用 `/rpgitem list` 时每页显示的物品数
* gist：
  * token：Github personal access token with at lease `gist` scope for exporting item config file to Gist.
  * publish：Whether to publish gist by default.

`/items` 目录下以 `[item_name]-item.yml` 形式保存创建的物品。创建和配置物品均通过游戏内指令进行，一般情况下无需手动修改物品配置。

`/backup` 目录下保存物品文件的备份。游戏内每次对物品进行更改时，在更新 `items` 目录下文件的同时，都会在备份目录下创建一个物品文件的备份。

## 权限

* `rpgitem` 使用全部 `/rpgitem` 指令。
* `rpgitems` 使用全部 /rpgitems 指令。
* `rpgitems.tomodel` 使用 `/rpgitems tomodel` 指令。
* `rpgitems.info` 使用 `/rpgitems info` 指令。
* `rpgitem.updateitem` 使用 `/rpgitem updateitem` 指令。
* `rpgitem.allowenchant.new` 附魔权限。
* `rpgitem.allowenchant.old` 附魔权限。

上述权限默认仅服务器管理员拥有。

