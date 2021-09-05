## How to upgrade this kernel on Armbian TV arm64 (AML s9xx)

1. Clone this repo (or the one from [150balbes](https://github.com/150balbes))

2. Build the kernel with these parameters: 
```
make -j$(nproc) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image dtbs
```

3. rsync source to destination arm64 device.
```
rsync -av -e ssh --progress linux-arm root@<s9xx_ssh_ip>:/root/
```

4. SSH on s9xx device and execute these commands as root:
```
make modules_install
cp /boot/zImage /boot/zImage.bak
cp ./arch/arm64/boot/Image /boot/zImage
cp /boot/uInitrd /boot/uInitrd.bak
update-initramfs -c -k <linux_kernel_version>
# check /lib/modules/ folder to get the linux_kernel_version, it's the one that's not `5.9.0-arm-64`
```

5. Hard shutdown and power on.
```
shutdown -h now
# Then power back on the device
```

Credits: \
@150balbes - For the main work \
[loverpi's wiki](http://wiki.loverpi.com/faq:sbc:libre-aml-s805x-install-newly-compiled-kernel) - For the post kernel build steps