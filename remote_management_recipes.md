## Remote Management Recipes

### Enter PSSession without a machine profile
*If you exclude the 'nomachineprofile' option, PSSession will leave your profile on the target machine.*
``` 
$creds = Get-Credential
$options = New-PSSessionOption -nomachineprofile

New-PSSession -ComputerName <TargetComputerName> -Name <SessionName> -SessionOption $options -Credentials $creds
Enter-PSSession -Name <SessionName>

Disconnect-PSSession -Name <SessionName>
Remove-PSSession -Name <SessionName>
```

### See who's logged on to a workstation/server
```
$ Get-WMIObject -class Win32_ComputerSystem -ComputerName <COMPUTERNAME> -Credential(Get-Credential)
```

### Detect Open Shares from a list of computers
```
$servers = get-content c:\temp\servers.txt
```

### Provide an account that has rights to enumerate the shares
```
$cred = get-credential
get-wmiobject Win32_Share -computer $servers -credential $cred | select server,name,description,path | out-GridView
```

### Find Renamed Local Admin Account
```
Add-Type -AssemblyName System.DirectoryServices.AccountManagement
$principalContext = New-Object System.DirectoryServices.AccountManagement.PrincipalContext([System.DirectoryServices.AccountManagement.ContextType]::Machine)
$userPrincipal = New-Object System.DirectoryServices.AccountManagement.UserPrincipal($principalContext)
$searcher = New-Object System.DirectoryServices.AccountManagement.PrincipalSearcher
$searcher.QueryFilter = $userPrincipal
$searcher.FindAll() | Where-Object { $_.Sid -Like "*-500" } | Select-Object SamAccountName
```

### See all members of local admin group
```
C:\> net localgroup administrators
```

### GPO

```


(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey

