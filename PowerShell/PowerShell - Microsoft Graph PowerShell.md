# PowerShell - Microsoft Graph PowerShell

## How to use Microsoft Graph PowerShell SDK with a custom app registration and delegated permissions
These steps enable you to use Microsoft Graph PowerShell SDK to connect to a custom app registration with delegated permissions you manage instead of connecting to the Microsoft Graph PowerShell enterprise application from Microsoft's tenant.

This enables you to set up custom app registrations with select permissions in your organization for greater control of delegated permissions.

1. Sign in to **portal.azure.com** and select **App registrations**.
1. Create a custom single-tenant app registration and note the **Directory (tenant) ID** and **Application (client) ID** values.
1. Select **Authentication**. Under **Platform configurations**, click **Add a platform**.
1. Select **Mobile and desktop applications**.
1. Check the box next to the **https://login.microsoftonline.com/common/oauth2/nativeclient** Redirect URI and click **Configure**.
1. Under **Advanced settings**, set **Allow public client flows** to **Yes**.
1. Using the Microsoft Graph PowerShell SDK, connect to the custom app registration using the following command:
    ```powershell
    Connect-MgGraph -TenantId <Tenant ID of your tenant> -ClientId <Client ID of your custom app>
    ```