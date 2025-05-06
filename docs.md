# LinoriaLib Fork Documentation

LinoriaLib is a powerful UI library for creating customizable menus in Roblox. This documentation covers all features and functionality of the library based on the example script.
DOCS BY MYE, CUSTOM LINORIA FORK WITH BETTER NOTIFS ALSO BY MYE!
## Table of Contents
- [Getting Started](#getting-started)
- [Windows](#windows)
- [Tabs & Groupboxes](#tabs--groupboxes)
  - [Tabs](#tabs)
  - [Groupboxes](#groupboxes)
  - [Tabboxes](#tabboxes)
- [UI Elements](#ui-elements)
  - [Toggles](#toggles)
  - [Buttons](#buttons)
  - [Labels](#labels)
  - [Dividers](#dividers)
  - [Sliders](#sliders)
  - [Textboxes](#textboxes)
  - [Dropdowns](#dropdowns)
  - [ColorPickers](#colorpickers)
  - [KeyPickers](#keypickers)
- [Dependency Boxes](#dependency-boxes)
- [Library Features](#library-features)
  - [Watermark](#watermark)
  - [Menu Keybind](#menu-keybind)
  - [Unloading](#unloading)
- [Add-ons](#add-ons)
  - [ThemeManager](#thememanager)
  - [SaveManager](#savemanager)

## Getting Started

To begin using LinoriaLib, you need to load the required scripts:

```lua
local repo = 'https://raw.githubusercontent.com/myethg/ui/refs/heads/main/'

local Library: any = loadstring(game:HttpGet(repo..'library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'theme.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'savemanager.lua'))()
```

## Windows

A window is the main container for your UI. Create a window as follows:

```lua
local Window = Library:CreateWindow({
    Title = 'Example menu',
    Center = true,        -- Center the window on screen
    AutoShow = true,      -- Show the window when created
    TabPadding = 8,       -- Padding between tabs
    MenuFadeTime = 0.2    -- Fade animation time
    -- Position and Size are also valid options (optional)
})
```

## Tabs & Groupboxes

### Tabs

Tabs allow you to organize your UI into separate sections:

```lua
local Tabs = {
    Main = Window:AddTab('Main'),
    ['UI Settings'] = Window:AddTab('UI Settings'),
}
```

### Groupboxes

Groupboxes are containers for UI elements within a tab. You can add them to the left or right side:

```lua
local LeftGroupBox = Tabs.Main:AddLeftGroupbox('Groupbox')
local RightGroupbox = Tabs.Main:AddRightGroupbox('Groupbox #3')
```

### Tabboxes

Tabboxes are collections of tabs within a tab, allowing for more complex organization:

```lua
local TabBox = Tabs.Main:AddRightTabbox()

local Tab1 = TabBox:AddTab('Tab 1')
local Tab2 = TabBox:AddTab('Tab 2')

-- You can add elements to these tabs just like with groupboxes
Tab1:AddToggle('Tab1Toggle', { Text = 'Tab1 Toggle' })
```

## UI Elements

### Toggles

Toggles are boolean switches that can be turned on or off:

```lua
local MyToggle = LeftGroupBox:AddToggle('MyToggle', {
    Text = 'This is a toggle',
    Default = true,       -- Default value (true / false)
    Tooltip = 'This is a tooltip', -- Information shown on hover
    
    Callback = function(Value)
        print('Toggle changed to:', Value)
    end
})
```

#### Toggle Methods

```lua
-- Get the current value
local state = Toggles.MyToggle.Value

-- Set the value programmatically
Toggles.MyToggle:SetValue(false)

-- React to value changes
Toggles.MyToggle:OnChanged(function()
    print('Toggle changed to:', Toggles.MyToggle.Value)
end)
```

### Buttons

Buttons execute code when clicked:

```lua
local MyButton = LeftGroupBox:AddButton({
    Text = 'Button',
    Func = function()
        print('You clicked a button!')
    end,
    DoubleClick = false,  -- If true, requires double-click to activate
    Tooltip = 'This is the main button'
})
```

#### SubButtons

You can add sub-buttons to an existing button:

```lua
local MyButton2 = MyButton:AddButton({
    Text = 'Sub button',
    Func = function()
        print('You clicked a sub button!')
    end,
    DoubleClick = true,
    Tooltip = 'This is the sub button (double click me!)'
})
```

You can also chain button creation:

```lua
LeftGroupBox:AddButton({ Text = 'Button One', Func = Function1 })
    :AddButton({ Text = 'Button Two', Func = Function2 })
```

### Labels

Labels display text information:

```lua 
-- Simple label
LeftGroupBox:AddLabel('This is a label')

-- Label with text wrapping
LeftGroupBox:AddLabel('This is a label\n\nwhich wraps its text!', true)
```

### Dividers

Dividers add a horizontal line to separate UI elements:

```lua
LeftGroupBox:AddDivider()
```

### Sliders

Sliders allow selecting a numeric value within a range:

```lua
LeftGroupBox:AddSlider('MySlider', {
    Text = 'This is my slider!',
    Default = 0,
    Min = 0,
    Max = 5,
    Rounding = 1,        -- Number of decimal places
    Compact = false,     -- If true, hides the title label
    HideMax = false,     -- If true, only shows value instead of value & max

    Callback = function(Value)
        print('Slider changed to:', Value)
    end
})
```

#### Slider Methods

```lua
-- Get the current value
local value = Options.MySlider.Value

-- Set the value programmatically
Options.MySlider:SetValue(3)

-- React to value changes
Options.MySlider:OnChanged(function()
    print('Slider changed to:', Options.MySlider.Value)
end)
```

### Textboxes

Textboxes allow text input:

```lua
LeftGroupBox:AddInput('MyTextbox', {
    Default = 'My textbox!',
    Numeric = false,      -- If true, only allows numbers
    Finished = false,     -- If true, only calls callback on Enter
    Text = 'This is a textbox',
    Tooltip = 'This is a tooltip',
    Placeholder = 'Placeholder text',
    -- MaxLength = 50,    -- Optional maximum text length
    
    Callback = function(Value)
        print('Text updated:', Value)
    end
})
```

#### Textbox Methods

```lua
-- Get the current value
local text = Options.MyTextbox.Value

-- React to value changes
Options.MyTextbox:OnChanged(function()
    print('Text updated:', Options.MyTextbox.Value)
end)
```

### Dropdowns

Dropdowns allow selecting from a list of options:

```lua
-- Single-selection dropdown
LeftGroupBox:AddDropdown('MyDropdown', {
    Values = { 'This', 'is', 'a', 'dropdown' },
    Default = 1,          -- Index of the value / string
    Multi = false,        -- Allow multiple selections
    Text = 'A dropdown',
    Tooltip = 'This is a tooltip',
    
    Callback = function(Value)
        print('Dropdown changed to:', Value)
    end
})
```

#### Multi-selection Dropdown

```lua
LeftGroupBox:AddDropdown('MyMultiDropdown', {
    Values = { 'This', 'is', 'a', 'dropdown' },
    Default = 1,
    Multi = true,         -- Allow multiple selections
    Text = 'A dropdown',
    Tooltip = 'This is a tooltip',
    
    Callback = function(Value)
        print('Multi dropdown changed')
    end
})
```

#### Special Player Dropdown

```lua
LeftGroupBox:AddDropdown('MyPlayerDropdown', {
    SpecialType = 'Player',
    Text = 'A player dropdown',
    Tooltip = 'This is a tooltip',
    
    Callback = function(Value)
        print('Player dropdown changed:', Value)
    end
})
```

#### Dropdown Methods

```lua
-- Get the current value
local value = Options.MyDropdown.Value

-- Set the value programmatically
Options.MyDropdown:SetValue('This')

-- For multi-dropdowns, set multiple values
Options.MyMultiDropdown:SetValue({
    This = true,
    is = true,
})

-- React to value changes
Options.MyDropdown:OnChanged(function()
    print('Dropdown changed to:', Options.MyDropdown.Value)
end)

-- For multi-dropdowns, iterate through values
Options.MyMultiDropdown:OnChanged(function()
    for key, value in next, Options.MyMultiDropdown.Value do
        print(key, value) -- Prints something like "This, true"
    end
end)
```

### ColorPickers

ColorPickers allow selecting a color:

```lua
LeftGroupBox:AddLabel('Color'):AddColorPicker('ColorPicker', {
    Default = Color3.new(0, 1, 0),      -- Default color
    Title = 'Some color',               -- Optional title
    Transparency = 0,                   -- 0-1 transparency value (nil to disable)
    
    Callback = function(Value)
        print('Color changed!', Value)
    end
})
```

#### ColorPicker Methods

```lua
-- Get the current value
local color = Options.ColorPicker.Value
local transparency = Options.ColorPicker.Transparency

-- Set the value programmatically
Options.ColorPicker:SetValueRGB(Color3.fromRGB(0, 255, 140))

-- React to value changes
Options.ColorPicker:OnChanged(function()
    print('Color changed!', Options.ColorPicker.Value)
    print('Transparency changed!', Options.ColorPicker.Transparency)
end)
```

### KeyPickers

KeyPickers allow binding keyboard and mouse input:

```lua
LeftGroupBox:AddLabel('Keybind'):AddKeyPicker('KeyPicker', {
    Default = 'MB2',            -- Default keybind (MB1, MB2 for mouse buttons)
    SyncToggleState = false,    -- Sync state with a toggle (for toggleable keybinds)
    Mode = 'Toggle',            -- Modes: Always, Toggle, Hold
    Text = 'Auto lockpick safes', -- Display text
    NoUI = false,               -- Hide from the keybind menu
    
    -- Triggered when keybind is activated/deactivated
    Callback = function(Value)
        print('Keybind clicked!', Value)
    end,
    
    -- Triggered when the keybind itself is changed
    ChangedCallback = function(New)
        print('Keybind changed!', New)
    end
})
```

#### KeyPicker Methods

```lua
-- Get the current state (pressed or not)
local state = Options.KeyPicker:GetState()

-- Set the keybind and mode programmatically
Options.KeyPicker:SetValue({ 'MB2', 'Toggle' })

-- React to clicks (only for Toggle mode)
Options.KeyPicker:OnClick(function()
    print('Keybind clicked!', Options.KeyPicker:GetState())
end)

-- React to keybind changes
Options.KeyPicker:OnChanged(function()
    print('Keybind changed!', Options.KeyPicker.Value)
end)
```

## Dependency Boxes

Dependency boxes allow showing or hiding UI elements based on the state of other elements:

```lua
-- Create a toggle that will control visibility
RightGroupbox:AddToggle('ControlToggle', { Text = 'Dependency box toggle' })

-- Create a dependency box
local Depbox = RightGroupbox:AddDependencyBox()

-- Add elements to the dependency box
Depbox:AddToggle('DepboxToggle', { Text = 'Sub-dependency box toggle' })

-- Set up dependencies (elements will only show when ControlToggle is true)
Depbox:SetupDependencies({
    { Toggles.ControlToggle, true } -- Pass false if you want elements to show when toggle is off
})
```

### Nested Dependency Boxes

You can nest dependency boxes for more complex visibility conditions:

```lua
-- Create a nested dependency box
local SubDepbox = Depbox:AddDependencyBox()

-- Add elements to the nested box
SubDepbox:AddSlider('DepboxSlider', { Text = 'Slider', Default = 50, Min = 0, Max = 100 })
SubDepbox:AddDropdown('DepboxDropdown', { Text = 'Dropdown', Default = 1, Values = {'a', 'b', 'c'} })

-- Set up additional dependencies (elements will only show when DepboxToggle is true)
SubDepbox:SetupDependencies({
    { Toggles.DepboxToggle, true }
})
```

## Library Features

### Watermark

Control the watermark in the top-right corner:

```lua
-- Set watermark visibility
Library:SetWatermarkVisibility(true)

-- Set watermark text
Library:SetWatermark('LinoriaLib demo')

-- Dynamic watermark example
Library:SetWatermark(('LinoriaLib demo | %s fps | %s ms'):format(
    math.floor(FPS),
    math.floor(game:GetService('Stats').Network.ServerStatsItem['Data Ping']:GetValue())
))
```

### Menu Keybind

Set a keybind to toggle the menu:

```lua
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { 
    Default = 'End', 
    NoUI = true, 
    Text = 'Menu keybind' 
})

-- Assign the keybind to the library
Library.ToggleKeybind = Options.MenuKeybind
```

### Unloading

Properly clean up when the script is no longer needed:

```lua
-- Add an unload button
MenuGroup:AddButton('Unload', function() Library:Unload() end)

-- Handle cleanup when unloaded
Library:OnUnload(function()
    -- Disconnect connections
    WatermarkConnection:Disconnect()
    
    print('Unloaded!')
    Library.Unloaded = true
})
```

## Add-ons

### ThemeManager

ThemeManager allows users to switch between different UI themes:

```lua
-- Initialize ThemeManager
ThemeManager:SetLibrary(Library)
ThemeManager:SetFolder('MyScriptHub')

-- Add theme options to a tab
ThemeManager:ApplyToTab(Tabs['UI Settings'])

-- Or add to a specific groupbox
-- ThemeManager:ApplyToGroupbox(MyGroupbox)
```

### SaveManager

SaveManager allows saving and loading UI configurations:

```lua
-- Initialize SaveManager
SaveManager:SetLibrary(Library)
SaveManager:IgnoreThemeSettings() -- Don't save theme settings in configs
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' }) -- Don't save menu keybind
SaveManager:SetFolder('MyScriptHub/specific-game')

-- Add config section to a tab
SaveManager:BuildConfigSection(Tabs['UI Settings'])

-- Load a config marked as auto-load
SaveManager:LoadAutoloadConfig()
```

## Best Practices

1. **Decoupling UI and Logic**:
   ```lua
   -- RECOMMENDED: Create UI elements first
   local MyToggle = Tab:AddToggle('MyToggle', { Text = 'My Toggle' })
   
   -- Then set up callbacks separately
   MyToggle:OnChanged(function()
      -- Your logic here
   end)
   ```

2. **Monitoring State Changes**:
   ```lua
   -- Check state in a loop
   task.spawn(function()
       while true do
           wait(1)
           
           local state = Options.KeyPicker:GetState()
           if state then
               print('KeyPicker is being held down')
           end
           
           -- Always check for unload
           if Library.Unloaded then break end
       end
   end)
   ```

3. **Using Descriptive Names**:
   - Use clear, descriptive names for UI elements.
   - Group related elements together in the same groupbox.

4. **Tooltips**:
   - Use tooltips to provide additional information for elements.
   - This helps users understand what each element does.

5. **Organize with Tabs and Groupboxes**:
   - Use tabs to separate major sections.
   - Use groupboxes to organize related elements.
   - Use tabboxes for advanced organization.
