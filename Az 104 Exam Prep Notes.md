# URLs
[**timothywarner**](https://github.com/timothywarner/az104) ["timothywarner frankenstein"](https://github.com/timothywarner/frankenstein)

[Az Architecute](https://learn.microsoft.com/en-us/azure/architecture/)

[Well Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)

[AZ-104 Administrator Associate Study Cram v2](https://www.youtube.com/watch?v=0Knf9nub4-k)


# #Policy and Governance
## In #EntraID (basically an Identity Store)
>> **Connection/Sync** bw. EntraID and onPrem AD
>> Entra is **flat** - no hierarchy. But in order to provide granularity is to us **AU**'s [Administrative Units].

# Sync w oPrem 
1. Through EntraID *Connect*: has an agent into the opPrem Windows Server.
2. Using EntraID *Cloud Sync*: runs entirely on Cloud.


A role: is a collection of permissions, that are aligned.

*Owner* is the __Super Role__ for SUBSCRIPTIONS.
*Contributor* almost as powerful. Grants full *read-write*.

Global Administrator is the __Super Role__ *in EntraID*.

*Service Principle* vs *Managed Identity*
 *User Principle* = UPC + mail-domain ?

## #RBAC vs Policy
**RBAC**:
 - for people 
 - doesnt include Guardrails. For example can't block Sara from creating a resource in a fobidden Region, or to create a gargantual VM-Size.

**Policy**:


## Bulk operations
Bulk Create is for internal users.
Bulk invite is for external users.

The password field can be left empty. 

> SSO Experience for Entra-Users; *https://myapps.microsoft.com/*

## Dynamic Group Membership / Dynamic Groups
There is no *hierarchy* in the users, or groups list!

*Partitioning* is possible in EntraID, on **au-level**.
*SSPR* ?

## #Mgmt Groups
Management groups are *__ONLY__ usefull when you have more than one subscriptions*.

## Initiatives, Initiative Definition
An Initiative = A container of policies.

**Policies can be applied also on machines on other Clouds!**

# #Clouds and Regions
https://datacenters.microsoft.com/ 


# #Storage 
## #Blob Storage 

## #SAS 
> To restrict the acces to SAS Tokens, on a container level, user a **Stored access policy**.
> If you create the policy after the SAS Toker, it wont get applied. 
> If the policy is already applied, and you change sth on it, it gets re-enforced onto the SAS-tokens.




*Exam Alerts*
------------
# #User Access and Management #IAM
EntraID Users **in a group** can be added, either as:
- Assigned: oldschool. One can manually assign users or groups (, devices, etc.). Groups can be nested in other groups (to good for troubleshooting). *__Assigned__ means you have to manage the membership yourself, of you have to nominate an owner for that.*
- Dynamic User : *Done over a query (some property, location, )*
- Dynamic Device

Policy in JSON, and get questions in a table, where one has to select what is allowed/disallowed through the policy.


# #Bastion [a managed "Jump Box"]
- Is integrated w Entra. So I can do things like Conditional Access Policies, so that u a strong authentication is enforces before allowing to access it.
- it has it's own *Azur Bastion Subnet* (/26)
- Developer / Basic and Standard SKU

# #Loadbalancing and Routing
- LB: "Regional Service". Can be either an **Internal** or **External**.
- AG [Appl Gw]: "Regional Service", General purpose LB. **Specifically for web workloads. There's a WAF, that has the capability for detecting the most common capability. Can terminate TLS/SSL Enctyption, if you'd need to do a packet istruction, and re-encrypt again".**
- Traffic Manager [*is a DNS*]: for protecting the VMs in **multiple regions**. If the entire region goes down, the AG also goes down.
- FrontDoor: **Layer 7** [has a WAF option]; for protecting the VMs in **multiple regions**. **It roles out features, that are specific for TM AND AG.** Not only for multiple regions, but also for **multiple clouds, incl. onPrem**. Combines all other in one, and has the CDN feature in it. It distributes globally. But it is certainly simplier to configure.


# Name Resolution ???
!!! Name Resolutino doesn't work across Pearing, beacuse they are totally different Sub-Domain, there is DNS bw. them, by detault.

# #DNS 
two types of DNS zones: *DNS* and *Privated DNS Zones*

**Public**
- Record
- Alias
- A, CNAME, Service, Text..

**Private**
- Manual
- Automatic 
- A, CNAME, Service, Text..

**Auto Registration:**
- a VNET can only auto registrate to 1 private DNS Zone.
- a privDNS Zone can be used to up to 100 VNETs, for that registration purposes.

**Resolution:**
- VNET can connect to up to 1000 pivDNS Zones, to look up records.
- a particular Zone can be used to up to 1000 VNETs, for resolution purposes.


**Azure DNS** is **ALWAYS** accessible on 168.63.129.16.
See [What is IP address 168.63.129.16?](https://learn.microsoft.com/en-us/azure/virtual-network/what-is-ip-address-168-63-129-16)


# #Virtual Networking #VNet #Networking
https://www.youtube.com/watch?v=9DuTWSvsLXM

## Net Masking, IP Adressing and Subnetting !!!
The first IPs in a net, are always lost:
.0 NW address
.1 Gateway
.2 DNS purposes
.3 DNS purposes
.255 Broadcast 


## #NSG vs #ASG?
**#Service Tags**
**NSG**: is funadamentally a set of rules. 
**Service Tag: a tag that we put on the NIC.**

[Azure Service Tags](https://www.youtube.com/watch?v=1Pm5-cH_45I)

**NOTE:** the NSG HAS to be in the region, of the VNET where I want to create it in. The region has to match where I want to use it from.

NSG: 
-  the first rule that matches, ENDs the conversation of the rule evaluation.

"Any"  is literally, any public or private IP address.


### #ASG [Application Security Groups]
Used when in combination with Service Tags.

## #S2S VPN

## #Express Route

**Express Route Global Reach**: Connect (or route) over my different ExpressRoutes onPrem Locations using the MS Backbone.
**Microsoft Peering**: using BGP route filter, the onCloud services canbe *advertised* to come through this route filter. **CHECK**

## #Virtual WAN

## #Service Endpoints

# #Firewall
three Types of Firewall



## Diff. bw. #Availability Set vs #Avail. Zone ??
- Avail. Zone: give u more protection. These are separate DCs in the region. If a DC goes down, the workload is switch over to the other DCs. 
- Avail. Sets: if we have much lower level level of avail. Those are just simply *flags*, that are set on the VM. They simply eensur, that the VMs are spread among different set of *server racks / Falut Domains* within the DC.

# #ARM Templates {JSON, Bcep, Terraform}
## Difference bw. Parameters and Variables:
   **Parameters**: are key-value pairs, that exist outside the template. That is when you do the deployment you can override those parameters, if u dont have the default. If u dont have the *default*, the deployment will actually top and force u to supply a value f those parameteres.
   **Variables**: are used only internally, in the deployment. These are usefull for *expressions*.


## #Arm Deployment
```PowerShell
New-Azresourcegroupdeployment -TemplateFile .\azuredeploy.json -ResourceGroupName nodeapp2-rn -verbose
New-Azresourcegroupdeployment -TemplateFile .\storage-account.bicep -ResourceGroupName breantwood-rn  -whatif
```

>> **-whatif** :  prints out "what would be new, what would be changed, what would be deleted"


# #Power Shell
```PowerShell
Get-AzEnvironment #lists the environments

Get-AzContext | f1 #check if logged in
connectAzAccount 
New-AzRoleDefinition -InputFile ./vm-custom.json
```


# Public Access from all network, Private Endpoints, Service Endpoint 
- **Public Access from all network**:
- **Private Endpoints**:
- **Service Endpoint**: gives the best of both worlds.


# #Monitor

# #Log Analytics Workspace

# #Application Insights
Load Testing, u can see where users are going at, development tests.

251:00
