---
title: Rejestrowanie i Diagnostyka w programie gRPC na platformie .NET
author: jamesnk
description: Dowiedz się, jak zbierać diagnostykę z aplikacji gRPC na platformie .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204346"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="cc3a2-103">Rejestrowanie i Diagnostyka w programie gRPC na platformie .NET</span><span class="sxs-lookup"><span data-stu-id="cc3a2-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="cc3a2-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="cc3a2-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="cc3a2-105">Ten artykuł zawiera wskazówki dotyczące zbierania danych diagnostycznych z aplikacji gRPC w celu ułatwienia rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="cc3a2-106">Rejestrowanie usług gRPC Services</span><span class="sxs-lookup"><span data-stu-id="cc3a2-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="cc3a2-107">Dzienniki po stronie serwera mogą zawierać poufne informacje z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="cc3a2-108">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="cc3a2-109">Ponieważ usługi gRPC Services są hostowane na ASP.NET Core, korzysta z systemu rejestrowania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="cc3a2-110">W konfiguracji domyślnej gRPC rejestruje bardzo mało informacji, ale można je skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="cc3a2-111">Szczegółowe informacje na temat konfigurowania rejestrowania ASP.NET Core można znaleźć w dokumentacji dotyczącej [rejestrowania ASP.NET Core](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="cc3a2-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="cc3a2-112">gRPC dodaje dzienniki w `Grpc` kategorii.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="cc3a2-113">Aby włączyć `Grpc` szczegółowe dzienniki z gRPC, skonfiguruj prefiksy `Debug` do poziomu w pliku *appSettings. JSON* , dodając następujące elementy do `LogLevel` podsekcji w: `Logging`</span><span class="sxs-lookup"><span data-stu-id="cc3a2-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7)]

<span data-ttu-id="cc3a2-114">Można to również skonfigurować w *Startup.cs* z `ConfigureLogging`:</span><span class="sxs-lookup"><span data-stu-id="cc3a2-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

<span data-ttu-id="cc3a2-115">Jeśli nie korzystasz z konfiguracji opartej na notacji JSON, ustaw w systemie konfiguracji następującą wartość konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="cc3a2-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="cc3a2-116">Zapoznaj się z dokumentacją systemu konfiguracyjnego, aby określić sposób określania zagnieżdżonych wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="cc3a2-117">Na przykład w przypadku używania zmiennych środowiskowych zamiast `_` `:` (na przykład `Logging__LogLevel__Grpc`) są używane dwa znaki.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="cc3a2-118">Zalecamy użycie `Debug` poziomu podczas zbierania bardziej szczegółowych informacji diagnostycznych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="cc3a2-119">Na `Trace` poziomie powstaje Diagnostyka bardzo niskiego poziomu i jest rzadko wymagana do diagnozowania problemów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="cc3a2-120">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="cc3a2-120">Sample logging output</span></span>

<span data-ttu-id="cc3a2-121">Oto przykład danych wyjściowych konsoli na `Debug` poziomie usługi gRPC:</span><span class="sxs-lookup"><span data-stu-id="cc3a2-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a><span data-ttu-id="cc3a2-122">Dostęp do dzienników po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="cc3a2-122">Access server-side logs</span></span>

<span data-ttu-id="cc3a2-123">Sposób dostępu do dzienników po stronie serwera zależy od środowiska, w którym jest uruchomiony program.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="cc3a2-124">Jako Aplikacja konsolowa</span><span class="sxs-lookup"><span data-stu-id="cc3a2-124">As a console app</span></span>

<span data-ttu-id="cc3a2-125">Jeśli używasz programu w aplikacji konsolowej, [Rejestrator konsoli](xref:fundamentals/logging/index#console-provider) powinien być domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="cc3a2-126">Dzienniki gRPC będą wyświetlane w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="cc3a2-127">Inne środowiska</span><span class="sxs-lookup"><span data-stu-id="cc3a2-127">Other environments</span></span>

<span data-ttu-id="cc3a2-128">Jeśli aplikacja jest wdrażana w innym środowisku (na przykład Docker, Kubernetes lub Windows Service), zobacz <xref:fundamentals/logging/index> , aby uzyskać więcej informacji na temat konfigurowania dostawców rejestrowania odpowiednie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="cc3a2-129">Rejestrowanie klienta gRPC</span><span class="sxs-lookup"><span data-stu-id="cc3a2-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="cc3a2-130">Dzienniki po stronie klienta mogą zawierać poufne informacje z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="cc3a2-131">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="cc3a2-132">Aby pobrać dzienniki z klienta .NET, można ustawić `GrpcChannelOptions.LoggerFactory` właściwość podczas tworzenia kanału klienta.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="cc3a2-133">Jeśli wywołujesz usługę gRPC z aplikacji ASP.NET Core, fabryka rejestratorów może zostać rozpoznana z iniekcji zależności (DI):</span><span class="sxs-lookup"><span data-stu-id="cc3a2-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="cc3a2-134">Alternatywnym sposobem włączenia rejestrowania klienta jest użycie [fabryki klienta gRPC](xref:grpc/clientfactory) do utworzenia klienta programu.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="cc3a2-135">Klient gRPC zarejestrowany w fabryce klienta i rozwiązany przez program DI automatycznie użyje skonfigurowanego rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="cc3a2-136">Jeśli aplikacja nie używa funkcji di, można utworzyć nowe `ILoggerFactory` wystąpienie za pomocą [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="cc3a2-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="cc3a2-137">Aby uzyskać dostęp do tej metody, Dodaj pakiet [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc3a2-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="cc3a2-138">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="cc3a2-138">Sample logging output</span></span>

<span data-ttu-id="cc3a2-139">Oto przykład danych wyjściowych konsoli na `Debug` poziomie klienta gRPC:</span><span class="sxs-lookup"><span data-stu-id="cc3a2-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="cc3a2-140">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc3a2-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
