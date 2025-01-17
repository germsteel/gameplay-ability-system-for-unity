# Unity GAS (Gameplay Ability System)

这是一个基于Unity引擎实现的类似虚幻引擎的游戏能力系统(GAS)。该系统提供了一个灵活且强大的框架，用于实现游戏中的技能、buff、属性等功能。

## 系统架构

### 1. 核心系统 (Core)

- **GameplayAbilitySystem**: 系统核心单例，负责管理所有AbilitySystemComponent
- **AbilitySystemComponent**: 能力系统组件，需要添加到具有技能的游戏对象上
- **GasHost**: 系统宿主对象，负责系统的生命周期和更新

### 2. 技能系统 (Ability)

- **AbstractAbility**: 技能抽象基类，定义技能的基本属性和行为
  - 名称、标签
  - 冷却时间
  - 消耗
  - 技能效果
- **AbilitySpec**: 技能实例规格说明
- **AbilityAsset**: 技能资产，用于配置技能数据
- **AbilityContainer**: 技能容器，管理技能的生命周期

### 3. 属性系统 (Attribute)

- **AttributeBase**: 属性基类
  - 支持基础值(BaseValue)和当前值(CurrentValue)
  - 提供值变化前后的事件系统
  - 支持最大/最小值限制
  - 支持不同计算模式
- **AttributeSet**: 属性集合，用于管理多个属性
- **AttributeValueProcessor**: 属性值处理器
- **AttributeAggregator**: 属性聚合器，处理属性修改

### 4. 效果系统 (Effects)

- **GameplayEffect**: 游戏效果的基础实现
  - 持续策略：
    * 瞬时效果(Instant)
    * 永久效果(Infinite)
    * 限时效果(Duration)
  - 效果提示(Cue)：
    * 执行时(OnExecute)
    * 移除时(OnRemove)
    * 添加时(OnAdd)
    * 激活时(OnActivate)
    * 停用时(OnDeactivate)
  - 效果堆叠
  - 效果修改器
  - 授予能力
  - 周期性执行

### 5. 事件系统 (EventSystem)

用于处理系统内的事件通信，包括：

- 技能触发事件
- 效果应用事件
- 属性变化事件

### 6. 标签系统 (Tags)

用于技能和效果的分类与控制：

- 技能标签
- 效果标签
- 激活条件标签
- 取消条件标签

## 使用方法

### 1. 初始化系统

```csharp
// 获取GAS实例
var gas = GameplayAbilitySystem.GAS;
```

### 2. 添加能力系统组件

```csharp
// 在游戏对象上添加AbilitySystemComponent
var abilitySystem = gameObject.AddComponent<AbilitySystemComponent>();
```

### 3. 创建技能

```csharp
// 继承AbstractAbility创建自定义技能
public class MyCustomAbility : AbstractAbility
{
    public MyCustomAbility(AbilityAsset abilityAsset) : base(abilityAsset)
    {
    }

    public override AbilitySpec CreateSpec(AbilitySystemComponent owner)
    {
        // 实现技能逻辑
    }
}
```

### 4. 应用效果

```csharp
// 创建和应用效果
var effect = new GameplayEffect(effectAsset);
abilitySystem.ApplyGameplayEffect(effect);
```

## 特点

1. **模块化设计**: 系统各个组件高度模块化，易于扩展和维护
2. **数据驱动**: 通过ScriptableObject配置技能和效果数据
3. **灵活的标签系统**: 使用标签系统实现复杂的技能互动
4. **完整的事件系统**: 提供丰富的事件回调
5. **性能优化**: 使用对象池和缓存优化性能

## 实现原理

系统通过以下方式实现类似虚幻引擎GAS的功能：

1. **技能系统**:

   - 使用组件化设计，将技能逻辑封装在独立的类中
   - 通过AbilitySpec管理技能实例的生命周期
   - 使用标签系统控制技能的激活条件和互斥关系
2. **属性系统**:

   - 实现基础属性和派生属性
   - 支持属性的修改器和计算器
   - 提供属性变化的事件通知
   - 支持最大/最小值限制
   - 支持多种计算模式
   - 完整的Pre/Post事件系统
3. **效果系统**:

   - 支持三种效果持续策略：
     * 瞬时效果：立即生效并结束
     * 永久效果：持续到被手动移除
     * 限时效果：在指定时间后自动结束
   - 丰富的效果提示系统：
     * 执行时提示
     * 移除时提示
     * 添加时提示
     * 激活/停用时提示
   - 支持效果堆叠和覆盖
   - 支持效果修改器
   - 支持授予能力
   - 支持周期性执行
   - 支持效果免疫
4. **事件系统**:

   - 使用观察者模式实现事件系统
   - 支持事件的优先级和过滤
   - 提供完整的事件生命周期回调
   - 支持属性变化的前置和后置事件

## 注意事项

1. 确保在使用技能系统前正确初始化GameplayAbilitySystem
2. 合理设计技能的标签系统，避免标签冲突
3. 注意管理效果的生命周期，防止内存泄漏
4. 在性能敏感的场景中谨慎使用事件系统
5. 正确处理效果的堆叠和覆盖规则
6. 合理设置属性的计算模式和修改器

## 最佳实践

1. **技能设计**:

   - 将复杂技能拆分为多个简单效果
   - 使用标签系统控制技能互斥
   - 合理设置技能的冷却和消耗
2. **效果设计**:

   - 选择合适的效果持续策略
   - 合理使用效果堆叠
   - 注意效果的应用条件
3. **属性设计**:

   - 合理设置属性的计算模式
   - 使用事件系统监控关键属性变化
   - 注意属性修改器的优先级
4. **性能优化**:

   - 合理使用对象池
   - 避免过多的事件监听
   - 优化效果的更新频率
