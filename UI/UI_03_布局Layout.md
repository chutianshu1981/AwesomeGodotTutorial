# Godot 4 UI 控件布局类型

在 Godot 4 中，`Layout Mode` 主动模式，主要分为两种：`Anchors` 和 `Position`。

但如果你把控件放入 **容器** **container** 类型父节点中，`Layout Mode` 就会变成 `Container` 且不允许改动，包括其他如 `Transform` 等定位用属性。

这 **三种模式** 决定了 UI 控件如何在父节点中进行定位和调整大小。让我们详细探讨这三种布局模式及其区别。

> 注意 ！！
>
> godot 4 中，我暂时没有发现和 Css 中类似的绝对定位，（如果有哪位小伙伴知道的话，请告知）
>
> godot 4 中的所有定位模式，都是相对于父节点的左上角顶点（默认的父节点 Pivot 位置）

## 1. Anchors 模式

`Anchors` 模式使用锚点（anchors）和 变形（Transform）来定位和调整控件的大小。

控件的边缘相对于父控件的边缘进行定位，锚点值在 0 到 1 之间，表示控件边缘相对于父控件边缘的比例。边距则用于在锚点基础上进行微调。

### 关键属性：
- **Anchors Points**：
  `anchor_left`, `anchor_top`, `anchor_right`, `anchor_bottom`。锚点值在 0 到 1 之间。
- **Anchors Offets**：
  `offset_left`, `offset_top`, `offset_right`, `offset_bottom`。偏移量用于在锚点基础上进行微调。
- **Grow Direction**:
  `grow_horizontal` 和 `grow_vertical` 配合 `set_h_grow_direction` 、`get_h_grow_direction` 、`set_v_grow_direction` 、`get_v_grow_direction`;设置控件增大的方向

### 示例：
```gdscript
var label = Label.new()
label.text = "Hello, World!"
label.anchor_left = 0.5
label.anchor_top = 0.5
label.anchor_right = 0.5
label.anchor_bottom = 0.5
label.offset_left = -50
label.offset_top = -15
label.offset_right = 50
label.offset_bottom = 15
add_child(label)
```
在这个示例中，`Label` 控件会根据其父控件的中心点进行定位，并通过边距设置其大小和位置。

## 2. Position 模式

`Position` 模式使用控件的 `Rect` 属性来直接设置位置和大小。这种模式下，控件的位置和大小是固定的，不会随父控件的变化而调整。

### 属性：
- **Rect**：包括 `rect_position` 和 `rect_size`。直接设置控件的位置和大小。

### 示例：
```gdscript
var label = Label.new()
label.text = "Hello, World!"
label.rect_min_size = Vector2(100, 30) # 设置最小大小
label.rect_position = Vector2(50, 50) # 设置位置
add_child(label)
```
在这个示例中，`Label` 控件的位置是 `(50, 50)`，并且大小是固定的，不会随父控件的变化而变化。

在 Godot 4 中，如果将控件放入 `Container` 容器控件中，确实会自动改为 `Container` 布局模式，并且不能更改。这是因为 `Container` 控件负责管理其子控件的布局，并自动调整子控件的位置和大小，以适应其内部的布局规则。

## 3. Container 模式 

### 3.1 Container 容器控件

`Container` 是一个基类，用于创建各种自动布局的容器控件。以下是一些常见的 `Container` 子类及其布局特性：

1. **HBoxContainer**：水平排列子控件，子控件从左到右依次排列。
2. **VBoxContainer**：垂直排列子控件，子控件从上到下依次排列。
3. **GridContainer**：按网格排列子控件，子控件在网格中按行和列排列。
4. **MarginContainer**：为所有子控件设置统一的边距。
5. **CenterContainer**：将子控件居中排列。
6. **PanelContainer**：带有装饰性边框和背景的容器控件。

### 3.2 Container 布局模式的特点

- **自动布局**：`Container` 会根据其特定的布局规则，自动调整其子控件的位置和大小。子控件的位置和大小由 `Container` 决定，而不是通过锚点和边距或直接设置位置和大小。
- **不可更改 Layout Mode**：当控件放入 `Container` 中时，`Layout Mode` 会自动设为 `Container`，并且无法更改，因为 `Container` 本身负责布局管理。

#### 示例代码

以下是一些使用 `Container` 容器控件的示例代码：

#### HBoxContainer 示例

```gdscript
var hbox = HBoxContainer.new()
add_child(hbox)

var label1 = Label.new()
label1.text = "Label 1"
hbox.add_child(label1)

var label2 = Label.new()
label2.text = "Label 2"
hbox.add_child(label2)
```

在这个示例中，`Label` 控件会被水平排列在 `HBoxContainer` 中，位置和大小由 `HBoxContainer` 自动管理。

#### VBoxContainer 示例

```gdscript
var vbox = VBoxContainer.new()
add_child(vbox)

var button1 = Button.new()
button1.text = "Button 1"
vbox.add_child(button1)

var button2 = Button.new()
button2.text = "Button 2"
vbox.add_child(button2)
```

在这个示例中，`Button` 控件会被垂直排列在 `VBoxContainer` 中，位置和大小由 `VBoxContainer` 自动管理。

#### GridContainer 示例

```gdscript
var grid = GridContainer.new()
grid.columns = 2
add_child(grid)

for i in range(4):
    var label = Label.new()
    label.text = "Item %d" % i
    grid.add_child(label)
```

在这个示例中，`Label` 控件会按网格排列在 `GridContainer` 中，位置和大小由 `GridContainer` 自动管理。

## 4. 总结

- **Anchors 模式**：
  - 使用锚点和边距来定位和调整控件大小。
  - 控件的位置和大小会随父控件的变化而自动调整。
  - 适用于响应式布局设计。

- **Position 模式**：
  - 直接使用 `Rect` 属性来设置控件的位置和大小。
  - 控件的位置和大小是固定的，不随父控件的变化而调整。
  - 适用于简单、固定布局。

- **被动 Container 模式**：
  - 将控件放入 `Container` 容器控件中时，`Layout Mode` 会自动改为 `Container`，并且不能更改。
  - `Container` 控件负责自动布局其子控件，位置和大小由 `Container` 决定。
  - 常见的 `Container` 子类包括 `HBoxContainer`、`VBoxContainer`、`GridContainer` 等，它们各自有不同的布局规则。

