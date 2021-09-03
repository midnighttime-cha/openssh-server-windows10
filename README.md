# วิธีติดตั้ง OpenSSH โดยใช้ PowerShell บน Windows10

## ก่อนอื่นต้อง run PowerShell ด้วย `Run as Administrator` ก่อน จากนั้นใช้คำสั่งต่อไปนี้ เพื่อตรวจสอบ Package ในการติดตั้ง
```cmd
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```
---Output---
```
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

## ทำการติดตั้ง OpenSSH-Server และ OpenSSH-Client ด้วยคำสั่งต่อไปนี้
```cmd
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
---Output---
```
Path          :
Online        : True
RestartNeeded : False
```

## ทำการ Start service ด้วยคำสั่งต่อไปนี้
คำสั่ง start sshd service
```cmd
Start-Service sshd
```
คำสั่งให้ sshd ทำการ Restart ตัวเองอัตโนมัติ
```cmd
Set-Service -Name sshd -StartupType 'Automatic'
```
ตรวจสอบ Firewall ว่ามี Service ดังกล่าว อยู่หรือป่าว
```cmd
Get-NetFirewallRule -Name *ssh*
```
คำสั่ง Enable `OpenSSH-Server-In-TCP`
```cmd
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

# อ้างอิง
- (https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)[Install OpenSSH using PowerShell]
