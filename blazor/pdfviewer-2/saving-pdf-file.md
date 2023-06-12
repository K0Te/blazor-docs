---
layout: post
title: Saving PDF file in Blazor SfPdfViewer Component | Syncfusion
description: Checkout and learn here all about saving PDF file in Syncfusion Blazor SfPdfViewer component and more.
platform: Blazor
control: SfPdfViewer
documentation: ug
---

# Saving PDF file in Blazor SfPdfViewer Component

After editing the PDF file with various annotation tools, you will need to save the updated PDF to the server, database, or local file system.

## Save PDF file to Server

You might need to save the PDF file back to the server.

```cshtml
@using Syncfusion.Blazor.SfPdfViewer
@using Syncfusion.Blazor.Buttons
@using System.IO

<SfButton OnClick="OnClick">Save</SfButton>
<SfPdfViewer2 DocumentPath="@DocumentPath" @ref="viewer" Height="100%" Width="100%"></SfPdfViewer2>

@code{  
    SfPdfViewer2 viewer;
    public async void OnClick(MouseEventArgs args)
    {
        byte[] data = await viewer.GetDocumentAsync();
        //PDF document file stream
        Stream stream = new MemoryStream(data);
        using (var fileStream = new FileStream(@"wwwroot/Data/PDF_Succinctly_Updated.pdf", FileMode.Create, FileAccess.Write))
        {
            //Saving the new file in root path of application
            stream.CopyTo(fileStream);
            fileStream.Close();
        }
        stream.Close();
    }
    public string DocumentPath { get; set; } = "wwwroot/Data/PDF_Succinctly.pdf";
}
```

## Save PDF file to Database

If you have plenty of PDF files stored in database and you want to save the updated PDF file back to the database, use the following code example.

```cshtml
@using Syncfusion.Blazor.SfPdfViewer
@using Syncfusion.Blazor.Buttons
@using System.Data.SqlClient

<SfButton OnClick="OnClick">Save</SfButton>
<SfPdfViewer2 DocumentPath="@DocumentPath" @ref="viewer" Height="100%" Width="100%">
</SfPdfViewer2>

@code{
    SfPdfViewer2 viewer;

    public async void OnClick(MouseEventArgs args)
    {
        string DocumentName = "PDF_Succinctly";
        byte[] data = await viewer.GetDocumentAsync();
        string connectionString = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=D:\database.mdf;";
        string queryStmt = "Update PDFFiles SET Content = @Content where DocumentName = '" + DocumentName + "'";
        using (SqlConnection con = new SqlConnection(connectionString))
        {
            using (SqlCommand cmd = new SqlCommand(queryStmt, con))
            {
                SqlParameter param = cmd.Parameters.Add("@Content", System.Data.SqlDbType.VarBinary);
                param.Value = data;
                con.Open();
                cmd.ExecuteNonQuery();
                con.Close();
            }
        }

    }
    private string DocumentPath { get; set; } = "wwwroot/Data/PDF_Succinctly.pdf";
}
```

## Download PDF file as a copy

In the built-in toolbar, you have an option to download the updated PDF to the local file system, you can use it to download the PDF file.

```cshtml
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.SfPdfViewer

<SfButton @onclick="OnClick">Download</SfButton>
<SfPdfViewer2 @ref="@viewer" Height="100%" Width="100%" DocumentPath="@DocumentPath" />

@code{
SfPdfViewer2 viewer;
public void OnClick(MouseEventArgs args)
{
    viewer.DownloadAsync();
}
public string DocumentPath { get; set; } = "wwwroot/data/PDF_Succinctly.pdf";
}
```