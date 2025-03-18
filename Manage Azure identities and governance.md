## **Manage Azure identities and governance**
- Microsoft Entra ID is a cloud-based identity and access management service that your employees can use to access external resources.
- Create user or invite external user
- Authentication & Authrorization

## RBAC
- Can assign permission on subscription level, resource group level or resource it self
- Data actions means what we can perform on data within a resource

# Virtual Machine permissions
 - Virtual Machine Administrator Login - View Virtual Machines in the portal and login as administrator
 - Virtual Machine Contributor - Create and manage virtual machines, manage disks, install and run software, reset password of the root user of the virtual machine using VM extensions, and manage local user accounts using VM extensions. This role does not grant you management access to the virtual network or storage account the virtual machines are connected to. This role does not allow you to assign roles in Azure RBAC.
 - Virtual Machine Local User Login - View Virtual Machines in the portal and login as a local user
 - Virtual Machine User Login - View Virtual Machines in the portal and login as a regular user.

Note :- VM should be registed with the EntraNet

# Customer Role
  - Creating custom role by selecting permissions (Actions and Data Actions)

# Entra ID role
- RBAC are giving inside the suscription. But Entra ID roles given permission to carry out within the Entra ID it self.
- To create custom role in entra ID we should have premium p1 or p2 license for Entra ID
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
-  P1 provides nybrid user acccess both on-prem and clod resources,MFA for on-premises applications,  conditional access, dynamic memberships, SSPR with writeback , self service password reset for onprem users (6$ per user per month)
-  P2 is giving risk based conditional access, SSPR with writeback to your apps. (9$ per user per month)

# Assign licenses to users / groups
-  User location have to set
-  We have to user mincrosoft admin center
-  Group cannot be delete if license is associated with this. Unassociated the license first
-  Azure AD user can be removed from a group even if the group has an assigned license
  

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
- Giving recommmendations for perfromance, relaibility, cost effectiveness and security


# Azure policy
- Can be used to defined the policies.
- Hepls govern the rules, and define rules in resources to comply by
- Advisory rules can be defined in subscrition and resource group level
- Two types of policies. Allow or Deny

#Management groups
- helps to manage different subscriptions
- if permission grant on management group this will be affecting all the resource in subcscription
- Azure polies can be assgined to Management groups
- helps to enforce compliance with seccurity standards
- The root management group can't be moved or deleted, unlike other management groups.
- No one has default access to the root management group. Microsoft Entra Global Administrators are the only users who can elevate themselves to gain access


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
 - In policy definition The mode determines which resource types are evaluated for a policy definition. The supported modes are:
 - 1. all: evaluate resource groups, subscriptions, and all resource types
 - 2. indexed: only evaluate resource types that support tags and location
- organization with strict compliance requirements and no hybrid identity - Federation with ADFS
- Entra ID role
- 1. Application Administrator	- Can create and manage all aspects of app registrations and enterprise apps.
  2. Authentication Administrator - Can access to view, set and reset authentication method information for any non-admin user. Can modify security questions in the SSPR settings
  3. Conditional Access Administrator	 - Can manage Conditional Access capabilities.
  4. Password Administrator	- Can reset passwords for non-administrators and Password Administrators.
  5. User Administrator - 	Can manage all aspects of users and groups, including resetting passwords for limited admins.
  6. Privileged Role Administrator -	Can manage role assignments in Microsoft Entra ID, and all aspects of Privileged Identity Management.
  7. Security Administrator - Azure role allows the user to configure, manage, and monitor security-related policies across the subscription, but does not allow access to resources
- Privileged Identity Management provides time-based and approval-based role activation to mitigate the risks of excessive, unnecessary, or misused access permissions on resources that you care about.
- Just-in-Time (JIT) role activation with Privileged Identity Management (PIM) -  enforce the principle of least privilege
- enforce that only compliant devices can access sensitive applications in Azure - Use a Conditional Access policy with device compliance requirements
- System-assigned identities are automatically deleted when the resource is deleted, while user-assigned identities persist
- FIDO2 Security Keys - password less authetication is the most secured for AD
- Azure Activity Log - Azure service helps enforce compliance by auditing resource changes and role
- Microsoft Entra Connect features
- 1. Password hash synchronization - A sign-in method that synchronizes a hash of a users on-premises AD password with Microsoft Entra ID.
  2. Pass-through authentication - A sign-in method that allows users to use the same password on-premises and in the cloud, but doesn't require the additional infrastructure of a federated environment.
  3. Synchronization - Responsible for creating users, groups, and other objects.
  4. Health Monitoring - Microsoft Entra Connect Health can provide robust monitoring
- Dynamic Groups - allows you to automatically assign roles to users based on membership in a group
- Giving access to a temporary Microsoft SharePoint document library named Library1. You need to create groups for the users
  - 1. a Microsoft 365 group that uses the Dynamic User membership type
  - 2. a Microsoft 365 group that uses the Assigned membership type
 - Group-based Entra ID licensing currently doesn't support groups that contain other groups (nested groups)
- From Cross-tenant access settings, configure the Tenant restrictions settings - to specify which tenants users can access
- External collaboration settings, configure the Collaboration restrictions settings - you can ensure that invitations can only be sent to users from that domain
- Standard SKU Azure container registry doesnot support geo-replication. only the premium ones
- By default, Azure applies a two-gate password reset policy for all administrator accounts. This two-gate policy requires two pieces of authentication data, such as an email address, authenticator app, or phone number, and does not permit the use of security questions for administrators. 




