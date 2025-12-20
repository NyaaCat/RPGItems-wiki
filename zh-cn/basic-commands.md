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

