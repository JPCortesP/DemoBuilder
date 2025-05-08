## Before we continue

If you've been following these steps correctly (when in doubt, correctly means what I said) you'll have at least 3 users:

| User | origin | reason|
|-------|------|--------|
|YoursSomething | Entra ID Global Admin | Created when you got the Sub|
|Creation User (Iluvatar)|VMs and Active Directory creator user | Created by DC01 and promoted as semi-god by Creating an AD with it|
|yourname (JP)|Active Directory|Manually created by your creator user|

To make things even more simpler, we will stop using the Iluvatar-equivalent user here. Just make sure your username created, has the Enterprise Admin permission. 

## AD Connect.
AD Connect should be quite straightforward for you to set up. What we need from it is simply:
1. Installed on the CloudCon o AD Connect Server, whatever you named it
   1. Use Entra Connect instead of Entra Connect Sync or whatever is called. Use this LInk https://www.microsoft.com/en-ie/download/details.aspx?id=47594&msockid=39a83cd0f13f65f732b429ccf0906475
   2. You can use defaults everywhere. That's called an **Express Installation** https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-express (we really want and need PHS)
2. Follow the wizard, use the two users required (your Entra ID user and your created user, forget Illuvatar now, it'll become your Break-Glass account)
   1. If your users UPN after the @ matches whatever domain you have on the cloud, then it'll work nicely.
   2. Lets use elfcorp as an example. Assuming you added elfcorp.com to your Entra, but not corp.Elfcorp.com (as I said you should) then:

| AD UP | Result |
|-------|--------|
|user@elfcorp.com | user@elfcorp.com|
user@corp.elfcorp.com | user@something.microsoftonline.com|

More info here: https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-existing-tenant

![alt text](/screenshots/ADConnectMatch.png)

*Here on this stolen screenshot is where you should see both your domain.com, and corp.yourdomain.com. Its okay to continue with a corp.yourdomain.com as not added, but yourdomain.com should be added. if both are "not added" then you need to stop and come back either to add the correct suffix, add yourdomain.com to Entra, or both.*

## Post Install Tasks
After the Installation, you can open the newly created shortcut on your VM desktop and click Configure, then:
1. On Configure Device Options
   1. Authenticate, then Enable one by one Hybrid Domain Join and optionally Device Writeback
   2. Hybrid Domain Join simply select Windows 10 or Later, ignore Windows Down-level
   3. ![alt text](/screenshots/SCP.png) *This is the required. Here you will use both your Entra user and also your Enterprise local, created user*
2. Kerberos Server Object (From DC01)
   1. We will need a KSO, so follow the instructions here: https://learn.microsoft.com/en-us/entra/identity/authentication/howto-authentication-passwordless-security-key-on-premises
   2. Use the Example #3 on the docs: 
```
# Specify the on-premises Active Directory domain. A new Microsoft Entra ID
# Kerberos Server object will be created in this Active Directory domain.
$domain = $env:USERDNSDOMAIN

# Enter a UPN of a Hybrid Identity Administrator, your Cloud Admin
$userPrincipalName = "administrator@contoso.onmicrosoft.com"

# Enter a Domain Administrator username and password. Your Local Admin
$domainCred = Get-Credential

# Create the new Microsoft Entra ID Kerberos Server object in Active Directory
# and then publish it to Azure Active Directory.
# Open an interactive sign-in prompt with given username to access the Microsoft Entra ID.
Set-AzureADKerberosServer -Domain $domain -UserPrincipalName $userPrincipalName -DomainCredential $domainCred
```
### Give your users full Admin on Entra
Now we have given enough time for the initial sync to happen, is time to go to Entra one last time with your cloud admin:
1. On the Entra portal, from the Users tab, All users
2. Find your user. Your User Principal Name should be something@yourdomain.com and not something@something.onmicrosoft.com
3. Assigned Roles, on Active, add an Assignment for Global Admin (you could go with Eligible instead here, to use PIM, but its out of the Scope today)
4. **Connect to Microsoft's AzureVPN**
5. Open a new browser profile, log in with your on premises user, which should be synced now. 
6. You should be required to add MFA, this is why we need the Microsoft's VPN here. 
7. After the MFA, you now should be only using this user from now on. 

### Common Errors
1. I can't add a new MFA for my newly created user
   1. Microsoft adds to our tenants a bunch of CA policies to elevate our security posture. The most important one here is `Security info registration for Microsoft partners and vendors`. Check that policy, you might want to temporarily disable or exclude yourself and troubleshoot your VPN later. 
2. My Browser profile always logs in automatically with my microsoft.com account
   1. For the very first time, you might need to log out, close the profile and open it again and re-login. This is expected as your PC is configured for SSO with your microsoft.com account. Next time on that portal should either ask you or auto-login you into your demo account. 


### Navigation
[Next >>](/StepbyStep/6%20-%20Entra%20ID%20Private%20Access.md)