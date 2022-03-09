---
layout: post
title: Teams and Office Installer
categories: []
tags: azure-ad office-integration
status: publish
type: post
published: true
---

As at the date of my publishing this post Content Manage 10.1 is still to be released but those in the beta program will have access to a preview in the near future. To assist in installing the zero footprint Office and Teams integrations there is a [powershell script](https://raw.githubusercontent.com/content-manager-sdk/Community/master/docs/files/101/az_install.ps1) to create the Azure AD app and the related Office365 manifests.  In the video below I demonstrate using this script.



 [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) is a pre-requisite for using this script.
 
 ### Protected APIs
 
 To use the Microsoft APIs to retrieve either channel posts or group chats you need special permission from Microsoft, this may be request using their [request form](https://docs.microsoft.com/en-us/graph/teams-protected-apis).  You will need to supply the ID of the Azure AD app created for the Teams Integration as well as the reason you need organisation wide access to posts, probably for compliance purposes.
 
<iframe src="https://player.vimeo.com/video/680202809?app_id=122963&amp;wmode=opaque" width="640" height="400" frameborder="0" title="Power BI" allow="autoplay; fullscreen" allowfullscreen=""></iframe>