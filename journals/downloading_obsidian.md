Issue: I need to understand how to download an AppImage file that will not read in the software store for Fedora.
Date: 7/29/25

First, we try the download method from the [Obsidian website](https://help.obsidian.md/install).

The problem is that when I run ```./Obsidian-1.8.10.appimage```
I get the repeating error:
```bash
[762807:0729/132957.320694:ERROR:gl_surface_presentation_helper.cc(260)] GetVSyncParametersIfAvailable() failed for 1 times!
[762807:0729/133001.324137:ERROR:gl_surface_presentation_helper.cc(260)] GetVSyncParametersIfAvailable() failed for 2 times!
```

What I found online (honestly just Claude AI), I was provided the following steps:
1. move it to a better location
```
# Move to a standard location
sudo mv Obsidian-1.8.10.appimage /opt/obsidian.appimage
```
2. Create a desktop entry for easy launching:
```
# Create desktop entry
cat > ~/.local/share/applications/obsidian.desktop << EOF
[Desktop Entry]
Name=Obsidian
Exec=/opt/obsidian.appimage
Icon=obsidian
Type=Application
Categories=Office;TextEditor;
Comment=A powerful knowledge base on top of a local folder of plain text Markdown files
EOF
```
The Desktop Entry option allows for:
- GUI integration - appears in your applications menu, can be pinned to dock/taskbar
- System-wide feel - behaves like any other installed application
- File associations - can be set as default app for markdown files
- Search integration - shows up when you search for "Obsidian" in your desktop environment
- User-friendly - non-technical users can find and launch it easily
  
3. Add to the ~/.bashrc file
```
# Add alias to ~/.bashrc (for terminal)
echo "alias obsidian='/opt/obsidian.appimage'" >> ~/.bashrc
source ~/.bashrc
```
Adding this to the ~/.bashrc file allows for:
- Terminal convenience - quick to launch if you live in the terminal
- Scriptable - easy to incorporate into shell scripts
- Minimal - doesn't clutter the applications menu
