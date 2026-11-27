# NeoShy Remote Tool

**NeoShy Remote Tool** is a Windows desktop GUI utility for Android device management, flashing, debugging, and screen mirroring. Built with **Python** and **PySide6**, it bundles essential utilities—including **ADB**, **Fastboot**, **Scrcpy**, **Heimdall**, and a built-in **Android OTA Payload Dumper**—into a single lightweight and modern interface.

---

## ✨ Key Features

* **Live Device Status Card**: Automatically detects connected devices across **ADB**, **Fastboot**, **Recovery**, **Sideload**, and **Unauthorized** modes, displaying model info (automatically resolving codenames to marketing names such as POCO/Redmi/Xiaomi/Pixel), Android version, ROM version, and Bootloader status.
* **Screen Mirroring (Scrcpy Integration)**:
  * **Normal Mode**: Floating window with a device screen bezel and a comprehensive side control panel (Power, Restart, Quick Settings, Install APK, Push/Pull files, Bitrate & FPS controls, plus Crash Fix options like `--no-audio`, Low Res, and Safe Mode).
  * **Lite Mode**: Runs Scrcpy in a standalone window with a compact control strip that automatically docks to the side of the screen.
* **ADB & Recovery Utilities**:
  * ADB server management (`Start Server`, `Kill Server`), multi-mode reboots, and **ADB Sideload** (`.zip`) with progress indicators.
  * **Batch File Transfer**: Push multiple files at once and pull files from device storage using an interactive checkbox list.
  * **Magisk Module Manager (Recovery)**: Scans and removes problematic Magisk modules in `/data/adb/modules` while the device is booted into Recovery mode.
* **Fastboot & Flashing Tools**:
  * Standard partition flashing (`boot`, `init_boot`, `vendor_boot`, `recovery`), temporary booting, and full **A/B Slot** support (check active slot, switch slots, and flash to slot A/B).
  * Unlock & security utilities: `flashing unlock`, `unlock_critical`, `Disable dm-verity`, `Disable VBMeta`, and `Format Data`.
* **Xiaomi Flasher & Mi Unlock**:
  * Executes `flash_all*.bat` scripts from Xiaomi Fastboot firmware folders (similar to Mi Flash Tool) with options to bypass product or anti-rollback checks, alongside built-in **Mi Unlock** integration.
* **Samsung Download Mode (Heimdall) & VBMeta Patcher**:
  * Supports flashing Odin packages (`.tar` / `.tar.md5`) across `BL`, `AP`, `CP`, `CSC`, `HOME_CSC`, and `PIT` slots via Heimdall.
  * Built-in **`vbmeta.img` Patcher** to disable AVB (Android Verified Boot).
* **Built-in Android OTA Payload Dumper**:
  * Extracts `.img` partitions directly from `payload.bin` or `.zip` ROM packages locally on your PC without external dependencies.
* **System & Driver Utilities**:
  * Quick shortcuts to Windows File Explorer, Terminal, Device Manager, WinUSB driver installation (`android_winusb.inf`), and registry fix scripts.

---

## 🛠️ Tech Stack & Third-Party Sources (Credits)

This application is powered by the following open-source projects and official binaries:

1. **Python 3**
   * Core programming language for application logic, `payload.bin` protobuf parsing, and subprocess management.
   * Source: [python.org](https://www.python.org/)
2. **PySide6 (Qt for Python)**
   * Graphical User Interface (GUI) framework.
   * Source: [Qt for Python / PySide6](https://doc.qt.io/qtforpython-6/)
3. **PyWin32**
   * Windows API integration (`win32gui`, `win32process`, `win32api`) for Scrcpy window positioning and focus management.
   * Source: [github.com/mhammond/pywin32](https://github.com/mhammond/pywin32)
4. **Android SDK Platform-Tools (`adb` & `fastboot`)**
   * Official Google binaries for ADB, Sideload, and Fastboot communication.
   * Source: [developer.android.com/tools/releases/platform-tools](https://developer.android.com/tools/releases/platform-tools)
5. **Scrcpy (Screen Copy)**
   * Used for real-time Android screen mirroring and control over USB/TCP.
   * Source: [github.com/Genymobile/scrcpy](https://github.com/Genymobile/scrcpy)
6. **Heimdall Suite**
   * Cross-platform open-source utility for flashing firmware onto Samsung Galaxy devices in Download Mode.
   * Source: [glassechidna.com.au/heimdall](https://glassechidna.com.au/heimdall/) / [GitLab Benjamin Dobell](https://gitlab.com/BenjaminDobell/Heimdall)
7. **Google USB Driver (`android_winusb.inf`)**
   * Standard USB drivers for recognizing Android ADB and Bootloader interfaces on Windows.
   * Source: [developer.android.com/studio/run/win-usb](https://developer.android.com/studio/run/win-usb)
8. **PyInstaller** *(Optional - for compilation)*
   * Packages the Python script and assets into a standalone Windows `.exe` executable.
   * Source: [pyinstaller.org](https://pyinstaller.org/)

---

## 📁 Directory Structure

Ensure your directory structure matches the layout below so all binaries load properly (especially to keep 32-bit Heimdall DLLs isolated from 64-bit ADB/Scrcpy binaries):

```text
NeoShy-Remote-Tool/
├── NeoShy_Remote_Tool.py      # Main application source code
├── logo.png                   # Application icon
├── bin/                       # Main binaries (64-bit)
│   ├── adb.exe
│   ├── AdbWinApi.dll
│   ├── AdbWinUsbApi.dll
│   ├── fastboot.exe
│   ├── scrcpy.exe
│   ├── scrcpy-server
│   ├── *.dll                  # Scrcpy dependencies (SDL2, FFmpeg, etc.)
│   └── heimdall/              # Isolated subfolder for Heimdall (32-bit)
│       ├── heimdall.exe
│       └── *.dll              # Bundled Heimdall DLLs (libusb, etc.)
└── usb_driver/                # Windows drivers & fix scripts
    ├── android_winusb.inf
    └── *.bat / *.reg          # Optional driver fix scripts
