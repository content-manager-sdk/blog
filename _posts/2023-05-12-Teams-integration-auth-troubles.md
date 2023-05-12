---
layout: post
title: Troubleshooting Teams / OpenID Connect server connections
categories: []
tags: teams openid
status: publish
type: post
published: true
---


A vital element of the Content Manager Teams integration is the ServiceAPI Azure AD authentication setup.  For the authentication to work it is essential that the ServiceAPI server be able to communicate with AzureAD, which may require web proxy configuration, adding the server to a white list or some other network configuration.  If the server is unable to communicate to Azure AD you may get some confusing errors in the ServiceAPI logs.  The PowerShell script below mimics the code used by the ServiceAPI when it first connects to the ServiceAPI allowing you to verify that the basic requirement of connectivity is met before proceeding to other troubleshooting.

To use this code: 
 1) create a ps1 file in the ServiceAPI bin folder
 2) copy the script below to the file,
 3) in the Azure portal, open an App registration, got to Endpoints and copy the 'OpenID Connect metadata document,
 4) replace the value in $url in the script with your 'OpenID Connect metadata document' URL
 5) run the script, NOTE: ensure that you are running the script logged in as the same user selected for the ServicAPI identity
 
You should see displayed the metadata for your app registration.

```ps1 
$assemblyPath1 = (Join-Path $PSScriptRoot "Microsoft.IdentityModel.Protocols.dll")
$assemblyPath2 = (Join-Path $PSScriptRoot "Microsoft.IdentityModel.Protocols.OpenIdConnect.dll")
$assemblyPath3 = (Join-Path $PSScriptRoot "Microsoft.IdentityModel.Tokens.dll")


try {
Add-Type -Path $assemblyPath1
Add-Type -Path $assemblyPath2



$retriever = New-Object Microsoft.IdentityModel.Protocols.OpenIdConnect.OpenIdConnectConfigurationRetriever
$url = "https://login.microsoftonline.com/08363ee4-6592-4325-9d5a-5a25e00d482b/v2.0/.well-known/openid-configuration"

$configurator = New-Object -TypeName Microsoft.IdentityModel.Protocols.ConfigurationManager[Microsoft.IdentityModel.Protocols.OpenIdConnect.OpenIdConnectConfiguration] @($url, $retriever)

$getConfig = $configurator.GetConfigurationAsync()

$getConfig.Wait()

$getConfig.Result

} 
catch [System.AggregateException]
    {
     $_.Exception.ToString() | out-host

      $_.Exception.Handle({
            param($ex)

            $ex.Message | out-host
           
            return $false
            
        })

    }
catch [System.Exception]
{
    $_.Exception.ToString() | out-host
} 

```