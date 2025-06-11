# Sierra Wireless EM7455

A simple systemd-based solution to automatically initialize and activate the Sierra Wireless EM7455 LTE modem on Linux.  
This fixes the issue where the modem stays in `low power state` after boot or resume from sleep, requiring manual FCC unlock and ModemManager restart.

## ✅ Features

- Detects EM7455 modem using `mmcli`
- Automatically applies FCC authentication via `qmicli`
- Restarts `ModemManager` when needed
- Works both on system boot and after resume from suspend/hibernate
- Includes retry logic to wait for the modem to become available

## 📁 File Structure

```

/usr/local/bin/em7455-init
/etc/systemd/system/em7455-init.service
/etc/systemd/system/em7455-resume.service

````

## ⚙️ Installation

1. Copy the `em7455-init` script to `/usr/local/bin/` and make it executable:

```bash
sudo cp em7455-init /usr/local/bin/
sudo chmod +x /usr/local/bin/em7455-init
````

2. Copy the systemd unit files:

```bash
sudo cp em7455-init.service /etc/systemd/system/
sudo cp em7455-resume.service /etc/systemd/system/
```

3. Enable and start the services:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now em7455-init.service
sudo systemctl enable --now em7455-resume.service
```

The resume service will be triggered automatically on suspend/resume events.

## 🧠 How It Works

On boot or resume, the script:

1. Waits for ModemManager to detect the modem.
2. Checks if the modem is stuck in `low power state`.
3. Sends FCC unlock using `qmicli`.
4. Restarts `ModemManager` to apply changes.
5. Verifies the modem is now in `power state: on`.

## 🛠 Dependencies

* `ModemManager`
* `qmicli`
* `mmcli`
* Linux system with `systemd`

## 📦 License

This project is licensed under the [MIT License](LICENSE).

---

**Tested with:**

* Sierra Wireless EM7455
* Linux (Debian / Arch / Ubuntu)

Feel free to fork, improve, or contribute!
