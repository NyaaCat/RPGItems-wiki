# 外部插件集成

RPGItems支持与多个流行插件集成以扩展功能。

## MythicMobs

MythicMobs集成允许RPGItems释放MythicMobs技能并作为MythicMobs掉落物使用。

### 配置

在`config.yml`中启用：
```yaml
support:
  MythicMobs:
    enable: true
```

### MythicSkillCast技能

从RPGItems中释放MythicMobs技能。

- `skill` 要释放的MythicMobs技能名称（必需）
- `cooldown` 冷却时间，单位Ticks（默认：20）
- `cost` 耐久消耗（默认：0）
- `suppressArrow` 当由BOW_SHOOT触发时取消箭矢（默认：false）

示例：
```
/rpgitem power add mystaff rpgitems:mythicskillcast skill:Fireball cooldown:40 triggers:RIGHT_CLICK
```

### 将RPGItems作为MythicMobs掉落物

使用`rpg.`前缀将RPGItems添加到MythicMobs掉落表：

```yaml
Drops:
  - rpg.mysword 1 0.5
```

格式：`rpg.<道具名> <数量> <概率>`

## PlaceholderAPI

PlaceholderAPI集成提供可在其他插件中使用的占位符，并在RPGItems表达式中启用占位符支持。

### 要求

- PlaceholderAPI 2.10.2或更高版本

### 配置

在`config.yml`中启用：
```yaml
support:
  PlaceholderAPI:
    enable: true
```

### 可用占位符

在其他插件中使用这些占位符：

| 占位符 | 描述 |
|--------|------|
| `%rpgitems_items%` | 已定义的RPGItems总数 |
| `%rpgitems_item_display_<名称>%` | 指定道具的显示名称 |
| `%rpgitems_item_lores_<名称>%` | 指定道具的描述文本 |

### 在RPGItems中使用占位符

PlaceholderAPI占位符可用于：

- **命令技能（Command）**：命令字符串中的占位符会被解析
- **表达式伤害（EvalDamage）**：表达式中的占位符会被解析
- **PlaceholderAPI条件**：比较占位符值

命令技能示例：
```
/rpgitem power add myitem rpgitems:command command:`tellraw {player} {"text":"你的等级是 %player_level%"}`
```

### PlaceholderAPI条件

比较PlaceholderAPI值以控制技能执行。

- `placeholder` PlaceholderAPI占位符（必需）
- `operator` 比较运算符（必需）
- `value` 要比较的值（必需）

**可用运算符：**

| 运算符 | 描述 |
|--------|------|
| `==` | 相等（区分大小写）|
| `!=` | 不相等（区分大小写）|
| `===` | 相等（不区分大小写）|
| `!==` | 不相等（不区分大小写）|
| `contains` | 包含子字符串 |
| `!contains` | 不包含 |
| `startwith` | 以...开头 |
| `!startwith` | 不以...开头 |
| `endwith` | 以...结尾 |
| `!endwith` | 不以...结尾 |
| `matches` | 匹配正则表达式 |
| `!matches` | 不匹配正则表达式 |
| `>=` | 大于等于（数值）|
| `>` | 大于（数值）|
| `<` | 小于（数值）|
| `<=` | 小于等于（数值）|

示例：
```
/rpgitem condition add myitem rpgitems:placeholderapicondition id:high-level placeholder:%player_level% operator:>= value:10
```

## WorldGuard

WorldGuard集成启用基于区域的RPGItems技能控制。

### 要求

- WorldGuard 7.0.0-beta2或更高版本

### 配置

在`config.yml`中启用：
```yaml
support:
  world_guard:
    enable: true
    force_refresh: false
    disable_in_no_pvp: false
    show_warning: true
```

| 选项 | 描述 |
|------|------|
| `enable` | 启用WorldGuard集成 |
| `force_refresh` | 每次使用技能时刷新玩家区域数据 |
| `disable_in_no_pvp` | 在禁止PvP的区域禁用所有技能 |
| `show_warning` | 技能被阻止时显示警告消息 |

### 区域标志

使用`worldguard_region.yml`中的WorldGuard区域标志配置每个区域的RPGItems行为：

```yaml
regions:
  my_region:
    disabled_items:
      - mysword
      - "*"  # 禁用所有道具
    enabled_items:
      - myhelmet  # 覆盖以允许特定道具
    disabled_powers:
      - "mysword:0-rpgitems:beam"
    enabled_powers: []
    warning_message: "&c此处禁用RPGItems技能！"
```

### 命令

| 命令 | 描述 |
|------|------|
| `/rpgitem worldguard` | 切换WorldGuard集成 |
| `/rpgitem wgforcerefresh` | 切换强制刷新模式 |
| `/rpgitem wgignore <道具>` | 使道具忽略WorldGuard限制 |

## Residence

Residence集成启用基于领地标志的RPGItems控制。

### 配置

在`config.yml`中启用：
```yaml
support:
  Residence:
    enable: true
```

### 自定义标志

RPGItems注册自定义标志`enable-rpgitems-power`：

- `true` - 允许在此领地使用RPGItems技能
- `false` - 阻止在此领地使用RPGItems技能

使用Residence命令设置标志：
```
/res set <领地> enable-rpgitems-power true
```

## Vault / 经济

通过Vault进行经济集成，允许道具与服务器经济交互。

### 经济技能（Economy）

技能激活时存入或取出金钱。

- `amountToPlayer` 给予（正数）或扣除（负数）的金额
- `coolDown` 冷却时间，单位Ticks（默认：0）
- `showFailMessage` 失败时显示错误消息（默认：true）
- `abortOnFailure` 失败时中止技能链（默认：true）

示例 - 使用需花费100金币：
```
/rpgitem power add myitem rpgitems:economy amountToPlayer:-100 abortOnFailure:true triggers:RIGHT_CLICK
```

示例 - 击中时奖励50金币：
```
/rpgitem power add mysword rpgitems:economy amountToPlayer:50 triggers:HIT
```
