# Servers Creation.
Here comes the fun part. These VMs will be usually quite straightforward to create, with just a single difference on the DC01 part, which, in case you want disaster recovery, should have all the data of the Domain itself in a different disk, otherwise, both should be quite similar. 

## DC01 creation
From the OrgName.CoreServers SG:

1. Basic Tab
   1. Click Create and search for Virtual Machine
   2.  Make sure subscription and RG are correct
   3.  Name it, **DC01** for this example (VMs inside Azure can't be renamed later)
   4.  Check for the region. In this example, **East 2**
   5.  In availability options, select **No infrastructure redundancy required**
   6.  Security Type: **Standard**
   7.  in Image, search for **Windows Server 2022 Datacenter: azure edition**. Note: don't select Hotpach Images, they will bring no value as these machines will be auto-off anyway. 
   8.  No Azure spot discount. Sometimes it makes sense, but not on these machines that we need them on as much as possible. 
   9.  Size. Normally I would go with **DS1_v2**, that should be enough. 
   10. On the accounts, use a username and remember it. Password should be long and complex. The first account you input here will be used as the Enterprise Admin on the AD, so make sure you remember it. 
   11. Inbound ports, Select None. The NSG we created earlier will let us in. 
   12. Licensing, select the check box since you have a MSDN subscription. 
 
2. Disks Tab
    1.  OS Disk Type: **Standard SSD** (let's save as much money as possible)
    2.  All the way down to Data Disk, click **Create and Attach a new Disk**
    3.  Name your disk how you want or leave the default name
    4.  Source Type: empty
    5.  Change the size to Standard SSD and select at most 64 GB
    6.  Uncheck the "Delete disk with VM" if checked. Click Ok
    7.  Change the caching to None
 3. Networking Tab
    1. Virtual Network: **Triple check that you selected your VNET created earlier**
    2. Subnet: for DC01, select CoreServers. For AD Connect, select Servers.
    3. Public IP: Let the default (New)DC01-IP to let Azure create us a new Public IP. 
    4. NIC Network Security Group: Azure will tell you that the Subnet already has an NSG (MotherOfNSG), so let's do what it says and leave it on **None**
    5. Check **Delete public IP and Nic....**
    6. Uncheck **Enable Accelerated Networking**
    7. Load Balancing Options on **NONE**
 4. Management Tab
    1. For now, if Defender for Cloud is already configured, we will let it. 
    2. For the rest, disable all the options on this tab. 
    3. Make sure Patch Orchestration is **Automatic by OS**. We will deal with this later.
 5. Monitoring
    1. Leave all defaults. **If you know what you're doing, disable boot diagnostics**. It could be useful later if you leave it on. 
 6. Advanced Tab
    1. Leave all defaults
 7. Tags
    1. Same as before: Name: **SolutionArea**, Value: **Active Directory**
 8. Click Review and Create, and if done correctly all you have to do is click create. 
 9. While you wait, you can simply add your second VM, name it **CloudCon** or **ADConnect**. The former being more true as we will use it for more stuffs than simply ADConnect. All the steps should be the same except for this second VM we will not add a new disk. 

After you finish these steps, you should have 3 VMs:
* One Client VM with Windows 11 on your local Hyper-V Machine
* Two Windows server VMs on Azure, unconfigured, with open RDP from your local Home/office IP. 

Next step is to install Active Directory Roles and promote your first server.   




### Navigation
[Next >>](/StepbyStep/4%20-%20Active%20Directory.md)