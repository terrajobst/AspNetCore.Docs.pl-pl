---
title: gRPC konfiguracji platformy ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087344"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="60fd5-103">gRPC konfiguracji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60fd5-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="60fd5-104">Skonfiguruj opcje usługi</span><span class="sxs-lookup"><span data-stu-id="60fd5-104">Configure services options</span></span>

<span data-ttu-id="60fd5-105">W poniższej tabeli opisano opcje dotyczące konfigurowania usługi gRPC:</span><span class="sxs-lookup"><span data-stu-id="60fd5-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="60fd5-106">Opcja</span><span class="sxs-lookup"><span data-stu-id="60fd5-106">Option</span></span> | <span data-ttu-id="60fd5-107">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="60fd5-107">Default Value</span></span> | <span data-ttu-id="60fd5-108">Opis</span><span class="sxs-lookup"><span data-stu-id="60fd5-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="60fd5-109">Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera.</span><span class="sxs-lookup"><span data-stu-id="60fd5-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="60fd5-110">Podjęto próbę wysłać komunikat, który przekracza skutkuje rozmiar skonfigurowany maksymalny komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="60fd5-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="60fd5-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="60fd5-111">4 MB</span></span> | <span data-ttu-id="60fd5-112">Maksymalny rozmiar wiadomości w bajtów odebranych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="60fd5-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="60fd5-113">Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="60fd5-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="60fd5-114">Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="60fd5-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="60fd5-115">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usług.</span><span class="sxs-lookup"><span data-stu-id="60fd5-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="60fd5-116">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="60fd5-116">The default is `false`.</span></span> <span data-ttu-id="60fd5-117">Ustawienie tej opcji na `true` można przecieku informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="60fd5-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="60fd5-118">Gzip</span><span class="sxs-lookup"><span data-stu-id="60fd5-118">gzip</span></span> | <span data-ttu-id="60fd5-119">Kolekcja dostawców kompresji, które umożliwiają kompresję i dekompresję wiadomości.</span><span class="sxs-lookup"><span data-stu-id="60fd5-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="60fd5-120">Można tworzyć i dodawać je do kolekcji dostawców niestandardowych kompresji.</span><span class="sxs-lookup"><span data-stu-id="60fd5-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="60fd5-121">Domyślnie skonfigurowany dostawca obsługuje **gzip** kompresji.</span><span class="sxs-lookup"><span data-stu-id="60fd5-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="60fd5-122">Algorytm kompresji używany do skompresowania komunikatów wysyłanych z serwera.</span><span class="sxs-lookup"><span data-stu-id="60fd5-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="60fd5-123">Algorytm musi być zgodny dostawca kompresji w `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="60fd5-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="60fd5-124">Dla algorthm kompresowały odpowiedzi klienta musi wskazywać obsługuje algorytm przez wysłanie go **grpc zaakceptować encoding** nagłówka.</span><span class="sxs-lookup"><span data-stu-id="60fd5-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="60fd5-125">Poziom kompresji, używany do skompresowania komunikatów wysyłanych z serwera.</span><span class="sxs-lookup"><span data-stu-id="60fd5-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="60fd5-126">Można skonfigurować opcje dla wszystkich usług, zapewniając delegat opcje do `AddGrpc` wywołania `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60fd5-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="60fd5-127">Opcje dla jednej usługi zastępują opcje globalne w `AddGrpc` i może być konfigurowana przy użyciu `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="60fd5-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="60fd5-128">Skonfiguruj opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="60fd5-128">Configure Kestrel options</span></span>

<span data-ttu-id="60fd5-129">Serwer kestrel ma opcji konfiguracji, które wpływają na zachowanie gRPC dla platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60fd5-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="60fd5-130">Limit szybkości danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="60fd5-130">Request body data rate limit</span></span>

<span data-ttu-id="60fd5-131">Domyślnie serwer Kestrel nakłada [szybkość danych treści żądania minimalne](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="60fd5-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="60fd5-132">Dla klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks stawki nie może zostać wykonane i może zostać przekroczony limit czasu połączenia. Minimalna żądania ograniczania liczby wywołań danych musi być wyłączona, gdy usługa gRPC obejmuje klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks treści:</span><span class="sxs-lookup"><span data-stu-id="60fd5-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="60fd5-133">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60fd5-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
