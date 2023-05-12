---
layout: post
title: Partial file download
categories: []
tags: serviceapi
status: publish
type: post
published: true
---

I was asked recently about downloading files from the ServiceAPI using the Content-Range header to support chunked downloads.  I was sure that this was supported but it was not working. After some hours of digging I found a problem that caused normal document downloads to ont recognize Content-Range, oddly enough the service to return multiple documents as a zip was working.

### Multiple files as a zip
The standard download URL may be used to return documents for multiple records as a zip by using comma separated URI's, for example:

```
https://mydev.dev/ServiceAPI/Record/9000000001,9000000002/File/document
```

If any of the records do not have a document attached they will be ignored.

### Custom download addin
If you need to use chunked downloads another solution may be to install the [sample plugin](https://github.com/content-manager-sdk/Community/tree/master/Samples/ServiceAPI/CSharp/PartialDownloadPlugin) in the samples.  When added to the ServiceAPI this will add a service called 'PartialDownload' that support the Content-Range header.  Some code that can be used as a starting point to test this can be found in the getDocumentInChunks sample in the [ServiceAPI console app sample](https://github.com/content-manager-sdk/Community/tree/master/Samples/ServiceAPI/CSharp/ConsoleServiceAPIClient).

