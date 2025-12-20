# 进阶技巧

本页面介绍常用的进阶技巧和高级命令。

## 模板系统

模板允许您创建带有可配置占位符的道具蓝图。

### 命令

| 命令 | 描述 |
|------|------|
| `/rpgitem template create <道具> <占位符1> <占位符2> ...` | 创建带占位符的模板 |
| `/rpgitem template delete <道具>` | 删除模板 |
| `/rpgitem template apply <道具>` | 将模板应用到所有实例 |
| `/rpgitem template placeholder add <道具> <占位符>` | 添加占位符 |
| `/rpgitem template placeholder remove <道具> <占位符>` | 移除占位符 |
| `/rpgitem template placeholder list <道具>` | 列出占位符 |

### 占位符语法

占位符使用格式`<技能ID:属性名>`：

```
/rpgitem template create mysword <0-rpgitems:beam:damage> <0-rpgitems:beam:length>
```

这将创建一个模板，其中伤害和长度可以针对每个实例进行自定义。

## 道具组

将道具组织成组以便于管理。

| 命令 | 描述 |
|------|------|
| `/rpgitem creategroup <名称> <道具1> <道具2> ...` | 用道具创建组 |
| `/rpgitem creategroup <名称> /<正则表达式>/` | 创建匹配正则表达式的组 |
| `/rpgitem addtogroup <道具> <组>` | 将道具添加到组 |
| `/rpgitem removefromgroup <道具> <组>` | 从组中移除道具 |
| `/rpgitem listgroup <组>` | 列出组中的道具 |
| `/rpgitem removegroup <组>` | 删除组 |

示例：
```
/rpgitem creategroup weapons sword_* dagger_*
/rpgitem creategroup armor /.*_armor$/
```

## 触发详解

触发器定义了技能何时被激活。所有触发器都有一个`priority`属性，用于确定多个触发器同时触发时的执行顺序。

部分触发器支持额外的过滤属性：
- `minDamage`/`maxDamage` - 伤害类触发器（HIT、HIT_GLOBAL、HIT_TAKEN、HURT、DYING）
- `minForce`/`maxForce` - BOW_SHOOT触发器

## 技能的执行顺序

技能按照添加的顺序执行，并且只在有对应触发时才会执行。使用Dummy技能可以控制执行流程。

## 使用 Dummy 技能统一管理

Dummy技能用于统一管理冷却和消耗。示例：

1. 添加Dummy技能：`cooldown:20 cooldownKey:left cooldownResult:ABORT cost:1 showCDWarning:false triggers:LEFT_CLICK`
2. 将其他技能的`cost`和`cooldown`设置为0

## 道具模式

### 更新模式

控制道具持有时的更新方式：

```
/rpgitem updatemode <道具> <模式>
```

| 模式 | 描述 |
|------|------|
| `FULL_UPDATE` | 更新所有属性 |
| `DISPLAY_ONLY` | 仅更新显示名称 |
| `LORE_ONLY` | 仅更新描述 |
| `ENCHANT_ONLY` | 仅更新附魔 |
| `NO_DISPLAY` | 跳过显示更新 |
| `NO_LORE` | 跳过描述更新 |
| `NO_ENCHANT` | 跳过附魔更新 |
| `NO_UPDATE` | 不自动更新 |

### 伤害模式

控制伤害计算：

```
/rpgitem damagemode <道具> <模式>
```

| 模式 | 描述 |
|------|------|
| `FIXED` | 使用固定伤害值 |
| `FIXED_WITHOUT_EFFECT` | 固定伤害，无效果 |
| `FIXED_RESPECT_VANILLA` | 固定伤害，应用原版修正 |
| `FIXED_WITHOUT_EFFECT_RESPECT_VANILLA` | 固定伤害无效果，应用原版修正 |
| `VANILLA` | 标准Minecraft伤害 |
| `ADDITIONAL` | 添加到原版伤害 |
| `ADDITIONAL_RESPECT_VANILLA` | 附加伤害，应用原版修正 |
| `MULTIPLY` | 乘以原版伤害 |

### 属性模式

```
/rpgitem attributemode <道具> <模式>
```

| 模式 | 描述 |
|------|------|
| `FULL_UPDATE` | 完全更新属性 |
| `PARTIAL_UPDATE` | 部分更新属性 |

### 附魔模式

```
/rpgitem enchantmode <道具> <模式>
```

| 模式 | 描述 |
|------|------|
| `DISALLOW` | 不能附魔 |
| `PERMISSION` | 需要权限才能附魔 |
| `ALLOW` | 可以正常附魔 |

## 元数据命令

### 品质系统

```
/rpgitem meta quality <道具> <品质>
```

品质前缀在`config.yml`中配置：
```yaml
item:
  quality:
    trash: "&7[垃圾] "
    normal: "&f[普通] "
    rare: "&9[稀有] "
    epic: "&5[史诗] "
    legendary: "&6[传说] "
```

### 类型元数据

```
/rpgitem meta type <道具> <类型>
```

## 修改器系统

将动态修改器应用于玩家手中的道具或存储的玩家数据。

| 命令 | 描述 |
|------|------|
| `/rpgitem modifier add <玩家/hand> <修改器> [属性]` | 添加修改器 |
| `/rpgitem modifier prop <玩家/hand> <ID> [属性]` | 修改属性 |
| `/rpgitem modifier remove <玩家/hand> <ID>` | 移除修改器 |
| `/rpgitem modifier list` | 列出可用修改器 |

## 自定义触发器

| 命令 | 描述 |
|------|------|
| `/rpgitem trigger add <道具> <技能> <名称> [属性]` | 添加自定义触发器 |
| `/rpgitem trigger prop <道具> <名称> [属性]` | 修改触发器 |
| `/rpgitem trigger remove <道具> <名称>` | 移除触发器 |
| `/rpgitem trigger list` | 列出触发器 |

## 技能命令

管理道具的技能。

| 命令 | 描述 |
|------|------|
| `/rpgitem power add <道具> <技能> [属性]` | 添加技能 |
| `/rpgitem power prop <道具> <索引> [属性]` | 查看/修改技能属性 |
| `/rpgitem power remove <道具> <索引>` | 移除技能 |
| `/rpgitem power reorder <道具> <从> <到>` | 调整技能顺序 |
| `/rpgitem power list` | 列出可用技能 |

## 条件命令

管理道具的条件。

| 命令 | 描述 |
|------|------|
| `/rpgitem condition add <道具> <条件> [属性]` | 添加条件 |
| `/rpgitem condition prop <道具> <索引> [属性]` | 查看/修改条件 |
| `/rpgitem condition remove <道具> <索引>` | 移除条件 |
| `/rpgitem condition list` | 列出可用条件 |

## 标记命令

管理道具的标记。

| 命令 | 描述 |
|------|------|
| `/rpgitem marker add <道具> <标记> [属性]` | 添加标记 |
| `/rpgitem marker prop <道具> <索引> [属性]` | 查看/修改标记 |
| `/rpgitem marker remove <道具> <索引>` | 移除标记 |
| `/rpgitem marker list` | 列出可用标记 |

## 导入/导出

通过GitHub Gist分享道具。

### 导出

```
/rpgitem export <道具> [GIST] [token:<令牌>] [publish:<true/false>] [description:<描述>]
```

### 导入

```
/rpgitem import GIST <gist_id> [token:<令牌>]
```

## GUI浏览器

```
/rpgitem gui [page:<页码>] [displayFilter:<文本>] [loreFilter:<文本>] [nameFilter:<文本>]
```

打开一个物品栏GUI，用于浏览和管理道具。

## Paper API功能（1.20+）

### 自定义模型数据

支持多种类型的扩展自定义模型数据：

```
/rpgitem customModel <道具> [floats:<值>] [strings:<值>] [flags:<值>] [colors:<值>]
```

- `floats:` 分号分隔的浮点值
- `strings:` 分号分隔的字符串（使用`\;`转义）
- `flags:` 分号分隔的布尔值
- `colors:` 分号分隔的RGB三元组（r,g,b）

### 道具模型

```
/rpgitem itemmodel <道具> <命名空间:键>
/rpgitem itemmodel <道具>  # 移除模型
```

## 调试工具

| 命令 | 描述 |
|------|------|
| `/rpgitem debug` | 调试手中的道具 |
| `/rpgitem dump <道具>` | 导出道具YAML配置 |
| `/rpgitem loadfile <路径>` | 从文件路径加载道具 |

## 道具管理

| 命令 | 描述 |
|------|------|
| `/rpgitem backupitem <道具>` | 创建备份（解锁编辑）|
| `/rpgitem cleanbackup` | 清理所有备份文件 |
| `/rpgitem reloaditem <道具>` | 从磁盘重新加载单个道具 |

## 其他属性

### 伤害类型

```
/rpgitem damageType <道具> [类型]
```

类型：`melee`、`ranged`、`magic`、`summon`

### 护甲表达式

```
/rpgitem armorExpression <道具> <表达式>
/rpgitem playerArmorExpression <道具> <表达式>
```

### 道具权限

```
/rpgitem canUse <道具> <true/false>
/rpgitem canPlace <道具> <true/false>
```

### 道具元数据

```
/rpgitem author <道具> [@玩家]
/rpgitem note <道具> [文本]
/rpgitem license <道具> [许可证]
```

## Wiki生成

为所有技能、条件和标记生成markdown文档：

```
/rpgitem gen-wiki [语言]
```
