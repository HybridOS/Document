Before beginning,you should check your hardware,this is needed:
* Intel vt-d support  
In my case,GPU:intel+AMD:  
Intel Corporation Broadwell-U Integrated Graphics (rev 09)  
Advanced Micro Devices, Inc. [AMD/ATI] Opal XT [Radeon R7 M265] (rev ff)  
Then you can follow next steps:  
#1. Modifying kernel config:  

    make menuconfig  
    set "Bus options (PCI etc.)" -> "PCI Stub driver" to "*"  
    set "Device Drivers" -> "IOMMU Hardware Support" to "*"  
    exit/save  

#2. build kernel:  

    make  
    make modules_install  
    make install  

#3. reboot and verify that your system has IOMMU support

     command into the Terminal   "dmesg | grep -e DMAR -e IOMMU"

     ...
     IOMMU enabled
     ...

    If you get no output you'll need to fix this before moving on. Check if your hardware supports VT-d and check that it has been enabled in BIOS.

NOTE: On Intel platforms it is necessary to add intel_iommu=on on the kernel commandline (add in to GRUB_CMDLINE_LINUX_DEFAULT in /etc/default/grub and run update-grub).   

#4. unbind device from host kernel driver (example PCI device 00:02.0)  

    Load the PCI Stub Driver if it is compiled as a module  

       modprobe pci_stub  

    lspci -n  
    locate the entry for device 01:00.0 and note down the vendor & device ID 8086:1616  

       ...  
       00:02.0 8086:1616    
       ...  

    echo "8086 1616" > /sys/bus/pci/drivers/pci-stub/new_id  
    echo 0000:00:02.0 > /sys/bus/pci/devices/0000:00:02.0/driver/unbind  
    echo 0000:00:02.0 > /sys/bus/pci/drivers/pci-stub/bind  

#5. load KVM modules:(this step is not necessary)  

    modprobe kvm  
    modprobe kvm-intel  

#6. assign device:  

    /usr/local/bin/qemu-system-x86_64 -m 512 -boot c -net none -cdrom /home/bjz/android4.4-rc4.iso -device pci-assign,host=00:02.0