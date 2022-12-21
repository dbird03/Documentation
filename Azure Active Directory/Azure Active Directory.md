# Azure Active Directory

## General
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

1. Visit Graph Explorer and sign in with a privileged account (e.g. Global Administrator).
1. Search for the service principal you want to update the OAuth2PermissionGrants for.
    ```
    GET https://graph.microsoft.com/v1.0/servicePrincipals?$search="displayName:your_app_name"
    ```
1. Note the service principal's Object ID (this is the "id" property in the response. NOT the "appId" property.)
1. Get all the OAuth2PermissionGrants in the tenant. In the response, search the Object ID from Step 3 and find the object where the Object ID matches the "clientId" property. Note the "id" property in this object. This "id" property is the "Grant ID".
    ```
    GET HTTPS://graph.microsoft.com/v1.0/oauth2PermissionGrants
    ```
    > **Note about property names** <br/>
    > The "id" property is the response from /servicePrincipals is the "clientId" property in the response from /oauth2PermissionGrants.<br/>
    > The "id" property in the response from /oauth2PermissionGrants is the Grant ID. The Grant ID is a unique value that represents the OAuth2PermissionGrants assigned to a service principal.

    > **Note about OAuth2PermissionGrants** <br/>
    > OAuth2PermissionGrants exist per user or group of users. For example, if a service principal has a set of admin consented permissions for ALL users in the organization, and then two global administrators have consented to their own set of permissions, there will be three OAuth2PermissionGrants for the service principal. This means the the /oauth2PermissionGrants endpoint will return three Grant ID objects, so the "clientId" value will be present in each one. The "consentType" and "principalId" properties will show who the OAuth2PermissionsGrant applies to.

1. Get the OAuth2PermissionsGrant objects using the Grant ID from Step 4.<br/>
    ```
    GET https://graph.microsoft.com/v1.0/oauth2PermissionGrants/{id}
    ```
1. At this point, you can use the DELETE method to completely delete the permission grant or the PATCH method to update the "scope" property.