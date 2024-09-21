# Instructions

Creating a developer environment on the device to interact with the DRM/KMS device is not a trivial process. There are no package repositories, so compiling & installing extra software can be diffucilt (especially as it's aarch64). However, thankfully, we are able to install a minmial debian environment to get around this.


* Enable developer mode, the option can be found in the "General -> Update" section. Note that this will factory reset the device.
* Get access to the device, connect the RM via USB and run the following command: 
```ssh root@10.11.99.1```
* Password is found in "General -> Help (About) -> Copyright & License"
* Download repository
```
wget https://github.com/Jayy001/rmpp-kms-cube/archive/refs/heads/main.zip && unzip main.zip
cd rmpp-kms-cube-main
```
* Enable modification of filesystem
```mount -o remount,rw /```
* Copy libs for perl
```cp libs/* /lib/```
* Make binaries executable
```
chmod +x bins/ar
chmod +x bins/wget
chmod +x repos/localperl/bin/perl
```
* Create debian-chroot folder
```
mkdir /home/root/.local/bin
```
* Add binaries to PATH
```export PATH=$PATH:$(pwd)/bins:$(pwd)/repos/localperl/bin:/home/root/.local/bin```
* Overwrite busybox wget
```cp wget /usr/bin/wget```
* Install debian-chroot
```
sh -c "$(wget https://raw.githubusercontent.com/Eeems-Org/remarkable-debian-chroot/master/install.sh -O-)"
```
* Inside debian-chroot run to download kmscube prerequisites 
```
apt install meson gcc cmake make libdrm-dev libegl1-mesa-dev libgles2-mesa-dev libgbm-dev pkg-config libpng-dev
git clone https://gitlab.freedesktop.org/mesa/kmscube.git
cd kmscube
```
* Build & Install
```
meson build
ninja -C build install
```
* Run
```
kmscube --gears --mode=rgba --nonblocking --surfaceless --format=RG16
```


