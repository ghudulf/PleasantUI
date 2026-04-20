# PleasantSettings

Central settings object for the PleasantUI library. Manages theme selection, accent color, language, window behaviour, color palettes, and app-version metadata. All properties implement `INotifyPropertyChanged` via `ViewModelBase`, so they can be bound directly in XAML.

## Lifecycle

`PleasantSettings` is created and owned by `PleasantTheme`. You never instantiate it yourself.

```xml
<!-- App.axaml — PleasantTheme creates and loads PleasantSettings.Current -->
<Application.Styles>
    <PleasantTheme />
</Application.Styles>
```

After `PleasantTheme` initialises, the singleton is available as:

```csharp
PleasantSettings? settings = PleasantSettings.Current;
```

`Current` is `null` until `PleasantTheme` has run its constructor, so always null-check before use.

## Persistence

Settings are serialised to JSON automatically:

- Loaded on startup from `<AppBaseDir>/Settings/PleasantUI.json`
- Saved on `ShutdownRequested`
- Also saved immediately when `Theme`, `Language`, or `NumericalAccentColor` change

The path constants are public if you need them:

```csharp
using PleasantUI.Core.Constants;

string path = Path.Combine(PleasantDirectories.Settings, PleasantFileNames.Settings);
// e.g. C:\MyApp\Settings\PleasantUI.json
```

Serialisation uses `System.Text.Json` source generation (`PleasantSettingsGenerationContext`), so it is AOT/trimming-safe.

---

## Properties

### `Current` — `static PleasantSettings?`

The singleton instance set by `PleasantTheme` at startup.

```csharp
if (PleasantSettings.Current is { } s)
    s.Theme = "Dark";
```

---

### `Theme` — `string`

Selects the active theme. Defaults to `"System"`.

| Value | Behaviour |
|---|---|
| `"System"` | Follows the OS light/dark preference |
| `"Light"` | Forces the built-in Light theme |
| `"Dark"` | Forces the built-in Dark theme |
| `"Custom"` | Activates `PleasantTheme.SelectedCustomTheme` |
| `"VGUI"` | Activates the VGUI style (loads extra control styles) |
| any custom name | Activates the matching `CustomTheme` by name |

```csharp
PleasantSettings.Current!.Theme = "Dark";
```

Changing this property triggers an immediate save to disk.

---

### `NumericalAccentColor` — `uint`

The accent color stored as a packed ARGB `uint`. When `PreferUserAccentColor` is `false` (the default), this is overwritten by the OS accent color on startup and whenever the OS color changes.

```csharp
using Avalonia.Media;

// Read
Color accent = Color.FromUInt32(PleasantSettings.Current!.NumericalAccentColor);

// Write (also set PreferUserAccentColor = true to prevent OS override)
PleasantSettings.Current!.PreferUserAccentColor = true;
PleasantSettings.Current!.NumericalAccentColor = Colors.CornflowerBlue.ToUInt32();
```

Changing this property immediately updates all `AccentColor`, `AccentLightColor1–3`, and `AccentDarkColor1–3` resources in the theme.

---

### `PreferUserAccentColor` — `bool`

When `true`, the accent color set in `NumericalAccentColor` is used as-is and OS color-change events are ignored. Defaults to `false`.

```csharp
PleasantSettings.Current!.PreferUserAccentColor = true;
PleasantSettings.Current!.NumericalAccentColor = myColor.ToUInt32();
```

---

### `CustomThemeId` — `Guid?`

The `Id` of the currently selected `CustomTheme`. Set automatically when you assign `PleasantTheme.SelectedCustomTheme`. Persisted to JSON so the custom theme is restored on next launch.

```csharp
// Prefer setting via PleasantTheme — it keeps CustomThemeId in sync
PleasantTheme.SelectedCustomTheme = PleasantTheme.CustomThemes.First(t => t.Name == "Ocean");
PleasantSettings.Current!.Theme = "Custom";
```

---

### `Language` — `string`

BCP-47 language code used by the built-in localisation system. Defaults to `"en"`. Changing this property triggers an immediate save to disk.

```csharp
PleasantSettings.Current!.Language = "ru";
```

---

### `ColorPalettes` — `AvaloniaList<uint>`

A user-defined list of saved accent colors (packed ARGB `uint`). Used by `PleasantColorView` to display a color palette picker. You can read and write it freely.

```csharp
// Add a color
PleasantSettings.Current!.ColorPalettes.Add(Colors.Teal.ToUInt32());

// Remove a color
PleasantSettings.Current!.ColorPalettes.Remove(Colors.Teal.ToUInt32());

// Iterate
foreach (uint packed in PleasantSettings.Current!.ColorPalettes)
{
    Color c = Color.FromUInt32(packed);
}
```

---

### `WindowSettings` — `WindowSettings`

Nested settings object for window appearance. See [WindowSettings](#windowsettings-1) below.

---

### `AppVersion` — `AppVersionSettings`

Nested settings object for your application's own version metadata. See [AppVersionSettings](#appversionsettings-1) below.

---

## Static version properties

These reflect the PleasantUI library version, not your application's version.

| Property | Type | Description |
|---|---|---|
| `Version` | `string` | Numeric assembly version, e.g. `"5.2.1.0"` |
| `InformationalVersion` | `string` | Full version string including pre-release tags, e.g. `"5.2.1-canary-20260410"` |
| `VersionType` | `PleasantVersionType` | Parsed release channel (see enum below) |
| `VersionTypeDescription` | `string` | Human-readable label, e.g. `"Canary Build"` |

```csharp
Console.WriteLine(PleasantSettings.InformationalVersion);   // 5.2.1-beta
Console.WriteLine(PleasantSettings.VersionTypeDescription); // Beta Pre-Release
```

---

## `PleasantVersionType` enum

Used by both `PleasantSettings` (library version) and `AppVersionSettings` (app version).

| Value | Example version string |
|---|---|
| `Stable` | `5.2.1` |
| `BugFix` | `5.2.1-fix` |
| `Alpha` | `5.2.1-alpha` |
| `Beta` | `5.2.1-beta` |
| `ReleaseCandidate` | `5.2.1-rc` |
| `Canary` | `5.2.1-canary-20260410-161942` |

---

## Reacting to changes

All properties raise `PropertyChanged`, so you can subscribe in code or bind in XAML:

```csharp
PleasantSettings.Current!.PropertyChanged += (_, e) =>
{
    if (e.PropertyName == nameof(PleasantSettings.Theme))
        Console.WriteLine($"Theme changed to {PleasantSettings.Current.Theme}");
};
```

XAML binding (works because `PleasantSettings` implements `INotifyPropertyChanged`):

```xml
<TextBlock Text="{Binding Source={x:Static core:PleasantSettings.Current}, Path=Theme}" />
```

---

## `WindowSettings`

Nested inside `PleasantSettings.WindowSettings`. Controls default window chrome behaviour.

| Property | Type | Default | Description |
|---|---|---|---|
| `EnableBlur` | `bool` | `true` | Enable acrylic/blur transparency on windows |
| `EnableCustomTitleBar` | `bool` | `true` | Show the PleasantUI custom title bar instead of the OS one |
| `OpacityLevel` | `double` | `0.8` | Background opacity when blur is active |

```csharp
var ws = PleasantSettings.Current!.WindowSettings;

ws.EnableBlur = false;
ws.EnableCustomTitleBar = true;
ws.OpacityLevel = 0.9;
```

`PleasantMiniWindow` reads `EnableCustomTitleBar` from here automatically on template apply.

---

## `AppVersionSettings`

Nested inside `PleasantSettings.AppVersion`. Stores your application's own version, independent of the PleasantUI library version. All properties are serialised to `PleasantUI.json`.

| Property | Type | Default | Description |
|---|---|---|---|
| `Version` | `string` | `""` | Your app's version string, e.g. `"2.1.0"` |
| `VersionType` | `PleasantVersionType` | `Stable` | Release channel of your app |
| `VersionLabel` | `string` | `"Version"` | Label shown next to the version in UI |
| `VersionTypeDescription` | `string` | *(derived)* | Human-readable channel name — not serialised |

```csharp
var av = PleasantSettings.Current!.AppVersion;

av.Version      = "2.1.0";
av.VersionType  = PleasantVersionType.Beta;
av.VersionLabel = "App Version";

// Read derived label
Console.WriteLine(av.VersionTypeDescription); // "Beta Pre-Release"
```

Bind in XAML:

```xml
<TextBlock>
    <Run Text="{Binding Source={x:Static core:PleasantSettings.Current}, Path=AppVersion.VersionLabel}" />
    <Run Text=" " />
    <Run Text="{Binding Source={x:Static core:PleasantSettings.Current}, Path=AppVersion.Version}" />
</TextBlock>
```

---

## Full example — settings page ViewModel

```csharp
using PleasantUI.Core;
using PleasantUI.Core.Settings;

public class SettingsViewModel : ViewModelBase
{
    private PleasantSettings Settings => PleasantSettings.Current!;

    // Theme
    public string Theme
    {
        get => Settings.Theme;
        set => Settings.Theme = value; // triggers save + theme switch automatically
    }

    // Accent color
    public bool UseCustomAccent
    {
        get => Settings.PreferUserAccentColor;
        set => Settings.PreferUserAccentColor = value;
    }

    public Color AccentColor
    {
        get => Color.FromUInt32(Settings.NumericalAccentColor);
        set
        {
            Settings.PreferUserAccentColor = true;
            Settings.NumericalAccentColor  = value.ToUInt32();
        }
    }

    // Language
    public string Language
    {
        get => Settings.Language;
        set => Settings.Language = value;
    }

    // Window
    public bool EnableBlur
    {
        get => Settings.WindowSettings.EnableBlur;
        set => Settings.WindowSettings.EnableBlur = value;
    }
}
```
