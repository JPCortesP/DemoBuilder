# References

## IP Address and Networks

### Networks
* 192.168.100.0/24
* 192.168.101.0/24
* 192.168.102.0/24
  
This networks should not collide with your home network, however, adjust as necessary. 

### IP Addresses
* 192.168.100.4/24 **DC01**

## Domains References
AD Domain: corp.yourdomain.com

## Demo Name
This might sounds trivial, but for customization and making sure you stay consistent, make sure to get a great sounding name to your entire environment. Think of a company and a few users. My example is:
* Company Name: **"Liquen Park Corporation"**
* Users: My peers at Security (Byron, Brian, Giovanni)
* My own SuperUser: jp
* My "Creator" user: Illuvatar

## Azure Regions
I Suggest using East 2 as its common to have our subscriptions blocked to some regions. 

### Out of Scope / Recommended Tasks for later 
* PIM
* Most Conditional Access.
* Azure Backup and stuff like that.
* Auto-On for VMs.
* Azure Bastion, but because its expensive, not because is hard.
* Azure Sentinel, but because you should know more than me about that. 
* Specific NSGs to limit traffic between Rings
* Additional Local web apps to test even more Private Access



### Navigation
[Next >>](0.5-Hyper-V.md)
