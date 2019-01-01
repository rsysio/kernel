# Build my linux kernel

## Docs
[Guide](https://kernelnewbies.org/KernelBuild)
[Guide 2](https://www.cyberciti.biz/tips/compiling-linux-kernel-26.html)

## Build

### install build tools
```
apt install ncurses-devel bison flex elfutils-libelf-devel openssl-devel
```

### Download linux
```
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.20.tar.xz
tar xf linux-4.20.tar.xz
sudo mv linux-4.20 /usr/src/
cd /usr/scr/linux-4.20/
```

### get current config
```
cp -v /boot/config-$(uname -r) .config
```

### config
```
make menuconfig
```

My changes:
- Processor type and features --->
  - removed AMD stuff
  - Processor family
  - Kernel Live Patching
  - Linux guest support

- Device Drivers --->
  - Input device support --->
    - Tablets
    - Touchscreens
    - X86 Platform Specific Device Drivers --->

### build
```
make -j 4
```

### install modules
```
sudo make INSTALL_MOD_STRIP=1 modules_install
```
[link](https://unix.stackexchange.com/questions/270390/how-to-reduce-the-size-of-the-initrd-when-compiling-your-kernel/270394)

copy `*.ko` and other files to `/lib/modules/4.20.0/`

### install kernel
```
sudo make install
```

runs:
```
sh ./arch/x86/boot/install.sh 4.20.0 arch/x86/boot/bzImage \
        System.map "/boot"
```

- copy `arch/x86/boot/bzImage` to `/boot/vmlinuz-4.20.0`
- copy `System.map` to `/boot/System.map-4.20.0`
- run `update-initramfs` and `update-grub2`


### install headers
```
sudo make headers_install
```
