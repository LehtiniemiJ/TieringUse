# TieringUse

This document provides an overview of the tiering use and the related components.

## Tiering
From Truesec Materials:  
![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/04600c25-05b8-4133-a25f-c4bf0fe8838d)



Tiering is part of Zero-trust model  
 - Assume that all other components have been breached, thus using high impact credentials on these systems would endanger whole environment  
   - Least privilege access  
   - Running unnecesary priviledges when performing day-to-day operations cause high risk for priviledge escalation when environment is under attack  
 - Implementation of basic tiering model is effective protection against ransomware and lateral movement in the environment.  

### T0

T0 includes most of the critical resources; Domain Controller, PKI, 

### T1

Production servers should be placed in T1
When LAPS is in use the old local passwords no longer work. If needed local admin use LAPS password. If doing normal admin works use T1 Admin account that has admin privileges in the machine.  For example use LAPS password if server disconnects from domain.

- SCCM
- 

### TE

TE is tier dedicated for endpoints, TE account is not an admin account. The account is used only to loginto the PAW/jump stations. should be the account used to access this tier. The Endpoints should be accessed with the TE user and processes ran with  Local admin account LAPS password. 

## PAW - Jumpstation


### t0 - Silo 

Description of t0 - Silo.

### RDP

Tiering – how to create a RDP shortcut

This instruction is used to create a RDP shortcut (settings) for use with jumphosts in a tiered (SILO) environment.  
Settings  
There are two settings needed to connect via RDP to a SILO:ed jumphost. In Microsofts RDP client config (.rdp) file:  
```
authentication level:i:2  
enablecredsspsupport:i:0  
```
There are equivalent settings in some other RDP clients, for example mRemoteNG.  
Create RDP file  
1.	Start ”Remote Desktop Connection” (mstsc.exe)  
 ![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/ab5954d4-510b-4109-8a30-fab450496058)

2.	Enter the jumphost fqdn or ip address and choose “Save as” to save the file  
 ![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/c996c218-4496-470c-8f89-72b7a2083be9)
3.	Open the newly created RDP file in a text editor, like notepad,exe, and change/add these values:  
 ```
 authentication level:i:2  
 enablecredsspsupport:i:0
```
4.	Save
Using mRemoteNG  
In mRemoteNG, the equivalent to ”authentication level” (1) and ”enablecredsspsupport” (2) is this:  
![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/d45d7ae7-5783-4f2e-9d73-4bc57ab299a2)

 
Troubleshooting
For connection errors:
•	check the CredSSP settings on the client side
•	check that NLA is disabled on the jumphost
•	make sure the jumphost have been restarted atleast twice after applying the jumphost GPO:s
•	try using ip address or NetBIOS name instead of FQDN. 

## Credentials

### EXT

Details about EXT.

### FTE

Information about FTE.
