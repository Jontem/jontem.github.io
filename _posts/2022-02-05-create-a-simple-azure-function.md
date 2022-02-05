---
title: Create a simple azure function
description: Crete a simple azure function that logs requests to an appendblob
date: 2022-02-05T00:00:00+00:00
layout: post
permalink: /create-a-simple-azure-function/
categories:
  - azure
tags:
  - serverless
  - Azure functions
  - dotnet
---

In this post I'll show you how to create a simple azure function which is triggered by a http call and logs the requests to an `appendblob` on azure blob storage.

## Create a new project

First of all you will need to create a project. I just used the visual studio project template for azure functions(The Azure SDK is needed). Choose to create a Http trigger function and set the authorization level to anonymous

## The code

Once created you will get a stub for a working http trigger function. Below is an example how to log the request to an appendblob.

```cs
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Azure.Storage.Blobs;
using System.Globalization;
using Azure.Storage.Blobs.Specialized;
using Azure.Storage.Blobs.Models;
using System.Text;
using Microsoft.Extensions.Primitives;
using System.Linq;

namespace MyAzureFunction
{
  public static class MyAzureFunction
    {
    [FunctionName("MyAzureFunction")]
    public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] HttpRequest req,
    [Blob("my-blob-container")] BlobContainerClient container,
    ILogger log)
    {
      var culture = new CultureInfo("sv-SE");
      var clientIp = GetClientIp(req.Headers);

      var logStr = $"{DateTime.Now.ToString("T", culture)} {clientIp}\n";
      log.LogInformation(logStr);

      await container.CreateIfNotExistsAsync();
      var blobName = $"access_{DateTime.Now.ToString("d", culture).Replace("-", "")}.log";
      var appendBlob = container.GetAppendBlobClient(blobName);
      await appendBlob.CreateIfNotExistsAsync();
      await appendBlob.SetHttpHeadersAsync(new BlobHttpHeaders() { ContentType = "text/plain" });
      byte[] blockContent = Encoding.UTF8.GetBytes(logStr);
      using (var ms = new MemoryStream(blockContent))
      {
        await appendBlob.AppendBlockAsync(ms);
      }

      return new OkResult();
    }

    private static string GetClientIp(IHeaderDictionary headers)
    {
      headers.TryGetValue("X-Forwarded-For", out StringValues value);
      var clientIp = value.FirstOrDefault() ?? "missing-ip";
      return clientIp;
    }
  }
}
```

The first interesting part is the `[Blob("my-blob-container")] BlobContainerClient container`. This injects a `BlobContainerClient` that will be used with our interactions with the blob storage of our configured azure storage account. 

It is important to use the new `Azure.Storage.Blobs` namespace. I had some troubles following old code examples that used deprecated namespaces and types. More information can be found here [https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob-output?tabs=csharp#additional-types](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob-output?tabs=csharp#additional-types ).

The function creates an appendblob for every new date and sets the `content-type` of the blob to `text/plain`. It then adds a row to the blob for every incoming http get request.
