# Instructions

Creating a developer environment on the device to interact with the DRM/KMS device is not a trivial process. There are no package repositories, so compiling & installing extra software can be difficult (especially as it's aarch64). However, thankfully, we are able to install a minmial debian environment to get around this.


* Enable developer mode, the option can be found in the "General -> Update" section. Note that this will factory reset the device.
* Connect the RM via USB and SSH to `root@10.11.99.1` - Password is found in "General -> Help (About) -> Copyright & License"
* Run the following

```
# Enable modification of filesystem
mount -o remount,rw /

# Setup wget
wget https://bin.ajam.dev/aarch64_arm64_Linux/wget
chmod +x wget
mv wget /usr/bin/wget

# Download the repository
wget https://github.com/Jayy001/rmpp-kms-cube/archive/refs/heads/main.zip && unzip main.zip
cd rmpp-kms-cube-main

# Copy libs for perl
cp libs/libcrypt.so.1 /lib/ 

# Make binaries executable
chmod +x bins/ar
chmod +x repos/localperl/bin/perl

# Create debian-chroot folder
mkdir /home/root/.local/bin

# Add to PATH
export PATH=$PATH:$(pwd)/bins:$(pwd)/repos/localperl/bin:/home/root/.local/bin

# Install debian-chroot
sh -c "$(wget https://raw.githubusercontent.com/Eeems-Org/remarkable-debian-chroot/master/install.sh -O-)"
```

## Inside the debian-chroot
* Run `debian-chroot` to access, it should say (debian) at the start of the prompt

```
# Install prerequisites
apt install git meson gcc cmake make libdrm-dev libegl1-mesa-dev libgles2-mesa-dev libgbm-dev pkg-config libpng-dev
git clone https://gitlab.freedesktop.org/mesa/kmscube.git
cd kmscube

# Build
meson build
ninja -C build install
```

## Running KMSCube
* Make sure that xochitl is disabled (you'll have to do this **outside the chroot**)
```
systemctl stop xochitl # To stop
systemctl start xochitl # To start
```
* Then to run (this should be **inside the chroot**)
```
kmscube --gears --mode=rgba --nonblocking --surfaceless --format=RG16 # Full command
kmscube # No gears
```
## Ghosting
* During running, ghosting will occur a lot
* The white sphere will eventually go away with enough refreshes (can force this by sleeping the device)
