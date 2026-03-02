# 🛡️ Kali Linux Virtual Machine Setup — Cybersecurity Home Lab

![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-M1-000000?style=for-the-badge&logo=apple&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-blue?style=for-the-badge)

> A step-by-step guide to building a cybersecurity home lab by setting up Kali Linux on VirtualBox using a **MacBook Pro M1 (Apple Silicon)**. This is a foundational project for anyone starting their journey in ethical hacking and penetration testing.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Why This Project?](#-why-this-project)
- [⚠️ Important: M1 Chip Considerations](#️-important-m1-chip-considerations)
- [Prerequisites](#-prerequisites)
- [Part 1: Install VirtualBox](#-part-1-install-virtualbox)
- [Part 2: Download Kali Linux](#-part-2-download-kali-linux)
- [Part 3: Create the Virtual Machine](#-part-3-create-the-virtual-machine)
- [Part 4: Install Kali Linux](#-part-4-install-kali-linux)
- [Part 5: Post-Installation Configuration](#-part-5-post-installation-configuration)
- [Troubleshooting](#-troubleshooting)
- [Key Takeaways](#-key-takeaways)
- [Resources](#-resources)

---

## 📌 Project Overview

This project documents the process of setting up a **Kali Linux virtual machine** using **Oracle VirtualBox** on a **MacBook Pro M1 (Apple Silicon)**. The goal is to create an isolated, safe environment for learning cybersecurity tools and techniques without affecting the host system.

| Detail | Info |
|---|---|
| **Host Machine** | MacBook Pro M1 |
| **Host OS** | macOS (Apple Silicon) |
| **RAM** | 8 GB |
| **Storage** | 512 GB SSD |
| **Virtualization Tool** | Oracle VirtualBox 7.2.6 (Apple Silicon) |
| **Guest OS** | Kali Linux 2025.4 ARM64 |
| **RAM Allocated to VM** | 4 GB (4096 MB)|
| **CPUs Allocated to VM** | 4 |
| **Disk Space Allocated** | 25.64 GB |
| **Completion Time** | ~45–60 minutes |

---

## 💡 Why This Project?

Setting up a virtual machine is **step zero** in cybersecurity. Here's why it matters:

- ✅ **Safe environment** — practice hacking tools without risking your main system
- ✅ **Isolated network** — test exploits in a sandboxed space
- ✅ **Snapshot feature** — revert to a clean state at any time
- ✅ **Industry standard** — security professionals use VMs daily for pen testing
- ✅ **Kali Linux** comes pre-loaded with 600+ security tools (Nmap, Metasploit, Wireshark, Burp Suite, etc.)

---

## ⚠️ Important: M1 Chip Considerations

> This is the most critical section for Apple Silicon users

The **Apple M1 chip uses ARM64 architecture**, which is fundamentally different from the x86_64 (Intel) architecture that most VMs are traditionally built on. This has two important implications:

**1. VirtualBox version matters.** Standard VirtualBox does not support Apple Silicon. You need **VirtualBox 7.2.6**, which includes ARM support for Apple Silicon. This version is stable enough for home lab use.

**2. You must use the ARM64 version of Kali Linux.** You cannot run the standard x86_64 Kali ISO on an M1 Mac. You must download the **ARM64** edition specifically.

### Alternative: UTM (Recommended if VirtualBox has issues)
If VirtualBox gives you trouble on M1, **UTM** is a free, Mac-native virtualization app that runs excellently on Apple Silicon with no compatibility issues. This guide covers VirtualBox, but UTM notes are included where relevant.

---

## ✅ Prerequisites

Before starting, make sure you have:

- [x] MacBook Pro with **Apple M1 chip**
- [x] **8 GB RAM** (we'll allocate 4 GB to the VM)
- [x] At least **50 GB of free disk space** on your 512 GB SSD
- [ ] macOS Ventura (13.x) or Sonoma (14.x) — update if needed
- [ ] Internet connection for downloads
- [ ] Admin access to your Mac

### Check Available Disk Space

Open **Terminal** (`Command + Space` → type Terminal) and run:
```bash
df -h /
```
Or go to **Apple Menu** → **System Settings** → **General** → **Storage**.

> 💡 With 512 GB total, aim to keep at least 100 GB free on your Mac. Allocate 30–40 GB for the Kali VM, this leaves you plenty of headroom. The VM in this setup uses 25.64 GB.

---

## 🔧 Part 1: Install VirtualBox

### Step 1.1 — Download VirtualBox for Apple Silicon

1. Go to: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Under **VirtualBox platform packages**, click **"macOS / Apple Silicon hosts"**
   - This downloads `VirtualBox-7.2.6-xxxxxx-macOSArm64.dmg`
3. Also download the **VirtualBox 7.2.6 Extension Pack** from the same page (version must match exactly)

> ⚠️ Make absolutely sure you click **"macOS / Apple Silicon"** — NOT the Intel macOS package. The Intel version will not run on your M1.
<img width="1762" height="439" alt="Screenshot 2026-02-25 at 9 43 17 PM" src="https://github.com/user-attachments/assets/b56e9d8b-39dc-4233-899f-7256a453b18f" />



### Step 1.2 — Install VirtualBox

1. Open the downloaded `.dmg` file
2. Double-click **VirtualBox.pkg** to launch the installer
3. Click **Continue** → **Continue** → **Install**
4. Enter your Mac password when prompted
5. macOS will show a **"System Extension Blocked"** notification — this is expected

### Step 1.3 — Allow the System Extension (Critical macOS Step)

This step is unique to macOS and must not be skipped:

1. Open **System Settings** → **Privacy & Security**
2. Scroll down until you see:
   *"System software from Oracle America, Inc. was blocked from loading"*
3. Click **Allow**
4. Authenticate with your password or Touch ID
5. **Restart your Mac** when prompted

> 🔒 This is safe. VirtualBox requires kernel-level access to emulate virtual hardware. Without this approval, VMs will fail to start.

### Step 1.4 — Install the Extension Pack

1. Open VirtualBox after restarting
2. Go to **VirtualBox** (menu bar) → **Settings** → **Extensions**
3. Click the **+** icon → select your downloaded `.vbox-extpack` file
4. Click **Install** and accept the license

The Extension Pack adds USB 2.0/3.0, RDP display, and webcam passthrough support.
<img width="708" height="593" alt="Screenshot 2026-03-01 at 3 35 58 PM" src="https://github.com/user-attachments/assets/262d6d2c-6ec6-4a3c-8f64-b0e0b38a5c1d" />
<img width="711" height="317" alt="Screenshot 2026-03-01 at 3 36 22 PM" src="https://github.com/user-attachments/assets/674444ba-7b25-438d-b4ec-550ef2b152b9" />

---

## 🐉 Part 2: Download Kali Linux (ARM64 Version)

### Step 2.1 — Download the ARM64 ISO

1. Go to: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
2. Click **"Installer Images"**
3. Find the **architecture selector** and switch to **ARM64**
4. Download: `kali-linux-2025.4-installer-arm64.iso`

> ⚠️ **Do NOT download the amd64 version.** It will not run on Apple Silicon. Confirm the filename contains `arm64` before downloading.

File size is approximately **3–4 GB** — download time depends on your connection.
<img width="931" height="596" alt="Screenshot 2026-02-26 at 11 54 28 PM" src="https://github.com/user-attachments/assets/670886f0-3c80-40cb-b6a8-f3bdcce617e2" />


### Step 2.2 — Verify the Download (Security Best Practice)

Open **Terminal** and run:

```bash
shasum -a 256 ~/Downloads/kali-linux-2025.4-installer-arm64.iso
```

Compare the output hash with the SHA256 value listed on the Kali Linux official download page. They must match exactly. This confirms the file is authentic and wasn't corrupted.
<img width="937" height="595" alt="Screenshot 2026-02-26 at 11 55 29 PM" src="https://github.com/user-attachments/assets/d84a1a09-1a5e-4e76-9dc5-4d6fef633f74" />
<img width="850" height="527" alt="Screenshot 2026-02-26 at 11 53 03 PM" src="https://github.com/user-attachments/assets/0014c6b3-1f3d-428c-b5e6-40dac9937d4c" />


---

## 💻 Part 3: Create the Virtual Machine

### Step 3.1 — Create a New VM in VirtualBox

1. Open **VirtualBox**
2. Click **New** (or **Machine** → **New**)
3. Fill in the following details:

| Field | Value |
|---|---|
| **Name** | `Kali Linux ARM64` |
| **Folder** | Choose a location with enough free space |
| **ISO Image** | Browse → select your `kali-linux-2025.4-installer-arm64.iso` |
| **Type** | Linux |
| **Version** | **Debian(64-bit ARM) or Debian 12 Bookworm (The Latest one)** |

4. Check ✅ **"Skip Unattended Installation"**
5. Click **Next**

> 💡 Choosing the correct **ARM** version type is essential on Apple Silicon.

### Step 3.2 — Allocate Hardware Resources

**Memory (RAM):**

With 8 GB total on your M1 Mac:

| Allocation | Notes |
|---|---|
| 2048 MB (2 GB) | Minimum — functional but slow |
| 3072 MB (3 GB) | Recommended for your setup |
| **4096 MB (4 GB)** | ✅Used in this setup - best performance |

> ⚠️ Never exceed half your total RAM. Keeping 4–5 GB for macOS ensures your host stays responsive.

**Processors:**
- Set to **4 CPUs**
- Your M1 has 8 cores, allocating 4 gives the VM good performance while leaving 4 cores for macOS

Click **Next**.

### Step 3.3 — Create the Virtual Hard Disk

1. Select **Create a Virtual Hard Disk Now**
2. Set size to **25.64 GB** (as configured in this setup)
3. Format: **VirtioSCSI** (selected automatically on ARM, better performance than SATA)
4. Type: **Dynamically Allocated** (only uses space as data fills it)
5. Click **Finish**

### Step 3.4 — Adjust Additional VM Settings

Right-click your VM → **Settings**:

- **Display** → **Screen**: Set Video Memory to **128 MB**, Graphics controller to VMSVGA
- **Network** → **Adapter 1**: Confirm it's set to **NAT** (gives internet access via your Mac)
- **USB**: Enable USB 2.0 or 3.0 (requires Extension Pack)
- **System** → **Motherboard**: EFI ✅ (required for ARM64 Kali)
<img width="757" height="743" alt="Screenshot 2026-02-27 at 12 12 53 AM" src="https://github.com/user-attachments/assets/e925d185-f550-41de-8a14-77432f6a31dd" />

---

## 🚀 Part 4: Install Kali Linux

### Step 4.1 — Start the VM

1. Select your Kali VM → click **Start**
2. A new window opens with the Kali Linux boot menu

### Step 4.2 — Boot Menu

Use arrow keys to select **Graphical Install** → press `Enter`.

> 💡 If the screen is black for more than 2 minutes, see the Troubleshooting section.

### Step 4.3 — Language & Region

1. **Language**: `English` → Continue
2. **Location**: Select your country → Continue
3. **Keyboard**: `American English` (or your layout) → Continue

The installer will load components for 1–2 minutes.

### Step 4.4 — Network Configuration

1. **Hostname**: `kali` → Continue
2. **Domain name**: Leave blank → Continue

### Step 4.5 — Create User Account

1. **Full name**: Your name (e.g., `Kali User`)
2. **Username**: Lowercase username (e.g., `kaliuser`), avoid using `root`
3. **Password**: Set a strong password and confirm it

> 🔒 Good security habits start here, use a strong, unique password even in a practice environment.

### Step 4.6 — Partition the Disk

1. Select **Guided — use entire disk**
2. Select the virtual disk (shows your allocated size)
3. Select **All files in one partition (recommended for new users)**
4. Select **Finish partitioning and write changes to disk**
5. Confirm with **Yes** → Continue

### Step 4.7 — Install the Base System

The system installs now, takes **10–20 minutes** depending on your Mac's speed. ☕

### Step 4.8 — Configure the Package Manager

1. When asked about a network mirror: **Yes**
2. HTTP proxy: Leave **blank** → Continue

The installer configures your software repositories.

### Step 4.9 — Software Selection

Keep the default selections:
- ✅ Kali desktop environment (XFCE — lightweight, perfect for a VM)
- ✅ Top 10 security tools
- ✅ Standard system utilities

Click **Continue** — package installation takes **15–30 minutes**.

### Step 4.10 — Install GRUB Bootloader

1. Select **Yes** to install GRUB
2. Select your virtual disk (`/dev/sda` or `/dev/vda`) → Continue

### Step 4.11 — Finish and Reboot

Click **Continue** to reboot. Your VM restarts and loads the Kali Linux login screen. 🎉

---

## ⚙️ Part 5: Post-Installation Configuration

### Step 5.1 — First Login

Enter the username and password you set during installation.

### Step 5.2 — Install VirtualBox Guest Additions

Before attempting Guest Additions, install the required kernel headers and build tools:

Open **Terminal** inside Kali and run:

```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

Verify headers are installed:
```
dpkg -' | grep linux-headers
```

### Step 5.3 - Attempt VirtualBox Guest Additions (ARM64 Limitation)
> ⚠️**Important**: This step documents a known ARM64 limitation. as I am using "arm" machine, the following method is not applicable.

**Method 1** - Via apt (will fail on ARM64):
```
sudo apt install -y virtualbox-guest-x11
# Result: Unable to locate package
sudo apt install -y virtualbox-guest-utils
# Result: Unable to locate package
```
These packages are **not available for ARM64** in the kali repositories.

**Method 2** - Via **BoxLinuxAdditions.run:**
1. In VirtualBox menu → Devices → **"Insert Guest Additions CD Image.."**
2. Or download `VBoxGuestAdditions_7.2.6.iso`
   from [https://download.virtualbox.org/virtualbox/7.2.6/](https://download.virtualbox.org/virtualbox/7.2.6/)
4. in Kali Terminal:
```
# Give execute permission
sudo chmod 777 VBoxLinuxAdditions.run

# Run the installer
sudo ./VBoxLinuxAdditions.run
```
**Result on Apple M1:**
```
Verifying archive integrity... 100% MD5 checksums are OK. All good.
Uncompressing VirtualBox 7.2.6 Guest Additions for Linux 100%
VirtualBox Guest Additions installer detected unsupported arm64 machine type
```

**Root Cause:** VirtualBox Guest additions 7.2.6 does **not support ARM64 architecture.** This is 
a confirmed limitation of VirtualBox on Apple Silicon.

The purpose of run the **VirtualBox Guest Additions** is to have the machine are able to **Auto screen resize, Share clipboard, Drag and drop**.
However, the Kali VM still works fine for learning tools. Just won't have the convenience features like the above described.
It perfectly usable for a beginner lab.

### Step 5.4 — Update the System

```bash
sudo apt update && sudo apt full-upgrade -y
sudo reboot now
```
> 💡 Why `full-upgrade instead` of `upgrade`?
   - `apt update`: refreshes the package list (checks what's available)
   - `apt upgrade`: installs newer versions of existing packages
   - `apt full-upgrade`: same as upgrade but also handles dependency changes
   Always run `update` before upgrade. On Kali Linux, `full-upgrade` is the officially
   recommended command.

Always update immediately after a fresh install to get the latest security patches and tool versions.
<img width="1180" height="751" alt="Screenshot 2026-03-01 at 5 41 00 PM" src="https://github.com/user-attachments/assets/0eb8bfb1-79bc-4e0b-8dcd-bbc7bc6c54f0" />


### Step 5.5 — Take a Snapshot (Very Important!)

Before doing anything else, save this clean state:

1. VirtualBox menu → **Machine** → **Take Snapshot**
2. Name: `Clean Install - Post Update`
3. Click **OK**

> 💡 Think of snapshots as a "save game" button. If something breaks during practice, you can revert here instantly.

### Step 5.6 — Set Up Shared Folder (Optional)

To pass files between your Mac and Kali:

1. VM **Settings** → **Shared Folders** → click **+**
2. **Folder Path**: Choose a Mac folder (e.g., `~/Desktop/kali-share`)
3. **Folder Name**: `kali-share`
4. ✅ **Auto-mount** and ✅ **Make Permanent**

Inside Kali, the folder appears at `/media/sf_kali-share`.

### Step 5.7 — Explore Pre-installed Security Tools

| Category | Tools Included |
|---|---|
| **Information Gathering** | Nmap, Maltego, Recon-ng |
| **Vulnerability Analysis** | Nikto, OpenVAS |
| **Web Application Testing** | Burp Suite, OWASP ZAP |
| **Password Attacks** | Hashcat, John the Ripper, Hydra |
| **Exploitation** | Metasploit Framework |
| **Forensics** | Autopsy, Binwalk |
| **Sniffing & Spoofing** | Wireshark, Ettercap |

<img width="862" height="766" alt="Screenshot 2026-03-01 at 4 10 40 PM" src="https://github.com/user-attachments/assets/8f0a12b0-584a-4ff2-a4b9-070e1e30958d" />

---

## 🔧 Troubleshooting

### "Kernel driver not installed" error on launch
The System Extension was blocked. Go to **System Settings** → **Privacy & Security** → scroll down → click **Allow** next to Oracle software → restart your Mac.

### Black screen or VM freezes on boot
Go to VM **Settings** → **Display** → increase Video Memory to 128 MB. Also try selecting **Install** (text mode) instead of **Graphical Install** in the boot menu.

### Wrong architecture / VM won't start
You likely downloaded the `amd64` ISO instead of `arm64`. Re-download `kali-linux-arm64.iso` from the official Kali site.

### Kali runs slowly
Close other Mac applications to free RAM for the VM. The M1 manages memory efficiently, but 8 GB is shared between macOS and the VM. Quit Chrome tabs and heavy apps while using Kali.

### No internet inside Kali
VM **Settings** → **Network** → **Adapter 1** → confirm it is enabled and set to **NAT**.

### VBoxLinuxAdditions - Unsupported ARM64 Machine Type
When attempting to install VirtualBox Guest Additions on Kali Linux ARM64,
the installer returns:

"VirtualBox Guest Additions installer detected unsupported arm64 machine type"

Root Cause: VirtualBox Guest Additions does not currently support ARM64
architecture. This is a known limitation of VirtualBox 7.2.6 on Apple Silicon.

**Solution:** Migrated to UTM, which natively supports Apple Silicon and includes
all Guest Additions equivalent features (screen resize, shared clipboard,
drag and drop) out of the box.

### VirtualBox still has issues on M1?
Try **UTM** as an alternative: [https://mac.getutm.app](https://mac.getutm.app). It's free, open-source, and purpose-built for Apple Silicon. Download the Kali Linux UTM image directly from [https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-virtual-machines).

---

## 🎯 Key Takeaways

Through this project, I learned:

- How **virtualization works on Apple Silicon (ARM64)** and why chip architecture matters
- The difference between **ARM64 and x86_64**, and why you must match the ISO to your processor
- Why **Debian 12 Bookworm** is the correct VM Type, because Kali Linux 2024.x is built directly on top of it
- How to safely **allocate VM resources** on a machine with shared RAM (8 GB)
- The Kali Linux **installation process** end-to-end on Apple Silicon
- How to approve **macOS System Extensions** for third-party software
- How to use **VirtualBox snapshots** as a safety net for experimentation
- The importance of **SHA256 checksum verification** for downloaded files
- How to keep a system **patched and up to date** immediately after install
- The breadth of **600+ pre-installed security tools** in Kali Linux

---

## 📚 Resources

| Resource | Link |
|---|---|
| Official Kali Linux Documentation | [https://www.kali.org/docs/](https://www.kali.org/docs/) |
| Kali on Apple Silicon (Official Guide) | [https://www.kali.org/docs/virtualization/](https://www.kali.org/docs/virtualization/) |
| VirtualBox Download (Apple Silicon) | [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) |
| UTM — Alternative for M1 | [https://mac.getutm.app](https://mac.getutm.app) |
| TryHackMe Beginner Path | [https://tryhackme.com/path/outline/beginner](https://tryhackme.com/path/outline/beginner) |
| OverTheWire Bandit (Linux Basics) | [https://overthewire.org/wargames/bandit/](https://overthewire.org/wargames/bandit/) |
| Cybersecurity Career Roadmap | [https://roadmap.sh/cyber-security](https://roadmap.sh/cyber-security) |

---

## 🖥️ My Setup

| Component | Spec |
|---|---|
| **Machine** | MacBook Pro |
| **Chip** | Apple M1 |
| **RAM** | 8 GB |
| **Storage** | 512 GB SSD |
| **Host OS** | macOS |
| **VM RAM Allocated** | 4 GB |
| **VM CPUs Allocated** | 4 |
| **VM Disk Allocated** | 25.64 GB |
| **Storage Controller** | VirtioSCSI |
| **EFI** | Enabled |
| **Network** | NAT |

---

<div align="center">

**⭐ If this guide helped you, please star this repository! ⭐**

*Made with ❤️ as part of my cybersecurity learning journey*

*MacBook Pro M1 · Kali Linux 2025.4 ARM64 · VirtualBox 7.2.6*

</div>
