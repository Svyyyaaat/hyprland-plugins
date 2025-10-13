# hyprwinwrap

A fork of hyprwinwrap which allows to toggle interactivity of the wallpaper app using "hyprctl dispatch hyprwinwrap_toggle"

# Install

## Install with `hyprpm`

To install this plugin, from the command line run:
```bash
hyprpm update
```
Then add this repository:
```bash
hyprpm add https://github.com/Svyyyaaat/hyprland-plugins
```
then enable the plugin with
```bash
hyprpm enable hyprwinwrap
```

See [the plugins wiki](https://wiki.hyprland.org/Plugins/Using-Plugins/#installing--using-plugins) and `hyprpm -h` for more details.

## Install on Nix

To use this plugin, it's recommended that you are already using the
[Hyprland flake](https://github.com/hyprwm/Hyprland).
First, add this flake to your inputs:

```nix
inputs = {
  # ...
  hyprland.url = "github:hyprwm/Hyprland";
  hyprwinwrap = {
    url = "https://github.com/Svyyyaaat/hyprland-plugins";
    inputs.hyprland.follows = "hyprland";
  };

  # ...
};
```

The `inputs.hyprland.follows` guarantees the plugin will always be built using
your locked Hyprland version, thus you will never get version mismatches that
lead to errors.

After that's done, you can use the plugin with the Home Manager module like
this:

```nix
{inputs, pkgs, ...}: {
  wayland.windowManager.hyprland = {
    enable = true;
    # ...
    plugins = [
      inputs.hyprwinwrap.packages.${pkgs.system}.hyprwinwrap
    ];
  };
}
```

If you don't use Home Manager:

```nix
{ lib, pkgs, inputs, ... }:
{
  environment.sessionVariables = {
    HYPR_PLUGIN_DIR = "${inputs.hyprwinwrap.packages.${pkgs.system}.hyprwinwrap}";
  };
}
```

And in `hyprland.conf`

```hyprlang
# load the plugin
exec-once = hyprctl plugin load "$HYPR_PLUGIN_DIR/lib/libhyprwinwrap.so"
```

# Configuration

See the [hyprwinwrap README](hyprwinwrap/README.md) for configuration options.

To toggle interactivity:
```bash
hyprctl dispatch hyprwinwrap_toggle" 
```
Set "new_optimizations" to false in hyprland.conf blur settings if you want tiled windows to render the app instead of your wallpaper.

If you are using waybar and it gets covered by the app, set "layer" to "top".
