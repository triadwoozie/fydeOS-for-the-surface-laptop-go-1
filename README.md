# Installing FydeOS on Surface Laptop Go (1st Gen)

> **Prerequisite Knowledge**: Basic Linux terminal skills required (e.g., `cd` into directories).

## Introduction
FydeOS is a ChromeOS-based operating system that brings a lightweight, ChromeOS-like experience to any compatible device. With FydeOS, you can install and run ChromeOS on hardware that doesn’t officially support it. This guide will walk you through installing FydeOS on the Surface Laptop Go.

---

## Requirements
- **Surface Laptop Go (1st Gen)**
- **USB Flash Drive** (16GB+ recommended)
- **OpenFyde Recovery** ([Latest Release](https://github.com/openFyde/overlay-amd64-openfyde/releases))
- **Brunch Framework** ([Latest Brunch Release](https://github.com/sebanc/brunch/releases))
- **Live Linux Distro USB**: Recommended: [Lubuntu](https://lubuntu.me/)
- **Linux Packages**: `pv`, `tar`, `unzip`, `cgpt`
- **Root Access**

> **Note**: This process will erase all data on the target disk/USB. Backup any essential files beforehand.

---

## Step 1: Create a Bootable USB

1. **Windows**: Use [Rufus](https://rufus.ie) to flash the Lubuntu ISO to a USB drive.
2. **Linux/MacOS**: Use [Etcher](https://www.balena.io/etcher/).
3. **Boot into the ISO**:
   - Restart and press common boot keys (usually `F12`, `Esc`, or `Del`).
   - Select the USB with Lubuntu and choose “Try Lubuntu.”

---

## Step 2: Download Required Files
1. In Lubuntu, connect to Wi-Fi and open Firefox.
2. Download:
   - **OpenFyde Recovery** from the link above.
   - **Brunch tar.gz** from the [Brunch GitHub release page](https://github.com/sebanc/brunch/releases).
   
3. **Rename Files for Ease**:
   - Rename the downloaded Brunch file to `brunch.tar.gz`.
   - Rename the OpenFyde recovery file to `openfyde.bin.zip`.

---

## Step 3: Prepare the Terminal
1. Open the terminal (`Ctrl + Alt + T`).
2. Install required packages:
   ```bash
   sudo apt update && sudo apt -y install pv cgpt tar unzip
   ```
   - **Note**: Add the universe repo if needed:
     ```bash
     sudo add-apt-repository universe
     ```
3. `cd` to your downloads directory:
   ```bash
   cd ~/Downloads
   ```
4. Extract Brunch:
   ```bash
   tar zxvf brunch.tar.gz
   ```
5. Extract OpenFyde:
   ```bash
   unzip openfyde.bin.zip
   ```

---

## Step 4: Identify Target Disk
1. Run `lsblk -e7` to list disks and partitions.
2. Confirm your target disk (e.g., `/dev/sdb`); ensure it’s the correct one.

---

## Step 5: Install OpenFyde
1. Run the installation command:
   ```bash
   sudo bash chromeos-install.sh -src openfyde.bin -dst /dev/disk
   ```
   - Replace `disk` with your actual target disk name, such as `sdb` or `nvme0n1`.
2. Confirm by typing `yes` when prompted.

> **Note**: Ignore any GPT header warnings.

---

## Step 6: Reboot and Configure
1. Reboot the system by typing:
   ```bash
   reboot
   ```
2. The first boot may take time. Use the **Brunch Configuration Menu** for setup options if needed.
   
## Step 7: Configure Brunch Settings on Boot

After installation, during the first boot into FydeOS, open the Brunch Configuration Menu. Follow these instructions for a stable setup:

1. **Select Kernel 6.1** on the first page.
2. On the next page, **choose only the following options**:
   - `enable_updates`: This allows ChromeOS updates. **Warning**: updates can render the system unstable if the Brunch framework or kernel is incompatible with the updated ChromeOS version.
   - `pwa`: Enables the Brunch PWA. You can install the official version from [sebanc.github.io/brunch-pwa](https://sebanc.github.io/brunch-pwa) or the alternative at [itesaurabh.github.io/brunch-pwa](https://itesaurabh.github.io/brunch-pwa).
   - `rtl8821cu`: Enables support for RTL8821CU wireless adapters.
   - `ipts_touchscreen`: Supports IPTS touchscreen devices, like the Surface Laptop Go.
   - `acpi_power_button`: Enables the power button functionality, which may fix issues with the power menu display.

> **Note**: Avoid modifying other settings unless you are familiar with them. Each option provides specific compatibility or fixes for hardware not natively supported by FydeOS.
---
Next Steps
You’re now ready to use FydeOS on your Surface Laptop Go. For further configurations, explore kernel settings and frameworks in the OpenFyde (Settings) boot option.
