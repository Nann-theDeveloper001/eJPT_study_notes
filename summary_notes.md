**login with rcpcclient**

>enumdomusers <list down all the exisiting users on the samba server

>lookupnames “admin”

>srvinfo <list out the server information inclu: OS ver>

```bash
# Anonymous connection (-N=no pass)
rpcclient -U “” -N <ip>

# Connection with user
rpcclient -U “user” <ip>

# Get information about the DC
srvinfo

# Get information about objects such as groups (enum*)
enumdomains
enumdomgroups
enumalsgroups builtin

# Get username for a defined user ?
getusername



**enum4linux —> samba server enumeration**


```bash
# Pulls OS information using smbclient, this can pull the service pack version on some versions of Windows
enum4linux -o target-ip
```

```bash
# List groups
enum4linux -G target-ip

# List shares
enum4linux -S target-ip
```

### General Information

`SMB1 => Win2000 / XP / 2003
SMB2.0 => Vista / 2008
SMB2.1 => Win7 / 2008R2
SMB3.0 => Win8 /  2012
SMB 3.02 => Win8.1 / 2012R2

# Configuration tips
# Can be usefull to configure /etc/samba/smb.conf with:
client min protocol = SMB2
client max protocol = SMB3

# Then
service smbd restart`

### Identification

`# Port 139
# Using nbtscan to identify host/domain
nbtscan IP   (identifier le nom/domaine)

# Identity SMB2 support using metasploit
use auxiliary/scanner/smb/smb2
set RHOST IP
run

# Discover real samba version if hidden     
ngrep -i -d tap0 ‘s.?a.?m.?b.?a.*[[:digit:]]’ & smbclient -L //IP

### Services and Resources Scanning

# Base nmap
nmap -v --script=xxxx -p T:139,445 <IP>

# Hard nmap
nmap -n -sV --version-intensity=5 -sU -sS -Pn -p T:139,445,U:137 --script=xxx <IP>

# SMB Relate NSE Scripts
# Try to retrieve NetBIOS and MAC
nbstat

# Enum
smb-enum-domains
smb-enum-groups
smb-enum-processes
smb-enum-sessions
smb-os-discovery
smb-server-stats
smb-system-info

# Attempts to retrieve useful information about files shared on SMB volumes
smb-ls

# Queries information managed by the Windows Master Browser
smb-mbenum

# Try to print something
smb-print-text

# Get security level information about SMB
smb-security-mode

# Vulns
smb-vuln-conficker (dangerous, can crash target)
smb-vuln-ms06-025 (Buffer overflow in RRAS)
smb-vuln-ms07-029 (Buffer overflow which can crash the RPC intrface in the DNS Server)
smb-vuln-ms08-067 (Buffer overflow/RCE. Dangerous, can crash the target)
smb-vuln-ms10-054 (Remote Memory Corruption. Result is BSOD -> DANGEROUS)
smb-vuln-ms10-061 (Print vulnerability. Safe and can\'t crash the target)
smb-vuln-ms17-010 (RCE, just checking if vulnerable)`

### Enumeration

`# Get NetBIOS from IP
nmblookup -A <IP>

# Enumeration using enum4linux
enum4linux -a -R 500-600,950-1150  (identifier le nom/domaine + users + shares)

# Smbclient
# List shares
smbclient -L //IP
smbclient -L <ip>

# Connect
smbclient \\\\x.x.x.x\\share
smbclient -U “DOMAINNAME\Username” \\\\IP\\IPC$ password

# Specify username and no pass
smbclient -U “” -N \\\\IP\\IPC$`