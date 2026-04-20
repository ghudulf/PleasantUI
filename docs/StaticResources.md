# Static Resources

All resources listed here are registered by `PleasantTheme` and available anywhere in your application via `{DynamicResource}` or `{StaticResource}`. Theme-sensitive resources (colors, brushes) are defined per theme variant and resolve automatically when the active theme changes.

---

## How to use

```xml
<!-- Color -->
<Border Background="{DynamicResource BackgroundColor1}" />

<!-- Brush -->
<Border Background="{DynamicResource AccentLinearGradientBrush}" />

<!-- ControlTheme -->
<Button Theme="{DynamicResource AccentButtonTheme}" Content="Save" />

<!-- Icon geometry -->
<PathIcon Data="{DynamicResource SearchRegular}" Width="16" Height="16" />
```

Use `DynamicResource` for anything that should react to theme changes at runtime. `StaticResource` is fine for geometry keys and layout tokens that never change.

---

## Theme-sensitive color tokens

These `Color` resources are defined per theme variant (Light, Dark, and all named themes). They update automatically when the user switches themes.

### Text

| Key | Light | Dark | Purpose |
|---|---|---|---|
| `TextFillColor1` | `#E4000000` | `#FFFFFF` | Primary text |
| `TextFillColor2` | `#9E000000` | `#C5FFFFFF` | Secondary / hint text |
| `TextFillColor3` | `#72000000` | `#87FFFFFF` | Disabled / placeholder text |

### Control fill

| Key | Light | Dark | Purpose |
|---|---|---|---|
| `ControlFillColor1` | `#0B000000` | `#0FFFFFFF` | Default control background |
| `ControlFillColor2` | `#13000000` | `#19FFFFFF` | Hovered control background |
| `ControlFillColor3` | `#18000000` | `#23FFFFFF` | Pressed / ripple fill |
| `ControlDarkFillColor1` | `#78000000` | `#4BFFFFFF` | Strong fill (selected items, badges) |
| `ControlDarkFillColor2` | `#82000000` | `#55FFFFFF` | Stronger fill |
| `ControlDarkFillColor3` | `#96000000` | `#64FFFFFF` | Strongest fill |
| `ControlBorderColor` | `#14000000` | `#59595959` | Default control border |

### Background

| Key | Light | Dark | Purpose |
|---|---|---|---|
| `BackgroundColor1` | `#FFF0F0F0` | `#FF191919` | Window / page background |
| `BackgroundColor2` | `#FFE6E6E6` | `#FF252525` | Panel / card background |
| `BackgroundColor3` | `#FFDEDEDE` | `#FF313131` | Nested panel background |
| `BackgroundColor4` | `#FFD9D9D9` | `#FF3F3F3F` | Deepest nested background |

### System status fills

| Key | Light | Dark | Purpose |
|---|---|---|---|
| `SystemFillColorCritical` | `#C42B1C` | `#C42B1C` | Error / critical |
| `SystemFillColorSuccess` | `#0F7B0F` | `#0F7B0F` | Success |
| `SystemFillColorCaution` | `#BF8500` | `#FFCE21` | Warning / caution |
| `SystemFillColorNeutral` | `#4B6ACA` | `#4B6ACA` | Informational / neutral |

### Danger

| Key | Purpose |
|---|---|
| `DangerColor` | Danger button / header background |
| `BorderDangerColor` | Danger button border |
| `SelectedDangerColor` | Danger button hover/pressed state |

### Focus ring

| Key | Light | Dark | Purpose |
|---|---|---|---|
| `FocusStrokeColorOuter` | `#E4000000` | `#FFFFFF` | Outer focus ring stroke |
| `FocusStrokeColorInner` | `#B3FFFFFF` | `#B3000000` | Inner focus ring stroke |

---

## Accent color tokens

Accent colors are computed from `PleasantSettings.Current.NumericalAccentColor` and updated dynamically. The VGUI theme overrides them with fixed green values.

| Key | Description |
|---|---|
| `AccentColor` | Base accent color |
| `AccentLightColor1` | Accent lightened +20% |
| `AccentLightColor2` | Accent lightened +35% |
| `AccentLightColor3` | Accent lightened +50% |
| `AccentDarkColor1` | Accent darkened −20% |
| `AccentDarkColor2` | Accent darkened −5% |
| `AccentDarkColor3` | Accent darkened +10% |
| `AccentGradientColor1` | Hue-shifted accent (+20°) for gradient start |
| `AccentGradientColor2` | Hue-shifted accent (−20°) for gradient end |

---

## Layout tokens

Theme-independent. Defined in `UnifiedResources.axaml`.

| Key | Type | Value | Purpose |
|---|---|---|---|
| `ControlCornerRadius` | `CornerRadius` | `8` | Standard control corner radius |
| `RoundedControlCornerRadius` | `CornerRadius` | `999` | Pill-shaped controls (buttons, tags) |
| `PopupCornerRadius` | `CornerRadius` | `12` | Flyouts, popups, tooltips |
| `ControlBorderThickness` | `Thickness` | `0` | Default border thickness |
| `ControlFocusedBorderThickness` | `Thickness` | `1` | Border thickness when focused |
| `GlobalFontSize` | `Double` | `14` | Base font size |

> In VGUI theme, `ControlCornerRadius`, `RoundedControlCornerRadius`, and `PopupCornerRadius` are all overridden to `0`.

---

## Brushes

Theme-independent brushes defined in `BaseColorsPalette.axaml`.

| Key | Type | Description |
|---|---|---|
| `AccentLinearGradientBrush` | `LinearGradientBrush` | Horizontal gradient using `AccentGradientColor1` → `AccentLightColor2` → `AccentGradientColor2`. Used by `AccentButtonTheme`. |
| `CheckeredBackgroundBrush` | `VisualBrush` | 8×8 px checkerboard pattern. Used as background in color pickers to show transparency. |

---

## Docking brushes (theme-sensitive)

Defined in `PleasantDockingStyles.axaml`. Used by the docking/sidebar system.

| Key | Purpose |
|---|---|
| `PleasantDockingDividerBrush` | Divider line between docked panels |
| `PleasantDockingSideBarDividerBrush` | Divider inside the sidebar |
| `PleasantDockingSideBarButtonBackground` | Sidebar button default background |
| `PleasantDockingSideBarButtonForeground` | Sidebar button default foreground |
| `PleasantDockingSideBarButtonBackgroundChecked` | Sidebar button checked background |
| `PleasantDockingSideBarButtonForegroundChecked` | Sidebar button checked foreground |
| `PleasantDockingSideBarButtonBackgroundPointerOver` | Sidebar button hover background |
| `PleasantDockingSideBarButtonForegroundPointerOver` | Sidebar button hover foreground |
| `PleasantDockingSideBarButtonBackgroundPressed` | Sidebar button pressed background |
| `PleasantDockingSideBarButtonForegroundPressed` | Sidebar button pressed foreground |
| `PleasantDockingSideBarButtonBackgroundDisabled` | Sidebar button disabled background |
| `PleasantDockingSideBarButtonForegroundDisabled` | Sidebar button disabled foreground |
| `PleasantDockingGhostBackground` | Drag ghost overlay background |

---

## Button control themes

| Key | Target | Description |
|---|---|---|
| `AccentButtonTheme` | `Button` | Gradient accent background, semibold text. Uses `AccentLinearGradientBrush`. |
| `DangerButtonTheme` | `Button` | Red background using `DangerColor`, semibold text. |
| `AppBarButtonTheme` | `Button` | Transparent background, rounded corners. For toolbar/command bar buttons. |
| `CloseButtonTheme` | `Button` | Transparent by default, turns `DangerColor` on hover. For close buttons in custom chrome. |

```xml
<Button Theme="{DynamicResource AccentButtonTheme}" Content="Save" />
<Button Theme="{DynamicResource DangerButtonTheme}" Content="Delete" />
<Button Theme="{DynamicResource AppBarButtonTheme}">
    <PathIcon Data="{DynamicResource SearchRegular}" Width="14" Height="14" />
</Button>
<Button Theme="{DynamicResource CloseButtonTheme}" Content="✕" />
```

---

## VGUI control themes

Available when the `VGUI` theme is active. These are always registered in `PleasantTheme.Resources` as fallbacks; in VGUI mode the standard named themes (`AccentButtonTheme`, etc.) are automatically remapped to their VGUI equivalents.

| Key | Target | Description |
|---|---|---|
| `VGUIButtonTheme` | `Button` | 3D bevel-style button, square corners |
| `VGUIAccentButtonTheme` | `Button` | VGUI bevel with accent fill |
| `VGUIDangerButtonTheme` | `Button` | VGUI bevel with danger fill |
| `VGUIAppBarButtonTheme` | `Button` | Flat VGUI button, transparent background |
| `VGUICloseButtonTheme` | `Button` | VGUI app bar button, danger hover |
| `VGUICheckBoxTheme` | `CheckBox` | Inset bevel checkbox, square corners |
| `VGUIRadioButtonTheme` | `RadioButton` | Inset circle radio button |
| `VGUITextBoxTheme` | `TextBox` | Inset bevel text input, square corners |
| `VGUIComboBoxTheme` | `ComboBox` | Inset bevel combo box |
| `VGUIComboBoxItemTheme` | `ComboBoxItem` | Square combo item |
| `VGUINavigationViewTheme` | `NavigationView` | Square content area, no rounded pane overlay |
| `VGUINavigationViewItemTheme` | `NavigationViewItem` | Flat left-bar indicator instead of pill |
| `VGUIOptionsDisplayItemTheme` | `OptionsDisplayItem` | Full bevel, square corners |
| `VGUIDatePickerTheme` | `DatePicker` | Inset bevel, square popup |
| `VGUIPleasantDatePickerTheme` | `PleasantDatePicker` | Inset bevel on click area |
| `VGUICalendarDatePickerTheme` | `CalendarDatePicker` | Inset bevel, square popup |
| `VGUIPinCodeItemTheme` | `PinCodeItem` | Inset bevel cell |
| `VGUISelectionListItemTheme` | `SelectionListItem` | Square, no border |
| `VGUISelectionListTheme` | `SelectionList` | Inset bevel border |

---

## Icon geometries

All icons are `Geometry` resources. Use them with `PathIcon` or any `Path`-based control.

```xml
<PathIcon Data="{DynamicResource SearchRegular}" Width="16" Height="16" />
```

### Navigation & UI chrome

| Key | Description |
|---|---|
| `MenuRegular` | Hamburger menu (three lines) |
| `ChevronDownRegular` | Chevron pointing down |
| `ChevronUpRegular` | Chevron pointing up |
| `ChevronLeftRegular` | Chevron pointing left |
| `ChevronRightRegular` | Chevron pointing right |
| `CaretLeftRegular` | Small filled caret left |
| `FillArrowRegular` | Filled upward arrow |
| `ArrowLeftRegular` | Arrow pointing left |
| `ArrowRightRegular` | Arrow pointing right |
| `DismissRegular` | × close / dismiss |
| `SubtractRegular` | Minus / subtract |
| `MaximizeRegular` | Maximize / restore square |
| `FullScreenEnterRegular` | Enter full screen (four-corner arrows) |
| `FullScreenExitRegular` | Exit full screen |
| `PinRegular` | Pin / keep on top |
| `MoreRegular` | Three-dot overflow menu |

### Actions

| Key | Description |
|---|---|
| `AddRegular` | Plus / add |
| `EditRegular` | Pencil / edit |
| `DeleteRegular` | Trash / delete |
| `CopyRegular` | Copy |
| `CutRegular` | Cut |
| `PasteRegular` | Paste |
| `SearchRegular` | Magnifying glass / search |
| `SettingsRegular` | Gear / settings |
| `TuneRegular` | Sliders / tune |
| `CheckmarkRegular` | Checkmark |
| `CheckmarkCircleRegular` | Checkmark in circle |
| `LinkVariant` | Chain link |

### Files & data

| Key | Description |
|---|---|
| `DocumentRegular` | Document / file |
| `FolderRegular` | Folder |
| `FileImportRegular` | Import file |
| `FileExportRegular` | Export file |
| `SquareMultipleRegular` | Multiple squares / copy pages |

### Status & feedback

| Key | Description |
|---|---|
| `WarningRegular` | Triangle warning |
| `AlertOutline` | Alert triangle outline |
| `InfoRegular` | Info circle |
| `ErrorCircleRegular` | Error × in circle |
| `InformationCircleOutline` | Information circle outline |

### Appearance & theming

| Key | Description |
|---|---|
| `BrushRegular` | Paintbrush |
| `BrushVariantRegular` | Brush variant |
| `PaletteRegular` | Color palette |
| `ThemeLightDark` | Light/dark theme toggle |

### Password fields

| Key | Description |
|---|---|
| `PasswordBoxRevealButtonIcon` | Eye — reveal password |
| `PasswordBoxHideButtonIcon` | Eye with slash — hide password |

### Miscellaneous

| Key | Description |
|---|---|
| `CodeBracesRegular` | Code braces `{}` |
| `BranchRegular` | Git branch |
| `PuzzlePieceRegular` | Puzzle piece / plugin |
| `ListRegular` | Bulleted list |
| `ViewDashboardOutline` | Dashboard grid |
| `FastForward` | Fast-forward / skip |
| `Laptop` | Laptop device |
| `HeartOutline` | Heart outline |
| `Heart` | Filled heart |
| `CurrencyUsd` | Dollar sign |
| `Github` | GitHub logo |
| `CloseCircleRegular` | × in circle |

---

## Named themes

PleasantUI ships several built-in named themes beyond Light and Dark. Set `PleasantSettings.Current!.Theme` to the string name to activate one.

| Theme name | Character |
|---|---|
| `"Light"` | Standard light |
| `"Dark"` | Standard dark |
| `"Mint"` | Green tones, light background |
| `"Strawberry"` | Pink/rose tones, light background |
| `"Ice"` | Teal/cyan tones, light background |
| `"Sunny"` | Yellow/warm tones, light background |
| `"Spruce"` | Dark forest green |
| `"Cherry"` | Dark crimson/rose |
| `"Cave"` | Dark teal/slate |
| `"Lunar"` | Dark blue-grey (One Dark style) |
| `"VGUI"` | Retro Source-engine green, square corners |
| `"System"` | Follows OS light/dark preference |
| `"Custom"` | Uses `PleasantTheme.SelectedCustomTheme` |

All named themes define the same set of color token keys (`TextFillColor1–3`, `ControlFillColor1–3`, `BackgroundColor1–4`, etc.) so your UI adapts automatically.

```csharp
PleasantSettings.Current!.Theme = "Lunar";
```
