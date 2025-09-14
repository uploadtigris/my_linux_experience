# GIF to animated wallpaper

Downloaded the [Plasma smart video wallpaper reborn](https://github.com/luisbocanegra/plasma-smart-video-wallpaper-reborn)

Converted the .gif file into a .mp4 video with [ffmpeg](https://github.com/FFmpeg/FFmpeg)

```
ffmpeg -i /home/tigris/Pictures/Wallpaper/rainyJapan.gif -movflags faststart -pix_fmt yuv420p -vf scale=3840:2160 ~/Videos/wallpaper.mp4
```
