# 基础命令

**RPGItems** 有着完善的命令补全，大部分情况下，使用游戏内的补全提示即可正常使用插件。

这里列出了 **RPGItems** 中用来管理与制作道具的基础命令。

## 创建道具

```
/rpgitem create myitem
```

## 移除道具

```
/rpgitem remove myitem
```

## 道具列表

```
/rpgitem list [page] <param>
```

`<param>`附加参数值可以用于筛选。

* `display` - 道具的展示名
* `name` - RPGItem数据库中的物品名

命令示例

```
/rpgitem list display:Hero
```

以上指令将会列出所有展示名中包含Hero的道具

## 给与道具

```
/rpgitem give [item] <target> <amount>
```

命令示例

```
/rpgitem give mysword Notch 2
```

以上指令将会给予玩家`Notch`两个展示名为`mysword`的道具

你同样也可以给与自己展示名为`mysword`的道具

```
/rpgitem give mysword
```

## 从yml文件中读取道具

```
/rpgitem loadfile mysword-item.yml
```

各个道具储存在RPGItems文件夹下的`items\`文件夹内

## 从文件中重载道具

```
/rpgitem reloaditem mysword
```

## 重载RPGItems插件

```
/rpgitem reload
```

## 导入与导出道具

```
/rpgitem import [GIST|URL] [value]
```

```
/rpgitem export mysword
```

* 你需要拥有一个Gist API Key才可以使用这个功能

## 复制道具

```
/rpgitem clone mysword mynewsword
```

## 转储道具文件

```
/rpgitem dump mysword
```

生成一个记录了道具原始设置的YAML文件，用于检查

## 保存所有道具

```
/rpgitem save-all
```

将所有道具保存到磁盘。

## 查看版本

```
/rpgitem version
```

显示当前RPGItems插件版本。

## 给与权限控制

```
/rpgitem giveperms
```

切换give命令是否需要权限 (`rpgitem.give.<道具名>`)。

## 道具信息

```
/rpgitem info
```

显示主手中RPGItem的详细信息，包括道具属性和是否为模型。

需要权限: `rpgitem.info`

## 转换为模型

```
/rpgitem tomodel
```

将主手中的RPGItem转换为模型（不具有技能功能的静态副本）。

需要权限: `rpgitem.tomodel`

## 镶嵌与物品等级

> 本节内容适用于 RPGItems `3.9` 版本。

### 打开镶嵌 GUI

```
/rpgitems socketing
```

- 需要权限：`rpgitems.socketing`（用户）
- 管理员兼容命令：`/rpgitem socketing`（`rpgitem.socketing`）
- 打开后使用 3 行箱子界面编辑主道具的镶嵌
- 取回中间槽位道具时会立即关闭 GUI

### 通过命令热更新镶嵌配置

```
/rpgitem socket <item> info
/rpgitem socket <item> socketitem <true|false>
/rpgitem socket <item> container <true|false>
/rpgitem socket <item> accepttags <list|set|add|remove|clear> [tags]
/rpgitem socket <item> tags <list|set|add|remove|clear> [tags]
/rpgitem socket <item> maxweight <value>
/rpgitem socket <item> insertline <line>
/rpgitem socket <item> weight <value>
/rpgitem socket <item> minlevel <value>
/rpgitem socket <item> lore <list|add|insert|set|remove|clear> [params]
```

- `socketitem true`：将道具设为镶嵌品（若无标签会默认给 `ANY`）
- `container true`：将道具设为容器（若无接收标签会默认给 `ANY`）
- `accepttags`：容器可接收标签
- `tags`：镶嵌品标签
- `tags` 参数支持逗号分隔，如 `GEM,RUNE`

### 查询/设置道具实例等级

```
/rpgitem level get <item>
/rpgitem level set <item> <level>
```

- 需要权限：`rpgitem.level`（管理员）
- 仅作用于主手中的**道具实例**
- `<item>` 必须与主手道具匹配
- 等级最小值为 `1`
