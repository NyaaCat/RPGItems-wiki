# 模板功能（Template）

RPGItems插件具有模板功能，可以更为方便地从一个父道具中制作各类子道具。

## 模板命令

**创建一个父道具作为模板**

```
/rpgitem template create
```

**对子道具应用父道具的属性**

```
/rpgitem template apply
```

**删除指定的模板**

```
/rpgitem template delete
```

## 技能占位符命令

使用命令对父道具中的指定技能的指定参数创建占位符，  
创建占位符的技能参数在应用至子道具时不会对子道具的对应技能参数造成修改。

**增加占位符**

```
/rpgitem template placeholder add
```

**列出已设置的占位符**

```
/rpgitem template placeholder list
```

**移除占位符**

```
/rpgitem template placeholder remove
```

## 简易使用方法说明

模板功能的使用并不复杂，这里给出一个简易的使用流程

**1. 为父道具创建模板**

`/rpgitem template create parent_item`

**2. 通过父道具创建一个子道具**

`/rpgitem clone parent_item subitem_1`

**3. 为父道具设置技能占位符（这里以技能ice为例）**

`/rpgitem template placeholder add parent_item ice-0:cooldown`

其中`ice-0`为设定ice技能时默认分配的`powerId`，这个参数可以自定义  
`cooldown`为ice技能中的冷却时间参数

**4. 模板应用**

完成占位符设置后，并且在修改子道具的对应的ice技能的`cooldown`后  
如果父道具发生了其他参数变化，在使用`/rpgitem template apply parent_item`时  
子道具的ice技能中的`cooldown`参数不会发生任何变化
