# PleasantUI API Quick Reference

Detailed reference for every major control. Namespace is `PleasantUI.Controls` unless noted.

---

## Windows

### PleasantWindow : PleasantWindowBase

Full application window with custom title bar, blur, splash screen, and caption button control.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `EnableCustomTitleBar` | `bool` | `true` | Show PleasantUI title bar instead of OS chrome |
| `TitleBarType` | `PleasantTitleBar.Type` | `Classic` | `Classic` / `ClassicExtended` / `Compact` |
| `Subtitle` | `string` | `""` | Secondary text shown in the title bar below the title |
| `DisplayIcon` | `object?` | `null` | Title bar icon — accepts `Geometry`, `IImage`, or any `Control` |
| `DisplayTitle` | `object?` | `null` | Override the title bar text with any content |
| `TitleContent` | `object?` | `null` | Arbitrary content placed in the center slot of the title bar |
| `LeftTitleBarContent` | `object?` | `null` | Content placed to the left of the title (e.g. hamburger button) |
| `CaptionButtons` | `PleasantCaptionButtons.Type` | `All` | Which caption buttons to show |
| `EnableBlur` | `bool` | `false` | Enable acrylic/blur window background |
| `ExtendsContentIntoTitleBar` | `bool` | `false` | Allow content to fill the title bar area |
| `TitleBarHeight` | `double` | `32` | Read-only. Height of the rendered title bar |
| `SplashScreen` | `IPleasantSplashScreen?` | `null` | Shown on open; removed after `RunTasks` completes and `MinimumShowTime` elapses |
| `IsCloseButtonVisible` | `bool` | `true` | Show/hide the close button |
| `IsMinimizeButtonVisible` | `bool` | `true` | Show/hide the minimize button |
| `IsRestoreButtonVisible` | `bool` | `true` | Show/hide the maximize/restore button |
| `IsFullScreenButtonVisible` | `bool` | `false` | Show/hide the full-screen toggle button |
| `OverrideMacOSCaption` | `bool` | `true` | Replace native macOS traffic-light buttons with PleasantUI ones |

**Usage**

```xml
<PleasantWindow Title="My App"
                Subtitle="v2.0"
                TitleBarType="Compact"
                EnableBlur="True"
                ExtendsContentIntoTitleBar="True">
    <Grid>
        <!-- content -->
    </Grid>
</PleasantWindow>
```

When `ExtendsContentIntoTitleBar` is `true`, add a top margin equal to `TitleBarHeight` to your content so it doesn't sit under the title bar chrome.

---

### PleasantMiniWindow : PleasantWindowBase

Compact floating tool window with optional pin-to-top and hide buttons.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `EnableCustomTitleBar` | `bool` | `true` | Show PleasantUI title bar |
| `EnableBlur` | `bool` | `false` | Enable acrylic/blur background |
| `ShowPinButton` | `bool` | `true` | Show the pin-to-top toggle button |
| `ShowHiddenButton` | `bool` | `false` | Show the minimize-to-tray button |

`EnableCustomTitleBar` is also read from `PleasantSettings.Current.WindowSettings.EnableCustomTitleBar` on template apply.

---

### PleasantCaptionButtons : TemplatedControl

Window caption button set. Placed automatically inside `PleasantTitleBar`. Set via `PleasantWindow.CaptionButtons`.

**Enum PleasantCaptionButtons.Type**

| Value | Buttons shown |
|---|---|
| `All` | Close, Minimize, Maximize/Restore |
| `CloseAndCollapse` | Close, Minimize |
| `CloseAndExpand` | Close, Maximize/Restore |
| `Close` | Close only |
| `None` | No buttons |

---

### PleasantTitleBar : TemplatedControl

Custom title bar rendered inside `PleasantWindow`. Rarely used directly — configure it through `PleasantWindow` properties.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `IsTitleVisible` | `bool` | `true` | Show/hide the icon+title+subtitle panel |
| `LeftClearance` | `double` | `40` | Width of the left reserved column (keeps title clear of hamburger button) |

**Attached property** — set on any `Window`:
```xml
<PleasantWindow pleasantChrome:PleasantTitleBar.IsTitleBarHitTestVisible="False" />
```

**Enum PleasantTitleBar.Type**

| Value | Description |
|---|---|
| `Classic` (0) | Standard height title bar |
| `ClassicExtended` (1) | Slightly taller title bar |
| `Compact` (2) | Minimal height, icon-only chrome |

---

## Navigation

### NavigationView : TreeView

Side/top/bottom navigation pane with content area, back button, and dynamic compact mode. Automatically selects the first item on load.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Position` | `NavigationViewPosition` | `Left` | `Left` / `Top` / `Bottom` — controls pane layout |
| `IsOpen` | `bool` | `false` | Whether the pane is expanded |
| `AlwaysOpen` | `bool` | `false` | Prevent the pane from collapsing |
| `DisplayMode` | `SplitViewDisplayMode` | `CompactInline` | Underlying SplitView display mode |
| `DynamicDisplayMode` | `bool` | `true` | Auto-switch to compact at narrow widths (≤1005px) |
| `OpenPaneLength` | `double` | — | Width of the expanded pane |
| `CompactPaneLength` | `double` | — | Width of the collapsed/compact pane |
| `SelectedContent` | `object?` | — | *(read-only)* Content of the currently selected item |
| `SelectedContentTemplate` | `IDataTemplate` | — | Data template applied to `SelectedContent` |
| `Icon` | `Geometry` | — | Logo/icon shown in the pane header |
| `ShowBackButton` | `bool` | `false` | Show a back button above the hamburger |
| `BackButtonCommand` | `ICommand?` | — | Command executed when the back button is clicked |
| `ButtonsPanelOffset` | `bool` | `false` | Push hamburger/back buttons below the title bar (auto-set when `TitleBarType=Compact`) |
| `DisplayTopIndent` | `bool` | `true` | Add top indent to the pane content |
| `NotMakeOffsetForContentPanel` | `bool` | `false` | Disable the automatic content panel offset |
| `IsFloatingHeader` | `bool` | `false` | Float the header above the pane items |
| `BindWindowSettings` | `bool` | `true` | Sync pane open state with `PleasantSettings` |
| `TransitionAnimation` | `Animation?` | — | Page transition animation played when `SelectedContent` changes |
| `TopItems` | `AvaloniaList<NavigationViewItem>` | — | Items for `Position=Top` bar (separate from `Items`) |
| `BottomItems` | `AvaloniaList<NavigationViewItem>` | — | Items for `Position=Bottom` bar (separate from `Items`) |
| `ItemsAsStrings` | `IEnumerable<string>?` | — | *(read-only)* Flat list of all item titles for search/autocomplete |

**Pseudo-classes:** `:normal`, `:compact`, `:left`, `:top`, `:bottom`

**Usage**

```xml
<NavigationView OpenPaneLength="220"
                CompactPaneLength="48"
                ShowBackButton="True"
                BackButtonCommand="{Binding GoBackCommand}">
    <NavigationViewItem Icon="{DynamicResource HomeRegular}" Title="Home">
        <local:HomePage />
    </NavigationViewItem>
    <NavigationViewItem Icon="{DynamicResource SettingsRegular}" Title="Settings">
        <local:SettingsPage />
    </NavigationViewItem>
</NavigationView>
```

For `Position=Top` or `Bottom`, populate `TopItems`/`BottomItems` instead of `Items`:

```xml
<NavigationView Position="Top">
    <NavigationView.TopItems>
        <NavigationViewItem Title="Home" Icon="{DynamicResource HomeRegular}">
            <local:HomePage />
        </NavigationViewItem>
    </NavigationView.TopItems>
</NavigationView>
```

---

### NavigationViewItem : TreeViewItem

Navigation item. Holds page content and can contain child items for sub-menus.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Title` | `object` | `"Title"` | Label shown in the pane |
| `Icon` | `Geometry?` | — | Icon shown in the pane |
| `Content` | `object?` | — | Page content displayed in the content area when this item is selected |
| `IsOpen` | `bool` | `false` | Whether the child items sub-menu is expanded |
| `SelectOnClose` | `bool` | `false` | Select this item when its sub-menu collapses |
| `ClickMode` | `ClickMode` | `Press` | `Press` / `Release` / `Hover` |
| `IsSubMenuOpen` | `bool` | `false` | Whether the compact-mode flyout popup is open |

**Events:** `Opened` (tunnel), `Closed` (tunnel) — fired when the item's sub-menu expands/collapses.

```xml
<!-- Item with children (group) -->
<NavigationViewItem Title="Tools" Icon="{DynamicResource TuneRegular}">
    <NavigationViewItem Title="Editor" Icon="{DynamicResource EditRegular}">
        <local:EditorPage />
    </NavigationViewItem>
    <NavigationViewItem Title="Console" Icon="{DynamicResource CodeBracesRegular}">
        <local:ConsolePage />
    </NavigationViewItem>
</NavigationViewItem>
```

---

## Tabs

### PleasantTabView : TabControl

Tab control with a smooth-scrolling tab strip and an add-tab (+) button.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `AdderButtonIsVisible` | `bool` | `true` | Show the + button at the end of the tab strip |
| `MaxWidthOfItemsPresenter` | `double` | `∞` | Maximum width of the tab strip |
| `SecondaryBackground` | `IBrush` | — | Background brush of the content area below the tabs |
| `ItemsMargin` | `Thickness` | `0` | Margin applied around each tab item |
| `MarginType` | `ViewMarginType` | `None` | `None` / `Little` / `Extended` — preset margin sizes |
| `HeightRemainingSpace` | `double` | — | *(read-only)* Remaining vertical space below the tab strip |
| `WidthRemainingSpace` | `double` | — | *(read-only)* Remaining horizontal space beside the tab strip |

**Events:** `ClickOnAddingButton` (bubble) — fired when the + button is clicked.

**Enum ViewMarginType:** `None` (0), `Little` (1), `Extended` (2)

```xml
<PleasantTabView AdderButtonIsVisible="True"
                 ClickOnAddingButton="OnAddTab">
    <PleasantTabItem Header="Tab 1" IsClosable="True">
        <TextBlock Text="Content 1" />
    </PleasantTabItem>
</PleasantTabView>
```

```csharp
private void OnAddTab(object? sender, RoutedEventArgs e)
{
    tabView.Items.Add(new PleasantTabItem { Header = "New Tab" });
}
```

---

### PleasantTabItem : TabItem

Tab item with a close button and an unsaved-changes indicator dot.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `IsClosable` | `bool` | `true` | Show the × close button on the tab |
| `IsClosing` | `bool` | — | *(read-only)* True while the close animation is running |
| `IsEditedIndicator` | `bool` | `false` | Show a dot indicator to signal unsaved changes |

**Events:**
- `Closing` (bubble) — fired when the × button is clicked, before the tab is removed
- `CloseButtonClick` (bubble) — fired specifically when the × button is clicked

```xml
<PleasantTabItem Header="Document.txt"
                 IsClosable="True"
                 IsEditedIndicator="{Binding HasUnsavedChanges}"
                 Closing="OnTabClosing">
    <local:EditorView />
</PleasantTabItem>
```

---

## Command Bar

### CommandBar : ContentControl

Toolbar with primary commands shown inline and secondary commands in an overflow popup. Supports dynamic overflow — items automatically move to the popup when there is not enough space.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `PrimaryCommands` | `IAvaloniaList<ICommandBarElement>` | — | Commands shown inline in the bar |
| `SecondaryCommands` | `IAvaloniaList<ICommandBarElement>` | — | Commands shown in the overflow popup |
| `IsOpen` | `bool` | `false` | Whether the overflow popup is open |
| `IsSticky` | `bool` | `true` | Keep the popup open when clicking outside (light-dismiss disabled) |
| `ClosedDisplayMode` | `CommandBarClosedDisplayMode` | `Compact` | How the bar looks when the popup is closed: `Compact` / `Minimal` / `Hidden` |
| `DefaultLabelPosition` | `CommandBarDefaultLabelPosition` | `Bottom` | Label position for primary buttons: `Bottom` / `Right` / `Collapsed` |
| `ItemsAlignment` | `CommandBarItemsAlignment` | `Left` | Align primary commands: `Left` / `Right` |
| `OverflowButtonVisibility` | `CommandBarOverflowButtonVisibility` | `Auto` | Show the ··· button: `Auto` / `Visible` / `Collapsed` |
| `IsDynamicOverflowEnabled` | `bool` | `true` | Auto-move items to overflow when bar is too narrow |

**Pseudo-classes:** `:open`, `:compact`, `:minimal`, `:hidden`, `:labelBottom`, `:labelRight`, `:labelCollapsed`, `:primaryOnly`, `:secondaryOnly`, `:itemsRight`

```xml
<CommandBar DefaultLabelPosition="Bottom"
            IsDynamicOverflowEnabled="True">
    <CommandBar.PrimaryCommands>
        <CommandBarButton Label="New"    Icon="{DynamicResource AddRegular}"
                          Command="{Binding NewCommand}" />
        <CommandBarButton Label="Save"   Icon="{DynamicResource DocumentRegular}"
                          Command="{Binding SaveCommand}" />
        <CommandBarToggleButton Label="Pin" Icon="{DynamicResource PinRegular}"
                                IsChecked="{Binding IsPinned}" />
        <CommandBarSeparator />
        <CommandBarButton Label="Delete" Icon="{DynamicResource DeleteRegular}"
                          Command="{Binding DeleteCommand}" />
    </CommandBar.PrimaryCommands>
    <CommandBar.SecondaryCommands>
        <CommandBarButton Label="Settings" Icon="{DynamicResource SettingsRegular}"
                          KeyboardAcceleratorText="Ctrl+," />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

---

### CommandBarButton : Button

Button for use inside `CommandBar.PrimaryCommands` or `SecondaryCommands`.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Icon` | `Geometry?` | — | Icon geometry displayed on the button |
| `Label` | `string?` | — | Text label shown below or beside the icon |
| `KeyboardAcceleratorText` | `string?` | — | Shortcut hint shown in the overflow menu (e.g. `"Ctrl+S"`) |
| `IsCompact` | `bool` | `false` | Icon-only mode with reduced padding |
| `IsInOverflow` | `bool` | — | *(read-only)* True when the button has been moved to the overflow popup |
| `DynamicOverflowOrder` | `int` | `0` | Priority for overflow — lower values move to overflow first |

---

### CommandBarToggleButton : ToggleButton

Toggle button for `CommandBar`. Same properties as `CommandBarButton` plus `IsChecked` from `ToggleButton`.

| Property | Type | Description |
|---|---|---|
| `Icon` | `Geometry?` | Icon geometry |
| `Label` | `string?` | Text label |
| `KeyboardAcceleratorText` | `string?` | Shortcut hint in overflow |
| `IsCompact` | `bool` | Icon-only mode |
| `IsChecked` | `bool?` | Checked state (inherited from `ToggleButton`) |
| `IsInOverflow` | `bool` | *(read-only)* |
| `DynamicOverflowOrder` | `int` | Overflow priority |

---

## Dialogs

### MessageBox : ContentDialog *(PleasantUI.ToolKit)*

Static factory for simple modal dialogs. Returns the clicked button's result string, or a typed result when using the generic overload.

**Static methods**

```csharp
// Simplest — single OK button
string result = await MessageBox.Show(window, "Title", "Message body");

// Custom buttons
string result = await MessageBox.Show(window, "Confirm", "Delete this file?",
    new[]
    {
        new MessageBoxButton { Text = "Delete", Result = "delete", Default = true },
        new MessageBoxButton { Text = "Cancel", Result = "cancel", IsKeyDown = true }
    },
    style: MessageBoxStyle.Danger);

// With stack trace / extra detail text
await MessageBox.Show(window, "Error", "Something went wrong.",
    new[] { new MessageBoxButton { Text = "OK", Result = "ok", Default = true } },
    additionalText: exception.ToString());

// With extra content and value capture
var check = new CheckBox { Content = "Don't show again" };
var r = await MessageBox.Show<bool>(window, "Info", "Welcome!",
    extraContent: check,
    valueSelector: _ => check.IsChecked == true,
    onContentReady: ctrl => ((CheckBox)ctrl).Focus());
// r.Button == "ok", r.Value == true/false
```

**MessageBoxButton properties**

| Property | Type | Description |
|---|---|---|
| `Text` | `string` | Button label |
| `Result` | `string` | Value returned when this button is clicked |
| `Default` | `bool` | Gets accent/danger styling; Enter key triggers this button |
| `IsKeyDown` | `bool` | Enter key triggers this button (without accent styling) |
| `ThemeKey` | `string?` | Resource key of a `ControlTheme` to apply to the button |

**MessageBoxStyle:** `Default`, `Danger` (red header with warning icon)

**MessageBoxResult\<T\>** (generic overload return type): `.Button` (string), `.Value` (T)

---

### PleasantDialog : ContentDialog *(PleasantUI.ToolKit)*

Rich modal dialog with header icon, subheader, command items (radio buttons, checkboxes, command links), progress bar, and expandable footer.

**Static factory**

```csharp
object result = await PleasantDialog.Show(window,
    header:          "Sync settings",
    body:            "Choose how your settings should be synchronized.",
    iconGeometryKey: "TuneRegular",
    subHeader:       "Affects all connected accounts.",
    commands: new PleasantDialogCommand[]
    {
        new PleasantDialogRadioButton { Text = "Sync automatically", IsChecked = true },
        new PleasantDialogRadioButton { Text = "Ask before syncing" },
        new PleasantDialogCheckBox   { Text = "Remember my choice" }
    },
    buttons: new[]
    {
        new PleasantDialogButton { Text = "Save",   DialogResult = PleasantDialogResult.OK,     IsDefault = true },
        new PleasantDialogButton { Text = "Cancel", DialogResult = PleasantDialogResult.Cancel }
    },
    footer:           new TextBlock { Text = "Changes take effect after restart." },
    footerExpandable: true,
    footerToggleText: "More details",
    style:            MessageBoxStyle.Default);

if (result is PleasantDialogResult.OK) { /* save */ }
```

**Show parameters**

| Parameter | Type | Description |
|---|---|---|
| `parent` | `IPleasantWindow` | Owner window |
| `header` | `string` | Bold title text |
| `body` | `string?` | Body text below the header |
| `buttons` | `IReadOnlyList<PleasantDialogButton>?` | Bottom buttons (defaults to single OK) |
| `commands` | `IReadOnlyList<PleasantDialogCommand>?` | Radio buttons, checkboxes, or command links |
| `iconGeometryKey` | `string?` | Resource key of a `Geometry` shown next to the header |
| `headerBackground` | `IBrush?` | Custom header background brush |
| `iconForeground` | `IBrush?` | Custom icon foreground brush |
| `subHeader` | `string?` | Secondary text below the title |
| `extraContent` | `Control?` | Arbitrary control placed below commands |
| `onContentReady` | `Action<Control>?` | Called after extra content is attached, before dialog shows |
| `onDialogReady` | `Action<PleasantDialog>?` | Called with the dialog instance after layout — use to call `SetProgressBarState` |
| `footer` | `object?` | Footer content |
| `footerExpandable` | `bool` | Hide footer behind a toggle button |
| `footerToggleText` | `string?` | Label for the footer toggle |
| `style` | `MessageBoxStyle` | `Default` or `Danger` |

**Instance method:** `SetProgressBarState(double value)` — set progress bar 0–100 from a background task.

**Events:** `Opening`, `Opened`, `Closing` (set `PleasantDialogClosingEventArgs.Cancel = true` to prevent close)

**PleasantDialogButton**

```csharp
new PleasantDialogButton { Text = "Save", DialogResult = PleasantDialogResult.OK, IsDefault = true }
```

Predefined statics: `PleasantDialogButton.OK`, `.Cancel`, `.Yes`, `.No`, `.Close`, `.Retry`

**PleasantDialogResult:** `None`, `OK`, `Cancel`, `Yes`, `No`, `Close`, `Retry`

**Command item types:** `PleasantDialogRadioButton`, `PleasantDialogCheckBox`, `PleasantDialogCommandLink`

`PleasantDialogCommandLink` extra properties: `Description` (string), `ClosesOnInvoked` (bool), `DialogResult` (object)

---

### ContentDialog : PleasantPopupElement

Base modal dialog. Use `MessageBox` or `PleasantDialog` in most cases. Subclass this for fully custom dialogs.

**Properties**

| Property | Type | Description |
|---|---|---|
| `BottomPanelContent` | `object` | Content placed in the bottom panel (e.g. custom buttons) |
| `IsClosed` | `bool` | *(read-only)* True after the dialog has closed |
| `IsClosing` | `bool` | *(read-only)* True while the close animation is running |
| `OpenAnimation` | `Animation?` | Animation played when the dialog opens |
| `CloseAnimation` | `Animation?` | Animation played when the dialog closes |
| `ShowBackgroundAnimation` | `Animation?` | Animation played on the dim overlay when opening |
| `HideBackgroundAnimation` | `Animation?` | Animation played on the dim overlay when closing |

**Methods**

```csharp
// Show on a window
await dialog.ShowAsync(window);
await dialog.ShowAsync(topLevel);

// Show and get a typed result
T? result = await dialog.ShowAsync<T>(window);

// Close programmatically
await dialog.CloseAsync();
await dialog.CloseAsync(myResult); // sets the dialog result
```

**Events:** `Closed` (EventHandler), `WindowClosed` (routed direct), `WindowOpened` (routed direct)

---

### PleasantDrawer : PleasantPopupElement

Panel that slides in from any screen edge, overlaying the main content. Supports light-dismiss, modal blocking, and typed result return.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Title` | `string?` | — | Header text shown at the top of the drawer |
| `Position` | `DrawerPosition` | `Right` | Which edge to slide in from: `Left` / `Right` / `Top` / `Bottom` |
| `CanLightDismiss` | `bool` | `true` | Close the drawer when the user clicks the overlay background |
| `IsModal` | `bool` | `false` | Block all interaction with the rest of the window while open |
| `FooterContent` | `object?` | — | Content placed at the bottom of the drawer (e.g. action buttons) |
| `PanelWidth` | `double` | — | Fixed width for Left/Right drawers (0 = auto) |
| `PanelHeight` | `double` | — | Fixed height for Top/Bottom drawers (0 = auto) |
| `OpenAnimation` | `Animation?` | — | Slide-in animation applied to the drawer panel |
| `CloseAnimation` | `Animation?` | — | Slide-out animation applied to the drawer panel |
| `ShowOverlayAnimation` | `Animation?` | — | Fade-in animation for the dim overlay |
| `HideOverlayAnimation` | `Animation?` | — | Fade-out animation for the dim overlay |

**Methods**

```csharp
// Show and wait for close
await drawer.ShowAsync(window);

// Show and get a typed result
string? result = await drawer.ShowAsync<string>(window);

// Close programmatically (optionally pass a result)
await drawer.CloseAsync();
await drawer.CloseAsync("confirmed");
```

**Events:** `Opened` (routed direct), `Closed` (routed direct)

**Pseudo-classes:** `:left`, `:right`, `:top`, `:bottom`, `:open`, `:closing`

```csharp
var drawer = new PleasantDrawer
{
    Title          = "Filters",
    Position       = DrawerPosition.Right,
    PanelWidth     = 320,
    CanLightDismiss = true,
    Content        = new FilterPanel(),
    FooterContent  = new StackPanel
    {
        Children =
        {
            new Button { Content = "Apply", Command = applyCommand },
            new Button { Content = "Reset", Command = resetCommand }
        }
    }
};
await drawer.ShowAsync(window);
```

---

### PopConfirm : ContentControl

Wraps any control and shows a lightweight confirmation popup before executing a command. The popup appears on click or focus depending on `TriggerMode`.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `PopupHeader` | `object?` | — | Title content shown inside the popup |
| `PopupHeaderTemplate` | `IDataTemplate?` | — | Data template for `PopupHeader` |
| `PopupContent` | `object?` | — | Body content shown inside the popup |
| `PopupContentTemplate` | `IDataTemplate?` | — | Data template for `PopupContent` |
| `ConfirmCommand` | `ICommand?` | — | Executed when the user clicks Confirm |
| `ConfirmCommandParameter` | `object?` | — | Parameter passed to `ConfirmCommand` |
| `CancelCommand` | `ICommand?` | — | Executed when the user clicks Cancel |
| `CancelCommandParameter` | `object?` | — | Parameter passed to `CancelCommand` |
| `TriggerMode` | `PopConfirmTriggerMode` | `Click` | `Click` — open on click; `Focus` — open on focus |
| `Placement` | `PlacementMode` | `BottomEdgeAlignedLeft` | Popup placement relative to the wrapped control |
| `Icon` | `object?` | — | Icon shown in the popup header area |
| `IsDropdownOpen` | `bool` | `false` | Whether the popup is currently open |
| `HandleAsyncCommand` | `bool` | `true` | Await async commands before closing the popup |

```xml
<PopConfirm PopupHeader="Delete this item?"
            PopupContent="This action cannot be undone."
            ConfirmCommand="{Binding DeleteCommand}"
            Placement="BottomEdgeAlignedLeft">
    <Button Content="Delete"
            Theme="{DynamicResource DangerButtonTheme}" />
</PopConfirm>
```

---

## Notifications

### PleasantSnackbar : PleasantPopupElement

Toast notification. Always use the static `Show` overloads — do not instantiate directly.

**Static Show overloads**

```csharp
PleasantSnackbar.Show(window, options);
PleasantSnackbar.Show(topLevel, options);
PleasantSnackbar.Show(pleasantWindow, options);
PleasantSnackbar.Show(options); // no parent — uses global queue
```

**PleasantSnackbarOptions**

| Property | Type | Default | Description |
|---|---|---|---|
| `Message` | `string` | *(required)* | Main notification text |
| `Title` | `string?` | `null` | Optional bold title shown above the message |
| `Icon` | `Geometry?` | `null` | Icon geometry shown on the left |
| `NotificationType` | `NotificationType` | `Information` | `Information` / `Success` / `Warning` / `Error` — sets color accent |
| `TimeSpan` | `TimeSpan` | `3s` | How long the snackbar stays visible (0 = 3s default) |
| `IsClosable` | `bool` | `false` | Show a × dismiss button |
| `ButtonText` | `string?` | `null` | Text for an optional action button |
| `ButtonAction` | `Action?` | `null` | Action executed when the text button is clicked |
| `ActionButton` | `Control?` | `null` | Custom control placed in the action slot (overrides `ButtonText`/`ButtonAction`) |
| `Closing` | `EventHandler<SnackbarClosingEventArgs>?` | — | Raised before close; set `Cancel=true` to prevent |
| `Closed` | `EventHandler<SnackbarClosedEventArgs>?` | — | Raised after the snackbar has closed |

`SnackbarClosingEventArgs.Reason` / `SnackbarClosedEventArgs.Reason` — `SnackbarCloseReason`: `Timeout`, `UserDismiss`, `CloseButton`, `Programmatic`

```csharp
PleasantSnackbar.Show(window, new PleasantSnackbarOptions("File saved successfully")
{
    Title            = "Saved",
    Icon             = ResourceExtensions.GetResource<Geometry>("CheckmarkRegular"),
    NotificationType = NotificationType.Success,
    TimeSpan         = TimeSpan.FromSeconds(4),
    IsClosable       = true,
    ButtonText       = "Undo",
    ButtonAction     = () => UndoSave(),
    Closing          = (_, args) => { if (!CanClose()) args.Cancel = true; }
});
```

**Instance properties** (available when subclassing or accessing the instance):

| Property | Type | Description |
|---|---|---|
| `Title` | `string?` | Bold title |
| `Icon` | `Geometry?` | Icon geometry |
| `NotificationType` | `NotificationType` | Notification type |
| `IsClosable` | `bool` | Show × button |
| `ActionButton` | `Control?` | Custom action control |
| `Command` | `ICommand?` | Executed when the snackbar body is clicked |
| `OpenAnimation` | `Animation?` | Custom open animation |
| `CloseAnimation` | `Animation?` | Custom close animation |

---

## Data Display

### BreadcrumbBar : ItemsControl

Breadcrumb trail. Items that overflow the available width are collapsed into an ellipsis (…) flyout.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `IsLastItemClickEnabled` | `bool` | `false` | Allow the last (current) item to be clicked; when `false` it renders as plain text |

**Events:** `ItemClicked` → `BreadcrumbBarItemClickedEventArgs` with `.Item` (object) and `.Index` (int)

```xml
<BreadcrumbBar ItemsSource="{Binding BreadcrumbPath}"
               IsLastItemClickEnabled="True"
               ItemClicked="OnBreadcrumbClicked">
    <BreadcrumbBar.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}" />
        </DataTemplate>
    </BreadcrumbBar.ItemTemplate>
</BreadcrumbBar>
```

```csharp
private void OnBreadcrumbClicked(object? sender, BreadcrumbBarItemClickedEventArgs e)
{
    NavigateTo(e.Item, e.Index);
}
```

---

### DashboardCard : HeaderedContentControl

Card with a structured header (title + optional subtitle + toolbar), scrollable content area, and footer.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Header` | `object` | — | Card title (inherited from `HeaderedContentControl`) |
| `Subtitle` | `string?` | — | Secondary text shown below the title |
| `HeaderContent` | `object?` | — | Arbitrary content placed in the header's right slot |
| `HeaderContentTemplate` | `IDataTemplate?` | — | Data template for `HeaderContent` |
| `FooterContent` | `object?` | — | Content shown in the card footer |
| `FooterContentTemplate` | `IDataTemplate?` | — | Data template for `FooterContent` |
| `ContentPadding` | `Thickness` | `16` | Padding applied to the scrollable content area |
| `ShowScrollViewer` | `bool` | `true` | Wrap the content in a `SmoothScrollViewer` |
| `ToolbarItems` | `AvaloniaList<object>` | — | Items rendered in the header toolbar (right side) |

**Pseudo-classes:** `:hasToolbar`, `:hasSubtitle`

```xml
<DashboardCard Header="Revenue" Subtitle="Last 30 days" ContentPadding="0">
    <DashboardCard.ToolbarItems>
        <Button Theme="{DynamicResource AppBarButtonTheme}">
            <PathIcon Data="{DynamicResource MoreRegular}" Width="14" Height="14" />
        </Button>
    </DashboardCard.ToolbarItems>
    <DashboardCard.FooterContent>
        <TextBlock Text="Updated 2 min ago" Opacity="0.6" />
    </DashboardCard.FooterContent>
    <local:RevenueChart />
</DashboardCard>
```

---

### SelectionList : ListBox

Multi-select list with per-item image, title, subtitle, and timestamp. Supports horizontal and vertical orientations and shows an empty-state message when there are no items.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `TitleMemberBinding` | `BindingBase?` | — | Binding path for each item's title |
| `SubtitleMemberBinding` | `BindingBase?` | — | Binding path for each item's subtitle |
| `ImageMemberBinding` | `BindingBase?` | — | Binding path for each item's image |
| `TimestampMemberBinding` | `BindingBase?` | — | Binding path for each item's timestamp |
| `ImageTemplate` | `IDataTemplate?` | — | Custom template for the image slot |
| `EmptyMessage` | `string` | `"No items found"` | Text shown when the list is empty |
| `Orientation` | `Orientation` | `Vertical` | `Vertical` or `Horizontal` item layout |

All standard `ListBox` properties (`ItemsSource`, `SelectedItem`, `SelectionMode`, etc.) apply.

```xml
<SelectionList ItemsSource="{Binding Contacts}"
               TitleMemberBinding="{Binding Name}"
               SubtitleMemberBinding="{Binding Email}"
               TimestampMemberBinding="{Binding LastSeen}"
               SelectionMode="Multiple"
               EmptyMessage="No contacts found" />
```

---

### ItemListPanel : TemplatedControl

Searchable, filterable list with a loading overlay, bulk-action toolbar, and a pagination footer slot.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `ItemsSource` | `IEnumerable?` | — | Data source |
| `ItemTemplate` | `IDataTemplate?` | — | Item data template |
| `SelectedItem` | `object?` | — | Currently selected item |
| `SearchText` | `string?` | — | Current search/filter text (two-way bindable) |
| `SearchWatermark` | `string` | `"Search..."` | Placeholder text in the search box |
| `SearchBoxCornerRadius` | `CornerRadius` | `4` | Corner radius of the search box |
| `IsLoading` | `bool` | `false` | Show the loading overlay |
| `LoadingTitle` | `string` | `"Loading..."` | Title shown in the loading overlay |
| `LoadingSubtitle` | `string?` | — | Subtitle shown in the loading overlay |
| `IsMultiSelectMode` | `bool` | `false` | Enable multi-select mode (shows bulk action bar) |
| `SelectedCount` | `int` | — | *(read-only)* Number of selected items in multi-select mode |
| `BulkActionsContent` | `object?` | — | Content shown in the bulk-action toolbar when `IsMultiSelectMode=true` |
| `FooterContent` | `object?` | — | Content shown below the list (e.g. pagination controls) |
| `EmptyStateTitle` | `string` | `"No items"` | Title shown when the list is empty |
| `EmptyStateSubtitle` | `string?` | — | Subtitle shown when the list is empty |

**Pseudo-classes:** `:loading`, `:multiSelect`, `:hasFilter`, `:hasItems`, `:hasBulkActions`

```xml
<ItemListPanel ItemsSource="{Binding Files}"
               ItemTemplate="{StaticResource FileItemTemplate}"
               SearchWatermark="Search files..."
               IsLoading="{Binding IsLoading}"
               EmptyStateTitle="No files found"
               EmptyStateSubtitle="Try a different search term">
    <ItemListPanel.FooterContent>
        <local:PaginationControl CurrentPage="{Binding Page}" />
    </ItemListPanel.FooterContent>
</ItemListPanel>
```

---

### TreeViewPanel : TemplatedControl

Searchable panel hosting multiple collapsible `TreeViewSection` items. Provides a filter text box and expand/collapse-all buttons. Raises a unified `SelectionChanged` event when any section's selection changes.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Sections` | `AvaloniaList<TreeViewSection>` | — | The sections to display |
| `FilterText` | `string?` | — | Text used to filter items across all sections |
| `FilterWatermark` | `string` | `"Filter..."` | Placeholder text in the filter box |
| `SelectedItem` | `object?` | — | Currently selected item (across all sections) |
| `SelectedSection` | `TreeViewSection?` | — | The section that owns the selected item |
| `ShowExpandButton` | `bool` | `true` | Show the expand-all button |
| `ShowCollapseButton` | `bool` | `true` | Show the collapse-all button |
| `SearchBoxCornerRadius` | `CornerRadius` | `4` | Corner radius of the filter box |

**Events:** `SelectionChanged` — fired when any section's selected item changes.

**Pseudo-classes:** `:hasFilter`

```xml
<TreeViewPanel FilterWatermark="Search project...">
    <TreeViewPanel.Sections>
        <TreeViewSection Header="Files"
                         SectionIcon="{DynamicResource FolderRegular}"
                         ItemsSource="{Binding Files}"
                         ItemTemplate="{StaticResource FileTemplate}" />
        <TreeViewSection Header="References"
                         SectionIcon="{DynamicResource LinkVariant}"
                         ItemsSource="{Binding References}"
                         IsLoading="{Binding LoadingRefs}" />
    </TreeViewPanel.Sections>
</TreeViewPanel>
```

---

### TreeViewSection : HeaderedItemsControl

Collapsible section inside a `TreeViewPanel`. Shows a header with icon badge, item count, and a virtualized list.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `SectionIcon` | `Geometry?` | — | Icon shown in the section header badge |
| `IconBackground` | `IBrush?` | — | Background of the icon badge |
| `IconForeground` | `IBrush?` | — | Foreground of the icon badge |
| `IsExpanded` | `bool` | `true` | Whether the section is expanded |
| `IsLoading` | `bool` | `false` | Show a loading indicator instead of items |
| `LoadingText` | `string` | `"Loading..."` | Text shown while loading |
| `SelectedItem` | `object?` | — | Currently selected item in this section |
| `MaxListHeight` | `double` | `∞` | Maximum height of the items list |
| `FilterText` | `string?` | — | Filter text applied to this section's items (set by `TreeViewPanel`) |

**Pseudo-classes:** `:loading`, `:empty`

All standard `ItemsControl` properties (`ItemsSource`, `ItemTemplate`, `Header`, etc.) apply.

---

### OptionsDisplayItem : TemplatedControl

Settings-page row with a header, description, left icon, and a right-side action control or expandable content area.

**Properties**

| Property | Type | Description |
|---|---|---|
| `Header` | `string` | Primary label |
| `Description` | `string` | Secondary label shown below the header |
| `Icon` | `Geometry` | Icon shown on the left |
| `ActionButton` | `Control` | Control placed on the right (e.g. `ToggleSwitch`, `ComboBox`) |
| `Navigates` | `bool` | Show a chevron → and make the row clickable |
| `NavigationCommand` | `ICommand?` | Command executed when `Navigates=true` and the row is clicked |
| `Expands` | `bool` | Show an expand/collapse chevron |
| `IsExpanded` | `bool` | Whether the expandable content is visible |
| `Content` | `object?` | Content shown when the row is expanded |

**Events:** `NavigationRequested` (bubble) — fired when the row is clicked and `Navigates=true`.

```xml
<!-- Toggle switch row -->
<OptionsDisplayItem Header="Dark mode"
                    Description="Switch the application theme"
                    Icon="{DynamicResource ThemeLightDark}">
    <OptionsDisplayItem.ActionButton>
        <ToggleSwitch IsChecked="{Binding IsDarkMode}" />
    </OptionsDisplayItem.ActionButton>
</OptionsDisplayItem>

<!-- Navigation row -->
<OptionsDisplayItem Header="Account"
                    Description="Manage your account settings"
                    Icon="{DynamicResource SettingsRegular}"
                    Navigates="True"
                    NavigationCommand="{Binding GoToAccountCommand}" />

<!-- Expandable row -->
<OptionsDisplayItem Header="Advanced"
                    Icon="{DynamicResource TuneRegular}"
                    Expands="True">
    <StackPanel Spacing="8">
        <CheckBox Content="Enable debug logging" />
        <CheckBox Content="Show developer tools" />
    </StackPanel>
</OptionsDisplayItem>
```

---

### InformationBlock : ContentControl

Simple content block with a leading icon. Use for callout boxes, tips, or status messages.

**Properties**

| Property | Type | Description |
|---|---|---|
| `Icon` | `Geometry` | Icon shown to the left of the content |

```xml
<InformationBlock Icon="{DynamicResource InfoRegular}">
    <TextBlock Text="This action requires administrator privileges." />
</InformationBlock>
```

---

## Input Controls

### PinCode : TemplatedControl

PIN / OTP entry control. Renders a row of individual character cells with keyboard navigation, clipboard paste support, and optional password masking.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Count` | `int` | `4` | Number of character cells |
| `Mode` | `PinCodeMode` | `LetterOrDigit` | Accepted input: `Digit` / `Letter` / `LetterOrDigit` |
| `PasswordChar` | `char` | `\0` | Mask character displayed instead of actual input (`\0` = no mask) |
| `Digits` | `IList<string>` | — | *(read-only)* Current values — one entry per cell, empty string = unfilled |
| `CompleteCommand` | `ICommand?` | — | Command invoked when all cells are filled |
| `Spacing` | `double` | `8` | Gap between cells in pixels |

**Events:** `Complete` → `PinCodeCompleteEventArgs` with `.Value` (string — all digits joined)

Changing `Count` resets `Digits` to a new list of empty strings.

```xml
<PinCode Count="6"
         Mode="Digit"
         PasswordChar="●"
         Spacing="12"
         CompleteCommand="{Binding VerifyCommand}" />
```

```csharp
pinCode.Complete += (_, e) => VerifyPin(e.Value); // e.Value == "123456"
```

---

### PathPicker : TemplatedControl

TextBox + browse button for picking file-system paths via the platform storage picker or the built-in PleasantUI file chooser.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Mode` | `PathPickerMode` | `OpenFile` | `OpenFile` / `OpenFolder` / `SaveFile` |
| `Title` | `string?` | — | Dialog title shown in the picker |
| `AllowMultiple` | `bool` | `false` | Allow selecting multiple files/folders |
| `FileFilter` | `string?` | — | Filter pattern in `[*.png,*.jpg]` format |
| `DefaultFileExtension` | `string?` | — | Default file extension for save dialogs |
| `SuggestedStartPath` | `string?` | — | Initial directory opened in the picker |
| `SuggestedFileName` | `string?` | — | Pre-filled file name for save dialogs |
| `SelectedPaths` | `IReadOnlyList<string>` | — | Currently selected paths |
| `SelectedPathsText` | `string?` | — | Comma-joined display text (two-way bindable) |
| `ButtonContent` | `object?` | — | Custom content for the browse button |
| `Command` | `ICommand?` | — | Executed after the user confirms a selection |
| `IsOmitCommandOnCancel` | `bool` | `false` | Skip `Command` if the picker was cancelled |
| `IsClearSelectionOnCancel` | `bool` | `false` | Clear `SelectedPaths` if the picker was cancelled |
| `UseCustomPicker` | `bool` | `false` | Use the PleasantUI file chooser instead of the OS picker |
| `VerticalContentAlignment` | `VerticalAlignment` | `Center` | Vertical alignment of the text box content |

**Pseudo-classes:** `:empty` (no selection), `:hasvalue` (at least one path selected)

```xml
<PathPicker Mode="OpenFile"
            Title="Select image"
            FileFilter="[*.png,*.jpg,*.jpeg]"
            AllowMultiple="False"
            SelectedPathsText="{Binding ImagePath}"
            Command="{Binding LoadImageCommand}" />
```

---

### PleasantDatePicker : DatePicker

Styled date picker with a custom click area decoupled from the internal flyout button. Allows full theme control over the outer shape while keeping Avalonia's internal date-picker wiring intact.

Use exactly like Avalonia's `DatePicker` — all standard properties (`SelectedDate`, `DisplayDateStart`, `DisplayDateEnd`, `DayVisible`, `MonthVisible`, `YearVisible`, etc.) apply.

```xml
<PleasantDatePicker SelectedDate="{Binding BirthDate}"
                    DayVisible="True"
                    MonthVisible="True"
                    YearVisible="True" />
```

---

### MarkedTextBox : TextBox

`TextBox` with a leading mark/symbol slot. Inherits all `TextBox` properties.

**Properties**

| Property | Type | Description |
|---|---|---|
| `Mark` | `object?` | Content placed to the left of the text input (any control or string) |

```xml
<MarkedTextBox Watermark="0.00" Text="{Binding Amount}">
    <MarkedTextBox.Mark>
        <TextBlock Text="$" VerticalAlignment="Center" />
    </MarkedTextBox.Mark>
</MarkedTextBox>
```

---

### MarkedNumericUpDown : NumericUpDown

`NumericUpDown` with a leading mark/symbol slot. Inherits all `NumericUpDown` properties.

**Properties**

| Property | Type | Description |
|---|---|---|
| `Mark` | `object?` | Content placed to the left of the numeric input |

```xml
<MarkedNumericUpDown Minimum="0" Maximum="100" Value="{Binding Opacity}">
    <MarkedNumericUpDown.Mark>
        <TextBlock Text="%" VerticalAlignment="Center" />
    </MarkedNumericUpDown.Mark>
</MarkedNumericUpDown>
```

---

## Progress & Feedback

### ProgressRing : RangeBase

Circular progress indicator. Supports both determinate (value-based) and indeterminate (spinning) modes.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `Value` | `double` | `0` | Current value (inherited from `RangeBase`) |
| `Minimum` | `double` | `0` | Minimum value (inherited from `RangeBase`) |
| `Maximum` | `double` | `100` | Maximum value (inherited from `RangeBase`) |
| `IsIndeterminate` | `bool` | `false` | Spin continuously instead of showing a value |
| `PreserveAspect` | `bool` | `true` | Keep the ring circular when width ≠ height |
| `Thickness` | `double` | `3` | Stroke thickness of the ring arc |
| `StartAngle` | `double` | `0` | Angle (degrees) where the arc starts |
| `EndAngle` | `double` | `360` | Angle (degrees) where the arc ends |
| `ValueAngle` | `double` | *(computed)* | *(read-only)* Current arc end angle based on `Value` |

**Pseudo-classes:** `:preserveaspect`, `:indeterminate`

```xml
<!-- Indeterminate spinner -->
<ProgressRing IsIndeterminate="True" Width="32" Height="32" Thickness="4" />

<!-- Determinate progress -->
<ProgressRing Value="{Binding Progress}"
              Minimum="0" Maximum="100"
              Width="64" Height="64"
              Thickness="6" />

<!-- Custom arc range (semicircle) -->
<ProgressRing Value="50" StartAngle="-90" EndAngle="270" Width="48" Height="48" />
```

---

## Visual Containers

### PleasantBorder : Border

`Border` with automatic background from the theme's background color scale. Inherits all `Border` properties (`CornerRadius`, `Padding`, `BorderBrush`, etc.).

**Properties**

| Property | Type | Description |
|---|---|---|
| `BackgroundLevel` | `BackgroundLeveling` | Sets `Background` to the matching theme color token |

**Enum BackgroundLeveling**

| Value | Theme token used |
|---|---|
| `Level1` | `BackgroundColor1` — window/page background |
| `Level2` | `BackgroundColor2` — panel/card background |
| `Level3` | `BackgroundColor3` — nested panel background |
| `Level4` | `BackgroundColor4` — deepest nested background |

```xml
<!-- Card on a page -->
<PleasantBorder BackgroundLevel="Level2"
                CornerRadius="{DynamicResource ControlCornerRadius}"
                Padding="16">
    <TextBlock Text="Card content" />
</PleasantBorder>

<!-- Nested panel inside a card -->
<PleasantBorder BackgroundLevel="Level3" CornerRadius="6" Padding="12">
    <TextBlock Text="Nested content" />
</PleasantBorder>
```

---

### ShadowBorder : Decorator

Renders a SkiaSharp drop shadow behind its single child. The shadow is cached and only recomputed when properties change or the child size changes.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `ShadowColor` | `Color` | `Black` | Shadow color (alpha controls opacity) |
| `BlurRadius` | `double` | `12` | Shadow blur radius in pixels |
| `Offset` | `Vector` | `(0, 4)` | Shadow offset `(x, y)` in pixels |
| `CornerRadius` | `CornerRadius` | `0` | Corner radius of the shadow shape — should match the child's corner radius |

```xml
<ShadowBorder ShadowColor="#40000000"
              BlurRadius="20"
              Offset="0,6"
              CornerRadius="12">
    <Border Background="{DynamicResource BackgroundColor2}"
            CornerRadius="12"
            Padding="20">
        <TextBlock Text="Elevated card" />
    </Border>
</ShadowBorder>
```

The `ShadowBorder` adds padding around its child equal to `BlurRadius + |Offset|` to make room for the shadow. Account for this in your layout.

---

### SmoothScrollViewer : ContentControl

Drop-in replacement for `ScrollViewer` with animated smooth scrolling. Implements `IScrollable` and `IScrollAnchorProvider`.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `SmoothScrollDuration` | `TimeSpan` | `0:0:0.015` | Duration of the scroll animation |
| `SmoothScrollEasing` | `Easing?` | — | Easing function for the scroll animation |
| `Offset` | `Vector` | `(0,0)` | Current scroll offset (settable to scroll programmatically) |
| `Extent` | `Size` | — | *(read-only)* Total scrollable content size |
| `Viewport` | `Size` | — | *(read-only)* Visible viewport size |
| `CanHorizontallyScroll` | `bool` | — | *(read-only)* Whether horizontal scrolling is possible |
| `CanVerticallyScroll` | `bool` | — | *(read-only)* Whether vertical scrolling is possible |

All standard `ScrollViewer` attached properties (`ScrollViewer.HorizontalScrollBarVisibility`, etc.) work on `SmoothScrollViewer`.

```xml
<SmoothScrollViewer SmoothScrollDuration="0:0:0.2"
                    HorizontalScrollBarVisibility="Disabled">
    <StackPanel Spacing="8">
        <!-- long content -->
    </StackPanel>
</SmoothScrollViewer>
```

---

### ModalWindowHost : ContentControl

Full-screen overlay host used internally by `ContentDialog`, `PleasantDrawer`, and `PleasantSnackbar`. Blocks all pointer events on the content beneath it. Rarely used directly.

**Properties**

| Property | Type | Default | Description |
|---|---|---|---|
| `HorizontalContentAlignment` | `HorizontalAlignment` | `Center` | Horizontal alignment of the hosted content |
| `VerticalContentAlignment` | `VerticalAlignment` | `Center` | Vertical alignment of the hosted content |

---

## Flyout

### PleasantFlyout : Flyout

Styled flyout with the PleasantUI presenter (rounded corners, shadow, theme-aware background). Use exactly like Avalonia's `Flyout` — all standard `Flyout` properties apply (`Placement`, `ShowMode`, `VerticalOffset`, `HorizontalOffset`, etc.).

```xml
<Button Content="Options">
    <Button.Flyout>
        <PleasantFlyout Placement="BottomEdgeAlignedLeft">
            <StackPanel Spacing="4" Margin="4">
                <Button Theme="{DynamicResource AppBarButtonTheme}"
                        Content="Rename"
                        Command="{Binding RenameCommand}" />
                <Button Theme="{DynamicResource AppBarButtonTheme}"
                        Content="Duplicate"
                        Command="{Binding DuplicateCommand}" />
                <Separator />
                <Button Theme="{DynamicResource CloseButtonTheme}"
                        Content="Delete"
                        Command="{Binding DeleteCommand}" />
            </StackPanel>
        </PleasantFlyout>
    </Button.Flyout>
</Button>
```

---

## Enums Reference

| Enum | Values |
|---|---|
| `PleasantTitleBar.Type` | `Classic` (0), `ClassicExtended` (1), `Compact` (2) |
| `PleasantCaptionButtons.Type` | `All`, `CloseAndCollapse`, `CloseAndExpand`, `Close`, `None` |
| `NavigationViewPosition` | `Left`, `Top`, `Bottom` |
| `SplitViewDisplayMode` | `Overlay`, `CompactOverlay`, `Inline`, `CompactInline` |
| `DrawerPosition` | `Left`, `Right`, `Top`, `Bottom` |
| `PathPickerMode` | `OpenFile`, `OpenFolder`, `SaveFile` |
| `PinCodeMode` | `Digit`, `Letter`, `LetterOrDigit` |
| `CommandBarClosedDisplayMode` | `Compact`, `Minimal`, `Hidden` |
| `CommandBarDefaultLabelPosition` | `Bottom`, `Right`, `Collapsed` |
| `CommandBarItemsAlignment` | `Left`, `Right` |
| `CommandBarOverflowButtonVisibility` | `Auto`, `Visible`, `Collapsed` |
| `PopConfirmTriggerMode` | `Click`, `Focus` |
| `PleasantDialogResult` | `None`, `OK`, `Cancel`, `Yes`, `No`, `Close`, `Retry` |
| `MessageBoxStyle` | `Default`, `Danger` |
| `PleasantTabView.ViewMarginType` | `None` (0), `Little` (1), `Extended` (2) |
| `PleasantBorder.BackgroundLeveling` | `Level1`, `Level2`, `Level3`, `Level4` |
| `PleasantVersionType` | `Stable`, `BugFix`, `Alpha`, `Beta`, `ReleaseCandidate`, `Canary` |
| `SnackbarCloseReason` | `Timeout`, `UserDismiss`, `CloseButton`, `Programmatic` |
| `NotificationType` | `Information`, `Success`, `Warning`, `Error` |

---

## Button Themes Quick Reference

Apply via `Theme="{DynamicResource ...}"` on any `Button` or `ToggleButton`.

| Key | Target | Description |
|---|---|---|
| `AccentButtonTheme` | `Button` | Gradient accent background, semibold text |
| `DangerButtonTheme` | `Button` | Red background, semibold text |
| `AppBarButtonTheme` | `Button` | Transparent background, rounded, for toolbars |
| `CloseButtonTheme` | `Button` | Transparent → red on hover, for close actions |
| `AppBarToggleButtonTheme` | `ToggleButton` | Transparent background, fills on checked/hover |

```xml
<Button Theme="{DynamicResource AccentButtonTheme}" Content="Save" />
<Button Theme="{DynamicResource DangerButtonTheme}" Content="Delete" />
<Button Theme="{DynamicResource AppBarButtonTheme}">
    <PathIcon Data="{DynamicResource SearchRegular}" Width="14" Height="14" />
</Button>
<ToggleButton Theme="{DynamicResource AppBarToggleButtonTheme}" Content="Pin" />
```
