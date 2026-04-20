# PleasantUI Migration Guide — Common Build Errors

This guide covers the most frequent compile errors that appear when upgrading a project to a newer version of PleasantUI, based on real build output. Each section names the error, explains what changed, and shows the corrected code.

---

## 1. `CS0246` — `MessageBoxButton` type not found

**Symptom**
```
error CS0246: The type or namespace name 'MessageBoxButton' could not be found
```

**Cause**  
`MessageBoxButton` moved into the `PleasantUI.Core.Structures` namespace. A custom wrapper type in your project that shadows the name will cause a type-mismatch at call sites.

**Fix**  
Remove any local `MessageBoxButton` wrapper class and use the one from PleasantUI directly. Add the correct using and use the struct as documented:

```csharp
using PleasantUI.Core.Structures; // or the namespace shown by your IDE

// Correct usage
var buttons = new[]
{
    new MessageBoxButton { Text = "OK",     Result = "OK",     Default = true, IsKeyDown = true },
    new MessageBoxButton { Text = "Cancel", Result = "Cancel" }
};

string result = await MessageBox.Show(window, "Title", "Body", buttons);
```

If you were passing `IReadOnlyList<YourMessageBoxButton>` to `MessageBox.Show`, the compiler will also emit **CS1503** (argument type mismatch). The fix is the same — use `PleasantUI.Core.Structures.MessageBoxButton` everywhere.

---

## 2. `CS0246` / `CS0103` — `TitleBarType` not found

**Symptom**
```
error CS0246: The type or namespace name 'TitleBarType' could not be found
error CS1503: Argument 2: cannot convert from 'PleasantUI.Controls.Chrome.PleasantTitleBar.Type' to 'TitleBarType'
```

**Cause**  
There is no standalone `TitleBarType` enum. The type is a nested enum inside `PleasantTitleBar`:

```csharp
PleasantUI.Controls.Chrome.PleasantTitleBar.Type
```

**Fix**  
Replace every reference to `TitleBarType` with the fully-qualified nested type, or add a using alias:

```csharp
using TitleBarType = PleasantUI.Controls.Chrome.PleasantTitleBar.Type;

// Then use normally
window.TitleBarType = TitleBarType.Compact;
window.TitleBarType = PleasantTitleBar.Type.Classic;
```

Available values: `Classic`, `ClassicExtended`, `Compact`.

---

## 3. `CS0103` — `PleasantUiSettings` / `PleasantThemeMode` not found

**Symptom**
```
error CS0103: The name 'PleasantUiSettings' does not exist in the current context
error CS0103: The name 'PleasantThemeMode' does not exist in the current context
```

**Cause**  
Both names were removed. Settings are now accessed through `PleasantSettings.Current` (a nullable singleton set by `PleasantTheme` at startup). Theme selection is a plain `string` property, not an enum.

**Fix — settings access**
```csharp
// Old (broken)
PleasantUiSettings.Instance.Theme = PleasantThemeMode.Dark;

// New
if (PleasantSettings.Current is not null)
    PleasantSettings.Current.Theme = "Dark"; // "Light", "Dark", "System", "Custom", or a custom theme name
```

**Fix — theme mode comparison**
```csharp
// Old (broken)
if (settings.Theme == PleasantThemeMode.System) { ... }

// New
if (PleasantSettings.Current?.Theme == "System") { ... }
```

Available built-in theme strings: `"System"`, `"Light"`, `"Dark"`, `"Custom"`.

---

## 4. `CS0117` — `PleasantSettings` has no definition for `Instance`

**Symptom**
```
error CS0117: 'PleasantSettings' does not contain a definition for 'Instance'
```

**Cause**  
The singleton accessor was renamed from `Instance` to `Current`.

**Fix**
```csharp
// Old
PleasantSettings.Instance.NumericalAccentColor = ...;

// New
PleasantSettings.Current!.NumericalAccentColor = ...;
```

Always null-check `Current` before use — it is `null` until `PleasantTheme` initialises.

---

## 5. `CS0103` — `RaisePropertyChanged` not found

**Symptom**
```
error CS0103: The name 'RaisePropertyChanged' does not exist in the current context
```

**Cause**  
Your ViewModel does not inherit from `PleasantUI.Core.ViewModelBase`, which is the class that exposes `RaisePropertyChanged` and `SetProperty`.

**Fix**  
Inherit from `ViewModelBase`:

```csharp
using PleasantUI.Core;

public class SettingsPageViewModel : ViewModelBase
{
    private string _theme = "System";

    public string Theme
    {
        get => _theme;
        set
        {
            SetProperty(ref _theme, value);   // sets field + raises PropertyChanged
            RaisePropertyChanged(nameof(SomeDerivedProperty));
        }
    }
}
```

If you already inherit from another base class, copy the pattern from `ViewModelBase` or implement `INotifyPropertyChanged` manually.

---

## 6. `CS1061` — `Theme` has no `SaveToJson` / `LoadFromJson` / `Colors`

**Symptom**
```
error CS1061: 'Theme' does not contain a definition for 'SaveToJson'
error CS0117: 'Theme' does not contain a definition for 'LoadFromJson'
error CS1061: 'Theme' does not contain a definition for 'Colors'
```

**Cause**  
`Theme` is now a lightweight model (`Name` + `ThemeVariant`) with no serialisation or color-dictionary methods. Custom theme persistence and color editing moved to `CustomTheme` and the `PleasantTheme` / `PleasantThemesLoader` APIs.

**Fix — saving a custom theme**
```csharp
// Old (broken)
theme.SaveToJson(path);

// New — PleasantThemesLoader handles persistence automatically on shutdown.
// To force a save manually:
PleasantThemesLoader.Save();
```

**Fix — loading a custom theme**
```csharp
// Old (broken)
var theme = Theme.LoadFromJson(path);

// New — themes are loaded automatically by PleasantTheme at startup.
// To reload manually:
// (re-instantiate PleasantTheme, or call the internal loader — not part of the public API)
```

**Fix — reading/writing colors**
```csharp
// Old (broken)
var colors = theme.Colors;

// New — use CustomTheme
CustomTheme customTheme = PleasantTheme.CustomThemes.First(t => t.Name == "MyTheme");
Dictionary<string, Color> colors = customTheme.Colors;

// Or get the color dictionary for any built-in theme:
Dictionary<string, Color> lightColors = PleasantTheme.GetColorsDictionary(
    PleasantTheme.Themes.First(t => t.Name == "Light"));
```

---

## 7. `CS1061` — `PleasantTheme` has no `UpdateCustomTheme` / `GetTheme` / `DisableUpdateTheme` / `CustomTheme` / `CompareWithDefaultTheme`

**Symptom**
```
error CS1061: 'PleasantTheme' does not contain a definition for 'UpdateCustomTheme'
error CS1061: 'PleasantTheme' does not contain a definition for 'GetTheme'
error CS1061: 'PleasantTheme' does not contain a definition for 'DisableUpdateTheme'
error CS1061: 'PleasantTheme' does not contain a definition for 'CustomTheme'
error CS1061: 'PleasantTheme' does not contain a definition for 'CompareWithDefaultTheme'
```

**Cause**  
The theme management API was redesigned. The current public surface is:

| Old call | New equivalent |
|---|---|
| `pleasantTheme.UpdateCustomTheme(...)` | `pleasantTheme.UpdateCustomThemes()` — rebuilds all custom themes from `PleasantTheme.CustomThemes` |
| `pleasantTheme.EditCustomTheme(current, next)` | `pleasantTheme.EditCustomTheme(current, next)` — still exists, signature unchanged |
| `pleasantTheme.GetTheme(...)` | `PleasantTheme.Themes` array — iterate to find by name |
| `pleasantTheme.DisableUpdateTheme()` | Removed — no direct replacement |
| `pleasantTheme.CustomTheme` | `PleasantTheme.SelectedCustomTheme` (static property) |
| `pleasantTheme.CompareWithDefaultTheme(...)` | `PleasantTheme.GetThemeTemplateDictionary()` returns the Light theme colors for comparison |

**Fix — selecting a custom theme**
```csharp
// Old
pleasantTheme.CustomTheme = myTheme;

// New
PleasantTheme.SelectedCustomTheme = myTheme;
// Also set the settings string so it persists:
PleasantSettings.Current!.Theme = "Custom";
```

**Fix — updating colors after editing**
```csharp
// Old
pleasantTheme.UpdateCustomTheme(theme);

// New — edit via the dedicated method which also re-resolves the theme
pleasantTheme.EditCustomTheme(existingTheme, updatedTheme);

// Or, if you mutated CustomThemes directly, call:
pleasantTheme.UpdateCustomThemes();
```

---

## 8. `CS1061` — `ToolWindow` / `LoadingWindow` / `OpenProjectWindow` / `DragAndDropWindow` has no `Show` or `Close`

**Symptom**
```
error CS1061: 'ToolWindow' does not contain a definition for 'Show'
error CS1061: 'LoadingWindow' does not contain a definition for 'Close'
```

**Cause**  
PleasantUI window types (`PleasantWindow`, `PleasantMiniWindow`, `PleasantModalWindow`) inherit from Avalonia's `Window` / `WindowBase`. The standard `Show()` / `Close()` methods are available, but only after the window is properly constructed and the application lifetime is running. If your custom window type no longer compiles, it likely lost its base class or the base class changed.

**Fix**  
Ensure your window inherits from the correct PleasantUI base:

```csharp
// Regular main/tool window
public class ToolWindow : PleasantWindow { }

// Small floating window
public class LoadingWindow : PleasantMiniWindow { }

// Then show/close as normal Avalonia windows
var win = new ToolWindow();
win.Show(ownerWindow);   // or win.Show() for top-level

win.Close();
```

For modal windows that need an async result, use `PleasantModalWindow` and `ShowDialog`:

```csharp
public class MyDialog : PleasantModalWindow { }

var dialog = new MyDialog();
await dialog.ShowDialog(ownerWindow);
```

---

## 9. `CS1061` — `DragEventArgs` has no `Data`

**Symptom**
```
error CS1061: 'DragEventArgs' does not contain a definition for 'Data'
```

**Cause**  
Avalonia renamed `DragEventArgs.Data` to `DragEventArgs.DataTransfer` in a breaking API change.

**Fix**
```csharp
// Old (broken)
if (e.Data.Contains(DataFormats.Files)) { ... }
var files = e.Data.GetFiles();

// New
if (e.DataTransfer.Contains(DataFormat.File)) { ... }
var files = await ((IAsyncDataTransfer)e.DataTransfer).TryGetFilesAsync();
```

---

## 10. `CS0619` / `CS0117` — `DataFormats` is obsolete / has no `Files`

**Symptom**
```
error CS0619: 'DataFormats' is obsolete: 'Use DataFormat instead'
error CS0117: 'DataFormats' does not contain a definition for 'Files'
```

**Cause**  
Avalonia replaced the static `DataFormats` class with `DataFormat`. The `Files` member was renamed to `File`.

**Fix**
```csharp
// Old (broken)
e.Data.Contains(DataFormats.Files)

// New
e.DataTransfer.Contains(DataFormat.File)

// Getting the files
IEnumerable<IStorageItem>? files =
    await ((IAsyncDataTransfer)e.DataTransfer).TryGetFilesAsync();
```

---

## 11. `CS1061` — `Color` has no `ToUint32`

**Symptom**
```
error CS1061: 'Color' does not contain a definition for 'ToUint32'
```

**Cause**  
The method name uses standard .NET capitalisation: `ToUInt32` (capital `I`), not `ToUint32`.

**Fix**
```csharp
// Old (broken)
uint value = color.ToUint32();

// New
uint value = color.ToUInt32();

// Reverse
Color color = Color.FromUInt32(value);
```

---

## 12. `CS1061` — `Application` has no `Clipboard`

**Symptom**
```
error CS1061: 'Application' does not contain a definition for 'Clipboard'
```

**Cause**  
Avalonia removed `Application.Clipboard`. Clipboard access now goes through `TopLevel`.

**Fix**
```csharp
// Old (broken)
await Application.Current.Clipboard.SetTextAsync(text);

// New — get TopLevel from any visual in scope
var topLevel = TopLevel.GetTopLevel(this); // 'this' = any Control/Window
if (topLevel?.Clipboard is { } clipboard)
    await clipboard.SetTextAsync(text);
```

In a ViewModel that has no direct visual reference, pass the `TopLevel` (or the window) in via constructor or method parameter.

---

## 13. `CS0411` — Type arguments for `AsyncEnumerable.Skip` cannot be inferred

**Symptom**
```
error CS0411: The type arguments for method 'AsyncEnumerable.Skip<TSource>(...)'
              cannot be inferred from the usage.
```

**Cause**  
The source expression is typed as a non-generic or ambiguous `IAsyncEnumerable`. Provide the type argument explicitly.

**Fix**
```csharp
// Old (broken)
var items = source.Skip(1);

// New — specify the element type
var items = source.Skip<MyItemType>(1);

// Or cast first
IAsyncEnumerable<MyItemType> typed = source;
var items = typed.Skip(1);
```

---

## 14. `CS0105` — Duplicate `using PleasantUI.ToolKit`

**Symptom**
```
warning CS0105: The using directive for 'PleasantUI.ToolKit' appeared previously
```

**Cause**  
The namespace is imported twice in the same file (often after a merge or copy-paste).

**Fix**  
Remove the duplicate `using` directive. Keep only one:

```csharp
using PleasantUI.ToolKit;
```

---

## 15. `CS0108` — Member hides inherited `Behavior.IsEnabledProperty` / `IsEnabled`

**Symptom**
```
warning CS0108: 'CommandOnDoubleTappedBehavior.IsEnabledProperty' hides inherited member
               'Behavior.IsEnabledProperty'. Use the new keyword if hiding was intentional.
```

**Cause**  
A custom `Behavior` subclass declares a property with the same name as one already defined in the base class.

**Fix**  
Either remove the duplicate declaration and use the inherited property, or add `new` if the shadow is intentional:

```csharp
// Option A — remove the duplicate and use the base property directly

// Option B — intentional shadow
public new static readonly StyledProperty<bool> IsEnabledProperty =
    AvaloniaProperty.Register<CommandOnDoubleTappedBehavior, bool>(nameof(IsEnabled), true);

public new bool IsEnabled
{
    get => GetValue(IsEnabledProperty);
    set => SetValue(IsEnabledProperty, value);
}
```

Option A is preferred unless you genuinely need different default values or behaviour.
