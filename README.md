# macOS Sequoia Bootable USB Installer

This guide covers two tasks:

1. Creating a bootable USB installer for macOS Sequoia.
2. Starting an Intel iMac with the Apple T2 Security Chip from that USB drive.

Expect it to take 30 to 60 minutes. Most of that time is spent downloading and copying files.

## Contents

- [Requirements](#requirements)
- [Part 1: Create the bootable USB installer](#part-1-create-the-bootable-usb-installer)
- [Part 2: Start the Intel iMac from the USB drive](#part-2-start-the-intel-imac-from-the-usb-drive)
- [Troubleshooting](#troubleshooting)
- [Links](#links)

---

## Requirements

- A USB flash drive of **32 GB or larger**. Everything on it will be erased.
- A Mac to create the installer on, with an internet connection.
- An administrator password for that Mac and for the target iMac.
- An internet connection for the target iMac during installation.

### Check your iMac model

To find your model, choose **Apple menu > About This Mac**.

| Model | Supports Sequoia | Has T2 chip | What to follow |
|---|---|---|---|
| iMac (Retina 5K, 27-inch, 2020) | Yes | Yes | Part 1 and all of Part 2 |
| iMac Pro (2017) | Yes | Yes | Part 1 and all of Part 2 |
| iMac (21.5-inch or 27-inch, 2019) | Yes | No | Part 1, then skip Step 2.1 |
| iMac 2017 or earlier | No | No | Sequoia cannot be installed |

To check for a T2 chip, hold **Option** and choose **Apple menu > System Information**, then select **Controller**. If the Mac has one, it is listed there as "Apple T2 Security Chip".

---

## Part 1: Create the bootable USB installer

Apple's official instructions for this part: [Create a bootable installer for macOS](https://support.apple.com/en-us/101578). This page is for reference only. Everything you need is already in the steps below.

### Step 1.1: Download the installer

This is the only file you need to download. It is the full installer app, **Install macOS Sequoia**, and it is about 12 to 15 GB. Use one of these two methods.

**Option A: App Store (recommended)**

1. On the Mac you are using to make the USB, open this link:

   **[Download macOS Sequoia from the App Store](https://apps.apple.com/app/macos-sequoia/id6596773750?mt=12)**

2. The App Store opens on the macOS Sequoia page. If the link opens in your browser instead, click **View in Mac App Store**.
3. Click **Get**. If a Software Update window opens and asks to confirm the download, click **Download**.
4. Wait for the download to finish.

**Option B: Terminal (if the App Store method does not work)**

1. Open Terminal. Press Command-Space, type `Terminal`, and press Return.
2. List the Sequoia versions you can download:

   ```bash
   softwareupdate --list-full-installers
   ```

3. Download the newest 15.x version on that list. Replace `15.7.2` with the exact version number shown:

   ```bash
   sudo softwareupdate --fetch-full-installer --full-installer-version 15.7.2
   ```

If the installer opens by itself after downloading, quit it with Command-Q. Do not install it.

**Check:** Open **Finder > Applications**. You should see **Install macOS Sequoia** there.

### Step 1.2: Rename the USB drive

1. Connect the USB drive.
2. In Finder, right-click the drive, choose **Rename**, and enter `MyVolume`.

The command in the next step looks for a drive with exactly this name. You do not need to format the drive yourself, because the command erases and formats it.

### Step 1.3: Run the createinstallmedia command

1. In Terminal, paste the following command and press Return:

   ```bash
   sudo /Applications/Install\ macOS\ Sequoia.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
   ```

2. Enter your administrator password. Nothing appears on screen while you type.
3. When asked whether to erase the volume, type `Y` and press Return.
4. Wait for the process to finish. It usually takes 10 to 30 minutes. The output looks similar to this:

   ```text
   Erasing disk: 0%... 10%... 20%... 30%... 100%
   Making disk bootable...
   Copying to disk: 0%... 10%... 20%... 100%
   Install media now available at "/Volumes/Install macOS Sequoia"
   ```

Keep the drive connected and Terminal open until you see `Install media now available`. Once you see that line, the USB installer is ready, and the drive is renamed **Install macOS Sequoia**.

---

## Part 2: Start the Intel iMac from the USB drive

Apple's official instructions for this part: [About Startup Security Utility on a Mac with the Apple T2 Security Chip](https://support.apple.com/en-us/102522). This page is for reference only. You don't need to download anything for Part 2.

### Step 2.1: Allow booting from external media

**This is the security setting you must change first.** By default, a Mac with a T2 chip does not start up from external drives, so it will ignore the USB installer until you change this setting. If your iMac has no T2 chip, skip this step.

1. Turn on or restart the iMac, then immediately press and hold **Command-R**.
2. Release the keys when you see the Apple logo or a spinning globe. The globe means Recovery is loading from the internet, which can take a few minutes.
3. If prompted, select a user and enter its password.
4. When the macOS Utilities window appears, choose **Utilities > Startup Security Utility** from the menu bar.
5. When asked to authenticate, click **Enter macOS Password**, choose an administrator account, and enter its password.
6. Under **Allowed Boot Media**, select **Allow booting from external or removable media**.
7. Leave **Secure Boot** set to **Full Security**. Sequoia is signed by Apple, so you do not need a lower setting.
8. Close the utility and choose **Apple menu > Shut Down**.

### Step 2.2: Start up from the USB installer

1. With the iMac shut down, plug the USB drive directly into the iMac. Avoid hubs and keyboard ports.
2. Make sure the iMac can connect to the internet over Wi-Fi or Ethernet.
3. Turn on the iMac and immediately press and hold **Option**.
4. Release the key when the screen shows your available startup drives.
5. Select **Install macOS Sequoia**, then press Return or click the arrow below it.
6. Choose a language if prompted.
7. In the macOS Utilities window, select **Install macOS Sequoia**, click **Continue**, and follow the instructions on screen.

### Step 2.3: Restore the security setting (optional)

After installation, you can turn off external booting again. Repeat Step 2.1, and at step 6 select **Disallow booting from external or removable media**.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Terminal reports `command not found` or `No such file or directory` | Confirm that Install macOS Sequoia is in Applications and that the drive is named exactly `MyVolume`. |
| Terminal reports that there is not enough space | Use a larger USB drive, at least 32 GB. |
| The USB drive does not appear when you hold Option | Repeat Step 2.1 and confirm the setting is Allow. Connect the drive directly to the iMac. |
| A prohibited symbol or security message appears at startup | Allowed Boot Media is still set to Disallow. Repeat Step 2.1. |
| "An internet connection is required" | Connect to Wi-Fi or Ethernet. Under Full Security, the Mac may need to verify the installer with Apple. |
| You are asked for a disk password | FileVault is turned on. Enter the password of a user on that Mac. |
| `softwareupdate` cannot find the version | Run `softwareupdate --list-full-installers` and use an exact version number from the list. |

---

## Links

### Download (the only file you need)

| File | Link |
|---|---|
| macOS Sequoia installer (Mac App Store) | [Open in the App Store](https://apps.apple.com/app/macos-sequoia/id6596773750?mt=12) |

### Apple's official documentation (for reference only, nothing to download)

| Topic | Link |
|---|---|
| Create a bootable installer for macOS | [support.apple.com/en-us/101578](https://support.apple.com/en-us/101578) |
| Startup Security Utility on a Mac with T2 | [support.apple.com/en-us/102522](https://support.apple.com/en-us/102522) |
| Macs that support macOS Sequoia | [support.apple.com/en-us/120282](https://support.apple.com/en-us/120282) |
| Start up from macOS Recovery | [support.apple.com/en-us/102518](https://support.apple.com/en-us/102518) |

---

Verified against Apple Support documentation in November 2026. Apple may update these procedures, so if your screen looks different, follow the official pages above.
