# All Windows 11 Bypasses to Create a Local Account

~~(The text formatting will be fixed later. It's a little messy at the moment)~~

## A. External methods:

### 1. Use Rufus to modify ISO

Check/tick the "Remove requirement for an online account" box.

It will appear in the "Windows User Experience" alert after pressing the 'START' button.

### 2. Create an ``autounattend.xml`` script (online)

```
https://schneegans.de/windows/unattend-generator/
```

## B. Installer methods:

‼️ Keep Ethernet cable unplugged and do not connect to a Wi-Fi network, unless specified.

### 1. Networking hardware interrupt

(Note: If this results in an infinite loading screen, then consider it patched)

- Connect to a mobile hotspot, Wi-Fi network or Ethernet cable.
- Be ready to either disable mobile hotspot or unplug the router. (Note: Time the disconnect either *just* before the email field disappears, or immediately as it disappears.)
- Type an invalid email address (e.g. ``test@test.com``) in the email field (to force the installer to try and validate it). Do NOT use ``no@thankyou.com`` as it is ***blacklisted*** on the server-side.
- Type a random password. Anything goes, as long as it's invalid.
- The moment you click "Next", turn off the hotspot or router.

Sometimes this forces it to revert to local account mode.

The timeout often triggers an "offline fallback" that takes you straight to the local account screen (Microsoft is trying to patch this with an infinite retry, but it still works on most OEM versions).

---

### 2. Chris Titus script

[working since Windows 11 HOME edition 26220.6772 (Dev channel)]


Connect to the Internet.



Shift + F10. Type the command below:

```curl -L christitus.com/bypass -o skip.cmd```

---

### 3. Developer console method

(works in 25H2 as of 20 Oct 2025)

``Ctrl + Shift + J``  to enter developer console (Use ``Esc`` or ``Ctrl + Shift + J`` again to exit)


```WinJS.Application.restart("ms-cxh://LOCALONLY")```


or


```WinJS.Application.restart("ms-cxh:LOCALONLY")```

---

### 4. Using Registry Editor to enable local account

(Note: Untested in older versions, working in 25H2)


Press Shift + F10.


```reg add HKLM\Software\Microsoft\Windows\CurrentVersion\OOBE /v BypassNRO /t REG_DWORD /d 1 /f```


```shutdown /r /t 0```


---

### 5. Audit Mode (SysPrepTool)

Go to the Network screen.


Ctrl + Shift + F3


Let it boot up again.


Do NOT close the System Preparation Tool (SysPrepTool) window yet.

- If there is no SysPrepTool window, then open it with:

  - Win + R, then enter ``sysprep``, and run it.
  - If that takes you into a folder, then open ``sysprep.exe``.



Open a Command Prompt window and create a local Admin account:



```net user "YourName" /add```


``` net localgroup administrators "YourName" /add ```



In the SysPrepTool window, set the following options:

- Enter System Out-of-Box Experience (OOBE)
- Reboot



Click OK.


---


### 6. Manually create local account and force skip (Variation 1)

Shift + F10

> net.exe user 'username' 'password' /add
> (password can be left blank)

> net.exe localgroup Administrators 'username' /add
> taskkill /F /IM msoobe.exe

---


### 7. Manually create local account and force skip - (Variation 2)

⚠️ Patched in 25H2/26H1 - infinite loading / black screen (March 2026)


‼️ May create a ghost admin account which you don't have access to ``defaultuser0``. Refer to the fix at the very bottom.


Shift + F10


```net.exe user 'username' 'password' /add```

(password can be left blank if you don't want a password)


```net.exe localgroup Administrators 'username' /add```


```cd oobe```


```msoobe.exe && shutdown.exe -r```


---


### 8. Start installer in local-only mode

⚠️ Patched in Dev 26220.6772 & Beta 26120.6772 (6 Oct 2025)

```start ms-cxh:LOCALONLY```


or


```start ms-cxh://LOCALONLY```


or


```start ms-cxh://SETADDLOCALONLY```


---


### 9. Start installer in local-only mode

⚠️ Patched in Dev 26220.6772 & Beta 26120.6772 (6 Oct 2025)


```start ms-cxh:LOCALONLY```

or

```start ms-cxh://LOCALONLY```

or

```start ms-cxh://SETADDLOCALONLY```

---


### 10. Domain join (in W11 PRO version)

"Set up for work or school"


"Sign-in options"


"Domain join instead"

---


### 11. Bypass NRO command (don't connect to any interface)

⚠️ Patched in 24H2 (Mar 2025) Dev 26200.5516 & Beta 26120.3653 (Mar 28, 2025).

Do NOT connect to Wi-Fi, and unplug the Ethernet cable.


Shift + F10


```OOBE\BYPASSNRO```

---


### 12. Terminate Connection Flow

⚠️ Patched in 22H2 (22621.1 @ Sept 20, 2022) & Dev 22557.1 (Feb 16, 2022)


Shift + F10


```taskkill /F /IM oobenetworkconnectionflow.exe```

---


### 13. E-mail field bypass

⚠️ Patched in 24H2 (Dev 26100+ @ Apr 2024) + server-side patches, affecting ALL versions)


Input ``no@thankyou.com`` as the email.


---

## Quick fixes:

To forget a wireless network (after already connecting):

### 🟡 Forget a specific network (case-sensitive)

netsh wlan show profiles
netsh wlan delete profile name='Example SSID 123'

Reboot.

### 🟡 Forget all networks at once

netsh wlan delete profile name =* i =*

Reboot.

### 🟣 Enabling or disabling the hidden Administrator account (if you get stuck with no Administrator account):

Enter the recovery menu (hold Shift + restart).
Troubleshooting > Advanced Options > Command Prompt
Enable:
net user administrator /active:yes
Disable:
net user administrator /active:no
Reboot.

### 🟠 Delete DefaultUser0

🔸 If you added your account to the Administrator group:
Open cmd.
net user defaultuser0 /delete
rd /s /q "C:\Users\defaultuser0"

🔸 If you didn't add your account to the Administrator group or have no other account to sign in with:
Enter the recovery menu (hold Shift + restart).
Troubleshooting > Advanced Options > Command Prompt

Then,

[ OPTION A ] Delete via Registry Editor (deep clean)
regedit
Navigate to HKEY LOCAL MACHINE (HKLM), and select it.
File (top left) > Load Hive...
Navigate to C:\Windows\System32\config
Select SOFTWARE, click OK.
Name the key OFF_SW, click OK.

Navigate to HKEY LOCAL MACHINE (HKLM), and select it again.
File (top left) > Load Hive...
Navigate to C:\Windows\System32\config
Select SAM, click OK.
Name the key OFF_SAM, click OK.

In the Registry Editor, navigate to HKEY_LOCAL_MACHINE\OFF_SW\Microsoft\Windows NT\CurrentVersion\ProfileList.
Select ProfileList.
Look under the ProfileImagePath tab for the entries starting with S-1-5-....
Delete ONLY the one with C:\Users\defaultuser0.
‼️ Do NOT delete anything else.
Now, navigate to HKEY_LOCAL_MACHINE\OFF_SAM\SAM\Domains\Account\Users\Names
Delete the defaultuser0 folder.
‼️ Do NOT delete anything else.

Go back to OFF_SW, select it.
Then, File > Unload Hive

Go back to OFF_SAM, select it.
Then, File > Unload Hive

In the Command Prompt, type:
rd /s /q "C:\Users\defaultuser0"
Reboot.

[ OPTION B ] Add your user to the Administrator group.
net localgroup administrators "yourActualUsername" /add
Reboot, sign into your user.
Open cmd.
net user defaultuser0 /delete
rd /s /q "C:\Users\defaultuser0"
