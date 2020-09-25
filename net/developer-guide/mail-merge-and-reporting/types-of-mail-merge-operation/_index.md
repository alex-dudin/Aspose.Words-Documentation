---
title: Types of Mail Merge Operation
aliases:
  - /how-to-execute-mail-merge/
type: docs
description: "Aspose.Words allows you to perform two different types of mail merge operations: simple mail merge and mail merge with regions. Simple mail merge repeats the entire document per each data source record, whereas mail merge with regions repeats only designated regions per record."
keywords: "how to execute mail merge c#"
weight: 20
url: /net/types-of-mail-merge-operations/
---

The main idea of mail merge is to automatically create a document or multiple documents based on your template and data fetched from your data source. Aspose.Words allows you to perform two different types of mail merge operations: simple mail merge and mail merge with regions.

The most common example of using simple mail merge is when you want to send a document for different clients by including their names at the beginning of the document. To do this, you need to create merge fields such as *First Name* and *Last Name* in your template, and then fill them in with data from your data source. Whereas the most common example of using mail merge with regions is when you want to send a document that includes specific orders with the list of all items within each order. To do this, you will need to create merge regions inside your template – own region for each order, in order to fill it with all required data for the items.

The main difference between both merge operations is that simple mail merge (without regions) repeats the entire document per each data source record, whereas mail merge with regions repeats only designated regions per record. You can think of a simple mail merge operation as a particular case of merge with regions where the only region is the whole document.

## **Simple Mail Merge Operation**

A simple mail merge is used to fill the mail merge fields inside your template with the required data from your data source (single table representation). So it is similar to the classic mail merge in Microsoft Word.

You can add one or more merge fields in your template and then execute the simple mail merge operation. It is recommended to use it if your template does not contain any merge regions.

The main limitation of using this type is the whole document content will be repeated for each record in the data source.

### **How to Execute a Simple Mail Merge Operation**

Once your template is ready, you can start performing the simple mail merge operation. Aspose.Words allows you to execute a simple mail merge operation using different [Execute methods](https://apireference.aspose.com/words/net/aspose.words.mailmerging/mailmerge/methods/execute/index) that accept various data objects as the data source.

The following code example shows how to execute a simple mail merge operation using one of the [Execute](https://apireference.aspose.com/words/net/aspose.words.mailmerging.mailmerge/execute/methods/5) method:

**.NET**
{{< highlight csharp >}}
Public void SimpleMailMerge()
{
	// Include the code for our template.
	Document doc = new Document();
	DocumentBuilder builder = new DocumentBuilder(doc);

​	// Create Merge Fields
​	builder.InsertField(" MERGEFIELD CustomerName ");
​	builder.InsertParagraph();
​	builder.InsertField(" MERGEFIELD Item ");
​	builder.InsertParagraph();
​	builder.InsertField(" MERGEFIELD Quantity ");

​	builder.Document.Save(ArtifactsDir + "MailMerge.TestTemplate.docx");
​	

​	// Fill the fields in the document with user data
​	doc.MailMerge.Execute(new string[] { "CustomerName", "Item", "Quantity" }, new object[] { "John Doe", "Hawaiian", "2"});

​	builder.Document.Save("Your local path to save the document" + "MailMerge.Simple.docx");
}
{{< /highlight >}}

You can notice the difference between the document before executing simple mail merge:

![simple_mail_merge_template](execute_simple_mail_merge_1.png)

And after executing simple mail merge:

![execute_simple_mail_merge](execute_simple_mail_merge_2.png)

### **How to Create Multiple Merged Documents**

In Aspose.Words, the standard mail merge operation fills only a single document with content from your data source. So, you will need to execute the mail merge operation multiple times to create multiple merged documents as an output.

The following code example shows how to generate multiple merged documents during a mail merge operation:

**.NET**
{{< highlight csharp >}}
//Put the path to the documents directory and open the template:
string dataDir = RunExamples.GetDataDir_MailMergeAndReporting();
Document doc = new Document(dataDir + "TestFile.doc");

// Open the database connection.
string connString = @"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" + dataDir + "Customers.mdb";
OleDbConnection conn = new OleDbConnection(connString);
conn.Open();

// Get data from a database.
OleDbCommand cmd = new OleDbCommand("SELECT * FROM Customers", conn);
OleDbDataAdapter da = new OleDbDataAdapter(cmd);
DataTable data = new DataTable();
da.Fill(data);

//Perform a loop through each DataRow to iterate through the DataTable.
//Clone the template document instead of loading it from disk for better speed performance before the mail merge operation.
//You can load the template document from a file or stream but it is faster to load the document only once and then clone it in memory before each mail merge operation.

int counter = 1;
foreach (DataRow row in data.Rows)
    {
        Document dstDoc = (Document)doc.Clone(true);
        dstDoc.MailMerge.Execute(row);
        dstDoc.Save(string.Format(dataDir + "TestFile_out{0}.doc", counter++));
    }

Console.WriteLine("\nProduce multiple documents performed successfully.\nFile saved at " + dataDir);
{{< /highlight >}}

{{% alert color="primary" %}}

You can download the sample file of this example from [Aspose.Words GitHub](https://github.com/aspose-words/Aspose.Words-for-.NET/tree/master/Examples/Data/Mail-Merge).

{{% /alert %}}

## **Mail Merge with Regions**

You can create different regions in your template to have special areas that you can simply fill with your data. Use the mail merge with regions if you want to insert tables, rows with repeating data to make your documents dynamically grow by specifying those regions within your template.

You can create nested (child) regions as well as merge regions. The main advantage of using this type is to dynamically increase parts inside a document. See more details in the next article "Nested Mail Merge with Regions".

### **How to Execute Mail Merge with Regions**

A mail merge region is a specific part inside a document that has a start point and an end point. Both points are represented as mail merge fields that have specific names *"TableStart:XXX"* and *"TableEnd:XXX"*. All content that is included in a mail merge region will automatically be repeated for every record in the data source.

Aspose.Words allows you to execute mail merge with regions using different [Execute methods](https://apireference.aspose.com/words/net/aspose.words.mailmerging/mailmerge/methods/executewithregions/index) that accept various data objects as the data source.

As a first step, we need to create the DataSet to pass it later as an input parameter to the ExecuteWithRegions method:

**.NET**
{{< highlight csharp >}}
private static DataSet CreateDataSet()
{
// Create the customers table
DataTable tableCustomers = new DataTable("Customers");
tableCustomers.Columns.Add("CustomerID");
tableCustomers.Columns.Add("CustomerName");
tableCustomers.Rows.Add(new object[] { 1, "John Doe" });
tableCustomers.Rows.Add(new object[] { 2, "Jane Doe" });

// Create the orders table
DataTable tableOrders = new DataTable("Orders");
tableOrders.Columns.Add("CustomerID");
tableOrders.Columns.Add("ItemName");
tableOrders.Columns.Add("Quantity");
tableOrders.Rows.Add(new object[] { 1, "Hawaiian", 2 });
tableOrders.Rows.Add(new object[] { 2, "Pepperoni", 1 });
tableOrders.Rows.Add(new object[] { 2, "Chicago", 1 });

// Add both tables to a data set
DataSet dataSet = new DataSet();
dataSet.Tables.Add(tableCustomers);
dataSet.Tables.Add(tableOrders);

// The "CustomerID" column, also the primary key of the customers table is the foreign key for the Orders table
dataSet.Relations.Add(tableCustomers.Columns["CustomerID"],    tableOrders.Columns["CustomerID"]);

return dataSet;
}
{{< /highlight >}}

The following code example shows how to execute mail merge with regions using the [ExecuteWithRegions(DataSet)](https://apireference.aspose.com/words/net/aspose.words.mailmerging.mailmerge/executewithregions/methods/2) method:

**.NET**
{{< highlight csharp >}}
public void MailMergeWithRegions()
{
Document doc = new Document();
DocumentBuilder builder = new DocumentBuilder(doc);

// The start point of mail merge with regions
// the dataset
builder.InsertField(" MERGEFIELD TableStart:Customers");
// Data from rows of the "CustomerName" column of the "Customers" table will go in this MERGEFIELD
builder.Write("Orders for ");
builder.InsertField(" MERGEFIELD CustomerName");
builder.Write(":");

// Create column headers
builder.StartTable();
builder.InsertCell();
builder.Write("Item");
builder.InsertCell();
builder.Write("Quantity");
builder.EndRow();

// We have a second data table called "Orders", which has a many-to-one relationship with "Customers"
// picking up rows with the same CustomerID value
builder.InsertCell();
builder.InsertField(" MERGEFIELD TableStart:Orders");
builder.InsertField(" MERGEFIELD ItemName");
builder.InsertCell();
builder.InsertField(" MERGEFIELD Quantity");
builder.InsertField(" MERGEFIELD TableEnd:Orders");
builder.EndTable();

// The end point of mail merge with regions
builder.InsertField(" MERGEFIELD TableEnd:Customers");

// pass our dataset to perform mail merge with regions            
DataSet customersAndOrders = CreateDataSet();
doc.MailMerge.ExecuteWithRegions(customersAndOrders);

// Save the result
doc.Save("Your local path to save the document" + "MailMerge.ExecuteWithRegions.docx");
}
{{< /highlight >}}

You can notice the difference between the document before executing mail merge with regions:

![mail_merge_with_regions_template](execute_mail_merge_with_regions_1.png)

And after executing mail merge with regions:

![mail_merge_with_regions_execute](execute_mail_merge_with_regions_2.png)

### **Limitations of Mail Merge with Regions**

There are some important points that you need to consider when performing a mail merge with regions:

* The start point *TableStart:Orders* and the end point *TableEnd:Orders* both need to be in the same row or cell. For example, if you start a merge region in a cell of a table, you must end the merge region in the same row as the first cell.
* The merge field name must match the column’s name in your DataTable. Unless you have specified mapped fields, the mail merge with regions will not be successful for any merge field that has a different name than the column’s name.

If one of these rules is broken, you will get unexpected results or an exception may be thrown.

{{% alert color="primary" %}}

If you do not use mail merge regions, then it will be similar to Microsoft Word mail merge, and the whole document content will be repeated for each record in the data source.

{{% /alert %}}
