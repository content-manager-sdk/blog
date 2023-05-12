---
layout: post
title: Configuring OpenId Connect for Azure AD in the Content Manager desktop client
categories: []
tags: azure-ad
status: publish
type: post
published: true
meta: {}
---

## Overview

OpenID Connect may be used to authenticate with the Content Manager desktop client, this document details how to configure this.

### Create the Azure AD Application

To create the Azure AD application:

1. From with portal.azure.com go to Azure AD.
1. Go to App Registrations and select New Registration.
1. Enter a name.
1. Select 'Public native client' in the Redirect URI
1. Enter this redirect Uri 'urn:ietf:wg:oauth:2.0:oob'

#### Example

![](/images/azuread_app_desktop_1.png)

### Configure permissions

From API Permissions add the following delegated Microsoft Graph permissions:
 * email
 * offline_access
 * openid
 * profile
 * User.Read
 
 Select 'Grant Admin Access' to grant access to all permissions.

#### Example

![](/images/azuread_app_desktop_2.png)

### Configure authentication in Content Manager Enterprise Studio

To use the Azure AD app created above open Content Manager Enterprise Studio and:
1. from the database choose Authentication > OpenID
1. The OpenID Issuer URL is taken from the Azure App - Ovrview > Endpoints > OpenID Connect metadata document
1. The Client Id is taken from the Azure App - Ovrview > Application ID
1. Client secret should be empty
1. Client app scope should contain 'openid email offline_access'



#### Example

![](/images/azuread_app_desktop_3.png)

### Configure Azure AD for Office Integration access

The office integration requires an access token to allow it ro authenticate with the Web Client, this can be configured in Azure AD:

1. create an Azure App for Web Authentication, you may use the one you created to authenticate with the CM Web Client,
1. create (or edit) the file ADFS\config.xml in your CM Web Client installation folder and set it as follows:
   * clientAuthority - 'https://login.windows.net/' followed by your domain: e.g. https://login.windows.net/cmofficedev.onmicrosoft.com
   * clientResourceUri - the Application ID URI from your CM Web Client Azure App
   * clientID - the Application Id from your CM Desktop Azure App
   * clientReturnUri - urn:ietf:wg:oauth:2.0:oob

#### Example Config

```xml
<adfsClient>
<clientAuthority>https://login.windows.net/cmofficedev.onmicrosoft.com</clientAuthority>
<clientResourceUri>api://auchurchla02:3000/09f0ec5c-87e9-4568-8b60-4eb3e20de75e</clientResourceUri>
<clientID>cf2501bd-10f0-4ad6-96dc-f5cf7b2b3bf5</clientID>
<clientReturnUri>http://MyWebClient</clientReturnUri>
</adfsClient>
```

### Troubleshooting Azure AD for Office Integration access

#### Error AADSTS500011: The resource principal named https://MYSERVER/contentmanager/ was not found in the tenant named XXXX-XXXX-XXXXX-XXXXXX. 
If you get this error try using the Client ID in clientResourceUri, rather than Application ID URI.