# Resource Groups and Network
These steps are to be done on an **empty subscription preferably.** 

## Resource Groups
You might be messy in real life, its fine, I am messy too, but in Azure organization and clear objectives are key. So, in the Azure Portal, create the following RGs: (you don't need to create the ones marked with NO today)

Using your OrgName/Domain Name:

| RG Name | Use | Create Today?|
|--------|-------|--------|
|OrgName.CoreServers| To Store DCs and other core servers| YES |
|OrgName.Servers | For Other servers connected to AD| YES |
|OrgName.Networking| For Network stuff, VNet and NSGs for example| YES |
|OrgName.Clients | If you ever create client vms, then they go here | NO |
|OrgName.DBs| In the future, you might want to have BDs for demos. They go here | NO |
|OrgName.Security| Security Stuff here, including **Sentinel** | NO|
|OrgName.Purview | Purview objects in Azure will go here | NO |
|OrgName.AnyOther...| In the future, any other should continue the name convention | NO |


