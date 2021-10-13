## Active Directory Recipes

### List all domain controllers
```
Get-ADComputer -Filter 'primarygroupid -eq "516"' -Properties Name,OperatingSystem,OperatingSystemVersion,IPv4Address |Sort-Object -Property OperatingSystem|Select-Object -Property Name,OperatingSystem,OperatingSystemVersion,IPv4Address
```

### List computers added by non-domain admins
```
Get-ADComputer -LDAPFilter "(ms-DS-CreatorSID=*)" -Properties ms-DS-CreatorSID
```

### List users group memebership
```
Get-ADPrincipalGroupMembership <user>|select name, groupcategory, groupscope
```

### Add user to Group
```
Add-GroupMember -Identity <GROUPNAME> -Members <USER1>,<USER2>,<USER3>
```

### List all memebers of security group
``` 
Get-ADGroupMember <GROUPNAME> |Select SamAccountName | Sort-Object -Property @{Expression="<GROUPNAMEPROPERTY>"; Descending=$True} 
```

### List all members of an OU
``` $ouPath = 'OU=Business Office,OU=YVCC Workstations,DC=YVCC,DC=local'
Get-ADComputer  - Filter *  -SearchBase $ouPath | select SamAccountName 
```

### Search AD by ObjectSID
*sometimes an account might not exist by its SamAccountName because it has been deleted, so you can search by objectSID instead.*
``` 
Get-ADObject -IncludeDeletedObjects -Filter * -Properties * | where{$_.objectSid -eq 'S-1-5-21-761688594-1769347782-925700815-502'}
```

### Remove a user from all security groups
```
Get-ADUser -Identity <USER> -Properties MemberOf |ForEach-Object {$_.MemberOf | Remove-ADGroupMember -Members $_.DistinguishedName}
```
