---
layout: post
title: Save PDF files to AWS S3 in Blazor PDF Viewer Component | Syncfusion
description: Learn here all about how to save PDF files to AWS S3 in Syncfusion Blazor PDF Viewer component and much more details.
platform: Blazor
control: PDF Viewer
documentation: ug
---

# Save PDF file to AWS S3

To save a PDF file to AWS S3, you can follow the steps below

**Step 1:** Create a Simple PDF Viewer Sample in blazor

Start by following the steps provided in this [link](https://blazor.syncfusion.com/documentation/pdfviewer/getting-started/server-side-application) to create a simple PDF viewer sample in blazor. This will give you a basic setup of the PDF viewer component.

**Step 2:** Include the following namespaces in the **Index.razor** file.

1. Import the required namespaces at the top of the file:

```csharp
using System.IO;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Specialized;
```

**Step 3:** Add the below code example to save the downloaded PDF files to Azure Blob Storage container

```csharp

@page "/"
<SfButton @onclick="OnClick">Download</SfButton>
<SfPdfViewerServer @ref="@viewer" Height="500px" Width="1060px" DocumentPath="@DocumentPath" />

@code {
    private SfPdfViewerServer viewer;
    private string DocumentPath { get; set; }

    private readonly string connectionString = "Your Connection string from Azure";
    private readonly string containerName = "Your container name in Azure";
    private readonly string fileName = "File Name to be loaded into Syncfusion PDF Viewer";

    public async void OnClick(MouseEventArgs args)
    {
        byte[] data = await viewer.GetDocument();

        BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

        BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);

        string result = Path.GetFileNameWithoutExtension(fileName);

        // Get a reference to the blob
        BlobClient blobClient = containerClient.GetBlobClient(result + "_downloaded.pdf");

        using (MemoryStream stream = new MemoryStream(data))
        {
            blobClient.Upload(stream, true);
        }
    }
}

```

N> Replace **Your Connection string from Azure** with the actual connection string for your Azure Blob Storage account, **File Name to be Loaded into Syncfusion PDF Viewer** with the file name to load from the Azure container to the PDF Viewer, and **Your container name in Azure** with the actual container name.

N> The **Azure.Storage.Blobs** NuGet package must be installed in your application to use the previous code example.

[View sample in GitHub](https://github.com/SyncfusionExamples/open-save-pdf-documents-in-azure-blob-storage).