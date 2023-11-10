# Azure Active Directory

## General
### Get Azure AD tenant details by domain name or tenant ID
The [tenantInformation](https://learn.microsoft.com/en-us/graph/api/resources/tenantinformation?view=graph-rest-1.0) Microsoft Graph API can be used to get an Azure AD tenant's details using either the domain name or tenant ID. 

### View an Azure AD tenant's public information
The following URL can be used to view an Azure AD tenant's public information such as their TenantId GUID and other details:

`https://login.microsoftonline.com/{Domain}/.well-known/openid-configuration`

**Example:** 

For Microsoft.com, the URL would be:
https://login.microsoftonline.com/microsoft.com/.well-known/openid-configuration

The response would be:

```
{
    "token_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/token",
    ...
}
```
So the Tenant ID for Microsoft.com is 72f988bf-86f1-41af-91ab-2d7cd011db47.

---

## Applications
### How to manage permissions (OAuth2PermissionGrants) for an Azure AD Enterprise Application (aka Service Principal)
These steps show how to use Graph Explorer to manage the permissions (OAuth2PermissionGrants) for an Azure AD enterprise application (also referred to as a service principal). These steps are useful for removing select permissions without needing to completely delete the service principal object and re-consent to the application in your tenant.

For example, there is a business application in your tenant that no longer requires Group.ReadWrite.All permissions in your tenant. You can use these steps to remove the permission without any down time since the service principal object remains in your tenant.

Original source: [How to update the OAuth2 permission grants for an Azure AD service principal (andrewmatveychuk.com)](
https://andrewmatveychuk.com/how-to-update-the-oauth2-permission-grants-for-an-azure-ad-service-principal/)

1. Sign into Azure AD and go to Enterprise Applications. Search for the service principal and note down the Object ID (**Note:** Do NOT use the Application ID (Client ID)).
1. Visit Graph Explorer and sign in.
1. Search for the OAuth2PermissionGrants objects belonging to the service principal using the filter functionality of Microsoft Graph.
    >**Note about property names**<br/>
    > The property names can be confusing between different Microsoft Graph APIs and what is displayed in the Azure AD portal. The filter query uses "clientId", but this is still used with the Object ID value of the service principal and NOT the Application ID / Client ID value.

    ```
    GET https://graph.microsoft.com/v1.0/oauth2PermissionGrants?$filter=clientId eq '{ObjectId}'
    ```
1. In the response, you will see an OAuth2PermissionGrants object for each type of consent. The "id" property represents each OAuth2PermissionGrants object.
    > **Note about OAuth2PermissionGrants**<br/>
    > OAuth2PermissionGrants exist per user or group of users.<br/><br/>
    > For example, if a service principal has a set of admin consented permissions for all users in the organization, and then two global administrators have consented to their own set of user consented permissions, there will be three OAuth2PermissionGrants objects for the service principal. The "consentType" and "principalId" properties will show who the OAuth2PermissionsGrant applies to.<br/><br/>
    > "ConsentType":"Allprincipals" is the Admin consented tab in the Permissions page of the Enterprise Application in the Azure portal UI.
1. At this point, you can use the DELETE method to completely delete the permission grants or the PATCH method with the "scope" property to update the permissions.
    
    **Example:**

    This will completely delete the OAuth2PermissionGrants object for {id}.

    ```
    DELETE https://graph.microsoft.com/v1.0/oauth2PermissionGrants/{id}
    ```
    **Example:**
    
    This will update the OAuth2PermissionGrants object for {id} to only contain the openid, profile, offline_access, User.Read, and Directory.AccessAsUser.All permissions. 
    
    ```
    PATCH https://graph.microsoft.com/v1.0/oauth2PermissionGrants/{id}
    ```
    ```
    {
        "scope": openid profile offline_access User.Read Directory.AccessAsUser.All 
    }
    ```
---