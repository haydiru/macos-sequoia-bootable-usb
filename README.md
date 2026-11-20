# 🍎 macOS Sequoia Bootable USB: Step-by-Step Guide

A plain-language guide for two things:

1. **Make a bootable USB installer** for macOS Sequoia
2. **Start an Intel iMac with the Apple T2 Security Chip** from that USB drive

> ⏱️ **Time needed:** about 30–60 minutes (mostly waiting for downloads and copying)
> 💡 **Skill level:** beginner. If you can copy, paste and press Return, you can do this.

---

## 📋 Contents

- [Before you start](#-before-you-start)
- [Part 1: Make the bootable USB installer](#-part-1-make-the-bootable-usb-installer)
- [Part 2: Start the Intel iMac (T2) from the USB](#-part-2-start-the-intel-imac-t2-from-the-usb)
- [Troubleshooting](#-troubleshooting)
- [What the AI got wrong or out of date](#-what-the-ai-got-wrong-or-out-of-date)
- [Official Apple links](#-official-apple-links)

---

## ✅ Before you start

### What you need

| Item | Details |
|---|---|
| 💾 **USB flash drive** | **32 GB or larger** recommended. ⚠️ Everything on it will be erased. |
| 💻 **A working Mac** | Used to *make* the USB. It needs internet and an admin password. |
| 🖥️ **The target iMac** | The Intel iMac you want to start from the USB. |
| 🌐 **Internet connection** | For the download, and later while installing. |
| 🔑 **Admin password** | For the Mac you are using and for the target iMac. |

### Does your iMac have a T2 chip?

Click  **Apple menu → About This Mac** and look at the model name:

| Your iMac | Runs Sequoia? | Has a T2 chip? | What to do |
|---|---|---|---|
| iMac (Retina 5K, 27-inch, **2020**) | ✅ Yes | ✅ **Yes** | Follow Part 1 **and** all of Part 2 |
| iMac Pro (**2017**) | ✅ Yes | ✅ **Yes** | Follow Part 1 **and** all of Part 2 |
| iMac (27-inch or 21.5-inch, **2019**) | ✅ Yes | ❌ No | Follow Part 1, then **skip Step 2.1** in Part 2 |
| iMac 2017 or older | ❌ No | ❌ No | Sequoia won't install on this iMac |

> 💡 Not sure? Hold **Option (⌥)** and click  **Apple menu → System Information → Controller**. If you see **"Apple T2 Security Chip"**, you have one.

---

## 🔧 Part 1: Make the bootable USB installer

📖 **Official Apple guide:** [Create a bootable installer for macOS](https://support.apple.com/en-us/101578)

### Step 1.1: Download the macOS Sequoia installer

You need the **full installer app**, called **`Install macOS Sequoia`**.

**Option A: from your browser (easiest)**

1. Open Apple's page 👉 **[How to download and install macOS](https://support.apple.com/en-us/102662)**
2. Find **macOS Sequoia** and click its link. It opens the App Store.
3. Click **Get** and wait for the download (about 15 GB).

**Option B: with Terminal**

Open **Terminal** (press **⌘ + Space**, type `Terminal`, press Return).

First, see which Sequoia versions are available:

```bash
softwareupdate --list-full-installers
```

You'll see a list like this:

```text
* Title: macOS Sequoia, Version: 15.7.2, Size: 15xxxxxxKiB, Build: ...
* Title: macOS Sequoia, Version: 15.7.1, ...
```

Then download one, replacing `15.7.2` with the **newest 15.x number from your list**:

```bash
sudo softwareupdate --fetch-full-installer --full-installer-version 15.7.2
```

> ⚠️ **Important:** when the download finishes, the installer may open by itself. **Don't install it.** Just quit it with **⌘ + Q**.

**✔️ Check:** open **Finder → Applications**. You should see **`Install macOS Sequoia`** there.

---

### Step 1.2: Plug in and rename the USB drive

1. Plug the USB drive into your Mac.
2. On the desktop or in Finder, **right-click the drive → Rename**.
3. Name it exactly:

   ```text
   MyVolume
   ```

> 💡 **Why `MyVolume`?** The command in the next step looks for a drive with this exact name. Match the capital letters.
>
> You don't need to format the drive first. The command does that for you.

---

### Step 1.3: Run the command in Terminal

1. Open **Terminal**.
2. **Copy and paste** this whole command, then press **Return**:

   ```bash
   sudo /Applications/Install\ macOS\ Sequoia.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
   ```

3. **Type your Mac's admin password** and press Return.
   > 🙈 Nothing shows on screen while you type. That's normal.
4. When it asks `Proceed? (y/n)`, type **`Y`** and press Return.
5. ☕ Wait. This usually takes **10–30 minutes**. You'll see something like this:

   ```text
   Erasing disk: 0%... 10%... 20%... 30%... 100%
   Making disk bootable...
   Copying to disk: 0%... 10%... 20%... 100%
   Install media now available at "/Volumes/Install macOS Sequoia"
   ```

6. When you see **`Install media now available`**, you're done. 🎉

**✔️ Check:** the USB drive is now called **`Install macOS Sequoia`**. Eject it safely, or leave it in if you'll use it on this Mac.

> ⚠️ **Don't** unplug the drive or close Terminal while it's copying.

---

## 🖥️ Part 2: Start the Intel iMac (T2) from the USB

📖 **Official Apple guides:**
- [About Startup Security Utility on a Mac with the Apple T2 Security Chip](https://support.apple.com/en-us/102522)
- [Create a bootable installer for macOS](https://support.apple.com/en-us/101578) (see "Any other Mac")

### Step 2.1: Allow the iMac to start from USB (one-time setting) 🔐

> 🔴 **This is the security setting you must change first.**
> A Mac with a T2 chip **blocks starting from USB drives by default**. If you skip this, the USB won't work.
>
> *(2019 iMac without a T2 chip: skip this step.)*

1. **Turn on** (or restart) the iMac, and **right away press and hold ⌘ Command + R**.
2. Let go when you see the **Apple logo** or a **spinning globe**.
   > 🌐 A spinning globe means it's loading Recovery from the internet. That's fine, just wait.
3. If asked, pick a user and enter its password.
4. You'll see the **macOS Utilities** window. In the **menu bar at the top**, click:

   ```text
   Utilities → Startup Security Utility
   ```

5. When it asks you to sign in, click **Enter macOS Password**, choose an **administrator** account, and type its password.
6. Under **Allowed Boot Media**, select:

   > ✅ **Allow booting from external or removable media**

7. Under **Secure Boot**, **leave it on "Full Security"**. Sequoia is an official Apple release, so you don't need to lower it.
8. Close the window. Then click  **Apple menu → Shut Down**.

```text
┌─────────────── Startup Security Utility ───────────────┐
│  Secure Boot                                           │
│    ● Full Security          ← leave this               │
│    ○ Medium Security                                   │
│    ○ No Security                                       │
│                                                        │
│  Allowed Boot Media                                    │
│    ○ Disallow booting from external or removable media │
│    ● Allow booting from external or removable media ← │
└────────────────────────────────────────────────────────┘
```
*(Simplified sketch, not a real screenshot.)*

---

### Step 2.2: Start from the USB drive 🚀

1. Make sure the iMac is **shut down**.
2. Plug the USB drive **straight into the iMac**, not into a hub or keyboard.
3. Connect the iMac to the **internet** (Wi-Fi or cable).
4. **Turn on** the iMac and **right away press and hold Option (⌥)**.
5. Let go when you see a screen with **drive icons**.
6. Click **`Install macOS Sequoia`** (the yellow/orange USB icon), then click the **↑ arrow** below it or press **Return**.

```text
   ┌──────────┐        ┌──────────┐
   │   💽     │        │   🟧     │
   │Macintosh │        │ Install  │
   │    HD    │        │  macOS   │
   │          │        │ Sequoia  │
   └──────────┘        └──────────┘
                            ↑  ← click this
```
*(Simplified sketch, not a real screenshot.)*

7. Pick your **language** if asked.
8. In the **macOS Utilities** window, choose **Install macOS Sequoia** → **Continue**, and follow the steps on screen. ✅

---

### Step 2.3 (optional): Switch the security setting back 🔒

When you're done, you can block USB starts again for extra safety:
repeat **Step 2.1**, but choose **Disallow booting from external or removable media**.

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| `command not found` or `No such file` in Terminal | The installer isn't in **Applications**, or the drive isn't named **`MyVolume`**. Check both and try again. |
| Terminal says there isn't enough space | Use a bigger USB drive (32 GB or more). |
| The USB drive doesn't show up when holding Option | Do **Step 2.1** again and check the setting is **Allow**. Plug the drive straight into the iMac. |
| You see a 🚫 symbol, or "security settings do not allow…" | Allowed Boot Media is still set to **Disallow**. Repeat Step 2.1. |
| "An internet connection is required" | Connect to Wi-Fi or Ethernet. With Full Security, the Mac checks the installer with Apple. |
| It asks for a disk password | FileVault is on. Enter the password you use to log in to that Mac. |
| `softwareupdate` says the version isn't found | Run `softwareupdate --list-full-installers` and copy an **exact** version number from the list. |

---

## 🤖 What the AI got wrong or out of date

I checked the AI's first answer against Apple's current support pages. Here is what needed fixing:

| # | What the AI said | What's actually right |
|---|---|---|
| 1 | Download with `--full-installer-version 15.x` | ❌ `15.x` is a placeholder and **won't work**. Run `softwareupdate --list-full-installers` and use an exact number, like `15.7.2`. |
| 2 | "32 GB is **required**; 16 GB fails" | ⚠️ Overstated. Apple says 32 GB is "more than enough" and 16 GB works for most older macOS versions. **32 GB is still the safe choice.** |
| 3 | "Format the drive in Disk Utility first" | ⚠️ Not needed. The `createinstallmedia` command erases and formats the drive for you. Use Disk Utility only if that fails. |
| 4 | "Shut down the iMac (or restart while holding Option…)" | ❌ Confusing. Correct order: **shut down → plug in the USB → turn on → hold Option**. |
| 5 | Stopped after "select the USB and press Return" | ❌ Missing steps: choose a language, then pick **Install macOS** in the Utilities window. The iMac also needs **internet**. |
| 6 | "Full Security works as long as internet is available during verification" | ⚠️ Close, but unclear. The Mac *may* need to download check data from Apple when it starts. If FileVault is on, it may ask for your disk password. |
| 7 | Old `HT201372` / `HT208198` support links | ✅ The AI was right that these are old. The current pages are **101578** and **102522**. |
| 8 | Mixing up Apple silicon and Intel startup keys | ✅ Correct warning. Holding the **power button** is for Apple silicon (M-series). Intel iMacs use **⌘ + R** and **Option**. |
| 9 | All Sequoia iMacs have T2 | ✅ Correct warning. **2019 iMacs have no T2**. Only the **2020 iMac** and **iMac Pro (2017)** do. |
| 10 | "Set Secure Boot to No Security" | ✅ Correct warning. Not needed. Only change **Allowed Boot Media**. |

> 📅 **Also out of date:** Sequoia (macOS 15) isn't the newest macOS anymore. The App Store shows newer versions first, so use Apple's [download page](https://support.apple.com/en-us/102662) to get Sequoia specifically. A 2020 iMac can also run the newer **macOS Tahoe 26**.

---

## 🔗 Official Apple links

| What | Link |
|---|---|
| 📖 Part 1: Create a bootable installer | https://support.apple.com/en-us/101578 |
| 📖 Part 2: Startup Security Utility (T2) | https://support.apple.com/en-us/102522 |
| ⬇️ **Download macOS Sequoia** | https://support.apple.com/en-us/102662 |
| 🖥️ Macs that support Sequoia | https://support.apple.com/en-us/120282 |
| 🛟 How to start up from macOS Recovery | https://support.apple.com/en-us/102518 |

---

<sub>Checked against Apple Support pages in November 2026. Apple may change its steps over time, so if something looks different, trust the official links above.</sub>
