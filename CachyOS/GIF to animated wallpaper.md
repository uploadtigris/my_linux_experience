# GIF to animated wallpaper

```
sudo pacman -S xwinwrap mpv imagemagick
```

You can use xwinwrap to place the GIF on the background, and mpv will be used to play it.

```
xwinwrap -ov -g 3840x2160+0+0 -- mpv --no-audio --loop --wid=%WID /path/to/your/gif.gif
```

