# fatdeck
Enables vfat (fat32) and exfat file systems on the steam deck.

## Features
Format sd cards as exfat
- Automatically given label "SDeck"

Automatically picks up sd cards already formatted to exfat or fat32 (good if you use the [official microsd card formatter](https://www.sdcard.org/downloads/formatter/) on a pc)

Have not tested fat32 since nobody should be using it anyways.

This script may or may not get updated as the steam deck gets updated. Should work regardless. Hopefully this hack becomes obsolete.

I straight up copied the btrfdeck downloader, but it should work regardless.

# Guide:
## 1. Download this project to your home directory of the deck.
    cd ~
    git clone https://github.com/Trevo525/btrfdeck
    cd btrfdeck
## 2. Setup `deck` user with a password so that you can run things that need sudo.
    passwd deck
## 3. Backup the old script.
    mkdir ./backup/
    cp /usr/lib/hwsupport/sdcard-mount.sh ./backup/sdcard-mount.sh
    cp /usr/lib/hwsupport/format-sdcard.sh ./backup/format-sdcard.sh
## 4. Change the filesystem to read-write so that you can make changes to the protected files. 
#### **NOTE: this makes your entire "SteamOS" partition read-write. Basically, removing all barriers keeping you from breaking your system. You've been warned**.
    sudo steamos-readonly disable
## 5. Replace with modified configs.
    sudo rm /usr/lib/hwsupport/sdcard-mount.sh && sudo rm /usr/lib/hwsupport/format-sdcard.sh
    sudo cp ./modified/sdcard-mount.sh /usr/lib/hwsupport/sdcard-mount.sh
    sudo cp ./modified/format-sdcard.sh /usr/lib/hwsupport/format-sdcard.sh
    sudo chmod 755 /usr/lib/hwsupport/sdcard-mount.sh /usr/lib/hwsupport/format-sdcard.sh
## 6. Change the filesystem back to read-only so that you can no longer make changes to the protected files.
    sudo steamos-readonly enable
## 7. Remove the password from the deck user. (Skip this and next step to use btrfdeck_post_update.sh to re-run steps 3-6 after an OS update)
    sudo passwd -d deck
## 8. Format the SD card.
    Press the "Format SD Card" button in the Steam Deck UI.

At this point it should automatically mount the drive because both files were changed. If it does not here is how I would troubleshoot:
* Try ejecting the microSD card and reinserting. (Sometimes it'll take a few seconds to mount a large drive.)
* Restart Steam.
* Go to the desktop and check if it is mounted.
* If that doesn't work, try to format in the SteamUI again. I need to figure out where this logs to so I can add that here.
* You could try formatting manually, either through KDE Partition Manager or though the terminal.
* Lastly, if all else fails, you can follow the [Undo the changes](#undo-the-changes) section below to make it as if you never made a change.

# After a SteamOS update

## Don't remove password and run the btrfdec_post_update.sh script by right-clicking it and selecting "Run in Konsole".

# Undo the changes: 
## 1. Setup `deck` user with a password.
    passwd deck
## 2. Change the filesystem to read-write.  
    sudo steamos-readonly disable
## 3. restore from the backup.
    sudo rm /usr/lib/hwsupport/sdcard-mount.sh && sudo rm /usr/lib/hwsupport/format-sdcard.sh
    cp ./backup/sdcard-mount.sh /usr/lib/hwsupport/sdcard-mount.sh
    cp ./backup/format-sdcard.sh /usr/lib/hwsupport/format-sdcard.sh
## 4. Change the filesystem back to read-only.
    sudo steamos-readonly enable
## 5. Remove the password from the deck user.
    sudo passwd -d deck

# btrf deck credits

Thanks to [u/ClinicallyInPain](https://www.reddit.com/user/ClinicallyInPain/) on reddit for compiling some of the resources and to [u/Hanntac](https://www.reddit.com/user/Hanntac/) and [u/leo_vir](https://www.reddit.com/user/leo_vir/) for their contributions! Both of the last two credits helped me through PM so I am incredibly thankful for the both of them. Also to [u/sporkyuncle](https://www.reddit.com/user/sporkyuncle/) for helping me make it more user-friendly.

The following Github users:
* [taotien](https://github.com/taotien)
* [ktully](https://github.com/ktully)

Sources:
* https://www.reddit.com/r/SteamDeck/comments/taixhw/new_user_questions_current_user_recommendations/
* https://docs.kde.org/trunk5/en/partitionmanager/partitionmanager/partitionmanager.pdf
* https://linuxhint.com/btrfs-filesystem-mount-options/
* https://www.reddit.com/r/SteamDeck/comments/t76wh6/compressing_storage_with_btrfs/
* https://www.reddit.com/r/SteamDeck/comments/t79fqj/comment/hziqcf5/
* https://serverfault.com/a/767079
* https://github.com/Trevo525/btrfdeck/issues/1
