## Creating Users And Groups.
In order to add a large number of users in Active Directory, there should be database which stores user details and script which will help the automation process of creating new users and add them into their specific groups. To do this a `Json`  and `Poweshell` file with  user information was created. `Json` file acts as a database with user details while `Powershell` script is used to automate the whole proccess.

1. **Json** file contains the followings:
	- [ ] **Domain name.**
	- [ ] **Groups.**
		- [ ] name.
	- [ ] **Users.**
		- [ ] names.
		- [ ] Passwords.
		- [ ] Groups.

2. **Powershell** script Operate as follows:
	- [ ] It takes an argument to pass the Json file.
	- [ ] Create new Active Directory Group for users.
	- [ ] Create new Active Directory Users.
	- [ ] Add User to his/her appropriate group.
	- [ ] Converts the the data FromJson.
	- [ ] Create Active Directory Users recursively.
	- [ ] Create Active Directory Groups recursively.
	
 You can check both `Json` and `Powershell` scripts in the following folder [code](https://github.com/gemstone-source/Active-Directory/tree/main/code).
 
**_Remember:_** _All  codes were written in the Management Machine which is Windows 11 GUI Machine and uploaded to Remote Machine._

### Connect to Remote Server through Management Machine.
#### Create variables for easy connection in next time.
1. Create a variable that accept New-PSSession.
```
$dc = New-PSSession 192.168.225.155 -Credential (Get-Credential)^C
```
2. Create variable that accepts Remote Machine Credentials.
```
$creds = (Get-Credential)
```
3. Create new `$dc` variable that concatenate session variable and credential variable.
```
$dc = New-PSSession 192.168.225.155 -Credential $creds
```
#### Connect to Remote Machine.
1. Change `New-PSSession` to `Enter-PSSession`.
```
$dc = Enter-PSSession 192.168.225.155 -Credential $creds
```
2. Copy both `Json` and `Powershell` script into remote Machine.
```
PS C:\Users\local_admin\Desktop\Active Directory\code> cp .\gen_ad.ps1 -ToSession $dc C:\Windows\Tasks
```
3. Connect to Session `$dc`
```
PS C:\Users\local_admin\Desktop\Active Directory\code> Enter-PSSession $dc
[192.168.225.155]: PS C:\Users\Administrator\Documents> 
```
4. Navigate to Tasks directory where `Json` and `Powershell` script have been uploaded.
```
[192.168.225.155]: PS C:\Users\Administrator\Documents> cd C:\Windows\Tasks

[192.168.225.155]: PS C:\Windows\Tasks> ls

	Directory: C:\Windows\Tasks

Mode LastWriteTime Length Name
---- ------------- ------ ----

-a---- 6/30/2022 8:54 AM 459 ad_schema.json

-a---- 6/30/2022 8:51 AM 1617 gen_ad.ps1
```
Both files can be seen as successfully uploaded to remote Machine.

#### Add Users to Active Directory.
Execute `Powershell` script and pass `Json` file as an argument.
```
[192.168.225.155]: PS C:\Windows\Tasks> .\gen_ad.ps1 .\ad_schema.json

DistinguishedName : CN=Employee,CN=Users,DC=xyz,DC=com
GroupCategory     : Security
GroupScope        : Global
Name              : Employee
ObjectClass       : group
ObjectGUID        : 3a97fcbc-9f50-47c8-bb2f-6b51569bc78f
SamAccountName    : Employee
SID               : S-1-5-21-2354715860-4278133880-137312399-1613

DistinguishedName : CN=Employee,CN=Users,DC=xyz,DC=com
GroupCategory     : Security
GroupScope        : Global
Name              : Employee
ObjectClass       : group
ObjectGUID        : 3a97fcbc-9f50-47c8-bb2f-6b51569bc78f
SamAccountName    : Employee
SID               : S-1-5-21-2354715860-4278133880-137312399-1613
```

#### Verify if new Users have been added to the Active Directory.
Use `Get-user` command and filter all with `*` to see all Active Directory users
```
[192.168.225.155]: PS C:\Windows\Tasks> Get-ADUser

cmdlet Get-ADUser at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
Filter: *

DistinguishedName : CN=Administrator,CN=Users,DC=xyz,DC=com
Enabled           : True
GivenName         :
Name              : Administrator
ObjectClass       : user
ObjectGUID        : e505268c-5859-4f74-a14f-cff5375c827f
SamAccountName    : Administrator
SID               : S-1-5-21-2354715860-4278133880-137312399-500
Surname           :
UserPrincipalName :

DistinguishedName : CN=Guest,CN=Users,DC=xyz,DC=com
Enabled           : False
GivenName         :
Name              : Guest
ObjectClass       : user
ObjectGUID        : 1e8ceef6-9c08-4569-8a8a-0bdcb870962a
SamAccountName    : Guest
SID               : S-1-5-21-2354715860-4278133880-137312399-501
Surname           : 
UserPrincipalName :

DistinguishedName : CN=krbtgt,CN=Users,DC=xyz,DC=com
Enabled           : False
GivenName         :
Name              : krbtgt
ObjectClass       : user
ObjectGUID        : 757bf4d5-a4e3-449b-b66b-a65ea9b1371a
SamAccountName    : krbtgt
SID               : S-1-5-21-2354715860-4278133880-137312399-502
Surname           :
UserPrincipalName :

DistinguishedName : CN=Alice Lice,CN=Users,DC=xyz,DC=com
Enabled           : True
GivenName         : Alice
Name              : Alice Lice
ObjectClass       : user
ObjectGUID        : 87419f6b-66fb-497f-bfdc-d90f40b71413
SamAccountName    : alice
SID               : S-1-5-21-2354715860-4278133880-137312399-1614
Surname           : Lice
UserPrincipalName : alice@xyz.com

DistinguishedName : CN=Bob Ob,CN=Users,DC=xyz,DC=com
Enabled           : True
GivenName         : Bob
Name              : Bob Ob
ObjectClass       : user
ObjectGUID        : feabe6db-4192-4dad-9ca4-1a0ea982da28
SamAccountName    : bob
SID               : S-1-5-21-2354715860-4278133880-137312399-1615
Surname           : Ob
UserPrincipalName : bob@xyz.com
```

#### Check for Group Employee.
```
[192.168.225.155]: PS C:\Windows\Tasks> Get-ADGroup -Identity Employee  

DistinguishedName : CN=Employee,CN=Users,DC=xyz,DC=com
GroupCategory     : Security
GroupScope        : Global
Name              : Employee
ObjectClass       : group
ObjectGUID        : 3a97fcbc-9f50-47c8-bb2f-6b51569bc78f
SamAccountName    : Employee
SID               : S-1-5-21-2354715860-4278133880-137312399-1613
```

#### Connect to Domain.
Start up any new Machine that is is connected to the Domain and login as User `bob` or `alice` with password `P@ssw0rd789`

