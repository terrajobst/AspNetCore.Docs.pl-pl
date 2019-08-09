---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862941"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="94017-103">Usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94017-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="94017-104">W tym dokumencie przedstawiono sposób rozpoczynania pracy z usługami gRPC Services przy użyciu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="94017-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94017-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="94017-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="94017-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94017-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="94017-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="94017-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="94017-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="94017-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="94017-109">Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94017-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="94017-110">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="94017-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="94017-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94017-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="94017-112">Szczegółowe instrukcje dotyczące tworzenia projektu gRPC można znaleźć w temacie Wprowadzenie do [usług gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="94017-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="94017-113">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="94017-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="94017-114">Uruchom `dotnet new grpc -o GrpcGreeter` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="94017-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="94017-115">Dodawanie usług gRPC do aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94017-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="94017-116">gRPC wymaga pakietu [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="94017-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="94017-117">Konfigurowanie gRPC</span><span class="sxs-lookup"><span data-stu-id="94017-117">Configure gRPC</span></span>

<span data-ttu-id="94017-118">gRPC jest włączona za pomocą `AddGrpc` metody:</span><span class="sxs-lookup"><span data-stu-id="94017-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="94017-119">Każda usługa gRPC jest dodawana do potoku routingu za `MapGrpcService` pomocą metody:</span><span class="sxs-lookup"><span data-stu-id="94017-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="94017-120">ASP.NET Core middlewares i funkcje korzystają z potoku routingu, w związku z czym aplikacja może być skonfigurowana w taki sposób, aby obsługiwała dodatkowe procedury obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="94017-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="94017-121">Dodatkowe programy obsługi żądań, takie jak kontrolery MVC, pracują równolegle ze skonfigurowanymi usługami gRPC.</span><span class="sxs-lookup"><span data-stu-id="94017-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="94017-122">Integracja z interfejsami API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94017-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="94017-123">usługi gRPC mają pełny dostęp do funkcji ASP.NET Core, takich jak [iniekcja zależności](xref:fundamentals/dependency-injection) (di) i [Rejestrowanie](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="94017-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="94017-124">Na przykład implementacja usługi może rozpoznać usługę rejestratora z kontenera DI za pośrednictwem konstruktora:</span><span class="sxs-lookup"><span data-stu-id="94017-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="94017-125">Domyślnie implementacja usługi gRPC może rozwiązywać inne usługi DI z dowolnym okresem istnienia (pojedynczym, zakresowym lub przejściowym).</span><span class="sxs-lookup"><span data-stu-id="94017-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="94017-126">Rozwiąż element HttpContext w metodach gRPC</span><span class="sxs-lookup"><span data-stu-id="94017-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="94017-127">Interfejs API gRPC zapewnia dostęp do niektórych danych komunikatów HTTP/2, takich jak metoda, host, nagłówek i przyczepy.</span><span class="sxs-lookup"><span data-stu-id="94017-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="94017-128">Dostęp odbywa się za `ServerCallContext` pomocą argumentu przesłanego do każdej metody gRPC:</span><span class="sxs-lookup"><span data-stu-id="94017-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="94017-129">`ServerCallContext`nie zapewnia pełnego dostępu do `HttpContext` wszystkich interfejsów API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="94017-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="94017-130">Metoda rozszerzająca zapewnia pełny dostęp `HttpContext` do reprezentowania podstawowego komunikatu http/2 w interfejsach API ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="94017-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="94017-131">gRPC i ASP.NET Core w macOS</span><span class="sxs-lookup"><span data-stu-id="94017-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="94017-132">Kestrel nie obsługuje protokołu HTTP/2 z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) w macOS.</span><span class="sxs-lookup"><span data-stu-id="94017-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="94017-133">ASP.NET Core szablon i przykłady gRPC domyślnie korzystają z protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="94017-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="94017-134">Podczas próby uruchomienia serwera gRPC zobaczysz następujący komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="94017-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="94017-135">Nie można utworzyć powiązania https://localhost:5001 z interfejsem sprzężenia zwrotnego IPv4: Protokół HTTP/2 za pośrednictwem protokołu TLS nie jest obsługiwany w programie macOS z powodu braku obsługi CLIENTHELLO ALPN.</span><span class="sxs-lookup"><span data-stu-id="94017-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="94017-136">Aby obejść ten problem, należy skonfigurować Kestrel i klienta gRPC do używania protokołu HTTP/2 **bez** szyfrowania TLS.</span><span class="sxs-lookup"><span data-stu-id="94017-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="94017-137">Należy to zrobić tylko podczas projektowania.</span><span class="sxs-lookup"><span data-stu-id="94017-137">You should only do this during development.</span></span> <span data-ttu-id="94017-138">Użycie protokołu TLS spowoduje, że komunikaty gRPC są wysyłane bez szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="94017-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="94017-139">Kestrel musi skonfigurować punkt końcowy HTTP/2 bez protokołu TLS `Program.cs`w:</span><span class="sxs-lookup"><span data-stu-id="94017-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="94017-140">Klient gRPC musi ustawić `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` przełącznik do `true` i używać `http` go w adresie serwera:</span><span class="sxs-lookup"><span data-stu-id="94017-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="94017-141">Protokołu HTTP/2 bez protokołu TLS należy używać tylko podczas opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="94017-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="94017-142">Aplikacje produkcyjne powinny zawsze korzystać z zabezpieczeń transportu.</span><span class="sxs-lookup"><span data-stu-id="94017-142">Production applications should always use transport security.</span></span> <span data-ttu-id="94017-143">Aby uzyskać więcej informacji, zobacz [zagadnienia dotyczące zabezpieczeń w programie gRPC ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="94017-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94017-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="94017-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
