# 配置文件

本页面介绍 RPGItems 配置文件 (`config.yml`)。

大多数情况下不需要修改此文件，但它提供了一些可能有用的全局选项。

## 默认配置

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

## 配置选项说明

### 通用设置

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `language` | en_US | 消息的默认语言 |
| `general.enabled_languages` | [en_US, zh_CN] | 启用的语言文件列表 |
| `general.give_perms` | false | give命令需要 `rpgitem.give.<道具名>` 权限 |
| `general.item.fs_lock` | false | 启用道具文件的文件系统锁定 |
| `general.item.show_loaded` | false | 启动时显示加载的道具 |
| `general.item.item_stack_uuid` | true | 为物品堆分配唯一UUID（提高性能，防止堆叠）|
| `general.item.show_cooldown_warning_to_actionbar` | false | 在动作栏而非聊天栏显示冷却警告 |

### 命令设置

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `command.list.item_per_page` | 9 | 列表命令每页显示的道具数 |
| `command.list.power_per_page` | 5 | 技能列表每页显示的技能数 |

### 插件集成

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `support.MythicMobs.enable` | true | 启用 MythicMobs 集成 |
| `support.PlaceholderAPI.enable` | true | 启用 PlaceholderAPI 集成 |
| `support.Residence.enable` | true | 启用 Residence 集成 |
| `support.world_guard.enable` | true | 启用 WorldGuard 集成 |
| `support.world_guard.force_refresh` | false | 每次使用技能时强制刷新区域数据 |
| `support.world_guard.disable_in_no_pvp` | true | 在禁止PvP的区域禁用技能 |
| `support.world_guard.show_warning` | true | 技能被区域阻止时显示警告 |

### Gist设置

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `gist.token` | '' | GitHub个人访问令牌，用于Gist导入/导出 |
| `gist.publish` | true | 默认创建公开的gist |

### 道具默认值

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `item.defaults.numeric_bar` | false | 默认使用数字耐久显示 |
| `item.defaults.force_bar` | false | 强制显示耐久条 |
| `item.defaults.license` | "All Right Reserved" | 新道具的默认许可证 |
| `item.defaults.enchant_mode` | DISALLOW | 默认附魔模式 (DISALLOW, PERMISSION, ALLOW) |
| `item.defaults.allow_anvil_enchant` | true | 默认允许在铁砧中附魔 |
| `item.defaults.note` | '' | 新道具的默认备注 |
| `item.defaults.author` | '' | 新道具的默认作者 |

### 品质前缀

`item.quality` 部分定义了与 `meta quality` 命令一起使用的道具品质等级颜色前缀：

```yaml
item:
  quality:
    trash: '&7'      # 灰色
    normal: '&f'     # 白色
    rare: '&b'       # 青色
    epic: '&3'       # 深青色
    legendary: '&e'  # 黄色
```

您可以通过在此部分添加新条目来添加自定义品质等级。
