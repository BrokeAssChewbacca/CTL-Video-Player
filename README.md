## Hardware:
- Raspberry Pi 4
  - 8 GB Ram (not upgradable)
  - 32 GB Micro SD (storage)
  - Video ports available on the PI
    - 2x Micro HDMI
  - USB C power adapter
    - Included in the kit was a power switch

## Software:
- LibreELEC (official) 9.2.8 (RPi4.arm)
  - Linux distribution, often used as a media center for Raspberry PI’s. 
  - Uses Kodi as it’s media player
  - Lightweight, just enough OS for Kodi to run

## Technical Documentation:
### Installing LibreELEC
- Visit [the LibreELEC downloads page](https://libreelec.tv/downloads_new/), select Raspberry Pi 4, and download the latest stable version
- Connect the MicroSD card to your computer
- Format MicroSD card as ExFAT using MBR
        
```
# Get drive name, will likely be /dev/disk2s2 as long as no other external drives are connected
diskutil list
# Unmount the disk
sudo diskutil unmountDisk /dev/<disk-name>
# Open Disk Utility and format it with ExFAT using MBR
```

- Uncompress the disk image downloaded earlier and write it to the drive
```
# Assuming your in the folder with the downloaded disk image
gunzip <disk_image_name>
sudo dd if=<disk_image_name> of=/dev/<disk_name> bs=1m
```
- Eject the MicroSD card from the computer and put it in the Pi and power it on

### Video Files Location
Video files should be placed at `/storage/.kodi/userdata/playlists/video/`

### Enable Autoplay Of A Video On Loop
Add the following to `/storage.kodi/userdata/autoexec.py`
```
import xbmc
xbmc.executebuiltin("PlayMedia(/storage/.kodi/userdata/playlists/video/<file-name>)")
xbmc.executebuiltin("PlayerControl(repeatall)")
```

### Testing Autoplay Without Rebooting
To test the `autoexec.py` file without rebooting you can run the following command:
`kodi-send -a "RunScript(/storage/.kodi/userdata/autoexec.py)"`

### Disable Dialog Seek Bar When Video Loops
You need to modify the XML file in charge of the dialog seek bar for the skin you are using (Estuary in this case).
First we need to copy the entire directory for the skin to another location so that we can edit it:
`cp -r /usr/share/kodi/addons/skin.estuary/ /storage/.kodi/addons/`

Now we'll edit `/storage/.kodi/addons/skin.estuary/xml/DialogSeekBar.xml` and remove `Player.DisplayAfterSeek` from [this line.](https://github.com/xbmc/xbmc/blob/master/addons/skin.estuary/xml/DialogSeekBar.xml#L3)

### Testing
Simply reboot the system and verify that the video plays on startup and that when it loops the dialog seek bar does not display.
