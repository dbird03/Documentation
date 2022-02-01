# Azure Active Directory

### View an Azure AD tenant's public information
The following URL can be used to view an Azure AD tenant's public information such as their TenantId GUID and other details:

`https://login.microsoftonline.com/{Domain}/.well-known/openid-configuration`

Example: 
```
https://login.microsoftonline.com/microsoft.com/.well-known/openid-configuration

{
    "token_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/token",
    "token_endpoint_auth_methods_supported": [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys",
    "response_modes_supported": [
        "query",
        "fragment",
        "form_post"
    ],
    "subject_types_supported": [
        "pairwise"
    ],
    "id_token_signing_alg_values_supported": [
        "RS256"
    ],
    "response_types_supported": [
        "code",
        "id_token",
        "code id_token",
        "token id_token",
        "token"
    ],
    "scopes_supported": [
        "openid"
    ],
    "issuer": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "microsoft_multi_refresh_token": true,
    "authorization_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/authorize",
    "device_authorization_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/devicecode",
    "http_logout_supported": true,
    "frontchannel_logout_supported": true,
    "end_session_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/logout",
    "claims_supported": [
        "sub",
        "iss",
        "cloud_instance_name",
        "cloud_instance_host_name",
        "cloud_graph_host_name",
        "msgraph_host",
        "aud",
        "exp",
        "iat",
        "auth_time",
        "acr",
        "amr",
        "nonce",
        "email",
        "given_name",
        "family_name",
        "nickname"
    ],
    "check_session_iframe": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/oauth2/checksession",
    "userinfo_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/openid/userinfo",
    "kerberos_endpoint": "https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db47/kerberos",
    "tenant_region_scope": "WW",
    "cloud_instance_name": "microsoftonline.com",
    "cloud_graph_host_name": "graph.windows.net",
    "msgraph_host": "graph.microsoft.com",
    "rbac_url": "https://pas.windows.net"
}
```