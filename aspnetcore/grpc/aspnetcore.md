---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług z platformą ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 387c3134efc04bec740fc66a5ca4b84715264d35
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209023"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="ad23e-103">Usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad23e-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="ad23e-104">W tym dokumencie pokazano, jak rozpocząć pracę z usługami gRPC przy użyciu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad23e-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="ad23e-105">Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad23e-105">Get started with gRPC service in ASP.NET Core</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ad23e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad23e-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ad23e-107">Zobacz [Rozpocznij pracę z usługami gRPC](xref:tutorials/grpc/grpc-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu gRPC.</span><span class="sxs-lookup"><span data-stu-id="ad23e-107">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ad23e-108">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ad23e-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="ad23e-109">Uruchom `dotnet new grpc -o GrpcGreeter` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ad23e-109">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="ad23e-110">Dodawanie usług gRPC do aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad23e-110">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="ad23e-111">gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="ad23e-111">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="ad23e-112">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="ad23e-112">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="ad23e-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) dla formatu protobuf komunikatu interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ad23e-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="ad23e-114">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="ad23e-114">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="ad23e-115">Konfigurowanie gRPC</span><span class="sxs-lookup"><span data-stu-id="ad23e-115">Configure gRPC</span></span>

<span data-ttu-id="ad23e-116">włączono gRPC `AddGrpc` metody:</span><span class="sxs-lookup"><span data-stu-id="ad23e-116">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="ad23e-117">Każda usługa gRPC jest dodawany do potoku routingu za pomocą `MapGrpcService` metody:</span><span class="sxs-lookup"><span data-stu-id="ad23e-117">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Startup.cs?name=snippet&highlight=16-19)]

<span data-ttu-id="ad23e-118">Funkcje i platformy ASP.NET Core middlewares Udostępnianie routingu potoku, w związku z tym można skonfigurować aplikację do obsługi dodatkowych żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="ad23e-118">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="ad23e-119">Procedury obsługi dodatkowych żądań, takich jak kontrolerów MVC równoległe Pracowanie z usług gRPC skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="ad23e-119">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="ad23e-120">Integracja za pomocą interfejsów API platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad23e-120">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="ad23e-121">gRPC usługi mają pełny dostęp do funkcji programu ASP.NET Core, takich jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) i [rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="ad23e-121">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="ad23e-122">Na przykład implementacji usługi można usunąć usługi rejestratora z kontenera DI za pośrednictwem konstruktora:</span><span class="sxs-lookup"><span data-stu-id="ad23e-122">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="ad23e-123">Domyślnie implementacji usługi gRPC można rozwiązać, innych usług DI przy użyciu dowolnego okresu istnienia (pojedyncze, polu lub niepowodzenie tymczasowe).</span><span class="sxs-lookup"><span data-stu-id="ad23e-123">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="ad23e-124">Rozwiąż HttpContext w metodach gRPC</span><span class="sxs-lookup"><span data-stu-id="ad23e-124">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="ad23e-125">GRPC interfejs API zapewnia dostęp do niektórych danych komunikatu protokołu HTTP/2, takie jak metody, host, nagłówek i przyczepy.</span><span class="sxs-lookup"><span data-stu-id="ad23e-125">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="ad23e-126">Dostęp jest za pośrednictwem `ServerCallContext` argument przekazany do każdej metody gRPC:</span><span class="sxs-lookup"><span data-stu-id="ad23e-126">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="ad23e-127">`ServerCallContext` nie zapewnia pełny dostęp do `HttpContext` w wszystkich interfejsów API platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad23e-127">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="ad23e-128">`GetHttpContext` — Metoda rozszerzenia zapewnia pełny dostęp do `HttpContext` reprezentujący komunikat o podstawowym protokołu HTTP/2 w interfejsach API platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="ad23e-128">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet1)]

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="ad23e-129">Limit szybkości danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="ad23e-129">Request body data rate limit</span></span>

<span data-ttu-id="ad23e-130">Domyślnie serwer Kestrel nakłada [szybkość danych treści żądania minimalne](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="ad23e-130">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="ad23e-131">Dla klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks stawki nie może zostać wykonane i może zostać przekroczony limit czasu połączenia. Minimalna żądania ograniczania liczby wywołań danych musi być wyłączona, gdy usługa gRPC obejmuje klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks treści:</span><span class="sxs-lookup"><span data-stu-id="ad23e-131">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ad23e-132">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ad23e-132">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
