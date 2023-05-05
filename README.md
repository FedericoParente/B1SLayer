# B1SLayer - SAP Business One Service Layer Client for .NET

![B1SLayer](https://img.shields.io/nuget/v/B1SLayer.svg?maxAge=3600&label=B1SLayer)

B1SLayer is a lightweight, fluent, and easy-to-use SAP Business One Service Layer client for .NET developers. It simplifies the process of consuming SAP Business One Service Layer by providing:

- Fluent and easy Service Layer requests
- Automatic session management
- Automatic retry of failed requests

## 📚 Documentation

For a detailed explanation and tutorial, please refer to [this blog post on SAP Community](https://blogs.sap.com/2022/05/23/b1slayer-a-clean-and-easy-way-to-consume-sap-business-one-service-layer-with-.net/).

## 🚀 Getting Started

### Installation

Install B1SLayer from NuGet using Package Manager Console:

```sh
PM> Install-Package B1SLayer
```

Or, using .NET CLI:

```sh
dotnet add package B1SLayer
```

### Usage

Below are some examples demonstrating various features and capabilities of B1SLayer:

#### Creating a Connection

```csharp
var serviceLayer = new SLConnection("https://sapserver:50000/b1s/v1", "CompanyDB", "manager", "12345");
```

#### Setting Up Request Monitoring/Logging

```csharp
serviceLayer.AfterCall(async call =>
{
    Console.WriteLine($"Request: {call.HttpRequestMessage.Method} {call.HttpRequestMessage.RequestUri}");
    Console.WriteLine($"Body sent: {call.RequestBody}");
    Console.WriteLine($"Response: {call.HttpResponseMessage?.StatusCode}");
    Console.WriteLine(await call.HttpResponseMessage?.Content?.ReadAsStringAsync());
    Console.WriteLine($"Call duration: {call.Duration.Value.TotalSeconds} seconds");
});
```

#### Performing GET, POST, PATCH, and DELETE Requests

```csharp
// GET request example
var order = await serviceLayer.Request("Orders", 823).GetAsync<MyOrderModel>();

// POST request example
var createdOrder = await serviceLayer.Request("Orders").PostAsync<MyOrderModel>(myNewOrderObject);

// PATCH request example
await serviceLayer.Request("BusinessPartners", "C00001").PatchAsync(new { CardName = "Updated BP name" });

// DELETE request example
await serviceLayer.Request("BusinessPartners", "C00001").DeleteAsync();
```

#### Adding or Updating Item Image

```csharp
await serviceLayer.Request("ItemImages", "A00001").PatchWithFileAsync(@"C:\ItemImages\A00001.jpg");
```

#### Uploading Attachments

```csharp
var attachmentEntry = await serviceLayer.PostAttachmentAsync(@"C:\files\myfile.pdf");
```

#### Executing Batch Requests

```csharp
HttpResponseMessage[] batchResult = await serviceLayer.PostBatchAsync(req1, req2, req3);
```

#### Logging Out

```csharp
await serviceLayer.LogoutAsync();
```

For more examples and detailed explanations, please refer to the [blog post on SAP Community](https://blogs.sap.com/2022/05/23/b1slayer-a-clean-and-easy-way-to-consume-sap-business-one-service-layer-with-.net/).

## 🙏 Special Thanks

B1SLayer is based on and depends on the fantastic [Flurl](https://github.com/tmenier/Flurl) library. A huge thanks to Todd Menier for creating Flurl!
