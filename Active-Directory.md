## Active Directory Setup and Remote Connection.
### Enable Remote connection on the Server Machine
```console
Enable-PSRemoting
```

### On Client Machine.
**Add user to trusted hosts.**
```console
get-item wsman:\localhost\Client\TrustedHosts
```
**Result.**
```console
Start WinRM Service
WinRM service is not started currently. Running this command will start the WinRM service.

Do you want to continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
get-item : Cannot find path 'WSMan:\localhost\Client\TrustedHosts' because it does not exist.
At line:1 char:1
+ get-item wsman:\localhost\Client\TrustedHosts
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (WSMan:\localhost\Client\TrustedHosts:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand
```

**The above commands did not succeed, but it can be solved by starting again terminal with admin privileges and type the following commands.**

```console
PS C:\Users\local_admin> get-item wsman:\localhost

Start WinRM Service
WinRM service is not started currently. Running this command will start the WinRM service.

Do you want to continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


   WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan

ComputerName                                  Type
------------                                  ----
localhost                                     Container
```

**Start WinRM Service**

```console
PS C:\Users\local_admin> Start-Service WinRM
```

**Continue**

```console
PS C:\Users\local_admin> ls wsman:\localhost


   WSManConfig: Microsoft.WSMan.Management\WSMan::localhost

Type            Name                           SourceOfValue   Value
----            ----                           -------------   -----
System.String   MaxEnvelopeSizekb                              500
System.String   MaxTimeoutms                                   60000
System.String   MaxBatchItems                                  32000
System.String   MaxProviderRequests                            4294967295
Container       Client
Container       Service
Container       Shell
Container       Listener
Container       Plugin
Container       ClientCertificate
```

**Continue**

```console
PS C:\Users\local_admin> ls wsman:\localhost\Client


   WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Client

Type            Name                           SourceOfValue   Value
----            ----                           -------------   -----
System.String   NetworkDelayms                                 5000
System.String   URLPrefix                                      wsman
System.String   AllowUnencrypted                               false
Container       Auth
Container       DefaultPorts
System.String   TrustedHosts
```

**Continue**

```console
PS C:\Users\local_admin> ls wsman:\localhost\Client\TrustedHosts


   WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Client

Type            Name                           SourceOfValue   Value
----            ----                           -------------   -----
System.String   TrustedHosts
```
#### Connect to Remote Machine.
**Set  Value of the remote Machine IP Address.**

```console
PS C:\Users\local_admin> set-item wsman:\localhost\Client\TrustedHosts -value  192.168.225.131
```

**Set New Session.**

```console
PS C:\Users\local_admin> New-PSSession -ComputerName 192.168.225.131 -Credential (Get-Credential)

cmdlet Get-Credential at command pipeline position 1
Supply values for the following parameters:
Credential

 Id Name            ComputerName    ComputerType    State         ConfigurationName     Availability
 -- ----            ------------    ------------    -----         -----------------     ------------
  5 WinRM5          192.168.225.131 RemoteMachine   Opened        Microsoft.PowerShell     Available
```

**Connect to  Remote via Session  Number.**
```console
PS C:\Users\local_admin> Enter-PSSession 5
[192.168.225.131]: PS C:\Users\Administrator\Documents> whoami
win-4tkcb5o73s1\administrator
```

**Alternatively you can use the following command to connect direct to the remote machine**

```console
PS C:\Users\local_admin> Enter-PSSession 192.168.225.131 -Credential (Get-Credential)
```
## Install Domain Controller.
1. Use `sconfig`  to:
	- Change the hostname.
	- Change the IP Address to Static.
	- Change the DNS server to our own IP Address.
	
2. Install Active Directory Windows Feature.
```console
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

3. Import module for the powershell on the Server machine.
```console
import-Module ADDSDeployment
```

4. Install Forest.

Provide the name for your Domain.

```console
PS C:\Users\Administrator> Install-ADDSForest

cmdlet Install-ADDSForest at command pipeline position 1
Supply values for the following parameters:
DomainName: xyz.com
SafeModeAdministratorPassword: *************
Confirm SafeModeAdministratorPassword: *************

The target server will be configured as a domain controller and restarted when this operation is complete.
Do you want to continue with this operation?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
```

### Change DNS Server Address on the Server Machine.
1. Check for the IPAddress of the Server.

```console
PS C:\Users\Administrator> Get-NetIPAddress -IPAddress 192.168.225.155

IPAddress         : 192.168.225.155
InterfaceIndex    : 5
InterfaceAlias    : Ethernet0
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore
```

2. Check the Index of Interface to see if the address match to that of the server.

```console
PS C:\Users\Administrator> Get-DNSClientServerAddress

InterfaceAlias               Interface Address ServerAddresses
                             Index     Family
--------------               --------- ------- ---------------
Ethernet0                            5 IPv4    {127.0.0.1}
Ethernet0                            5 IPv6    {::1}
Loopback Pseudo-Interface 1          1 IPv4    {}
Loopback Pseudo-Interface 1          1 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
```

3. Change the IPAddress of the specified index number for interface.

```console
PS C:\Users\Administrator> Set-DNSClientServerAddress -InterfaceIndex 5 -ServerAddresses 192.168.225.155
```

4. Verify again to see if the index and the IPAddress match the DNS IPAddress.

```console
PS C:\Users\Administrator> Get-DNSClientServerAddress

InterfaceAlias               Interface Address ServerAddresses
                             Index     Family
--------------               --------- ------- ---------------
Ethernet0                            5 IPv4    {192.168.225.155}
Ethernet0                            5 IPv6    {::1}
Loopback Pseudo-Interface 1          1 IPv4    {}
Loopback Pseudo-Interface 1          1 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
```

### Power up Client Machine to connect to the domain.
1. Check for the IPAddress of the Server.

```console
PS C:\Users\local_admin> Get-DNSClientServerAddress

InterfaceAlias               Interface Address ServerAddresses
                             Index     Family
--------------               --------- ------- ---------------
Ethernet0                           13 IPv4    {192.168.225.2}
Ethernet0                           13 IPv6    {}
Loopback Pseudo-Interface 1          1 IPv4    {}
Loopback Pseudo-Interface 1          1 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
```

2. Change the IPAddress of the specified index number for interface.

```console
Set-DNSClientServerAddress -InterfaceIndex 13 -ServerAddresses 192.168.225.155
```

3. Verify again to see if the index and the IPAddress match the DNS IPAddress.

```console
PS C:\Users\local_admin> Get-DNSClientServerAddress

InterfaceAlias               Interface Address ServerAddresses
                             Index     Family
--------------               --------- ------- ---------------
Ethernet0                           13 IPv4    {192.168.225.155}
Ethernet0                           13 IPv6    {}
Loopback Pseudo-Interface 1          1 IPv4    {}
Loopback Pseudo-Interface 1          1 IPv6    {fec0:0:0:ffff::1, fec0:0:0:ffff::2, fec0:0:0:ffff::3}
```

5. Connect to Domain

```console
PS C:\Users\local_admin> Add-Computer -DomainName xyz.com -Credential xyz\Administrator -Force -Restart
```