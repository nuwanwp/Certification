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
- 
