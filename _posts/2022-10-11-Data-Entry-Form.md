---
layout: post
title: Form Definitions
categories: []
tags: dotnet-sdk
status: publish
type: post
published: true
---

For anyone wanting to build a user interface in Content Manager the FormDefinition class is essential.  Some of the object types allow a form defintionto be created from a the FormDefintion contructor for new or existing objects.  For records might also choose to use the RecordType.RecordPropertiesFormDefinition property.

In this very simple code I use the form definition to determine the mandatory fields for a particular RecordType.

```
var recordType = new RecordType(db, "Document");

var formDef = recordType.RecordPropertiesFormDefinition;

for (uint pageCount = 0; pageCount < formDef.PageCount; pageCount++)
{
    var page = formDef.GetPage(pageCount);

    for (uint itemCount = 0; itemCount < page.ItemCount; itemCount++)
    {
        var prop = page.GetItem(itemCount);
        Console.WriteLine($"{prop.Caption} - {prop.IsMandatory}");
    }
}
```