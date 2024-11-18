### Showmount Command

    showmount -e <IP Address> 

### mounts the share to /mnt/nfs without locking it

    mount <IP Address>:/vol/share /mnt/nfs  -nolock

### Mount Windows CIFS / SMB share on Linux at /mnt/cifs if you remove password it will prompt on the CLI (more secure as it wont end up in bash_history) 

    mount -t cifs -o username=user,password=pass,domain=blah //<IP Address>/share-name /mnt/cifs

### Mount a Windows share on Windows from the command line 

    net use Z: \\<IP Address>\share password  /user:domain\janedoe /savecred /p:no 

## Install smb4k on Kali, useful Linux GUI for browsing SMB shares

    apt-get install smb4k –y
