# 安装与配置
!> 本帮助文档基于最新版本的 **RPGItems** 所写。各类命令与系统机制可能会发生变化并且不再适用于旧版本。

对于1.13.2及以上的版本，**RPGItems** 依赖于 **Nyaacore** 与其他库来进行运作。对于1.12.2及以下的版本，它不依赖于任何其他的内容。

在下载完所有必须的jar文件后，请将它们放入plugins文件夹内，并且**重启**服务器。请勿使用reload命令，它不会起任何作用并且会导致你的服务器出错。

你可以在下方寻找对应您服务器版本的 **RPGItems**。

## 1.15.2 - v3.8

- [Vault](https://www.spigotmc.org/resources/vault.34315/)
- [NyaaCore](https://ci.nyaacat.com/job/NyaaCore/362/artifact/build/libs/NyaaCore-mc1.15.2-7.1.362-shadowed.jar)
- [LangUtils](https://ci.nyaacat.com/job/LanguageUtils/25/artifact/build/libs/LangUtils-mc1.15.1-2.3.25.jar)
- [RPGItems](https://ci.nyaacat.com/job/RPGItems-reloaded/job/1.15/77/artifact/build/libs/RPGItems-mc1.15-3.8-77-release.jar)

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

- EssentialsX
- WorldEdit / FAWE
- WorldGuard
- [NyaaUtils](https://ci.nyaacat.com/job/NyaaUtils/261/artifact/build/libs/NyaaUtils-mc1.15.1-7.1.261.jar)

# 插件设置

在使用**RPGItems**时插件会默认生成一份配置文件。

在大多数情况下您不需要去修改这个文件，但是设置文件中的部分设置可能会对您有用。

这是一份插件的默认设置文件：

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

