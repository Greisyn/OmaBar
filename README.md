# OmaBar — full-screen-friendly Omarchy bar

Full `bar` plugin fork of Omarchy's built-in `omarchy.bar`, cloned via
`omarchy plugin clone omarchy.bar` into `grei-eon-studio.bar`.

The only behavior change vs stock: **windows cover the bar**.

- `WlrLayershell.layer: WlrLayer.Bottom` (stock: `Top`)
- `exclusionMode: ExclusionMode.Ignore` (stock: `Auto` unless bar-hidden)

Result: tiled / maximized windows use the full screen height and render
above the bar. Dwell / focus behavior is otherwise stock Omarchy.

Plugin id: `grei-eon-studio.bar` — matches `bar.id` in
`~/.config/omarchy/shell.json`, so enabling this repo replaces the
built-in bar with no layout migration.

## Install

```bash
omarchy plugin add https://github.com/Greisyn/OmaBar.git --enable --yes
omarchy restart shell
```

Or by hand:

1. Copy this repo to `~/.config/omarchy/plugins/grei-eon-studio.bar/` with
   `manifest.json` at the root.
2. `omarchy-shell shell rescanPlugins`
3. Set `"bar": { "id": "grei-eon-studio.bar", ... }` in
   `~/.config/omarchy/shell.json` (already set if you cloned).

## Files

- `manifest.json` — `id: grei-eon-studio.bar`, `kinds: ["bar"]`, `bar: Bar.qml`
- `Bar.qml` — Omarchy bar engine + 2-line fork (Bottom + Ignore, marked `// Custom:`)
- `BarModel.js` — layout normalize helpers
- `widgets/` — first-party widgets (Workspaces, Tray, Indicators, etc.)
- `indicators/` — Dnd, Reminder, NightLight, etc.
- `LICENSE` — MIT

## Custom diff vs `omarchy.bar` (stock `/usr/share/omarchy/shell/plugins/bar`)

```diff
-    exclusionMode: root.barHidden ? ExclusionMode.Ignore : ExclusionMode.Auto
+    // Custom: let tiled windows use the full screen and render above the bar,
+    // so windows cover the bar. (Default was Auto + Top layer.)
+    exclusionMode: ExclusionMode.Ignore
-    WlrLayershell.layer: WlrLayer.Top
+    // Custom: Bottom layer renders below normal windows, so windows cover the bar.
+    WlrLayershell.layer: WlrLayer.Bottom
```

To revert to stock behavior, restore those two lines or run
`omarchy refresh shell`.

## Validate

```bash
omarchy plugin validate ~/.config/omarchy/plugins/grei-eon-studio.bar
omarchy restart shell
```

## Uninstall / revert

```bash
omarchy plugin remove grei-eon-studio.bar
omarchy refresh shell   # restores stock omarchy.bar
```

## License

MIT — see `LICENSE`.
