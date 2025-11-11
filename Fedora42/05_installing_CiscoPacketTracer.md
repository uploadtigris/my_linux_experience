# Installing Cisco Packet Tracer

## Issue: There is no easy software download in the software store on Fedora 42

## Resolution:
1. Grab the .deb file from [this site](https://www.netacad.com/resources/lab-downloads?courseLang=en-US), after logging in with your Cisco credentials
2. Use [follow the steps from this Github Repo](https://github.com/thiagoojack/packettracer-fedora?tab=readme-ov-file)
3. Once that is done, confirm the application will start by typing <code>/opt/pt/packettracer</code> in the terminal
4. To add a corresponding icon, we point to <code>/opt/pt/packettracer</code> via a desktop extension. To do so, enter
   
   <code>gedit ~/.local/share/applications/packettracer.desktop</code>

   Enter this into the gedit window that opens

   <code>[Desktop Entry]
    Version=1.0
    Type=Application
    Terminal=false
    Exec=/opt/pt/packettracer
    Name=Cisco Packet Tracer
    Icon=/opt/pt/art/app.png
    Categories=Education;Network;
    Comment=A network simulation tool
   </code>
