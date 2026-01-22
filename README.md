---

# Sierra Wireless EM7455

A systemd-based solution to initialize and activate the **Sierra Wireless EM7455 LTE modem** on Linux.

This project exists to fix cases where the modem remains in **`low power state`** after boot or resume and requires **manual FCC unlock**, especially on systems where ModemManager does not handle it automatically.

---

## 🧠 Important Note — ModemManager ≥ 1.18.4 (FCC Unlock)

Starting from **ModemManager 1.18.4**, the FCC unlock procedure is **built into ModemManager itself**.

In most modern distributions **this script is NOT required anymore**.

### ✅ What changed in ModemManager

Since version **1.18.4**, ModemManager:

* Includes **built-in FCC unlock scripts**
* Supports **automatic FCC unlock** for supported modems (including Sierra Wireless)
* Does **not enable FCC unlock by default**
* Allows users or distributions to explicitly enable it

---

## 📌 Official ModemManager behavior

According to the official documentation:

* FCC unlock scripts are shipped in:

```
/usr/share/ModemManager/fcc-unlock.available.d/
```

* They are **not enabled automatically**
* Users may enable them manually via:

```
/etc/ModemManager/fcc-unlock.d/
```

---

## ✅ Recommended Solution (No Script Needed)

If your system uses:

* **ModemManager ≥ 1.18.4**
* **Sierra Wireless EM7455**
* A standard Linux distribution (Debian / Ubuntu / Arch)

You can enable FCC unlock with:

```bash
sudo ln -s /usr/share/ModemManager/fcc-unlock.available.d/* \
           /etc/ModemManager/fcc-unlock.d/
```

Then restart ModemManager:

```bash
sudo systemctl restart ModemManager
```

✅ After this, **no custom scripts are required** — FCC unlock will be handled automatically.

📖 Official documentation:
[https://modemmanager.org/docs/modemmanager/fcc-unlock/](https://modemmanager.org/docs/modemmanager/fcc-unlock/)

---

## ⚠️ When This Project Is Still Useful

This project is useful if:

* You use **older ModemManager (< 1.18.4)**
* FCC unlock scripts are **missing or disabled**
* You run a **minimal or custom Linux system**
* The modem **fails to unlock after suspend/resume**
* You want **explicit control and logging**
* You need a **reliable fallback solution**

In these cases, this script provides a robust workaround.

---

# 🚀 Sierra Wireless EM7455 Init Script

A systemd-based solution to automatically initialize and activate the Sierra Wireless EM7455 LTE modem.

This fixes the issue where the modem remains in `low power state` after boot or resume and requires manual FCC unlock.

---

## ✅ Features

* Detects EM7455 modem using `mmcli`
* Automatically applies FCC authentication via `qmicli`
* Works on boot and resume
* Handles delayed modem initialization
* Includes retry logic
* Works even when ModemManager fails to unlock FCC

---

## 📁 File Structure

```
/usr/local/bin/em7455-init
/etc/systemd/system/em7455-init.service
/etc/systemd/system/em7455-resume.service
```

---

## ⚙️ Installation

### 1. Install script

```bash
sudo cp em7455-init /usr/local/bin/
sudo chmod +x /usr/local/bin/em7455-init
```

### 2. Install systemd units

```bash
sudo cp em7455-init.service /etc/systemd/system/
sudo cp em7455-resume.service /etc/systemd/system/
```

### 3. Enable services

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now em7455-init.service
sudo systemctl enable --now em7455-resume.service
```

The resume service is triggered automatically after suspend/hibernate.

---

## 🧠 How It Works

On boot or resume, the script:

1. Waits for ModemManager to detect the modem
2. Detects the modem control interface (`cdc-wdmX`)
3. Checks modem power state
4. Sends FCC unlock via `qmicli`
5. Verifies the modem is operational

---

## 🛠 Dependencies

* `ModemManager`
* `mmcli`
* `qmicli`
* `systemd`

---

## 📦 License

This project is licensed under the **MIT License**.

---

## ✅ Tested With

* Sierra Wireless EM7455
* Debian / Ubuntu / Arch Linux
* ModemManager ≥ 1.14

---
