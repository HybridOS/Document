# Download, Install and Run Builds


## Preparing

- An android_x86_64.img image which can boot into UEFI.
- A PC whic can Boot in UEFI mode
- A larger than 1GB USB disk

### How to get an openthos image

If you want to try our system, you can download our images from baidu cloud disk:

 ```
http://pan.baidu.com/s/1i4vSEf7 PW:z8wu
 ```

Then you can watch [[Installation guide|Installation and Running Builds]] to run our images in your PC, or [[try it by virtualbox|How to boot with virtualbox]]
## Creating bootable USB disk

### Howto on Windows

Open android_x86_64.img with 7zip，and copy all files to your USB disk。

### Howto on linux

You may create a bootable USB disk by

 ```
sudo umount your_usb_device(with partition number e.g. /dev/sdc1)
sudo dd if=android_x86_64.img of=your_usb_device（no partition number e.g. /dev/sdc)
 ```

## Installation

1. Bootint your USB disk in UEFI

  1. Enter your PC's setup，change boot tab to open uefi mode and reboot.
  2. Enter your PC's boot menu and select your USB disk to boot into UEFI grub.
  
2. Using installer

  After entered UEFI grub, you will see 4 selections:run system live, run system live(DEBUG Mode), installer and boot into windows.Select installer to start installation.
  
3. Create partition（if you have a partiton to install, please go to 4）

  1. Select create/modify partitions.
  2. Select a disk to create new partition.
  3. Use arrow keys select usable space and select "new".
  4. Input the partition information to set a new partition.
  5. Use arrow keys select "write" to save your changes.
  
4. Install

  1. Select the partition you want to install the system into.
  2. Format the partition you selected to EXT4 fs.
  3. Select yes to install a read/write system dir.
  4. Wait for installation complete and reboot.
  
 
5. Boot system

  Boot with rEFInd:If you have a rEFInd in your efi parition, the installer will add a menuentry for Android-x86 and you can boot with it.We suggest to use rEFInd as your bootmanager. For more information please read [rEFInd's home page](http://www.rodsbooks.com/refind/)
  Boot with grub2：Enter your PC's setup and set all boot selection to disable. After you do that and reboot, you will enter grub2 and you can select a menuentry(openthos,openthos debug,windows) to boot the system you want.
  
  **Reports of successes or failures on other distributions are welcome.**