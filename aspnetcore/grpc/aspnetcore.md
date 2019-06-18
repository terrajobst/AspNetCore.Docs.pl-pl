---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług z platformą ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: ca06478e6168c59d9abf43d99213fa8a7091e178
ms.sourcegitcommit: 756114cab5e24e99ec23de62b0c1c16b15197ac2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67169522"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="1a8ba-103">Usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a8ba-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="1a8ba-104">W tym dokumencie pokazano, jak rozpocząć pracę z usługami gRPC przy użyciu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="1a8ba-105">Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a8ba-105">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="1a8ba-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1a8ba-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a8ba-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a8ba-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a8ba-108">Zobacz [Rozpocznij pracę z usługami gRPC](xref:tutorials/grpc/grpc-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-108">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1a8ba-109">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1a8ba-109">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="1a8ba-110">Uruchom `dotnet new grpc -o GrpcGreeter` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-110">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="1a8ba-111">Dodawanie usług gRPC do aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a8ba-111">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="1a8ba-112">gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-112">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="1a8ba-113">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="1a8ba-113">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="1a8ba-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) dla formatu protobuf komunikatu interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="1a8ba-115">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="1a8ba-115">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="1a8ba-116">Konfigurowanie gRPC</span><span class="sxs-lookup"><span data-stu-id="1a8ba-116">Configure gRPC</span></span>

<span data-ttu-id="1a8ba-117">włączono gRPC `AddGrpc` metody:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-117">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="1a8ba-118">Każda usługa gRPC jest dodawany do potoku routingu za pomocą `MapGrpcService` metody:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-118">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="1a8ba-119">Funkcje i platformy ASP.NET Core middlewares Udostępnianie routingu potoku, w związku z tym można skonfigurować aplikację do obsługi dodatkowych żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-119">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="1a8ba-120">Procedury obsługi dodatkowych żądań, takich jak kontrolerów MVC równoległe Pracowanie z usług gRPC skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-120">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="1a8ba-121">Integracja za pomocą interfejsów API platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a8ba-121">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="1a8ba-122">gRPC usługi mają pełny dostęp do funkcji programu ASP.NET Core, takich jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) i [rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1a8ba-122">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="1a8ba-123">Na przykład implementacji usługi można usunąć usługi rejestratora z kontenera DI za pośrednictwem konstruktora:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-123">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="1a8ba-124">Domyślnie implementacji usługi gRPC można rozwiązać, innych usług DI przy użyciu dowolnego okresu istnienia (pojedyncze, polu lub niepowodzenie tymczasowe).</span><span class="sxs-lookup"><span data-stu-id="1a8ba-124">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="1a8ba-125">Rozwiąż HttpContext w metodach gRPC</span><span class="sxs-lookup"><span data-stu-id="1a8ba-125">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="1a8ba-126">GRPC interfejs API zapewnia dostęp do niektórych danych komunikatu protokołu HTTP/2, takie jak metody, host, nagłówek i przyczepy.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-126">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="1a8ba-127">Dostęp jest za pośrednictwem `ServerCallContext` argument przekazany do każdej metody gRPC:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-127">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="1a8ba-128">`ServerCallContext` nie zapewnia pełny dostęp do `HttpContext` w wszystkich interfejsów API platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1a8ba-128">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="1a8ba-129">`GetHttpContext` — Metoda rozszerzenia zapewnia pełny dostęp do `HttpContext` reprezentujący komunikat o podstawowym protokołu HTTP/2 w interfejsach API platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="1a8ba-129">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="1a8ba-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1a8ba-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
