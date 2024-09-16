# Theme Overrides 使用方式

`Theme Overrides` 在 Godot 4 中通常是为单个节点设置的，它允许你在特定控件上覆盖全局 `Theme` 或场景 `Theme` 的某些属性。然而，在某些情况下，你可能希望对多个节点进行统一的 `Theme Overrides` 设置。

### 1. **必须在单个节点上设置吗？**
`Theme Overrides` 通常是通过在 `Control` 节点的 `Theme Overrides` 属性中直接进行配置。这种覆盖是局部的，仅影响当前节点及其子节点。这意味着每个节点可以有自己独立的 `Theme Overrides`，不会影响其他节点。

### 2. **统一对多个节点进行设置的方式**
虽然 `Theme Overrides` 本质上是局部的，但仍然可以通过一些方法对多个节点进行统一的设置：

- **继承与复用**：将需要相同 `Theme Overrides` 设置的控件放在一个共同的父节点下。然后，在父节点上设置 `Theme Overrides`，所有子节点将自动继承这些覆盖。如果需要某些子节点有特定的不同设置，可以进一步在这些节点上局部覆盖。

- **自定义控件**：如果需要多个类似的控件使用相同的 `Theme Overrides`，你可以创建一个自定义控件类，并在这个类的 `_ready()` 方法中设置 `Theme Overrides`。然后在项目中复用这个自定义控件类，这样每个实例都会应用相同的 `Theme Overrides`。

  ```gdscript
  extends Button

  func _ready():
      # Set the Theme Overrides
      var style = StyleBoxFlat.new()
      style.bg_color = Color.red
      add_stylebox_override("normal", style)
  ```

- **批量代码设置**：你可以通过代码遍历多个节点并设置相同的 `Theme Overrides`。这在需要动态或基于特定条件进行样式调整时非常有用。

  ```gdscript
  func _ready():
      var nodes = [$Button1, $Button2, $Button3]
      for node in nodes:
          var style = StyleBoxFlat.new()
          style.bg_color = Color.blue
          node.add_stylebox_override("normal", style)
  ```

### 总结
虽然 `Theme Overrides` 通常在单个节点上进行设置，但你可以通过继承、创建自定义控件类，或者使用代码批量处理的方式，统一对多个节点进行设置。这种灵活性允许开发者更高效地管理复杂的 UI 样式需求，同时保持项目的维护性和一致性