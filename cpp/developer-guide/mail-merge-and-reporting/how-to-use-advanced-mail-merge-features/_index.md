---
title: How to Use Advanced Mail Merge Features
type: docs
weight: 20
url: /cpp/how-to-use-advanced-mail-merge-features/
---

## How to Use Advanced Mail Merge Features

The **MailMerge** class provides some additional properties and methods that allow further customization of the mail merge process.

### Using Mapped Fields

The **MailMerge** class allows you to automatically map between names of fields in your data source and names of mail merge fields in the document. To perform this, use the **MailMerge.MappedDataFields** property that returns a **MappedDataFieldCollection** object. **MappedDataFieldCollection** is a collection of string keys and string values. The keys are the names of mail merge fields in the document and the values are the names of fields in your data source. The class provides all properties and methods typical for a regular collection such as **MappedDataFieldCollection.Add**, **MappedDataFieldCollection.Clear**, **MappedDataFieldCollection.Remove**, etc. The code sample below shows how to add a mapping when a merge field in a document and a data field in a data source have different names.

{{< gist "aspose-com-gists" "518f03cac02abb105e02f55edb7de9f9" "cpp-Programming-Documents-Fields-GetFieldNames-MappedDataFields.cpp" >}}

### Obtaining Merge Field Names

You can get a collection of the merge field names available in the document. Call **MailMerge.GetFieldNames** that returns an array of string that contains the names. The method supports extended syntax in field names. A new string array is created on every call. The method does not eliminate duplicate field names. The following code example shows how to get names of all merge fields in a document.

{{< gist "aspose-com-gists" "518f03cac02abb105e02f55edb7de9f9" "cpp-Programming-Documents-Fields-GetFieldNames-GetFieldNames.cpp" >}}

### Deleting Merge Fields

Sometimes you may want to remove any mail merge fields from a document without executing mail merge, for instance, if you are processing a document and wish to remove any merge fields before conversion. To achieve this you can use **MailMerge.DeleteFields** to remove all remaining mail merge fields. This method removes MERGEFIELD and NEXT fields from the document.

This method is not influenced by any flags set under MailMerge.CleanupOptions and executing this method only removes merge fields – not any containing fields or empty paragraphs. If such options are desired then the **MailMerge.CleanupOptions** property should be used instead. The following code example demonstrates how to delete all merge fields from a document without executing mail merge.

{{< gist "aspose-com-gists" "518f03cac02abb105e02f55edb7de9f9" "cpp-Programming-Documents-Fields-GetFieldNames-DeleteFields.cpp" >}}
