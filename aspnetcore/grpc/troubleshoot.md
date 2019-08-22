---
title: Rozwiązywanie problemów z gRPC na platformie .NET Core
author: jamesnk
description: Rozwiązywanie problemów dotyczących błędów podczas korzystania z gRPC na platformie .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/17/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 7621266dfe26b7126d1607e195dd5dcaab4efa55
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886490"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="7fef1-103">Rozwiązywanie problemów z gRPC na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="7fef1-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="7fef1-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="7fef1-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="7fef1-105">Niezgodność między konfiguracją protokołu SSL klienta i usługi TLS</span><span class="sxs-lookup"><span data-stu-id="7fef1-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="7fef1-106">Szablon gRPC i przykłady używają usługi [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) do domyślnego zabezpieczania usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="7fef1-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="7fef1-107">Klienci gRPC muszą używać bezpiecznego połączenia w celu pomyślnego wywołania zabezpieczonych usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="7fef1-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="7fef1-108">Możesz sprawdzić, ASP.NET Core usługa gRPC korzysta z protokołu TLS w dziennikach, które zostały zapisane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7fef1-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="7fef1-109">Usługa nasłuchuje na punkcie końcowym HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7fef1-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="7fef1-110">Aby umożliwić nawiązywanie połączeń z `https` bezpiecznym połączeniem, klient .NET Core musi użyć adresu serwera:</span><span class="sxs-lookup"><span data-stu-id="7fef1-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="7fef1-111">Wszystkie implementacje klienta gRPC obsługują protokół TLS.</span><span class="sxs-lookup"><span data-stu-id="7fef1-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="7fef1-112">Klienci gRPC z innych języków zwykle wymagają kanału skonfigurowanego z `SslCredentials`.</span><span class="sxs-lookup"><span data-stu-id="7fef1-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="7fef1-113">`SslCredentials`Określa certyfikat, który będzie używany przez klienta i musi być używany zamiast niezabezpieczonych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="7fef1-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="7fef1-114">Przykłady konfigurowania różnych implementacji klienta gRPC do korzystania z protokołu TLS można znaleźć w temacie [GRPC Authentication (uwierzytelnianie](https://www.grpc.io/docs/guides/auth/)).</span><span class="sxs-lookup"><span data-stu-id="7fef1-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="7fef1-115">Wywoływanie niezabezpieczonych usług gRPC z klientem .NET Core</span><span class="sxs-lookup"><span data-stu-id="7fef1-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="7fef1-116">Dodatkowa konfiguracja jest wymagana do wywołania niezabezpieczonych usług gRPC za pomocą klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fef1-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="7fef1-117">Klient gRPC musi ustawić `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` przełącznik do `true` i używać `http` go w adresie serwera:</span><span class="sxs-lookup"><span data-stu-id="7fef1-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="7fef1-118">Nie można uruchomić aplikacji ASP.NET Core gRPC na macOS</span><span class="sxs-lookup"><span data-stu-id="7fef1-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="7fef1-119">Kestrel nie obsługuje protokołu HTTP/2 z protokołem TLS w macOS i starszych wersjach systemu Windows, takich jak Windows 7.</span><span class="sxs-lookup"><span data-stu-id="7fef1-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="7fef1-120">ASP.NET Core szablon i przykłady gRPC domyślnie korzystają z protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="7fef1-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="7fef1-121">Podczas próby uruchomienia serwera gRPC zobaczysz następujący komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="7fef1-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="7fef1-122">Nie można utworzyć powiązania https://localhost:5001 z interfejsem sprzężenia zwrotnego IPv4: Protokół HTTP/2 za pośrednictwem protokołu TLS nie jest obsługiwany w programie macOS z powodu braku obsługi CLIENTHELLO ALPN.</span><span class="sxs-lookup"><span data-stu-id="7fef1-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="7fef1-123">Aby obejść ten problem, należy skonfigurować Kestrel i klienta gRPC do używania protokołu HTTP/2 *bez* szyfrowania TLS.</span><span class="sxs-lookup"><span data-stu-id="7fef1-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="7fef1-124">Należy to zrobić tylko podczas projektowania.</span><span class="sxs-lookup"><span data-stu-id="7fef1-124">You should only do this during development.</span></span> <span data-ttu-id="7fef1-125">Użycie protokołu TLS spowoduje, że komunikaty gRPC są wysyłane bez szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="7fef1-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="7fef1-126">Kestrel musi skonfigurować punkt końcowy HTTP/2 bez protokołu TLS w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7fef1-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="7fef1-127">Gdy punkt końcowy protokołu HTTP/2 jest skonfigurowany bez protokołu TLS, [ListenOptions. protokoły](xref:fundamentals/servers/kestrel#listenoptionsprotocols) punktu końcowego muszą mieć ustawioną wartość `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="7fef1-127">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="7fef1-128">`HttpProtocols.Http1AndHttp2`nie można użyć, ponieważ protokół TLS jest wymagany do negocjowania protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="7fef1-128">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="7fef1-129">Bez protokołu TLS wszystkie połączenia z punktem końcowym domyślne do protokołu HTTP/1.1 i wywołania gRPC kończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="7fef1-129">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="7fef1-130">Klienta gRPC należy również skonfigurować tak, aby nie korzystał z protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="7fef1-130">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="7fef1-131">Aby uzyskać więcej informacji, zobacz Wywoływanie niezabezpieczonych [usług gRPC przy użyciu programu .NET Core Client](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="7fef1-131">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="7fef1-132">Protokołu HTTP/2 bez protokołu TLS należy używać tylko podczas opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7fef1-132">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="7fef1-133">Aplikacje produkcyjne powinny zawsze korzystać z zabezpieczeń transportu.</span><span class="sxs-lookup"><span data-stu-id="7fef1-133">Production apps should always use transport security.</span></span> <span data-ttu-id="7fef1-134">Aby uzyskać więcej informacji, zobacz [zagadnienia dotyczące zabezpieczeń w programie gRPC ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="7fef1-134">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="7fef1-135">elementy C# zawartości gRPC nie są generowane w kodzie z  *\*plików. proto*</span><span class="sxs-lookup"><span data-stu-id="7fef1-135">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="7fef1-136">gRPC generowanie kodu dla konkretnych klientów i klas podstawowych usług wymaga przywoływania plików protobuf i narzędzi z projektu.</span><span class="sxs-lookup"><span data-stu-id="7fef1-136">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="7fef1-137">Należy uwzględnić:</span><span class="sxs-lookup"><span data-stu-id="7fef1-137">You must include:</span></span>

* <span data-ttu-id="7fef1-138">pliki *. proto* , które mają być używane w `<Protobuf>` grupie elementów.</span><span class="sxs-lookup"><span data-stu-id="7fef1-138">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="7fef1-139">[Zaimportowane pliki *proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) muszą odwoływać się do projektu.</span><span class="sxs-lookup"><span data-stu-id="7fef1-139">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="7fef1-140">Odwołanie do pakietu dla pakietu narzędzi gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="7fef1-140">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="7fef1-141">Więcej informacji o generowaniu zasobów gRPC C# można znaleźć w <xref:grpc/basics>temacie.</span><span class="sxs-lookup"><span data-stu-id="7fef1-141">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="7fef1-142">Domyślnie `<Protobuf>` odwołanie generuje konkretny klient i klasę bazową usługi.</span><span class="sxs-lookup"><span data-stu-id="7fef1-142">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="7fef1-143">`GrpcServices` Atrybut elementu odwołania może służyć do ograniczania C# generacji zasobów.</span><span class="sxs-lookup"><span data-stu-id="7fef1-143">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="7fef1-144">Prawidłowe `GrpcServices` opcje to:</span><span class="sxs-lookup"><span data-stu-id="7fef1-144">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="7fef1-145">`Both`(domyślnie, gdy nie istnieje)</span><span class="sxs-lookup"><span data-stu-id="7fef1-145">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="7fef1-146">ASP.NET Core usługi gRPC Web App hosting Services wymagają tylko wygenerowania klasy bazowej usługi:</span><span class="sxs-lookup"><span data-stu-id="7fef1-146">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="7fef1-147">Aplikacja kliencka gRPC wykonująca wywołania gRPC potrzebuje tylko konkretnego wygenerowanego klienta:</span><span class="sxs-lookup"><span data-stu-id="7fef1-147">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
