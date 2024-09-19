# Godot中的主题样式 Theme 概述 - UI系列04

### 1. 什么是 Theme

在 Godot 4 中，`Theme` 是一种资源类型，用于定义用户界面 (UI) 控件的外观和风格，类似于`皮肤`。

通过 Theme，开发者可以统一设置控件的颜色、字体、边框、图片等视觉元素，确保项目的 UI 具有一致性和专业性。

Theme 是基于 `Resource` 类的资源文件，通常保存为 `.theme` 或 `.tres` 格式，并可以在项目中重复使用。

它是构建美观、统一 UI 的关键工具，适用于所有 `Control` 类型的节点。

### 2. Godot 4 项目中 Theme 的应用原则

在实际开发中，Godot 4 的 Theme 使用通常遵循以下原则：

- **全局一致性**：推荐为整个项目设置一个全局 `Theme` 文件，以确保所有 UI 控件在不同场景中保持一致的外观。这样可以减少样式定义的重复工作，并简化项目的维护。
  
- **局部定制**：虽然全局 Theme 是最佳实践，但在特定场景或控件上可能需要特殊的样式定制。此时，可以创建从全局 Theme 继承的局部 `Theme` 覆盖，或对特定控件进行样式调整，而不影响全局的样式设置。

- **避免冗余**：不建议为每个控件单独创建 `Theme` 文件，因为这会导致样式管理的复杂化，增加开发和维护的成本。尽量通过继承和覆盖机制，减少样式文件的数量和冗余度。

### 3. Theme 的具体用法

#### **创建与应用 Theme**

- **创建 Theme**：可以在资源管理器中右键选择“新建资源”，然后选择 `Theme` 资源类型。创建后的 Theme 可以通过 Theme Editor 进行编辑，定义颜色、字体、StyleBox、图标等属性。

- **应用 Theme**：可以将 Theme 资源应用到 `Control` 节点的 `theme` 属性上，这样该节点及其子节点会继承此 Theme。如果需要全局应用 Theme，可以在项目设置中设置默认 Theme，确保所有控件使用相同的风格。

- **局部覆盖**：在某些情况下，可以在特定控件上设置独立的 Theme 覆盖其默认样式，例如为某个按钮设置特殊的颜色或字体。局部覆盖通常用于处理特定的设计需求。

### 4. 如何用代码动态控制 Theme

在 Godot 4 中，可以通过代码动态控制和修改 Theme。以下是一些常见的操作示例：

```gdscript
# 动态加载并应用 Theme
var my_theme = load("res://path_to_theme_file.tres")
$"ControlNode".theme = my_theme

# 动态修改 Theme 中的属性
var my_theme = $ControlNode.theme.duplicate() # 创建当前 Theme 的副本
my_theme.set_color("font_color", "Button", Color.red) # 修改 Button 控件的字体颜色
$ControlNode.theme = my_theme # 应用修改后的 Theme

# 动态移除 Theme，恢复默认样式
$ControlNode.theme = null
```

通过代码控制 Theme，开发者可以在运行时根据需要动态调整 UI 的外观，例如在夜间模式和日间模式之间切换，或者根据玩家的设置更改 UI 的颜色或风格。这种灵活性使得 Theme 成为构建自定义和响应式 UI 的重要工具【9†source】【10†source】。

### 5. Godot 中 主题样式实现的检测规则：

对于任意控件，其主题项的查找会是这样的：

1. 检查相同数据类型和名称的本地重写。

2. Using control's type variation, class name and parent class names
   利用控件的类型变换，类名和父类名:

   * a: 从自身开始检查每个控件，看看它是否设置了主题属性；

   * b: 如果设置了，就在该主题中查找名称、数据、主题类型都相同的项目；

   * c: 如果没有自定义主题，或者主题中没有匹配的条目，就前往父控件；

   * d: 重复步骤 a 至 c，到场景树的根节点或者非控件节点为止。

3. Using control's type variation, class name and parent class names check the project-wide theme, if it's present.

4. Using control's type variation, class name and parent class names check the default theme.

> 注意：
>
> 上面的说法是直接粘贴的官方文档，看起来有些晦涩难懂，实际上，只需要搞清楚主题的优先级即可：
>
> **项目主题 < 场景主题 < 父节点控件主题 < 子节点控件主题 < Theme Overrides**

