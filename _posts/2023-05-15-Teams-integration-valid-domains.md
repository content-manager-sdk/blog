---
layout: post
title: Troubleshooting Teams - setting validDomains
categories: []
tags: teams
status: publish
type: post
published: true
---


If Content Manager specifies a URL for the Web Client then the Teams integration will include this in the tab links when adding the integration to a channel.  If the full web domain is not specified in the Teams integration manifest that was uploaded to install the integration then the process of adding a tab to a channel will fail.

In this example the domain `intranet.acme.com` must be included in the list of valid domains.

![System options](/images/cm_url.png)

```js
"validDomains": [
        "intranet.acme.com"
    ],
```