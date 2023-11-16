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
- SCCM
- 

### TE

TE is tier dedicated for endpoints, TE account is not an admin account. The account is used only to loginto the PAW/jump stations. should be the account used to access this tier. The Endpoints should be accessed with the TE user and processes ran with  Local admin account LAPS password. 

## PAW - Jumpstation

### t0 - Silo 

Description of t0 - Silo.

### RDP

Information about RDP.

## Credentials

### EXT

Details about EXT.

### FTE

Information about FTE.
