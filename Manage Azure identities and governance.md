## **Manage Azure identities and governance**
- Microsoft Entra ID is a cloud-based identity and access management service that your employees can use to access external resources.
- Authentication & Authrorization
- Microsoft Entra ID (formerly called Azure Active Directory or Azure AD) is Microsoft’s cloud-based identity and access management (IAM) service
- Key features
  1. SSO (Single Sign-On)
  2. MFA (Multi-Factor Auth)
  3. Conditional Access
  4. Guest Access (B2B)
  5. Device Management
  6. App Registration
  7. Hybrid Identity - Connect your on-prem AD using Azure AD Connect

# Identity fundamentals
- A digital identity is a collection of unique identifiers or attributes
  1.  An email address
  2.  Sign-in credentials (username/password)
  3.  Bank account number
  4.  Government issued ID
  5.  MAC address or IP address
- Three  types of identities
  1. Human identities
  2. Workload identities
  3. Device identities
 
# Authenticating, authorizing, and accessing resources
1. The user (resource owner) initiates an authentication request with the identity provider/authorization server from the client application.
2. If the credentials are valid, the identity provider/authorization server first sends an ID token containing information
3. The identity provider/authorization server also obtains end-user consent and grants the client application authorization to access the protected resource
4. The access token is attached to subsequent requests made to the protected resource server from the client application.
5. The identity provider/authorization server validates the access token. If successful the request for protected resources is granted, and a response is sent back to the client application.

# Microsoft Entra ID Tenant
- A tenant is a dedicated and isolated instance of the Microsoft Entra ID service for your organization.
  ![image](https://github.com/user-attachments/assets/0ba90c67-aade-471b-9855-b5445485d01a)
- Microsoft Entra ID provides the identity and access management (IAM) service. You can add company branding that applies to all these experiences to create a consistent sign-in experience for your users.
- It requires Organizational Branding Administrator role to Manage all aspects of organizational branding in a tenant
- Tenant Creator role is required to create tenants
- Adding custom branding requires P1 or P2 licenses

# Authentication and authorization standards
- OAuth 2.0
- OPen ID
- JSON web tokens
- SAML - Security Assertion Markup Language
- System for Cross-Domain Identity Management
- Web Services Federation

# Microsoft Entra ID vs Active Directory Domain Services (AD DS)
- Microsoft Entra ID (Azure AD) - Cloud-based identity service/Used by Cloud apps/OAuth2, OpenID Connect, SAML / Supports Azure AD Join, Hybrid Join
- AD DS - On-premises directory service/Traditional on-prem apps, Windows logins/LDAP, Kerberos, NTLM /Supports Windows domain join

# Azure AD Connect
- Azure AD Connect is a free tool from Microsoft that synchronizes your on-prem AD with Microsoft Entra ID (Azure AD)
- Syncs users, groups, contacts, and passwords from AD DS to Entra ID

# User creation
- Types of users (internal memeber/ Internal guest/ external memeber/ external guest)
- User creation (basic tab -> User principal name/Display nanme / Password/ Enabled) Propertiese tab -> (Identity , Job information, Contact information, Parental controls, settings), Assiggnment Tab -> (add group . role, administrative unit)
- Invalite external user have redirect link in  Basic tab
- When assigning role to user there is check box called "Permanently eligible". otherwise we have to the stat and end dates
- When assiging roles there is two option Eligible and Active
- Eligible - Only when having P2 license
- Active  - P2 License, or Free or Microsoft Entra ID P1 license

# Default user permissions
- Member user can do (enumerate the list of all users, groups, and other directory objects/ Invite guests /Change their own password/ Create security groups/ Register (create) new applications / numerate the list of all devices / Read all licensing subscriptions/ Read all administrative roles and memberships)
- Guest user can do (Read their own properties,Read display name, email / Change their own password / Read properties of registered and enterprise applications)

# Reset user password
- You must have at least  Password Administrator ole to restore and permanently delete users.
- When using Microsoft Entra ID, a temporary password is auto-generated for the user.
- When using Active Directory on-premises, you create the password for the user.
- The temporary password never expires

# Update user profile info
- Create a new user - User Administrator
- Invite an external guest - Guest Inviter
- Assign Microsoft Entra roles - Privileged Role Administrator

# Delete user
- The user can be seen on the Deleted users page for the next 30 days and can be restored during that time.  any licenses consumed by the user are made available for other users. But has option to permanently delete the user as well.
- Once deleted account is in a suspended state
- User Administrator role is required to delete users
- Privileged Authentication Administrator role can delete any users including other administrators
- The license is freed as soon as the user is soft-deleted. It can be immediately assigned to another user. If the user is restored, you’ll need to reassign the license manually.
- When hard delete license goes back to the pool.

# Groups
- Two types of groups Security groups  and Microsoft 365 groups
- Membership types
 1. Assigned groups - Lets you add specific users as members of a group
 2. Dynamic membership group for users - Lets you use rules to automatically add and remove users as members
 3. Dynamic membership group for devices - Lets you use rules to automatically add and remove devices as members
- You can create a dynamic group for either devices or users, but not for both
- a license can be automatically assigned when you add a user to a group, if that group has been configured for group-based licensing in Microsoft Entra ID.
- when leaving from the group, User loses all licenses assigned via that group

# Group assignment types
1. Direct assignment - The resource owner directly assigns the user to the resource.
2. Group assignment - The resource owner assigns a Microsoft Entra group to the resource, which automatically gives all of the group members access to the resource
3. Rule-based assignment - The resource owner creates a group and uses a rule to define which users are assigned to a specific resource
4. External authority assignment - the resource owner assigns a group to provide access to the resource and then the external source manages the group members

# Add Microsoft Entra roles can be assigned to the group
- This option is only available with P1 or P2 licenses.
- have at least the Privileged Role Administrator role.
- Enabling this option automatically selects Assigned as the Membership type.
- The ability to add roles while creating the group is added to the process.
- When creating the group owner and memeber can be added to the group

# Add a group to another group
- Security group type, you can add an existing group to another group
- don't support:
1. Adding groups to a group synced with on-premises Active Directory.
2. Adding security groups to Microsoft 365 groups.
3. Assigned membership to shared resources and apps for nested security groups.
4. Applying licenses to nested security groups.
5. When you change an existing static group to a dynamic group, all existing members are removed from the group, and then the membership rule is processed to add new members. If the group is used to control access to apps or resources, the original members might lose access until the membership rule is fully processed.

# Dynamic membership groups
- Dynamic groups allow you to automatically add or remove users based on their attributes. The license will automatically be assigned or revoked when the group membership changes.
- You can configure the dynamic membership rule to target specific users (e.g., users with a particular department or location)

# Deleting user group
- soft delete 30 days
- The removed user loses all licenses that were assigned through that group.
- Licenses are returned to the pool.
- The user may immediately lose access to related services. (Licenses from other groups will not affect)

# Invite guest user
- Invite external user and then assign RBAC for azure resources

# Entra ID licensing
- Licenses can be assigned to any security group in Microsoft Entra ID. Security groups can be synced from on-premises, by using Microsoft Entra Connect.
- Microsoft Entra ID automatically manages license modifications that result from group membership changes. Typically, license modifications are effective within minutes of a membership change.
- A user can be a member of multiple groups with license policies specified. A user can also have some licenses that were directly assigned, outside of any groups. The resulting user state is a combination of all assigned product and service licenses. If a user is assigned the same license from multiple sources, the license is consumed only once

-  You must remove all licenses assigned to a group before you can delete the group
-  license assignment errors on users members of a group when using group based licensing
  1. Not enough licenses
  2. Conflicting service plans
  3. Missing dependent service plans
  4. Usage location not specified - Before you can assign a license to a user, you must specify the Usage location property for the user /When Microsoft Entra ID assigns group licenses, any users without a specified usage location inherit the location of the directory
  5. Duplicate proxy addresses

# Entra ID licenses
- Free / P1/ P2
- Free - Included with Microsoft cloud subscriptions such as Microsoft Azure, Microsoft 365
-  P1 - provides hybrid user acccess both on-prem and clod resources,MFA for on-premises applications,  conditional access, dynamic memberships, SSPR with writeback , self service password reset 
           for onprem users (6$ per user per month)
-  P2 is giving risk based conditional access, SSPR with writeback to your apps. (9$ per user per month)
-  Microsoft Entra Suite ( A Microsoft Entra ID P1 subscription is required ) It includes 5 products.

# Licensing features
- Free - (SSPR)
- P1 / P2 - SSPR with writeback	/ (SSPR) / MFA for on-premises applications / MFA Reports / Fraud alert / Users at risk detected alerts

# Removing group license
- You must remove all licenses assigned to a group before you can delete the group

# Microsoft Entra Privileged Identity Management
- It is part of the Microsoft Entra suite and  is a tool in Azure Active Directory (Azure AD) that helps organizations manage, control, and monitor access to privileged accounts and resources
- You need either Microsoft Entra ID Governance licenses or Microsoft Entra ID P2 licenses to use PIM
- Feature of PIM - Approval Workflow/ Access Reviews / Audit Logs / Time-Bound Access / Notification and Alerts
- When PIM license expired
  1. Permanent role assignments to Microsoft Entra roles are unaffected.
  2. The Privileged Identity Management service in the Microsoft Entra admin center will be removed
  3. Eligible role assignments of Microsoft Entra roles are removed
  4. Any ongoing access reviews of Microsoft Entra roles ends, and Privileged Identity Management configuration settings are removed.
  5. Privileged Identity Management no longer sends emails on role assignment changes

# Managed identities
- There are no licensing requirements for using Managed identities for Azure resources.

# Entra ID role
- RBAC are giving inside the suscription. But Entra ID roles given permission to carry out within the Entra ID it self.
- To create custom role in entra ID we should have premium p1 or p2 license for Entra ID
- Global Administrator - Can manage all aspects of Microsoft Entra ID and Microsoft services that use Microsoft Entra identities.
- User who creates a Microsoft Entra tenant is automatically assigned the Global Administrator role

 # Diffrence of Azure role and Entra ID role
 - Azure resources at different levels:
 - 1. Subscription
   2. Resource Group
   3. Individual Resources
- Entra ID :- Manages access to identity and directory-level permissions (e.g., user management, app registrations
- License Administrator	:- Can manage product licenses on users and groups.
  
# Assign licenses to users / groups
-  User location have to set
-  We have to user mincrosoft admin center
-  Group cannot be delete if license is associated with this. Unassociated the license first
-  Azure AD user can be removed from a group even if the group has an assigned license
-  Manage B2B collaboration is inviting guest users to partificate with Entra Id. users is typically set to "guest" 
  
# Self service password reset
- Rquire p1 or p2 license
- onoprem write back to their AD
- Password reset policy can be assigned to all users or groups or selected users

# Entra ID monitoring
- View: Monitoring Reader / Log Analytics Reader roles required    
- View and modify settings: Monitoring Contributor/ Log Analytics Contributor
- Read: Reports Reader / Security Reader / Global Reader
- Update: Security Administrator

# RBAC
- Can assign permission on subscription level, resource group level or resource it self
- Data actions means what we can perform on data within a resource
- Once a user is restored, licenses that were assigned to the user at the time of deletion are also restored 

# Virtual Machine permissions
 - Virtual Machine Administrator Login - View Virtual Machines in the portal and login as administrator
 - Virtual Machine Contributor - Create and manage virtual machines, manage disks, install and run software, reset password of the root user of the virtual machine using VM extensions, and manage local user accounts using VM extensions. This role does not grant you management access to the virtual network or storage account the virtual machines are connected to. This role does not allow you to assign roles in Azure RBAC.
 - Virtual Machine Local User Login - View Virtual Machines in the portal and login as a local user
 - Virtual Machine User Login - View Virtual Machines in the portal and login as a regular user.

Note :- VM should be registed with the EntraNet

# Customer Role
  - Creating custom role by selecting permissions (Actions and Data Actions)
  - 
# Resource Tags
- Extra meta data to organizing

# Moving resource 
- moving resource group will not change the location. since RG is a logical grouping
- When moving from suscriptions to another suscriptions we have to move all dependent resource. Ex: pulic IP assignrf to vm has to disassociate first

## Locking resource
- Delete and Read Only locks

## Azure advisor tool
- Giving recommmendations for perfromance, relaibility, cost effectiveness and security 
- Advisory rules can be defined in subscrition and resource group level


# Azure policy
- Can be used to defined the policies.
- Hepls govern the rules, and define rules in resources to comply by
- Two types of policies. Allow or Deny

# Management groups
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




