---

# Sierra Wireless EM7455

A systemd-based solution to initialize and activate the **Sierra Wireless EM7455 LTE modem** on Linux.

This project fixes cases where the modem remains in **`low power state`** after boot or resume and requires **manual FCC unlock**, especially on systems where ModemManager does not handle it automatically.

---

## 🧠 Important Note — ModemManager ≥ 1.18.4

Starting from **ModemManager 1.18.4**, FCC unlock support is **built into ModemManager**.

On most modern systems **this script is not required**.

### What changed

Since version **1.18.4**, ModemManager:

* Includes built-in FCC unlock scripts
* Supports automatic FCC unlock for supported modems
* Does not enable FCC unlock by default
* Allows users or distributions to enable it manually

---

## 📌 Official ModemManager Behavior

FCC unlock scripts are shipped in:

```
/usr/share/ModemManager/fcc-unlock.available.d/
```

They are **not enabled automatically**.

To enable them manually:

```
/etc/ModemManager/fcc-unlock.d/
```

---

## ✅ Recommended Solution (No Script Required)

If your system uses:

* ModemManager ≥ 1.18.4
* Sierra Wireless EM7455
* A standard Linux distribution

Enable FCC unlock with:

```bash
sudo ln -s /usr/share/ModemManager/fcc-unlock.available.d/* \
           /etc/ModemManager/fcc-unlock.d/
```

Then restart ModemManager:

```bash
sudo systemctl restart ModemManager
```

After that, **no custom scripts are required**.

📖 Documentation:
[https://modemmanager.org/docs/modemmanager/fcc-unlock/](https://modemmanager.org/docs/modemmanager/fcc-unlock/)

---

## ⚠️ When This Project Is Still Useful

This project is useful if:

* You use ModemManager < 1.18.4
* FCC unlock scripts are missing or disabled
* You run a minimal or custom Linux system
* The modem fails to unlock after suspend/resume
* You want explicit control or logging
* You need a reliable fallback solution

---

# 🚀 Sierra Wireless EM7455 Init Script

A systemd-based solution to automatically initialize and activate the EM7455 modem.

It ensures the modem exits `low power state` and becomes usable after boot or resume.

---

## ✅ Features

* Detects EM7455 using `mmcli`
* Applies FCC unlock via `qmicli`
* Works on boot and resume
* Handles delayed modem initialization
* Includes retry logic
* Works even if ModemManager fails to unlock FCC

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

---

## 🧠 How It Works

1. Waits for ModemManager to detect the modem
2. Detects the modem control interface (`cdc-wdmX`)
3. Checks modem power state
4. Sends FCC unlock
5. Verifies modem readiness

---

## 🛠 Dependencies

* ModemManager
* mmcli
* qmicli
* systemd

---

## 📦 License

MIT License

---

## ✅ Tested With

* Sierra Wireless EM7455
* Debian / Ubuntu / Arch Linux
* ModemManager ≥ 1.14

---
