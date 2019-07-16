# 命令

RPGItems 插件拥有非常完善的命令补全，大部分情况下，使用游戏内的补全提示即可正常使用插件。本页面提供命令使用方面的帮助。  
  
在没有特殊说明的情况下，本帮助文档中，方括号 `[ ]` 中的参数为必须参数，尖括号 `< >` 的参数为可选参数。

RPGItems 中所有文本均支持颜色代码 `&`。如果文本中需要包含空格，请使用反引号 ````` 将文本括起。

对于所有与物品相关的命令，例如 `/rpgitem [item] display <text>` 指令，其第二字段（`[item]`）与第三字段（`display`）的内容可以互换。以下不再作详细说明。

## 查询

查询物品、查看帮助。

* `/rpgitem help`  查看插件帮助。 
* `/rpgitem list <page>`  列出所有的物品，查看列表第 `<page>` 页的物品。 
* `/rpgitem list <parameter:value> ...`  查找物品并列出结果。参数可以为：
  * `display` 物品的显示名称；
  * `name` 物品 ID；
  * `type` 物品种类。 
* `/rpgitem listpower <page>`  列出所有的技能帮助，查看列表第 `<page>` 页的技能。 
* `/rpgitem listpower <name:value>`  按名称查找技能并查看帮助。

## 物品创建与管理

基础的物品创建、查看、获取、删除、备份、克隆指令。

* `/rpgitem create [item]`  新创建一个 ID 为 `[item]` 的物品。 
* `/rpgitem print [item]` 或 `/rpgitem [item]`  显示 `[item]` 的物品信息。可以将鼠标放置在物品名上查看物品信息。 
* `/rpgitem dump [item]`  显示 `[item]` 的配置文件信息，这包括全部的物品参数和技能配置，十分适合在调试物品时使用。如果物品的信息过长，请在客户端 log 中查看。 
* `/rpgitem give [item] <player>`  


  获得一份 `[item]` 物品；  
  指定玩家参数后，给予 `<player>` 玩家一份 `[item]` 物品。`<player>` 参数支持 Minecraft 原版选择器。  

* `/rpgitem remove [item]`  删除 `[item]` 物品。世界中已存在的该物品将无法再被使用。在备份文件夹还会保留该物品的备份。 
* `/rpgitem backupitem [item]`  在备份文件加创建一份 `[item]` 物品的备份。注意，执行此命令之后，物品将无法再被编辑。如需继续编辑物品，请使用 `/rpgitem reloaditem [item]` 。 
* `/rpgitem reloaditem [item]`  重新载入 `[item]` 物品的配置文件。 
* `/rpgitem clone [item] [new_item]`  以 `[new_item]` 为新 ID，创建一份 `[item]` 物品的克隆。

## 物品外观与描述

修改物品的显示外观与描述文本。

* `/rpgitem author [item] <text>`  查看 `[item]` 物品的作者信息； 设置 `[item]` 物品的作者信息为 `<text>` 。 
* `/rpgitem licence [item] <text>`  查看 `[item]` 物品的许可信息； 设置 `[item]` 物品的许可信息为 `<text>` 。 
* `/rpgitem note [item] <text>`  查看 `[item]` 物品的备注信息； 设置 `[item]` 物品的备注信息为 `<text>` 。 
* `/rpgitem item [item] <item>`  查看 `[item]` 的物品模型； 修改 `[item]` 的物品模型为 `<item>`。 
* `/rpgitem display [item] <text>`  查看 `[item]` 物品的显示名称； 设置 `[item]` 物品的显示名称为 `<text>` 。 
* `/rpgitem hand [item] <text>`  查看 `[item]` 物品的手持信息； 设置 `[item]` 物品的手持信息为 `<text>` 。 手持信息显示在物品名下一行左侧。 
* `/rpgitem type [item] <text>`  查看 `[item]` 物品的类型； 设置 `[item]` 物品的类型为 `<text>` 。 类型显示在物品名称下一行右侧。 
* `/rpgitem togglearmorlore [item]`  切换是否隐藏 `[item]` 的物品信息栏，这包括手持信息、类型、伤害、减伤值。 
* `/rpgitem togglepowerlore [item]`  切换是否隐藏 `[item]` 的技能信息栏。 
* `/rpgitem toggbar [item]`  切换是否显示 `[item]` 的耐久值。 
* `/rpgitem barformat [item] [bar_format]`  设置 `[item]` 的耐久值显示样式。`[bar_format]` 可以为：
  * `DEFAULT` 耐久条指示；
  * `NUMERIC` 数字值指示；
  * `NUMERIC_MINUS_ONE` 数字值指示，但比实际耐久值少 1 ，可用于指示弹药量等等；
  * `NUMERIC_HEX` 十六进制数字指示；
  * `NUMERIC_HEX_MINUS_ONE` 十六进制数字减去 1 ；
  * `NUMERIC_BIN` 二进制数字指示；
  * `NUMERIC_BIN_MINUS_ONE` 二进制数字减去 1 。 
* `/rpgitem description [item] add [text]`  在 `[item]` 的最下方添加一行 `[text]` 描述。 
* `/rpgitem description [item] remove [index]`  移除 `[item]` 的第 `[index]` 行描述，并将之后的描述依次前移一行。`[index]` 从 0 开始计数。 如果需要使第 `[index]` 行显示为空行，可以设置该行描述为 ```````` 。 
* `/rpgitem description [item] set [index] [text]`  修改 `[item]` 第 `[index]` 行的描述为 `[text]` 。 
* `/rpgitem description [item] insert [index] [text]`  在 `[item]` 第 `[index]` 行前插入一行描述 `[text]` 。

## 物品参数设置

修改物品的基础参数。

* `/rpgitem damage [item] <damage> <max_damage>`  查看 `[item]` 的伤害值； 设置 `[item]` 的伤害值为固定值 `<damage>` ； 设置 `[item]` 的伤害值范围为 `<damage>` 至 `<max_damage>` 。 
* `/rpgitem armour [item] <percentage>`  查看 `[item]` 的减伤百分比； 设置 `[item]` 的减伤百分比为 `<percentage>`。 当玩家将具有减伤百分比的物品穿戴在身上时，将会在原版防御外，额外按百分比减少伤害。多件装备的减伤百分比互相**乘算**。 
* `/rpgitem durability [item] <durability>`  查看 `[item]` 的耐久值信息； 修改 `[item]` 的最大耐久值和默认耐久值为 `<durability>` 。 
* `/rpgitem durability [item] default [durability]`  修改 `[item]` 的默认耐久值为 `<durability>` 。 
* `/rpgitem durability [item] bound [lowerbound] [upperbound]`  修改 `[item]` 的耐久值上下限分别为 `[upperbound]` 和 `[lowerbound]` 。  关于耐久上下限，详见 [耐久使用方法](advanced.md#nai-jiu-de-te-shu-shi-yong) 。 
* `/rpgitem durability [item] infinite`  将 `[item]` 的耐久设置为无限。使用 `/rpgitem durability [item] -1` 可以达到同样效果。 
* `/rpgitem additemflag [item] [flag]`  为 `[item]` 物品添加标签。支持的物品标签 `[flag]` 有：
  * `HIDE_ATTRIBUTES` 隐藏物品 attribute 属性，默认拥有此标签；
  * `HIDE_DESTROYS` 隐藏物品在冒险模式可破坏方块信息；
  * `HIDE_ENCHANTS` 隐藏物品附魔；
  * `HIDE_PLACED_ON` 隐藏物品在冒险模式可放置方块信息；
  * `HIDE_POTION_EFFECTS` 隐藏药水效果；
  * `HIDE_UNBREAKABLE` 隐藏不可破坏信息。 
* `/rpgitem removeitemflag [item] [flag]`  移除 `[item]` 物品的 `[flag]` 标签。 
* `/rpgitem attrbutemode [item] [mode]`  修改 `[item]` 的 attribute 属性更新模式。支持的模式 `[mode]` 有：
  * `FULL_UPDATE` 在更新已有物品时，强制使物品的 attribute 属性与 attributemodifier 技能所指定的值完全相同。推荐设置为此值。
  * `PARTIAL_UPDATE` 在更新已有物品时，仅修改与 attributemodifier 技能中 UUID 相同的 attribute 属性。如果找不到对应 UUID 的属性，则新添加一个 attribute 属性。默认为此值。 
* `/rpgitem damagemode [item] [mode]`  
  
  修改 `[item]` 的伤害模型。支持的模型 `[mode]` 有：

  * `FIXED` 固定伤害模型。玩家每次造成的伤害均为武器伤害值。默认为此值。
  * `ADDITIONAL` 加成伤害模型。玩家造成的伤害为武器伤害值加上原版伤害值。对于近战，需要额外计入 attribute 属性加成、力量加成、附魔加成；对于弹射物，需要额外计入弹射物本身的伤害（例如箭矢的伤害与速度相关）
  * `VANILLA` 原版伤害模型。玩家造成的伤害仅为原版伤害值。
  * `MULTIPLY` 乘法伤害模型。玩家造成的伤害为武器伤害值乘以原版伤害值。

  
  关于伤害模型，详见 [伤害管理方法](advanced.md#shang-hai-yu-gong-ji-fang-shi-guan-li) 。  

* `/rpgitem enchantment [item] clone`  将 `[item]` 物品的初始附魔设置为与手中物品附魔相同，初始附魔的等级无法被清除。该指令会**全局地**修改 `[item]` 的附魔。如果想要为某一件特定的物品添加附魔，请修改物品的附魔模式为 ALLOW 或 PERMISSION 后，再使用类似原版 `/enchant` 指令的方式为特定物品附魔。 
* `/rpgitem enchantment [item] clear`  将 `[item]` 物品的初始附魔清空。如果该物品的附魔模式为 DISALLOW，则会全局地清空该物品的附魔。 
* `/rpgitem enchantmode [item] [mode]`  修改 `[item]` 的附魔模式。支持的模式 `[mode]` 有：
  * `ALLOW` 允许为其附魔；
  * `DISALLOW` 禁止为其附魔，默认模式。
  * `PERMISSION` 需要相关的权限才可以为其附魔。 
* `/rpgitem cost [item] [usage] <cost>`  查看 `[item]` 在 `[usage]` 方式下的一般使用消耗； 修改 `[item]` 在 `[usage]` 方式下的一般使用消耗为 `<cost>` 。使用方式 `[usage]` 可以为：
  * `hit` 穿戴时被攻击的消耗。该方式造成的消耗无法损毁护甲。[使用 dummy 技能代替](advanced.md#shi-yong-dummy-ji-neng-tong-yi-guan-li) 可以使护甲能够被损毁。
  * `hitting` 近战攻击命中后的消耗。
  * `breaking` 使用物品破坏方块时的消耗。 
* `/rpgitem cost [item] toggle`  在以下二者间切换 `[item]` 在穿戴受伤后的消耗模式：
  * 按照 hit 消耗设置的值消耗。默认状态。
  * 消耗 hit cost 值 \* 受到的伤害量 / 100 。将 hit cost 值设置为 100 后即消耗与所受伤害等量的耐久。

## 物品技能增改

添加、删除、修改、整理物品技能。

* `/rpgitem power [item]`  依次列出 `[item]` 的所有技能，并包含它们的触发。 
* `/rpgitem power [item] [power] <parameter:value> ...`  为 `[item]` 添加技能 `[power]`，并设置部分参数。各技能的使用方法详见 [技能列表](power-list.md) 。 
* `/rpgitem removepower [item] [power] <No.>`  移除 \[item\] 物品上从前往后第 `<No.>` 个 `[power]` 技能。`<No.>` 从 1 开始计数，如不指定则移除第一个。 例如物品上拥有两个 potionself 技能，无论物品的总技能数量，`<No.>` 只计 potionself 的数量，因此它可以被指定为 1 或 2 。下同。 
* `/rpgitem get [item] [power] [No.] <parameter>`  不指定 `<parameter>` 时，获取 `[item]` 物品上第 `[No.]` 个 `[power]` 技能的所有信息； 指定 `<parameter>` 时，获取 `[item]` 物品上第 `[No.]` 个 `[power]` 技能中 `<parameter>` 参数的值。 
* `/rpgitem set [item] [power] [No.] [parameter] [value]`  
  
  修改 `[item]` 物品上第 `[No.]` 个 `[power]` 技能的 `[parameter]` 参数的值为 `[value]` 。

* `/rpgitem reorder [item] [from_index] [to_index]`  将 `[item]` 物品上序号为 `[from_index]` 的技能调整至序号为 `[to_index]` 的技能之前。 序号从 0 开始计数。使用 `/rpgitem dump [item]` 可以查看各个技能的序号。 技能的顺序对执行十分重要，详见 [技能的执行顺序](advanced.md#ji-neng-de-zhi-hang-shun-xu) 。

## 物品组设置

RPGItems 支持创建物品组，这会为部分功能的使用提供便利。

* `/rpgitem creategroup [group]`  创建 ID 为 `[group]` 的物品组。 
* `/rpgitem removegroup [group]`  删除 ID 为 `[group]` 的物品组。其中的物品并不会被删除。 
* `/rpgitem addtogroup [item] [group]`  将 `[item]` 添加至物品组 `[group]` 。 
* `/rpgitem removefromgroup [item] [group]`  将 `[item]` 从物品组 `[group]` 中移除。

## 物品杂项设置

关于物品的一些杂项功能。

* `/rpgitem customitemmodel [item]`  开启/关闭 `[item]` 的自定义物品模型。开启后，可以为该物品指定单独的材质。 
* `/rpgitem drop [item] [entity] <chance>`  查看 `[item]` 在生物 `[entity]` 死亡后的掉率； 设置 `[item]` 在生物 `[entity]` 死亡后的掉率为 `<chance>` 。 
* `/rpgitem permission [item] [permission] [true/false]`  最后参数为 `true` 时，使 `[item]` 的使用需要权限 `[permission]`； 最后参数为 `false` 时，使 `[item]` 无需权限即可使用。默认无需权限。 `[permission]` 默认值为 `rpgitems.item.use.[item_name]` 。 
* `/rpgitem recipe [item]`  为 `[item]` 添加合成配方。 
* `/rpgitem removerecipe [item]`  移除 `[item]` 的所有合成配方。 
* `/rpgitem wgignore [item]`  使 `[item]` 的使用无视 WorldGuard 保护区限制。 
* `/rpgitem updatecmdandentity [item]`  将 `[item]` 中的旧版本命令与物品标签更新为 1.13 版本。可能不稳定。

## 玩家命令

以下为玩家命令，如需要允许玩家使用，需要设置相关权限。

* `/rpgitems info`  类似 `/rpgitem print [item]` ，但只能查看手中物品的信息。 
* `/rpgitems tomodel`  移除手中的 RPGItems 物品的所有功能，但保留外形和描述文本。

## 导入/导出

* `/rpgitem import GIST`  从 GIST 导入物品。 
* `/rpgitem export [item]`  通过 GIST 导出 `[item]` 物品。 
* `/rpgitem loadfile [file]`  导入本地文件 `[file]` 中的物品。 
* `/rpgitem gen-wiki`  - -

## 调试命令

以下指令为显示插件信息、调整插件配置用。

* `/rpgitem version`  获取版本信息。 
* `/rpgitem save-all`  手动保存所有文件。正常情况下配置文件会自动保存。 
* `/rpgitem reload`  重新载入插件。 
* `/rpgitem debug`  显示手中物品的物品标签和插件信息（如果是 RPGItems 插件物品）。 
* `/rpgitem cleanbackup`  清空备份目录下的所有备份。 
* `/rpgitem giveperms`  状态切换：是否需要额外的权限才能使用 `/rpgitem give` 命令。 
* `/rpgitem wgforcerefresh`  状态切换：使用物品是否强制刷新 WorldGuard 保护区。一般不需要修改，仅当出现相关问题时开启。 
* `/rpgitem worldguard`  WorldGuard 相关指令。



