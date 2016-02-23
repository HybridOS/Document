# How to Run the System

## QEMU-KVM method
### run ISO img
```
${QEMU_PATH}/qemu-system-x86_64 \
        -enable-kvm \
	-m 1024 \
	-serial stdio \
	-cdrom  ${ANDROID_IMAGE_PATH}/android_x86_64.iso
```