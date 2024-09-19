# Server-Enterprise-Install-Guide
**ABOUT** <br>
LTSC (Long-Term Servicing Channel) and Windows Server editions are streamlined versions of Windows that focus on stability and long-term support.
They receive no feature updates, only security updates, and are supported for up to 10 years.
These editions are designed to minimize bloat, making them ideal for enterprise environments and servers where stability and security are prioritized over new features.
Assuming you don't need certain Windows features, these editions are excellent for gamers who want a minimal, bloat-free, and stable operating system.

**DOWNLOADS**
- **W10 Server 21h2 2022** <br>
https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022 <br>
Click "Download the ISO" <br>
Enter fake information <br>
Click "ISO download, 64-bit edition"

- **W11 Server 24h2 2025**  <br>
https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025 <br>
Click "Download the ISO" <br>
Enter fake information <br>
Click "ISO download, 64-bit edition"

- **W10 LTSC 21h2 2021** <br>
https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise <br>
Click "Download the ISO – LTSC Enterprise" <br>
Enter fake information <br>
Click "ISO – Enterprise LTSC downloads, 64-bit edition"

- **W11 LTSC 23h2 2024** <br>
https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-iot-enterprise-ltsc <br>
Click "Download Windows 11 IoT Enterprise LTSC" <br>
Enter fake information <br>
Click "ISO - Windows 11 IoT Enterprise LTSC 2024 Evaluation, x64 or AMD64 edition"

**BURN TO USB**
- **Rufus** <br> 
https://rufus.ie/en/ <br>
Click and download "Portable, Windows x64" <br>
Plug in USB <br>
Open Rufus <br>
Select USB <br>
Select ISO <br>
Click start <br>
Leave "Windows User Experience" settings blank or you will brick the install <br>
Disable bitlocker in windows settings <br>

**SERVER PREPARE ETHERNET DRIVERS**
- https://www.thomas-krenn.com/de/wiki/Intel_i225-V_und_i226-V_Treiber_in_Windows_Server_2022_installieren <br>
- [i225 i226.zip](https://github.com/fr33thytweaks/Server-Enterprise-Install-Guide/raw/main/i225%20i226.zip)
- Some ethernet cards wont work on server such as "i225-V, i226-V"
Download the latest driver for your ethernet card, extract it <br>
Edit "e2f.inf" in notepad <br>
Under **"[Intel.NTamd64.10.0...17763]"** add . . .

- **%E15F3NC.DeviceDesc% = E15F3.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_01 <br>
%E15F3_2NC.DeviceDesc% = E15F3_2.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_02 <br>
%E15F3_3NC.DeviceDesc% = E15F3_3.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_03 <br>
%E125CNC.DeviceDesc% = E125C.10.0.1..17763, PCI\VEN_8086&DEV_125C**

- A similar method for other ethernet cards can be used if you find the device manager ID'S <br>
Save file and drag driver files to the USB Stick

- As these drivers are unsigned, they will need to be installed with driver signature off <br> <br>
**OFF** <br>
In cmd run <br>
bcdedit -set loadoptions DISABLE_INTEGRITY_CHECKS <br>
bcdedit -set TESTSIGNING ON <br>
bcdedit -set NOINTEGRITYCHECKS ON <br>
Restart <br>
Install driver in device manager

- **ON** <br> 
In cmd run <br>
bcdedit /deletevalue loadoptions <br>
bcdedit -set TESTSIGNING OFF <br>
bcdedit -set NOINTEGRITYCHECKS OFF <br>
Restart

**TO BIOS**
- Restart PC, spam F2 or DEL and go to bios <br>
Enable TPM (for W11) <br>
Disable secure boot (for some oem prebuilts & laptops) <br>
Change boot order to USB <br>
Install the ISO (Server users, install desktop version) <br>
LTSC users, be sure to use "domain join instead" during setup <br>
Install network driver

**SERVER CONVERT EVAL TO NORMAL EDITION**
- In powershell run <br>
**Install-WindowsFeature -Name VolumeActivation -IncludeAllSubFeature –IncludeManagementTools**

- W10 Server 21h2 2022 <br>
**dism /online /set-edition:ServerStandard /productkey:VDYBN-27WPP-V4HQT-9VMD4-VMK7H /accepteula** <br>
- W11 Server 24h2 2025 <br>
**dism /online /set-edition:ServerStandard /productkey:TVRH6-WHNXV-R9WG3-9XRFY-MY832 /accepteula** <br>
Restart

**LTSC CONVERT EVAL TO NORMAL EDITION**
- https://github.com/victorlish/Convert_to_Windows_10_LTSC <br> <br>
- [Convert LTSC Eval.zip](https://github.com/fr33thytweaks/Server-Enterprise-Install-Guide/raw/main/Convert%20LTSC%20Eval.zip) <br>
Extract <br>
Run "run.bat" as admin

**ACTIVATE**
- https://github.com/massgravel/Microsoft-Activation-Scripts <br>
https://massgrave.dev/kms38 <br> <br>
- In powershell run <br>
**irm https://get.activated.win | iex** <br>
Select 3, then 1

**GEFORCE NOT WORKING ON W10 Server 21h2 2022**
- In powershell run <br>
**Install-WindowsFeature -Name Wireless-Networking**

**AMD/INTEL CHIPSET DRIVERS NOT INSTALLING/WORKING**
- Extract files <br>
On AMD these may be already extracted if you ran the installer <br>
Found in "C:\AMD\Chipset_Software\Packages\IODriver" <br>
Install in device manager
 
**AMD GRAPHICS DRIVERS NOT INSTALLING/WORKING**
- Extract files <br>
Install in device manager

**XBOX CONTROLLER/DS4 NOT WORKING**
- [ViGEmBus Driver.zip](https://github.com/fr33thytweaks/Server-Enterprise-Install-Guide/raw/main/ViGEmBus%20Driver.zip) <br>
[Xbox Driver.zip](https://github.com/fr33thytweaks/Server-Enterprise-Install-Guide/raw/main/Xbox%20Driver.zip)

**OTHER DRIVERS**
- Snappy Driver Installer Origin <br>
https://www.glenn.delahoy.com/snappy-driver-installer-origin/

**NEED WINDOWS STORE**
- In powershell run <br>
wsreset -i

**DISABLE SERVER MANAGER ON BOOT**
![image](https://github.com/user-attachments/assets/3500f7f6-0ced-4524-b8ef-316d167da885)

**REMOVE PASSWORD REQUIREMENT** <br>
Run "gpedit.msc" <br>
![image](https://github.com/user-attachments/assets/ec992915-c0a7-498f-800f-e76164d6a208)

Change new password to blank
![image](https://github.com/user-attachments/assets/d2e98128-369d-4f74-abd3-6bdf40dc058c)

**DISABLE SHUTDOWN EVENT TRACKER** <br>
Run "gpedit.msc"
![image](https://github.com/user-attachments/assets/c33d3828-a006-4178-ad1e-13a626489c2d)

## Video
[Video](<https://youtu.be/jpKIiimS9Ho>)

[![Video](https://img.youtube.com/vi/jpKIiimS9Ho/maxresdefault.jpg)]([https://www.youtube.com/watch?v=jpKIiimS9Ho](https://youtu.be/jpKIiimS9Ho))
