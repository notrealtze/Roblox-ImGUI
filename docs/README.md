# Roblox ImGui — Reference

## Setup

```lua
local SourceURL = "https://raw.githubusercontent.com/notrealtze/Roblox-ImGUI/refs/heads/main/ImGui_lua.txt"
local ImGui = loadstring(game:HttpGet(SourceURL))()
```

---

## Window

Every element lives inside a tab, which lives inside a window.

```lua
local Window = ImGui:CreateWindow({
    Title    = "My Window",
    Size     = UDim2.new(0, 350, 0, 300),
    Position = UDim2.new(0.5, 0, 0, 70),
})
Window:Center()
```

| Option | Type | Default | Description |
|---|---|---|---|
| `Title` | string | `"Depso UI"` | Title bar text |
| `Size` | UDim2 | — | Initial window size |
| `Position` | UDim2 | — | Initial position |
| `MinSize` | Vector2 | `160, 90` | Minimum resize size |
| `NoTitleBar` | bool | false | Hide the title bar |
| `NoCollapse` | bool | false | Hide the collapse button |
| `NoClose` | bool | false | Hide the close button |
| `NoResize` | bool | false | Disable resize handle |
| `NoDrag` | bool | false | Disable title bar dragging |
| `NoSelectEffect` | bool | false | Disable border highlight on hover |
| `TabsBar` | bool | true | Show the tab bar. Set `false` to hide |
| `AutoSize` | string | — | `"X"`, `"Y"`, or `"XY"` — auto size to content |
| `ConfigFile` | string | Window title | Default config save name |
| `CloseCallback` | function | — | Called when window closes |
| `Colors` | table | — | Color overrides per element type |

### Window Methods

```lua
Window:Center()                     -- Center on screen
Window:SetTitle("New Title")
Window:SetPosition(UDim2.new(...))
Window:SetSize(UDim2.new(...))      -- or Vector2
Window:SetVisible(true/false)
Window:SetOpen(true/false)          -- collapse/expand
Window:Close()
Window:Remove()
```

---

## Tabs

```lua
local Tab = Window:CreateTab({ Name = "Main" })
Window:ShowTab(Tab)   -- make this tab visible
```

| Option | Type | Description |
|---|---|---|
| `Name` | string | Tab button label |
| `Visible` | bool | Show this tab on creation |
| `NoAnimation` | bool | Skip slide-in animation |

All elements are created on a tab or inside a container element.

---

## Elements

### Button

```lua
Tab:Button({
    Text     = "Click Me",
    Callback = function(self)
        print("clicked")
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Text` | string | Button label |
| `Size` | UDim2 | Override size |
| `BackgroundColor3` | Color3 | Background color |
| `CornerRadius` | UDim | Rounding |
| `Callback` | function(self) | Fired on click |

---

### Label

```lua
local Lbl = Tab:Label({
    Text        = "Hello World",
    TextColor3  = Color3.fromRGB(255, 255, 0),
    TextWrapped = true,
    RichText    = true,
})
-- update later
Lbl.Text = "Updated"
```

| Option | Type | Description |
|---|---|---|
| `Text` | string | Display text |
| `TextWrapped` | bool | Wrap long text |
| `RichText` | bool | Enable RichText tags |
| `TextColor3` | Color3 | Text color |

---

### Checkbox (Toggle)

```lua
local Toggle = Tab:Checkbox({
    Label    = "Enable Feature",
    Value    = false,
    Callback = function(self, Value)
        print(Value)  -- true/false
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Label` | string | Display label |
| `Value` | bool | Initial state |
| `Callback` | function(self, value) | Fired on change |

**Methods:**
```lua
Toggle:SetTicked(true)   -- set state, fires callback
Toggle:Toggle()          -- flip current state
Toggle:GetValue()        -- returns bool
```

> Any Checkbox with a `Label` is automatically tracked by the config system.

---

### RadioButton

Same API as Checkbox. Rendered as a circle instead of a box.

```lua
Tab:RadioButton({
    Label    = "Option A",
    Value    = true,
    Callback = function(self, Value) end,
})
```

---

### InputText (Textbox)

```lua
local Input = Tab:InputText({
    Label       = "Username",
    Value       = "default",
    PlaceHolder = "Type here...",
    Callback    = function(self, Value)
        print(Value)
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Label` | string | Display label |
| `Value` | string | Initial text |
| `PlaceHolder` | string | Placeholder text |
| `MultiLine` | bool | Enable multiline input |
| `Callback` | function(self, value) | Fired on every keystroke |

**Methods:**
```lua
Input:SetValue("new text")
Input:GetValue()   -- returns string
Input:Clear()
```

> Any InputText with a `Label` is automatically tracked by the config system.

---

### InputTextMultiline

Shorthand for a multiline InputText with a fixed height.

```lua
Tab:InputTextMultiline({
    PlaceHolder = "Enter notes...",
})
```

---

### Slider

```lua
local Slider = Tab:Slider({
    Label    = "Speed",
    Value    = 10,
    MinValue = 0,
    MaxValue = 100,
    Format   = "%.d",
    ReadOnly = false,
    Callback = function(self, Value)
        print(Value)
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Label` | string | Display label |
| `Value` | number | Initial value |
| `MinValue` | number | Minimum |
| `MaxValue` | number | Maximum |
| `Format` | string | Printf format string. `"%.d"` = integer, `"%.2f"` = 2 decimal places, `"%.d/%s"` = `value/max` |
| `ReadOnly` | bool | Prevent interaction |
| `CornerRadius` | UDim | Rounding |
| `Callback` | function(self, value) | Fired on change |

**Methods:**
```lua
Slider:SetValue(50)
Slider:GetValue()  -- returns number
```

> Any Slider with a `Label` is automatically tracked by the config system.

---

### ProgressSlider

Slider that fills left-to-right. Same options as Slider.

```lua
Tab:ProgressSlider({
    Label    = "Volume",
    Value    = 8,
    MinValue = 0,
    MaxValue = 10,
})
```

---

### ProgressBar

Read-only fill bar. Value is a percentage 0–100.

```lua
local Bar = Tab:ProgressBar({
    Label = "Loading...",
})
Bar:SetPercentage(75)
```

---

### Combo (Dropdown)

```lua
local Combo = Tab:Combo({
    Label       = "Mode",
    Selected    = "Auto",
    Placeholder = "Select...",
    Items       = { "Auto", "Manual", "Off" },
    Callback    = function(self, Value)
        print(Value)
    end,
})
```

Dictionary items — the key is displayed, the value is passed to the callback:

```lua
Tab:Combo({
    Label = "Fruit",
    Items = {
        ["Apple"]  = "good",
        ["Banana"] = "bad",
    },
    Callback = function(self, Value)
        print(Value)  -- "good" or "bad"
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Label` | string | Display label |
| `Selected` | string | Default selected item |
| `Placeholder` | string | Text when nothing selected |
| `Items` | table | Array or dictionary of options |
| `Callback` | function(self, value) | Fired on selection |

**Methods:**
```lua
Combo:SetValue("Manual")
Combo:SetOpen(true/false)
```

---

### Keybind

```lua
Tab:Keybind({
    Label              = "Toggle UI",
    Value              = Enum.KeyCode.RightShift,
    NullKey            = Enum.KeyCode.Backspace,  -- clears the bind
    IgnoreGameProcessed = false,
    Callback           = function(self, KeyCode)
        print("fired", KeyCode)
    end,
})
```

| Option | Type | Description |
|---|---|---|
| `Label` | string | Display label |
| `Value` | KeyCode | Initial keybind |
| `NullKey` | KeyCode | Key that clears the bind. Default: Backspace |
| `IgnoreGameProcessed` | bool | Fire even when game handled the input |
| `Callback` | function(self, keycode) | Fired when bound key is pressed |

Click the keybind element in-game to rebind it.

---

### Separator

```lua
Tab:Separator()               -- line only
Tab:Separator({ Text = "Section" })  -- line with label
```

---

### Row

Horizontal container. Put elements inside it to lay them out side by side.

```lua
local Row = Tab:Row({ Spacing = 8 })
Row:Button({ Text = "A" })
Row:Button({ Text = "B" })
Row:Fill()  -- distribute children evenly across full width
```

| Option | Type | Description |
|---|---|---|
| `Spacing` | number | Pixel gap between children |

---

### CollapsingHeader

Expandable section with a clickable title bar.

```lua
local Section = Tab:CollapsingHeader({
    Title = "Advanced",
    Open  = false,
})
Section:Button({ Text = "Inside" })
```

| Option | Type | Description |
|---|---|---|
| `Title` | string | Header label |
| `Open` | bool | Start expanded |
| `NoAnimation` | bool | Skip open/close animation |
| `Image` | string | Override toggle arrow image |

**Methods:**
```lua
Section:SetOpen(true/false)
```

---

### TreeNode

Same as CollapsingHeader with tab-style animation.

```lua
local Node = Tab:TreeNode({ Title = "Node" })
Node:Label({ Text = "Child" })
```

---

### ScrollingBox

A scrollable container. Add elements inside it like a tab.

```lua
local Box = Tab:ScrollingBox({ Size = UDim2.new(1, 0, 0, 150) })
Box:Label({ Text = "Item 1" })
Box:Label({ Text = "Item 2" })
```

---

### Table

Grid layout with rows and columns.

```lua
local Tbl = Tab:Table({
    RowBackground = true,
    Border        = true,
    RowsFill      = false,
})

local Row = Tbl:CreateRow()
local Col = Row:CreateColumn()
Col:Label({ Text = "Cell" })
```

| Option | Type | Description |
|---|---|---|
| `RowBackground` | bool | Alternating row background |
| `Border` | bool | Cell borders |
| `RowsFill` | bool | Rows stretch to fill table height |
| `Fill` | bool | Table fills remaining container height |
| `Align` | string | Column vertical align: `"Center"`, `"Top"`, `"Bottom"` |

**Methods:**
```lua
Tbl:CreateRow()        -- returns a row
Row:CreateColumn()     -- returns a column container (supports all elements)
Tbl:ClearRows()        -- remove all rows
```

---

### Console

Read/write log area.

```lua
local Con = Tab:Console({
    ReadOnly    = true,
    LineNumbers = false,
    Fill        = true,
    Enabled     = true,
    AutoScroll  = true,
    RichText    = true,
    MaxLines    = 100,
})
```

| Option | Type | Description |
|---|---|---|
| `ReadOnly` | bool | Prevent typing |
| `LineNumbers` | bool | Show line numbers |
| `Fill` | bool | Fill remaining height |
| `AutoScroll` | bool | Scroll to bottom on new text |
| `RichText` | bool | Enable RichText tags |
| `MaxLines` | number | Trim oldest lines when exceeded |

**Methods:**
```lua
Con:AppendText("line")
Con:SetText("full replace")
Con:GetValue()     -- returns full text string
Con:Clear()
Con:UpdateScroll() -- jump to bottom
```

---

### Viewport

Renders a 3D model inside the UI.

```lua
local VP = Tab:Viewport({
    Size   = UDim2.new(1, 0, 0, 200),
    Clone  = true,
    Border = false,
})
local Model = VP:SetModel(workspace.MyModel, CFrame.new(0, -2, -5))
```

| Option | Type | Description |
|---|---|---|
| `Size` | UDim2 | Frame size |
| `Clone` | bool | Clone the model instead of parenting it |
| `Border` | bool | Show border |

**Methods:**
```lua
VP:SetModel(model, pivotCFrame)
VP:SetCamera(camera)
```

---

### Image

```lua
Tab:Image({
    Image    = "rbxassetid://12345678",  -- or just the number
    Size     = UDim2.new(1, 0, 0, 80),
    Callback = function(self) end,
})
```

---

## Modal

A blocking popup window. Other windows become non-interactive while it's open.

```lua
local Modal = ImGui:CreateModal({
    Title    = "Confirm",
    AutoSize = "Y",
})
Modal:Label({ Text = "Are you sure?", TextWrapped = true })
Modal:Button({
    Text     = "Yes",
    Callback = function() Modal:Close() end,
})
```

Modal exposes the same element methods as a tab. Close with `Modal:Close()`.

---

## Config System

A Config tab is automatically injected as the last tab in every window that has a tab bar. Nothing to set up.

**What gets tracked automatically:**

Any element with a `Label` field of these types is saved and loaded without any extra code:
- Checkbox / RadioButton
- Slider / ProgressSlider
- InputText

**How to use:**

1. Go to the Config tab in your window
2. Type a profile name in the Save As field
3. Press **Save** — current state of all tracked elements is written to `imgui_configs/<name>.json`
4. To restore: pick a profile from the dropdown, press **Load**

The dropdown lists all existing saves in `imgui_configs/`. File extension is handled internally.

**Note:** Elements without a `Label` are not tracked. If two elements share the same label string, the last one created wins.
