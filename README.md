# bootable-usb
Instructions for creating a bootable USB

## mac OS X
*Tested on El Capitan 10.11.6*

### Format USB key

* Open `Disk Utility`
* Select USB Key on left column
* Select `Erase` button on top row of buttons
* Confirm default options: Format `OS X Extended (Journaled)` and Scheme `GUID Partition Map`

### Convert iso to img

```
hdiutil convert -format UDRW <path to .iso> -o <path to .img>
```

### Remove .dmg from output

Mac OS X helpfully adds .dmg to the output of `hdiutil`.  Remove it

```
mv <path to .img.dmg> <path to .img>
```

### Find USB with diskutil

`diskutil list`

Look for disk with size matching USB key.  **Don't pick your Mac hard drive or other attached storage**

```
/dev/disk3 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *15.9 GB    disk3
   1:                        EFI EFI                     209.7 MB   disk3s1
   2:                  Apple_HFS Untitled                15.6 GB    disk3s2
```

Use `/dev/diskN` for later commands where N is the disk number found above

### Unmount usb

```
diskutil unmountDisk /dev/diskN
```

### Move .img to USB key

*Important:  Make sure the output path is rdiskN not diskN.  Makes transfers go faster*

*May need to use `bs=1M` if you receive error `dd: Invalid number '1m'`*

```
sudo dd if=<path to .img> of=/dev/rdiskN bs=1m
```

When finished, Mac OS X will pop up a dialog stating

`The disk you inserted was not readable by this computer.`

Leave dialog as is and proceed to next step

### Eject USB key

```
diskutil eject /dev/diskN
```

### Close Dialog

* Remove USB key from computer
* Click `Ignore` on dialog