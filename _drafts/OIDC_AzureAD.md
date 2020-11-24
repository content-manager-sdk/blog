---
layout: post
title: Configuring OpenId Connect for Azure AD
categories: []
tags: azure-ad serviceapi authentication web-client webdrawer
status: publish
type: post
published: true
meta: {}
---


## Overview

As of CM 10 the CM web applications (ServiceAPI, WebDrawer and Web Client) have an OpenId Connect authentication provider built in. This document describes creating an Azure Ad application and configuring the CM web application.

### Create the Azure AD Application

To create the Azure AD application:

1. From with portal.azure.com go to Azure AD.
1. Go to App Registrations and select New Registration.
1. Enter a name.
1. Under Redirect URI leave Web selected.
1. The value in the Redirect URI is important, it must be lowercase and it must be the URL to your application (e.g. https://mydomain.com/cmwebdrawer) followed by the path to the authentication provider (for example /auth/openid). The /auth/ component is fixed but the 'openid' is the name you will supply in hptrim.config later and so can be any string, as long as it matches the value in hptrim.config.
For the web client the path must include the path to the ServiceAPI, for example: https://mydomain.com/contentManager/serviceapi/auth/openid.

#### Example

![](/images/azuread_app_1.png)

### Add a secret

From Certificates and Secrets add a secret and copy it some where you can find it later.

#### Example

![](/images/azuread_secret.png)

### Configure authentication in hptrim.config

To use the Azure AD app created above edit hptrim.config (hprmServiceAPI.config in the Web Client) so that it has am authentication similar to the one below.

1.  The name must match the last segment of the Redirect URI path.
2.  Client ID is the application ID from the Azure Ad Overview.
3.  The secret is the one saved when creating the App. If it was not saved created a new one in Certificates and Secrets.
4.  Get the issuerUri from Overview > Endpoints > OpenID Connect metadata document

#### Example config

```xml
	<authentication allowAnonymous="false" slidingSessionMinutes="60">
		<openIdConnect>
			<add
				name="openid"
				clientID="ae39011d-52e7-4ecc-b9eb-87d6d876dd"
				clientSecret="_MqXXXXXXXXXXXXXXXG[sp3GrMfD:"
				issuerURI="https://login.microsoftonline.com/08363ee4-6592-4325-9d5a-123456789/v2.0/.well-known/openid-configuration"
			/>
		</openIdConnect>
	</authentication>
```

### Enable redirect

The web client will not re-direct the authentication endpoint unless the Html feature is enabled in hprmServiceAPI.config. To do this:

1.  edit hprmServiceAPI.config
2.  find the property named 'serviceFeatures'
3.  Add the feature Html

### Logout

For WebDrawer the logout link is configured in the uiSettings. It should contain '~/auth/logout'. In the Web Client a logout link will be displayed automatically when OpenId Connect authentication is enabled.

#### Example

```xml
  <uiSettings
    logoutLink="~/auth/logout"
	...
  />
```

### Allow anonymous access in the IIS

IIS will not be handling authentication so we use IIS Manager to allow anonymous access only.
![image 1](/images/iis_anon.PNG)
