# 物品等级系统（Item Level）

> 本页内容适用于 RPGItems `3.9`。

## 1. 设计理念

等级系统的目标是把“道具强度成长”放到**物品实例**层，而不是只停留在配置层：

- 同一 RPGItem 模板，不同实例可拥有不同等级
- 等级可与镶嵌协同工作
- 等级变化应实时反映到能力判定与描述显示

## 2. 核心行为

- 每个物品实例默认等级：`1`
- 等级持久化在实例 PDC（`rgi_item_level`）
- 通过管理员命令直接读取/设置实例等级

## 3. 与能力系统的关系（大致实现）

等级主要影响两部分：

1. `itemlevelcondition` 判定
2. `LevelDescription` 动态描述

当实例等级变化时，运行时组合缓存会依据新签名重建或复用，确保能力与显示一致。

## 4. LevelDescription 机制

`LevelDescription` 可按等级分段替换 lore（基于配置中的原始 `description`）。

示例：

```yaml
description:
  - "Great Sword"
  - "Damage: &650"
leveldescription:
  "0":
    level: 3
    operation: replaceline
    line: 1
    newlines:
      - "Damage: &6100"
      - "Critical: &c20%"
  "1":
    level: 5
    operation: replacewhole
    line: 0
    newlines:
      - "Great Sword"
      - "Damage: &6200"
      - "Critical: &c25%"
      - "&7This is another lore"
```

规则：

- 行号 `line` 为 `0-based`
- `replaceline`：从 `line` 起替换为 `newlines`
- `replacewhole`：忽略 `line`，替换整个 description
- 选择规则：取满足 `level >= 当前等级` 的最小规则
- 若没有匹配规则，使用原始 `description`
- 不会基于上一等级结果继续叠加替换

## 5. 与镶嵌系统的关系

等级会直接参与镶嵌可用性判定：

- 当实例等级 `< socketMinLevel` 时，该镶嵌无效
- GUI 状态提示会显示等级相关错误

3.9 当前限制：

- 暂不支持“镶嵌品自身等级”的持久化与判定
- 镶嵌上下文中的 `itemlevelcondition` / `LevelDescription` 以容器实例等级为准

## 6. 使用流程

```bash
# 1) 查看主手实例等级
/rpgitem level get <item>

# 2) 设置主手实例等级
/rpgitem level set <item> <level>

# 3) 在配置中添加 itemlevelcondition 或 leveldescription
#    然后观察实例能力/描述变化
```

## 7. 命令详解

> 需要权限：`rpgitem.level`

### 7.1 查询等级

```bash
/rpgitem level get <item>
```

- 读取主手道具实例等级
- `<item>` 必须与主手 RPGItem 一致，否则拒绝

### 7.2 设置等级

```bash
/rpgitem level set <item> <level>
```

- 设置主手道具实例等级
- `level` 最小为 `1`
- 设置后立即刷新实例显示与运行时逻辑

## 8. Condition：itemlevelcondition

新增条件：`rpgitems:itemlevelcondition`

属性：

- `id`：条件 ID（必填）
- `level`：目标等级
- `operation`：比较操作
  - `eq`：等于
  - `gt`：大于
  - `egt`：大于等于
  - `lt`：小于
  - `elt`：小于等于
- `isStatic`：默认 `true`
- `isCritical`：默认 `false`

示例：

```bash
/rpgitem condition add mysword rpgitems:itemlevelcondition id:need-lv5 level:5 operation:egt
```

含义：仅当实例等级 `>= 5` 时该条件通过。

## 9. 实践建议

- 把“解锁新能力”放在 `itemlevelcondition`
- 把“展示数值变化”放在 `LevelDescription`
- 如果同时使用镶嵌，优先明确每类镶嵌的 `socketMinLevel`
- 对关键武器建议固定等级提升来源，避免运营期实例等级混乱
