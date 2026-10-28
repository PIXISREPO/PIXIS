# How to sign in to Raspberry Pi Connect from the command line

Commands checked against official Raspberry Pi documentation on 28 October 2026.

## Prerequisites

- A Raspberry Pi running Raspberry Pi OS Bookworm or later, connected to the internet.
- Terminal access on the Pi, either locally or through SSH.
- A Raspberry Pi ID and a browser on any internet-connected device.
- Connect installed: current Raspberry Pi OS Desktop/Full includes Connect; OS Lite includes Connect Lite for remote shell access.

If the command is missing, install the appropriate package. For a headless Pi:

```bash
sudo apt update
sudo apt install rpi-connect-lite
```

For a desktop requiring screen sharing, install `rpi-connect` instead.

## 1. Start Connect

Run these commands on the Pi as the user whose account you want to access. Connect operates as a user service; run its commands without `sudo`.

```bash
rpi-connect on
```

This enables and starts Connect. Account linking is a separate step.

## 2. Sign in and verify in a browser

```bash
rpi-connect signin
```

The terminal prints a verification link resembling:

```text
https://connect.raspberrypi.com/verify/XXXX-XXXX
```

Open the actual URL printed by your Pi on your computer, phone, or tablet. `XXXX-XXXX` is a placeholder. Sign in with your Raspberry Pi ID and complete the prompts to name and link the device. Keep the terminal open until sign-in completes.

## 3. Check the result

```bash
rpi-connect status
```

Confirm that it reports `Signed in: yes`. It also shows which remote access methods are allowed; screen sharing is unavailable with Connect Lite.

At [Raspberry Pi Connect](https://connect.raspberrypi.com/), open **Devices**, find the Pi, and choose **Connect via → Remote shell**.

## 4. Keep headless access available

From the same Pi user account, run:

```bash
loginctl enable-linger
```

Lingering keeps the user's services available without an interactive login, including after reboot. Raspberry Pi recommends it for headless OS Lite installations. Before relying on unattended access, close your SSH session and test Connect again; also test after a planned reboot.

Lingering does not create a graphical desktop session. Screen sharing requires an existing supported Wayland desktop session; for automatic startup, the official guide describes Desktop Autologin.

## Troubleshooting

- **Command missing:** install the appropriate package above.
- **Connect stopped:** run `rpi-connect on`, then check `rpi-connect status`.
- **Still signed out:** rerun `rpi-connect signin` and complete the newly printed browser link.
- **Unavailable after logout/reboot:** enable lingering for the same user who signed in.
- **Remote shell disabled:** run `rpi-connect shell on`.
- **Network or authentication failure:** run the diagnostic checks:

  ```bash
  rpi-connect doctor
  ```

  Consult the official troubleshooting section for failed tests and firewall requirements.

To inspect service logs:

```bash
journalctl --follow --user-unit rpi-connect.service
```

Press **Ctrl+C** to stop following logs.

## Official references

- [Raspberry Pi Connect documentation — prerequisites and CLI linking](https://www.raspberrypi.com/documentation/services/connect.html)
- [Official documentation source — lingering and service management](https://github.com/raspberrypi/documentation/blob/master/documentation/asciidoc/services/connect/manage.adoc)
- [Official documentation source — troubleshooting, status, diagnostics and logs](https://github.com/raspberrypi/documentation/blob/master/documentation/asciidoc/services/connect/troubleshooting.adoc)
- [Official documentation source — remote shell access controls](https://github.com/raspberrypi/documentation/blob/master/documentation/asciidoc/services/connect/access.adoc)
