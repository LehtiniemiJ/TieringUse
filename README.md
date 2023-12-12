# TieringUse

This document provides an overview of the tiering use and the related components.
Tiering uses Restricted Admin Mode for protecting the RDP in Tn(T1,T2,T3,etc..)

## Restricted Admin Mode (RAM)

The goal is to prevent laterall movement within the tier. The proper wey of doing this is to prevent RPD from sending the credentials to the target device over RDP connections.

![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/4081014d-86eb-4cee-b8ba-edb84ed8c1dc)


Restricted Admin Mode was indeed introduced for Windows 8.1 and Server 2012 R2 to enhance security by preventing the storage of an RDP user’s credentials in memory on the machine to which an RDP connection is made.
  
When you log in with admin credentials using RAM, you will have the same permissions on the RDP session as the account you used to log in. However, if you try to access other network resources from the remote host during the RDP session, those resources will see the connection as coming from the remote host’s account, not the domain admin account you used to establish the RDP session.

<details>
  <summary>Example</summary>

  
 An admin using Tier 0 (T0) Admin credentials jumps from a Privileged Access Workstation (PAW) to a non-primary Domain Controller (DC), they face issues when trying to use the Group Policy Object (GPO) editing tool.  
This is because the GPO editing tool tries to authenticate to the Primary Domain Controller (PDC) using the credentials on the DC. However, in Restricted Admin Mode (RAM), the DC can’t authenticate any further, which results in the buttons being grayed out.  

 --- 
</details>




This is because in RAM, the credentials used to establish the RDP session are not used when accessing other network resources from the remote host. Instead, the identity of the remote host is used. This helps to protect your domain admin credentials by not exposing them over the network to the remote device.

So, while you can perform administrative tasks on the remote host with your domain admin credentials, you won’t be able to use those same credentials to access other network resources during the RDP session. This is a security feature designed to limit the exposure of sensitive credentials.

If an admin account is created for tier 1, it doesn’t have permissions by default to move from the PAW to an endpoint if it doesn’t have admin permission on the targeted machine. So, if you’re trying to log on to a device that is in the “Database Server - OU” and the account only has admin access to “Generic Server - OU”, you cannot make the jump. This is useful for limiting the permissions by principle of least privilege (PoLP) in the tier. This way, the potential damage is limited if an account is compromised.  
  
This approach is part of a broader strategy for effective privilege management, which is a key aspect of organizational security. It helps to limit the potential for unauthorized access and reduces the attack surface.  
 
This approach is indeed highly useful in environments where developers need high-level access to a DB server that is in the IT infrastructure, but their access to non-relevant IT resources are restricted. Similarly, access from IT admins to the development environment can be limited.  

This is a practical application of the principle of least privilege, which is all about giving users only the access that they need to perform their jobs and no more. In an ideal scenario, production, development, and IT would be separate entities, each with its own set of access controls and privileges. This not only enhances security but also helps in maintaining a clean and organized IT infrastructure.

Downsides:  
 - This prevents users from doing jump after jump with RDP  
Upsides:
 - SSO-like signin, secure.

   
### Restricted Admin Mode 
Prevents Authentication if one does not have admin permission on the target.

~~Downsides~~ Upsides:  
 - Causes a headache when trying to use the environment like one did before they had tiering implemented.
 - Credentials aren't sent to the remote host ()
 - The Remote Desktop session connects to other resources as the remote host's identity
 - An attacker can't act on behalf of the user and any attack is local to the server


#### OFF TOPIC Remote Credential Guard(RCG)

It might be usefull to understand the differences between RCG and RAM

Is a security feature (Windows 10->, Server 2016->) that helps to protect credentials over RDP. It works by redirecting Kerberos requests back to the device that is requiring the connection. The benefit is that the credentials never pass over network to the target device, thus if the targed device would be compromised the credentials cannot be dug out of the memory on that device. Enables SSO, Multihop RDP.   



## Tiering
From Truesec Materials:  
![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/04600c25-05b8-4133-a25f-c4bf0fe8838d)



Tiering is part of Zero-trust model  
 - Assume that all other components have been breached, thus using high impact credentials on these systems would endanger whole environment  
   - Least privilege access  
   - Running unnecesary priviledges when performing day-to-day operations cause high risk for priviledge escalation when environment is under attack  
 - Implementation of basic tiering model is effective protection against ransomware and lateral movement in the environment.  


![image](https://github.com/LehtiniemiJ/TieringUse/assets/44233030/04ba8b72-9536-4989-b7c3-b4b00285eeb1)
(https://learn.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard?tabs=intune)

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

Silo is a container that is supposed to have the T0 credentials and T0 PAWs/Jump stations. The servers in the environment should be in the OU, and not in the silo.

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


### Good Questions from clients

##Doesnt EDR protect the environment from a attacker who is trying to do lateral movement and priviledge escalation
EDR can protect the environment from attackers who try to move laterally and escalate their privileges, but not always. Some systems may have vulnerabilities that EDRs cannot detect. For example, an office product may have a privilege escalation vulnerability that allows an attacker to gain local admin privileges. With these privileges, the attacker can turn off the EDR software and exploit the device easily. If the Domain Admin has used that device, the attacker can also steal their credentials from that endpoint. The tiering project prevents high-privilege credentials from accessing devices that are not in their tier.
