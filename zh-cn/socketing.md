# 镶嵌系统（Socketing）

> 本页内容适用于 RPGItems `3.9`。

## 1. 设计理念

镶嵌系统的目标是让 RPGItem 从“固定能力的单一物品”变成“可组合、可成长的实例化物品”：

- 一个物品实例可以挂载多个镶嵌品
- 能力组合以实例为单位生效，而不是改全局配置
- 支持通过 GUI 进行低门槛调整
- 支持纯命令热更新配置，不依赖手改 yml

## 2. 核心概念

- `容器物品`：可接受镶嵌的主物品
- `镶嵌品`：提供额外 power/marker/condition 的 RPGItem
- `镶嵌槽`：实例上的一个镶嵌位置
- `有效镶嵌`：同时满足标签、等级、重量约束

### 2.1 角色互斥

同一个 RPGItem 不能同时作为“容器”与“镶嵌品”。

如果你尝试开启冲突角色，命令会被拒绝。必须先关闭冲突角色，例如：

```bash
/rpgitem socket myitem container false
/rpgitem socket myitem socketitem true
```

## 3. 数据与配置模型

### 3.1 容器字段

```yaml
socketAcceptTags:
  - GEM
socketMaxWeight: 10
socketInsertLine: 1
```

- `socketAcceptTags`：容器可接受标签（命中任意一个即可）
- `socketMaxWeight`：容器可承受最大总重量
- `socketInsertLine`：将镶嵌描述插入 lore 的行号（`0-based`）

### 3.2 镶嵌品字段

```yaml
socketTags:
  - GEM
socketWeight: 3
socketMinLevel: 2
socketingDescription:
  - "&7[Gem] +10 Damage"
```

- `socketTags`：镶嵌品标签
- `socketWeight`：重量
- `socketMinLevel`：容器最低等级要求
- `socketingDescription`：镶嵌后插入容器 lore 的文本

### 3.3 标签与匹配规则

- `socketAcceptTags` 为空：不接受任何镶嵌（默认）
- `socketTags` 为空：不匹配任何标签
- 特殊标签 `ANY`：可匹配任意标签

匹配失败、等级不足、重量超限都会导致该镶嵌无效。

### 3.4 实例持久化（PDC）

镶嵌与等级都存储在物品实例的 PDC 中：

- `rgi_sockets`：按顺序存储镶嵌 RPGItem ID
- `rgi_item_level`：实例等级
- `rgi_instance_cache_key`：实例运行时缓存键

> 镶嵌只保存 RPGItem ID，不保存整个 ItemStack。

## 4. 运行时生效逻辑（大致实现）

当系统识别到“该实例存在等级或镶嵌变化”时，会构建运行时组合物品。

组合顺序：`容器 -> 镶嵌1 -> 镶嵌2 -> ...`

生效规则：

- `condition`：共享并统一判定
- `marker`：按顺序依次处理
- `power`：按 trigger 依次触发

如果某个镶嵌 ID 无效（例如该 ID 物品已被删除），会保留该记录但在运行时禁用，不会导致崩溃。

## 5. 性能与缓存

为避免每次触发都重建组合逻辑，RPGItems 会为实例构建运行时缓存：

- 基于 `instance_cache_key` + 实例签名复用
- 物品配置变更/重载时清空缓存
- 玩家物品刷新采用“按玩家分帧队列”处理，每 tick 处理有限玩家数

## 6. GUI 使用

打开命令：

```bash
/rpgitems socketing
```

管理员兼容命令：

```bash
/rpgitem socketing
```

权限：

- 用户：`rpgitems.socketing`
- 管理员：`rpgitem.socketing`

界面为 3 行箱子：

- 左侧 3x3 仅中心槽可放容器
- 右侧为镶嵌槽
- 最后一列为状态提示物品（lore 汇总所有错误/状态）

## 7. 快速上手示例

```bash
# 1) 设置镶嵌品
/rpgitem socket ruby_gem socketitem true
/rpgitem socket ruby_gem tags set GEM
/rpgitem socket ruby_gem weight 3
/rpgitem socket ruby_gem minlevel 2
/rpgitem socket ruby_gem lore clear
/rpgitem socket ruby_gem lore add &7[Gem] +10 Damage

# 2) 设置容器
/rpgitem socket great_sword container true
/rpgitem socket great_sword accepttags set GEM
/rpgitem socket great_sword maxweight 10
/rpgitem socket great_sword insertline 1

# 3) 检查配置
/rpgitem socket ruby_gem info
/rpgitem socket great_sword info

# 4) 打开 GUI 调整实例镶嵌
/rpgitems socketing
```

## 8. 命令详解（管理员配置）

> 以下均需 `rpgitem.socket` 权限。

### 8.1 查看配置

```bash
/rpgitem socket <item> info
```

显示容器参数、镶嵌参数与 socketingDescription 当前内容。

### 8.2 角色开关

```bash
/rpgitem socket <item> socketitem <true|false>
/rpgitem socket <item> container <true|false>
```

- `socketitem true`：启用镶嵌品角色（空标签时自动补 `ANY`）
- `container true`：启用容器角色（空 acceptTags 时自动补 `ANY`）
- 冲突角色会被拒绝，需先手动关闭

### 8.3 容器标签（accepttags）

```bash
/rpgitem socket <item> accepttags list
/rpgitem socket <item> accepttags set GEM,RUNE
/rpgitem socket <item> accepttags add GEM
/rpgitem socket <item> accepttags remove GEM
/rpgitem socket <item> accepttags clear
```

- 支持逗号或空格分隔
- 启用镶嵌品角色时不能设置非空 `accepttags`

### 8.4 镶嵌品标签（tags）

```bash
/rpgitem socket <item> tags list
/rpgitem socket <item> tags set GEM,RUNE
/rpgitem socket <item> tags add GEM
/rpgitem socket <item> tags remove GEM
/rpgitem socket <item> tags clear
```

- 启用容器角色时不能设置非空 `tags`

### 8.5 数值参数

```bash
/rpgitem socket <item> maxweight <value>
/rpgitem socket <item> insertline <line>
/rpgitem socket <item> weight <value>
/rpgitem socket <item> minlevel <value>
```

- `maxweight`：容器最大重量（>=0）
- `insertline`：插入行（>=0，`0-based`）
- `weight`：镶嵌品重量（>=0）
- `minlevel`：镶嵌品最低等级（>=1）

### 8.6 镶嵌描述（lore）

```bash
/rpgitem socket <item> lore list
/rpgitem socket <item> lore add <text>
/rpgitem socket <item> lore insert <line> <text>
/rpgitem socket <item> lore set <line> <text>
/rpgitem socket <item> lore remove <line>
/rpgitem socket <item> lore clear
```

- 行号均为 `0-based`
- 修改后立即热更新并持久化

## 9. 常见问题

### Q1：为什么我放入镶嵌品后显示不生效？

优先检查：

- 标签是否匹配
- 当前实例等级是否达到 `socketMinLevel`
- 总重量是否超过 `socketMaxWeight`
- 镶嵌品 ID 是否有效

### Q2：为什么配置改了没生效？

`/rpgitem socket ...` 命令会自动热更新。
如果是老物品实例，建议重新放入 GUI 或重新持有触发一次实例刷新。
