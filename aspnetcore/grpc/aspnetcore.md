---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług z platformą ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 5937ca9f2a783c4dabe324dae828b97953782938
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555858"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="b4ef7-103">Usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4ef7-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="b4ef7-104">W tym dokumencie pokazano, jak rozpocząć pracę z usługami gRPC przy użyciu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4ef7-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b4ef7-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4ef7-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4ef7-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b4ef7-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b4ef7-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b4ef7-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b4ef7-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="b4ef7-109">Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4ef7-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="b4ef7-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b4ef7-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4ef7-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4ef7-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b4ef7-112">Zobacz [Rozpocznij pracę z usługami gRPC](xref:tutorials/grpc/grpc-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu gRPC.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b4ef7-113">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b4ef7-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b4ef7-114">Uruchom `dotnet new grpc -o GrpcGreeter` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="b4ef7-115">Dodawanie usług gRPC do aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4ef7-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="b4ef7-116">gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-116">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="b4ef7-117">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="b4ef7-117">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="b4ef7-118">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) dla formatu protobuf komunikatu interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-118">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="b4ef7-119">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="b4ef7-119">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="b4ef7-120">Konfigurowanie gRPC</span><span class="sxs-lookup"><span data-stu-id="b4ef7-120">Configure gRPC</span></span>

<span data-ttu-id="b4ef7-121">włączono gRPC `AddGrpc` metody:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-121">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="b4ef7-122">Każda usługa gRPC jest dodawany do potoku routingu za pomocą `MapGrpcService` metody:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-122">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="b4ef7-123">Funkcje i platformy ASP.NET Core middlewares Udostępnianie routingu potoku, w związku z tym można skonfigurować aplikację do obsługi dodatkowych żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-123">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="b4ef7-124">Procedury obsługi dodatkowych żądań, takich jak kontrolerów MVC równoległe Pracowanie z usług gRPC skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-124">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="b4ef7-125">Integracja za pomocą interfejsów API platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4ef7-125">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="b4ef7-126">gRPC usługi mają pełny dostęp do funkcji programu ASP.NET Core, takich jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) i [rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="b4ef7-126">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="b4ef7-127">Na przykład implementacji usługi można usunąć usługi rejestratora z kontenera DI za pośrednictwem konstruktora:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-127">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="b4ef7-128">Domyślnie implementacji usługi gRPC można rozwiązać, innych usług DI przy użyciu dowolnego okresu istnienia (pojedyncze, polu lub niepowodzenie tymczasowe).</span><span class="sxs-lookup"><span data-stu-id="b4ef7-128">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="b4ef7-129">Rozwiąż HttpContext w metodach gRPC</span><span class="sxs-lookup"><span data-stu-id="b4ef7-129">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="b4ef7-130">GRPC interfejs API zapewnia dostęp do niektórych danych komunikatu protokołu HTTP/2, takie jak metody, host, nagłówek i przyczepy.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-130">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="b4ef7-131">Dostęp jest za pośrednictwem `ServerCallContext` argument przekazany do każdej metody gRPC:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-131">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="b4ef7-132">`ServerCallContext` nie zapewnia pełny dostęp do `HttpContext` w wszystkich interfejsów API platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4ef7-132">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="b4ef7-133">`GetHttpContext` — Metoda rozszerzenia zapewnia pełny dostęp do `HttpContext` reprezentujący komunikat o podstawowym protokołu HTTP/2 w interfejsach API platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b4ef7-133">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="b4ef7-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b4ef7-134">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
