---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 28e6b8589bbe0b6a3723b64736c723c883302571
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238164"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="13b46-103">Usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b46-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="13b46-104">W tym dokumencie przedstawiono sposób rozpoczynania pracy z usługami gRPC Services przy użyciu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13b46-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13b46-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="13b46-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="13b46-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13b46-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="13b46-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13b46-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="13b46-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13b46-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="13b46-109">Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b46-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="13b46-110">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="13b46-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="13b46-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13b46-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="13b46-112">Szczegółowe instrukcje dotyczące tworzenia projektu gRPC można znaleźć w temacie Wprowadzenie do [usług gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="13b46-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="13b46-113">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="13b46-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="13b46-114">Uruchom `dotnet new grpc -o GrpcGreeter` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="13b46-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="13b46-115">Dodawanie usług gRPC do aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b46-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="13b46-116">gRPC wymaga pakietu [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="13b46-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="13b46-117">Konfigurowanie gRPC</span><span class="sxs-lookup"><span data-stu-id="13b46-117">Configure gRPC</span></span>

<span data-ttu-id="13b46-118">W *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="13b46-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="13b46-119">gRPC jest włączona z `AddGrpc` metodą.</span><span class="sxs-lookup"><span data-stu-id="13b46-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="13b46-120">Każda usługa gRPC jest dodawana do potoku routingu za `MapGrpcService` pomocą metody.</span><span class="sxs-lookup"><span data-stu-id="13b46-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="13b46-121">ASP.NET Core middlewares i funkcje korzystają z potoku routingu, w związku z czym aplikacja może być skonfigurowana w taki sposób, aby obsługiwała dodatkowe procedury obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="13b46-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="13b46-122">Dodatkowe programy obsługi żądań, takie jak kontrolery MVC, pracują równolegle ze skonfigurowanymi usługami gRPC.</span><span class="sxs-lookup"><span data-stu-id="13b46-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="13b46-123">Konfigurowanie Kestrel</span><span class="sxs-lookup"><span data-stu-id="13b46-123">Configure Kestrel</span></span>

<span data-ttu-id="13b46-124">Punkty końcowe Kestrel gRPC:</span><span class="sxs-lookup"><span data-stu-id="13b46-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="13b46-125">Wymagaj protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="13b46-125">Require HTTP/2.</span></span>
* <span data-ttu-id="13b46-126">Powinien być zabezpieczony przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="13b46-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="13b46-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="13b46-127">HTTP/2</span></span>

<span data-ttu-id="13b46-128">gRPC wymaga protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="13b46-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="13b46-129">gRPC dla ASP.NET Core sprawdza poprawność elementu [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="13b46-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="13b46-130">Kestrel [obsługuje protokół HTTP/2](xref:fundamentals/servers/kestrel#http2-support) w większości nowoczesnych systemów operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="13b46-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="13b46-131">Punkty końcowe Kestrel są domyślnie skonfigurowane do obsługi połączeń HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="13b46-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="https"></a><span data-ttu-id="13b46-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="13b46-132">HTTPS</span></span>

<span data-ttu-id="13b46-133">Punkty końcowe Kestrel używane dla gRPC powinny być zabezpieczone przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="13b46-133">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="13b46-134">W `https://localhost:5001` trakcie opracowywania, punkt końcowy https jest tworzony automatycznie, gdy jest obecny ASP.NET Core certyfikat deweloperski.</span><span class="sxs-lookup"><span data-stu-id="13b46-134">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="13b46-135">Nie jest wymagana żadna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="13b46-135">No configuration is required.</span></span>

<span data-ttu-id="13b46-136">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="13b46-136">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="13b46-137">W poniższym przykładzie pliku *appSettings. JSON* jest dostępny punkt końcowy HTTP/2 zabezpieczony przy użyciu protokołu https:</span><span class="sxs-lookup"><span data-stu-id="13b46-137">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="13b46-138">Alternatywnie można skonfigurować punkty końcowe Kestrel w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13b46-138">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="13b46-139">Gdy punkt końcowy protokołu HTTP/2 jest skonfigurowany bez protokołu HTTPS, [ListenOptions. protokoły](xref:fundamentals/servers/kestrel#listenoptionsprotocols) punktu końcowego muszą mieć ustawioną wartość `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="13b46-139">When an HTTP/2 endpoint is configured without HTTPS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="13b46-140">`HttpProtocols.Http1AndHttp2`nie można użyć, ponieważ HTTPS jest wymagany do negocjowania protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="13b46-140">`HttpProtocols.Http1AndHttp2` can't be used because HTTPS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="13b46-141">Bez protokołu HTTPS wszystkie połączenia z punktem końcowym domyślne do protokołu HTTP/1.1 i wywołania gRPC kończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="13b46-141">Without HTTPS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="13b46-142">Aby uzyskać więcej informacji na temat włączania protokołów HTTP/2 i HTTPS za pomocą Kestrel, zobacz [Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="13b46-142">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="13b46-143">macOS nie obsługuje ASP.NET Core gRPC z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="13b46-143">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="13b46-144">Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="13b46-144">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="13b46-145">Aby uzyskać więcej informacji, zobacz [nie można uruchomić aplikacji ASP.NET Core gRPC w witrynie macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="13b46-145">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="13b46-146">Integracja z interfejsami API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b46-146">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="13b46-147">usługi gRPC mają pełny dostęp do funkcji ASP.NET Core, takich jak [iniekcja zależności](xref:fundamentals/dependency-injection) (di) i [Rejestrowanie](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="13b46-147">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="13b46-148">Na przykład implementacja usługi może rozpoznać usługę rejestratora z kontenera DI za pośrednictwem konstruktora:</span><span class="sxs-lookup"><span data-stu-id="13b46-148">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="13b46-149">Domyślnie implementacja usługi gRPC może rozwiązywać inne usługi DI z dowolnym okresem istnienia (pojedynczym, zakresowym lub przejściowym).</span><span class="sxs-lookup"><span data-stu-id="13b46-149">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="13b46-150">Rozwiąż element HttpContext w metodach gRPC</span><span class="sxs-lookup"><span data-stu-id="13b46-150">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="13b46-151">Interfejs API gRPC zapewnia dostęp do niektórych danych komunikatów HTTP/2, takich jak metoda, host, nagłówek i przyczepy.</span><span class="sxs-lookup"><span data-stu-id="13b46-151">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="13b46-152">Dostęp odbywa się za `ServerCallContext` pomocą argumentu przesłanego do każdej metody gRPC:</span><span class="sxs-lookup"><span data-stu-id="13b46-152">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="13b46-153">`ServerCallContext`nie zapewnia pełnego dostępu do `HttpContext` wszystkich interfejsów API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13b46-153">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="13b46-154">Metoda rozszerzająca zapewnia pełny dostęp `HttpContext` do reprezentowania podstawowego komunikatu http/2 w interfejsach API ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="13b46-154">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="13b46-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="13b46-155">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
