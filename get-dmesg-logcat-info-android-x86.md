In debug mode after the last exit command that cause GUI to start  
press [ALT]+[F1]   
plug an USB flash disk, that will appear usually as /storage/usb1  
and use following commands:

```bash
cd /storage/usb1
dmesg > dmesg_app_that_crashed_YYYY-MM-DD.txt
cp /data/log.txt logcat_app_that_crashed_YYYY-MM-DD.txt
```
