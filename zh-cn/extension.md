# 扩展插件

RPGItems支持通过扩展插件来添加新的技能、条件、标记，也可以同其他插件进行联动，比如InfinteInfernal的Rage与Mana系统。

## 安装扩展插件

将扩展插件的jar文件放入 `plugins/RPGItems/ext` 文件夹内, 并且执行 `/rpgitem reload` 就可以使扩展内容生效。

## 开发相关

请先看一下下方列表中的插件以便于了解如何去编写一个RPGItems的扩展插件。

## Extension list

这些是我们（NyaaCat）开发的并用于我们自己服务器中的扩展插件。

如果您编写了一个扩展插件，您可以将它加入到此列表内！请编辑此文档并PR文件以便我们检查并将它整合入文档内（链接在页面上方）。

- [RPGItems-ext-inf](https://github.com/NyaaCat/rpgitems-ext-inf) 用于联动 [InfiniteInfernal](https://github.com/NyaaCat/InfiniteInfernal) 的扩展插件, 一款也提供了Rage/Mana系统的自定义怪物控制插件。
- [RPGItems-ext-throwable](https://github.com/NyaaCat/rpgitems-ext-throwable) 让每一样物品都可以发射出去！ - 发射任何物品，支持自定义模型数据并且可以像箭矢一样造成伤害。需要 [ProtocolLib](https://www.spigotmc.org/resources/protocollib.1997/)才可以正常使用。
- [RPGItems-ext-minion](https://github.com/NyaaCat/RPGItems-ext-minion) 召唤随从同你作战！
