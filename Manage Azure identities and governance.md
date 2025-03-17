## **Manage Azure identities and governance**
- Microsoft Entra ID is a cloud-based identity and access management service that your employees can use to access external resources.
- Create user or invite external user
- Authentication & Authrorization

## RBAC
- Can assign permission on subscription level, resource group level or resource it self
- Data actions means what we can perform on data within a resource

# Vierual Machine permissions
 - Virtual Machine Administrator Login - View Virtual Machines in the portal and login as administrator
 - Virtual Machine Contributor - Create and manage virtual machines, manage disks, install and run software, reset password of the root user of the virtual machine using VM extensions, and manage local user accounts using VM extensions. This role does not grant you management access to the virtual network or storage account the virtual machines are connected to. This role does not allow you to assign roles in Azure RBAC.
 - Virtual Machine Local User Login - View Virtual Machines in the portal and login as a local user
 - Virtual Machine User Login - View Virtual Machines in the portal and login as a regular user.

Note :- VM should be registed with the EntraNet

# Customer Role
  - Creating custom role by selecting permissions (Actions and Data Actions)

# Entra ID role
- RBAC are giving inside the suscription. But Entra ID roles given permission to carry out within the Entra ID it self.
- To create custome role in Mentra ID we should have premium p1 or p2 license for Entra ID
- Global Administrator - Can manage all aspects of Microsoft Entra ID and Microsoft services that use Microsoft Entra identities.

 ## Diffrence of Azure role and Entra ID role
 - Azure resources at different levels:
 - 1. Subscription
   2. Resource Group
   3. Individual Resources
- Entra ID :- Manages access to identity and directory-level permissions (e.g., user management, app registrations
- License Administrator	:- Can manage product licenses on users and groups.

## Invite guest user
-- Invite external user and then assign RBAC for azure resources

## Entra ID licensing
- Free / P1/ P2
-  PI provides nybrid user acccess both on-prem and clod resources,MFA for on-premises applications,  conditional access, dynamic memberships, SSPR with writeback , self service password reset for onprem users (6$ per user per month)
-  P3 is giving risk based conditional access, SSPR with writeback to your apps. (9$ per user per month)

# Assign licenses to users / groups
-  User location have to set
-  We have to user mincrosoft admin center
-  Group cann be delete if license is associated with this. Unassociated the license first

# Self service password reset
-  Rquire p1 or p2 license
-  onoprem write back to their AD
- Password reset policy can be assigned to all users or groups or selected users


# Resource Tags
- Extra meta data to organizing

# Moving resource 
- moving resource group will not change the location. since RG is a logical grouping
- When moving from suscriptions to another suscriptions we have to move all dependent resource. Ex: pulic IP assignrf to vm has to disassociate first

## Locking resource
- Delete and Read Only locks

## Azure advisor tool
- Giving recommmendations for perfromance, relaibility, cost effectiveness
- Hepls govern the rules, and define rules in resources to comply
- Advisory rules can be defined in subscrition and resourc egroup level
- Two types of policies. Aloow and 

# Azure policy
Can be used to defined the policies.

#Management groups
- helps to manage different subscriptions
- if permission grant on management group this will be affecting all the resource in subcscription
- Azure polies can be assgined to Management groups
- helps to enforce compliance with seccurity standards


NOTE :- 
- Azure AD B2C enables users to sign in using their personal Microsoft accounts (like Outlook.com)
- Azure AD user account is deleted - The user is moved to the Recycle Bin and can be restored within 30 days
- Power command to create new user New-AzureADUser
- New-AzADGroup
- Conditional Access policies are enforced after first-factor authentication is completed. Conditional Access isn't intended to be an organization's first line of defense for scenarios like denial-of-service (DoS) attacks, but it can use signals from these events to determine access.
- These signals include:
- 1. User or group membership
  2. IP Location information
  3. Device
  4. Application
  5. Real-time and calculated risk detection
  6. Microsoft Defender for Cloud Apps
- Administrators with the Conditional Access Administrator role can manage policies.
- Azure AD Connect - tool is used to synchronize on-premises AD users to Azure AD
- Password hash synchronization authentication method is recommended for hybrid Azure AD environments
- Identity Protection feature of Azure AD protects against leaked credentials
- Blueprints are a declarative way to orchestrate the deployment of various resource templates and other artifacts such as:
  1. Role Assignments
  2. Policy Assignments
  3. Azure Resource Manager templates (ARM templates)
  4. Resource Groups
 - Azure AD Audit Logs - sed to track user sign-ins and activities in Azure AD (sign in logs)
 - Licenses are not automatically provisioned when a new user is created in Microsoft Enterprise ID.
 - Each user in Microsoft Enterprise ID can have multiple licenses assigned to them, depending on the services and features they require access to
 - Policies can be used to enforce tags on resources.
