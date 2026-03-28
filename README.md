# All Windows 11 Bypasses to Create a Local Account

## A. External methods:

### Use Rufus to modify ISO
Check/tick the "Remove requirement for an online account" box. It will appear in the "Windows User Experience" alert after pressing the 'START' button.

### Create an ``autounattend.xml`` script (online)
```
https://schneegans.de/windows/unattend-generator/
```

## B. Installer methods:

‼️ Keep Ethernet cable unplugged and do not connect to a Wi-Fi network, unless specified.

### Networking hardware interrupt
(If this results in an infinite loading screen, then consider it patched)

- Connect to a mobile hotspot, Wi-Fi network or Ethernet cable.
- Be ready to either disable mobile hotspot or unplug the router. (Note: Time the disconnect either *just* before the email field disappears, or immediately as it disappears.)
- Type an invalid email address (e.g. ``test@test.com``) in the email field (to force the installer to try and validate it). Do NOT use ``no@thankyou.com`` as it is blacklisted on the server-side. 
- Type a random invalid password.
- The moment you click "Next", turn off the hotspot or router.
Sometimes this forces it to revert to local account mode.

The timeout often triggers an "offline fallback" that takes you straight to the local account screen (Microsoft is trying to patch this with an infinite retry, but it still works on most OEM versions).

### Chris Titus script
[working since Windows 11 HOME edition 26220.6772 (Dev channel)]

Connect to the Internet. Shift + F10. Type the command below:
> curl -L christitus.com/bypass -o skip.cmd

### Developer console method
(works in 25H2 as of 20 Oct 2025)

``Ctrl + Shift + J``  to enter developer console (``Esc`` or ``Ctrl + Shift + J`` again to exit)
> WinJS.Application.restart("ms-cxh://LOCALONLY")

### Using regedit to enable local account (works in 25H2)
Shift + F10
> reg add HKLM\Software\Microsoft\Windows\CurrentVersion\OOBE /v BypassNRO /t REG_DWORD /d 1 /f

>  shutdown /r /t 0

### Audit Mode (SysPrepTool)

Go to the Network screen.

Ctrl + Shift + F3

Let it boot up again.

Do NOT close the System Preparation Tool (SysPrepTool) window yet.

If there is no SysPrepTool window, then open it with:

Win + R, then enter ``sysprep``, run. 

Then open ``sysprep.exe``.

Open a Command Prompt window and create a local Admin account:

> net user "YourName" /add

> net localgroup administrators "YourName" /add

In the SysPrepTool window, set the following options:
- Enter System Out-of-Box Experience (OOBE)
- Reboot

Click OK.

### Manually create local account and force skip - Method 2

Shift + F10

> net.exe user 'username' 'password' /add
(password can be left blank)

> net.exe localgroup Administrators 'username' /add
taskkill /F /IM msoobe.exe

### Manually create local account and force skip - Method 1
⚠️ Patched in 25H2/26H1 - infinite loading / black screen (March 2026)

‼️ May create a ghost admin account which you don't have access to ``defaultuser0``. Refer to the fix at the very bottom.

Shift + F10

> net.exe user 'username' 'password' /add
(password can be left blank)

> net.exe localgroup Administrators 'username' /add

> cd oobe

> msoobe.exe && shutdown.exe -r

### Start installer in local-only mode
⚠️ Patched in Dev 26220.6772 & Beta 26120.6772 (6 Oct 2025)
> start ms-cxh:LOCALONLY
or
> start ms-cxh://LOCALONLY
or
> start ms-cxh://SETADDLOCALONLY
