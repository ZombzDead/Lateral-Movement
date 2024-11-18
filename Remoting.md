
## File Share Access 

If file shares are available on a box you can use dir on the path without ever getting prompted for a password given that you already have a shell as the user you want to authenticate as.
    dir \\BOX01\c$\

### Valid Creds - Runas 

    /netonly : Indicates that the user information specified is for remote access only. 
Run the following command to execute PowerShell and get on a remote box

    runas /netonly /FQDN\user cmd.exe 

Run the following command to execute PowerShell and get on a remote box 
    
    runas /netonly /user:customer\ank powershell 

## Valid Creds - Wmic

Using WMIC, check which users have sessions on a remote machine. Local administrator privileges are not necessary to execute this remotely.

    wmic /node:box01.lab.local path win32_loggedonuser get antecedent

## Winrm 

Execute a command on a remote box the current user has access to only using tokens, (no prompt for password or PTH). Here the hostname command is used for verification, but you can essentially replace it with any command, like powershell.

    Invoke-Command -ComputerName BOX01 -Scriptblock {hostname}
