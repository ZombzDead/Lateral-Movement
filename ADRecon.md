# ADRECON

ADRecon is a tool which extracts and combines various artefacts (as highlighted below) out of an AD environment. The information can be presented in a specially formatted Microsoft Excel report that includes summary views with metrics to facilitate analysis and provide a holistic picture of the current state of the target AD environment.

Reference: https://github.com/adrecon/ADRecon

## Recon on Domain Member Host
    
    PS C:\>.\ADRecon.ps1

To run ADRecon on a domain member host as a different user.
Dumps content in tools/adrecon/Adrecon-report

## Recon on Non-Domain Member Host using LDAP

    PS C:\>.\ADRecon.ps1 -Method LDAP -DomainController <IP or FQDN> -Credential <domain\username> 

To run ADRecon on a non-member host using LDAP 

## Recon on Domain Member Host as different user

    PS C:\>.\ADRecon.ps1 -DomainController <IP or FQDN> -Credential <domain\username>

## Specify Module

    PS C:\>.\ADRecon.ps1 -Method ADWS -DomainController <IP or FQDN> -Credential <domain\username> -Collect Domain, DomainControllers

To run ADRecon with specific modules on a non-member host with RSAT. (Default OutputType is STDOUT with -Collect parameter)

## Create Excel Report

    PS C:\>.\ADRecon.ps1 -GenExcel C:\ADRecon-Report-<timestamp>

To generate the ADRecon-Report.xlsx based on ADRecon output (CSV Files).

## Help Menu

    Get-Help .\ADRecon

## Collects User and exports to excel report

    PS C:\>.\ADREcon.ps1 -Protocol LDAP -DomainCOntroller <ip> -Credential <domainname>\<username> -Collect Users -OutputType Excel
