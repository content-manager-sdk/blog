---
layout: post
title: Record Type sorting
categories: []
tags: dotnet-sdk
status: publish
type: post
published: true
---

The Content Manager user options allow you to sort Record Types, how do you achieve this using the SDK?

First you want to find our whether the option to display your favourite Record Types first is switched on, and if not to switch it on (or off).

```
var options = new TrimUserOptionSet(db, UserOptionSetIds.SelectRecordType);
var favFirst = options.GetPropertyBool(PropertyIds.SelectRecordTypeUserOptionsFavoritesFirst);

Console.WriteLine(favFirst.ToString());

options.SetPropertyBool(PropertyIds.SelectRecordTypeUserOptionsFavoritesFirst, !favFirst);
options.Save();
```

Next you will want to make one or more Record Types a favourite for this user and then move it up or down in the list.

```
var recordType = new RecordType(db, "Document");
recordType.AddToFavorites();
recordType.ChangePositionWithinFavorites(LabelPositionChange.MoveUp);
```