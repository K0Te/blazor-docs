---
layout: post
title:  Open PDF files from Azure Blob storage  in Blazor PDF Viewer Component | Syncfusion
description: Learn here all about how to Open PDF files from Azure Blob storage  in Syncfusion Blazor PDF Viewer component and much more details.
platform: Blazor
control: PDF Viewer
documentation: ug
---

# Open PDF file from Azure Blob storage 

To load a PDF file from Azure Blob storage  in a PDF Viewer, you can follow the steps below

**Step 1:** Create a Simple PDF Viewer Sample in blazor

Start by following the steps provided in this [link](https://blazor.syncfusion.com/documentation/pdfviewer/getting-started/server-side-application) to create a simple PDF viewer sample in blazor. This will give you a basic setup of the PDF viewer component.

**Step 2:** Include the following namespaces in the **Index.razor** file.

1. Import the required namespaces at the top of the file:

```csharp
using System.IO;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Specialized;
```

**Step 3:** Add the below code example to load a PDF from `Azure blob storage` container.

```csharp

@page "/"

<SfPdfViewerServer Height="500px" Width="1060px" DocumentPath="@DocumentPath" />

@code {
    private SfPdfViewerServer viewer;
    private string DocumentPath { get; set; }

    private readonly string connectionString = "Your Connection string from Azure";
    private readonly string containerName = "Your container name in Azure";
    private readonly string fileName = "File Name to be loaded into Syncfusion PDF Viewer";
	
    protected override void OnInitialized()
    {
        //Connection String of Storage Account
        BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);
        BlobContainerClient blobContainerClient = blobServiceClient.GetBlobContainerClient(containerName);
        BlockBlobClient blockBlobClient = blobContainerClient.GetBlockBlobClient(fileName);
        MemoryStream memoryStream = new MemoryStream();
        blockBlobClient.DownloadTo(memoryStream);
        DocumentPath = "data:application/pdf;base64," + Convert.ToBase64String(memoryStream.ToArray());
    }
}
```

N> Replace **Your Connection string from Azure** with the actual connection string for your Azure Blob Storage account, **File Name to be Loaded into Syncfusion PDF Viewer** with the file name to load from the Azure container to the PDF Viewer, and **Your container name in Azure** with the actual container name.

N> The **Azure.Storage.Blobs** NuGet package must be installed in your application to use the previous code example.

[View sample in GitHub](https://github.com/SyncfusionExamples/open-save-pdf-documents-in-azure-blob-storage).