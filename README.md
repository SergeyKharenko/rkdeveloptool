# Terran rkdeveloptool

**rkdeveloptool** is Rockchip's official USB development tool, supporting firmware flashing, partition management, and more.  

> **This version migrates the original autotools build system to CMake** for easier cross-platform building and maintenance.  
> In this release, we proudly introduce the **Terran** concept into the project name — **because the original tool seemed designed for an alien species with infinite patience**.  
> Now, it’s **finally more intuitive and actually usable by HUMANS**.  
> Furthermore, the backend APIs have been refactored and packaged into a static library named **`rkutils`**, enabling easier secondary development and integration into other projects.

---

## 📦 Dependencies

### Required
- CMake >= 3.20
- C++ compiler (GCC / Clang)
- libusb-1.0 development package
- pkg-config
- Ninja build system

### Installing Dependencies

#### Debian / Ubuntu
```shell
sudo apt update
sudo apt install build-essential cmake ninja-build pkg-config libusb-1.0-0-dev 
```

#### RHEL / CentOS / Rocky / AlmaLinux
```shell
# Enable EPEL repository
sudo yum install epel-release

# Install dependencies
sudo yum install cmake ninja-build gcc-c++ pkgconfig libusb1-devel
```

#### Fedora
```shell
sudo dnf install cmake ninja-build gcc-c++ pkgconf-pkg-config libusb1-devel
```

---

## 🔧 Build

```shell
# 1. Get source code
git clone https://github.com/SergeyKharenko/rkdeveloptool.git
cd rkdeveloptool

# 2. Create build directory
mkdir build && cd build

# 3. Run CMake with Ninja (custom install prefix is optional)
cmake -G Ninja .. \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/your/custom/path

# 4. Build
ninja

# 5. Install (optional)
# If you set a custom install prefix, it will be installed there instead of /usr/local
sudo ninja install

# 6. (Recommended) Install udev rules for Rockchip devices
# Simply copy it to the udev rules directory
sudo cp 99-rk-rockusb.rules /etc/udev/rules.d/
# Reload and apply the rules
sudo udevadm control --reload-rules
sudo udevadm trigger
# Reconnect your Rockchip device, and rkdeveloptool should now work without needing `sudo`.
```

---

## 🚀 Usage

After building, you can find the `rkdeveloptool` binary in the `build` directory:

```shell
./rkdeveloptool --help
```

Examples:
```shell
# 1. List all connected Rockchip devices
rkdeveloptool ld

# 2. Upload Miniloader (required before most operations)
rkdeveloptool db loader.bin

# 3. Check device chip information
rkdeveloptool rci

# 4. Read from a device (example: read 1MB starting at 0x0)
rkdeveloptool rl 0x0 0x100000 read_dump.bin

# 5. Write to a device (example: write an image to offset 0x0)
rkdeveloptool wl 0x0 image.bin

# 6. Upgrade firmware from an update.img
rkdeveloptool uf update.img

# 7. Erase flash
rkdeveloptool ef

# 8. Reboot the device
rkdeveloptool rd
```

---

## 📜 License

GPLv2 License. See [LICENSE](license.txt) for details.
