# Resource Migration Guide

This document maps old PleasantUI resource keys — WinUI-style brush names, removed control themes, and renamed tokens — to their current equivalents. Use it when upgrading a project that references resources that no longer exist.

---

## How to diagnose missing resources

A missing `DynamicResource` key does not throw at compile time; it silently produces a transparent/default value at runtime. If a control looks invisible or unstyled, check whether it references a removed key.

---

## Color / brush tokens

These WinUI-inspired names were removed. PleasantUI uses a simpler, flatter naming scheme.

### Background brushes

| Old key | Current equivalent | Notes |
|---|---|---|
| `SolidBackgroundFillColorBaseBrush` | `BackgroundColor1` | Main window/page background |
| `SolidBackgroundFillColorSecondaryBrush` | `BackgroundColor2` | Panel / card background |
| `SolidBackgroundFillColorTertiaryBrush` | `BackgroundColor3` | Nested panel background |
| `SolidBackgroundFillColorQuarternaryBrush` | `BackgroundColor4` | Deepest nested background |
| `LayerFillColorDefaultBrush` | `BackgroundColor2` | Closest semantic match |
| `LayerFillColorAltBrush` | `BackgroundColor3` | — |
| `CardBackgroundFillColorDefaultBrush` | `BackgroundColor2` | Cards sit on `BackgroundColor2` |
| `CardBackgroundFillColorSecondaryBrush` | `BackgroundColor3` | — |
| `MicaBackgroundBrush` | `BackgroundColor1` | No mica layer in PleasantUI |
| `AcrylicBackgroundBrush` | `BackgroundColor1` | Blur is handled by `PleasantWindow.EnableBlur` |

### Control fill brushes

| Old key | Current equivalent | Notes |
|---|---|---|
| `ControlFillColorDefaultBrush` | `ControlFillColor1` | Default control background |
| `ControlFillColorSecondaryBrush` | `ControlFillColor2` | Hovered control background |
| `ControlFillColorTertiaryBrush` | `ControlFillColor3` | Pressed / ripple fill |
| `ControlFillColorDisabledBrush` | `ControlFillColor1` + `Opacity="0.5"` | Disabled state uses opacity |
| `SubtleFillColorTransparentBrush` | `ControlFillColor1` | — |
| `SubtleFillColorSecondaryBrush` | `ControlFillColor2` | — |
| `SubtleFillColorTertiaryBrush` | `ControlFillColor3` | — |
| `ControlOnImageFillColorDefaultBrush` | `ControlDarkFillColor1` | — |
| `ControlOnImageFillColorSecondaryBrush` | `ControlDarkFillColor2` | — |

### Border / stroke brushes

| Old key | Current equivalent | Notes |
|---|---|---|
| `ControlStrokeColorDefaultBrush` | `ControlBorderColor` | Standard control border |
| `ControlStrokeColorSecondaryBrush` | `ControlBorderColor` | Same token, no secondary variant |
| `ControlStrokeColorOnAccentDefaultBrush` | `ControlBorderColor` | — |
| `CardStrokeColorDefaultBrush` | `ControlBorderColor` | — |
| `DividerStrokeColorDefaultBrush` | `ControlBorderColor` | — |

### Text brushes

| Old key | Current equivalent | Notes |
|---|---|---|
| `TextFillColorPrimaryBrush` | `TextFillColor1` | Primary text |
| `TextFillColorSecondaryBrush` | `TextFillColor2` | Secondary / hint text |
| `TextFillColorTertiaryBrush` | `TextFillColor3` | Disabled / placeholder text |
| `TextFillColorDisabledBrush` | `TextFillColor3` | — |
| `TextOnAccentFillColorPrimaryBrush` | Use `{ForegroundBasedAccent AccentColor}` markup extension | Computes readable foreground over accent |

### Accent fill brushes

These keys are used internally by PleasantUI templates but are **not registered as named public resources**. Do not reference them from your own XAML — use the accent color tokens directly.

| Internal key (do not use) | Use instead |
|---|---|
| `AccentFillColor1` | `AccentColor` |
| `AccentFillColorDefaultBrush` | `AccentColor` |
| `AccentFillColorSecondaryBrush` | `AccentLightColor1` |
| `AccentFillColorTertiaryBrush` | `AccentLightColor2` |

```xml
<!-- Old (broken) -->
<Border Background="{DynamicResource AccentFillColorDefaultBrush}" />

<!-- New -->
<Border Background="{DynamicResource AccentColor}" />
```

### Focus / system brushes

| Old key | Current equivalent |
|---|---|
| `FocusStrokeColorOuterBrush` | `FocusStrokeColorOuter` |
| `FocusStrokeColorInnerBrush` | `FocusStrokeColorInner` |
| `SystemFillColorCriticalBrush` | `SystemFillColorCritical` |
| `SystemFillColorSuccessBrush` | `SystemFillColorSuccess` |
| `SystemFillColorCautionBrush` | `SystemFillColorCaution` |
| `SystemFillColorNeutralBrush` | `SystemFillColorNeutral` |

> PleasantUI exposes raw `Color` resources, not pre-wrapped `SolidColorBrush` resources. Avalonia automatically converts a `Color` to a brush when you bind it to a `Background`, `Foreground`, or `BorderBrush` property.

---

## Control themes

### `RadioButtonLikeButton`

Removed. This was a `Button` styled to look like a radio button. Replace with a standard `RadioButton` or a `ToggleButton` with `AppBarToggleButtonTheme`:

```xml
<!-- Old (broken) -->
<Button Theme="{DynamicResource RadioButtonLikeButton}" Content="Option A" />

<!-- New — use a RadioButton -->
<RadioButton Content="Option A" GroupName="options" />

<!-- Or a ToggleButton if you need button semantics -->
<ToggleButton Theme="{DynamicResource AppBarToggleButtonTheme}" Content="Option A" />
```

### `AppBarToggleButtonTheme`

Still exists — no change needed. It is defined in `ToggleButton.axaml` and targets `ToggleButton`. Transparent background by default, rounded corners, fills on hover/checked.

```xml
<ToggleButton Theme="{DynamicResource AppBarToggleButtonTheme}" Content="Pin" />
```

### `NavigationViewItemTheme` (standalone)

Removed as a standalone named key. The default `NavigationViewItem` theme is applied via `{x:Type NavigationViewItem}`. For the top/bottom bar variant use `TopBottomNavigationViewItemTheme`:

```xml
<!-- Old (broken) -->
<NavigationViewItem Theme="{DynamicResource NavigationViewItemTheme}" />

<!-- New — default theme applies automatically, no Theme= needed -->
<NavigationViewItem Header="Home" />

<!-- Top/bottom bar variant -->
<NavigationViewItem Theme="{DynamicResource TopBottomNavigationViewItemTheme}" />
```

---

## Data templates

### `FontItemTemplate`

Removed. There is no built-in font picker data template. Implement your own:

```xml
<DataTemplate x:Key="FontItemTemplate" DataType="{x:Type FontFamily}">
    <TextBlock Text="{Binding Name}" FontFamily="{Binding}" />
</DataTemplate>
```

---

## Layout / sizing tokens

| Old key | Current equivalent | Value |
|---|---|---|
| `ContentDialogMaxWidth` | Not a resource — set `MaxWidth` directly | — |
| `FlyoutBorderThemeThickness` | `ControlBorderThickness` | `0` |
| `FlyoutBorderRadius` | `PopupCornerRadius` | `12` |
| `OverlayCornerRadius` | `PopupCornerRadius` | `12` |
| `ControlCornerRadiusDefault` | `ControlCornerRadius` | `8` |
| `CircleCornerRadius` | `RoundedControlCornerRadius` | `999` |

---

## Quick reference table

| Old key | New key | Type |
|---|---|---|
| `SolidBackgroundFillColorBaseBrush` | `BackgroundColor1` | `Color` |
| `SolidBackgroundFillColorSecondaryBrush` | `BackgroundColor2` | `Color` |
| `CardBackgroundFillColorDefaultBrush` | `BackgroundColor2` | `Color` |
| `ControlFillColorDefaultBrush` | `ControlFillColor1` | `Color` |
| `ControlFillColorSecondaryBrush` | `ControlFillColor2` | `Color` |
| `ControlFillColorTertiaryBrush` | `ControlFillColor3` | `Color` |
| `ControlStrokeColorDefaultBrush` | `ControlBorderColor` | `Color` |
| `TextFillColorPrimaryBrush` | `TextFillColor1` | `Color` |
| `TextFillColorSecondaryBrush` | `TextFillColor2` | `Color` |
| `TextFillColorTertiaryBrush` | `TextFillColor3` | `Color` |
| `AccentFillColorDefaultBrush` | `AccentColor` | `Color` |
| `AccentFillColorSecondaryBrush` | `AccentLightColor1` | `Color` |
| `FocusStrokeColorOuterBrush` | `FocusStrokeColorOuter` | `Color` |
| `FocusStrokeColorInnerBrush` | `FocusStrokeColorInner` | `Color` |
| `SystemFillColorCriticalBrush` | `SystemFillColorCritical` | `Color` |
| `SystemFillColorSuccessBrush` | `SystemFillColorSuccess` | `Color` |
| `SystemFillColorCautionBrush` | `SystemFillColorCaution` | `Color` |
| `SystemFillColorNeutralBrush` | `SystemFillColorNeutral` | `Color` |
| `RadioButtonLikeButton` | `AppBarToggleButtonTheme` or `RadioButton` | `ControlTheme` |
| `NavigationViewItemTheme` | *(implicit, no key needed)* | `ControlTheme` |
| `FontItemTemplate` | *(implement manually)* | `DataTemplate` |
| `ControlCornerRadiusDefault` | `ControlCornerRadius` | `CornerRadius` |
| `CircleCornerRadius` | `RoundedControlCornerRadius` | `CornerRadius` |
| `FlyoutBorderRadius` | `PopupCornerRadius` | `CornerRadius` |
