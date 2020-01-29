---
title: gRPC w aplikacjach przeglądarki
author: jamesnk
description: Dowiedz się, jak skonfigurować usługi gRPC na ASP.NET Core, aby możliwe było wywoływanie z aplikacji przeglądarki za pomocą gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830661"
---
# <a name="grpc-in-browser-apps"></a><span data-ttu-id="ab3bd-103">gRPC w aplikacjach przeglądarki</span><span class="sxs-lookup"><span data-stu-id="ab3bd-103">gRPC in browser apps</span></span>

<span data-ttu-id="ab3bd-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="ab3bd-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab3bd-105">**gRPC — obsługa sieci Web w programie .NET jest eksperymentalna**</span><span class="sxs-lookup"><span data-stu-id="ab3bd-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="ab3bd-106">gRPC-Web for .NET to eksperymentalny projekt, a nie produkt zatwierdzony.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="ab3bd-107">Chcemy:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-107">We want to:</span></span>
>
> * <span data-ttu-id="ab3bd-108">Przetestuj, że nasze podejście do implementacji gRPC-Web działa.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="ab3bd-109">Uzyskaj opinię na temat tego, czy takie podejście jest przydatne dla deweloperów platformy .NET w porównaniu do tradycyjnego sposobu konfigurowania gRPC-sieci Web za pośrednictwem serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="ab3bd-110">Wystaw opinię w [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) , aby upewnić się, że kompilujemy coś, co deweloperzy są podobne i wydajne.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="ab3bd-111">Nie można wywołać usługi gRPC protokołu HTTP/2 z poziomu aplikacji opartej na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="ab3bd-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) to protokół, który umożliwia aplikacjom JavaScript i Blazor w przeglądarce wywoływanie usług gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="ab3bd-113">W tym artykule wyjaśniono, jak używać gRPC-Web w programie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="ab3bd-114">Konfigurowanie gRPC-sieci Web w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab3bd-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="ab3bd-115">usługi gRPC hostowane w ASP.NET Core można skonfigurować do obsługi gRPC-Web, a nie gRPC protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="ab3bd-116">gRPC — sieć Web nie wymaga żadnych zmian w usługach.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="ab3bd-117">Jedyną modyfikacją jest konfiguracja uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="ab3bd-118">Aby włączyć usługę gRPC-Web za pomocą usługi gRPC ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="ab3bd-119">Dodaj odwołanie do pakietu [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="ab3bd-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="ab3bd-120">Skonfiguruj aplikację do używania gRPC-Web, dodając `AddGrpcWeb` i `UseGrpcWeb` do *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

<span data-ttu-id="ab3bd-121">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-121">The preceding code:</span></span>

* <span data-ttu-id="ab3bd-122">Dodaje oprogramowanie pośredniczące gRPC-Web, `UseGrpcWeb`po stronie routingu i przed punktami końcowymi.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="ab3bd-123">Określa, `endpoints.MapGrpcService<GreeterService>()` Metoda obsługuje gRPC-Web z `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="ab3bd-124">Alternatywnie można skonfigurować wszystkie usługi do obsługi gRPC-Web, dodając `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` do ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

<span data-ttu-id="ab3bd-125">Do wywołania gRPC-Web z przeglądarki może być wymagana dodatkowa konfiguracja, taka jak konfigurowanie ASP.NET Core do obsługi mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="ab3bd-126">Aby uzyskać więcej informacji, zobacz temat [Obsługa mechanizmu CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="ab3bd-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="ab3bd-127">Wywołaj gRPC-Web z przeglądarki</span><span class="sxs-lookup"><span data-stu-id="ab3bd-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="ab3bd-128">Aplikacje przeglądarki mogą używać gRPC-Web do wywoływania usług gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="ab3bd-129">Istnieją pewne wymagania i ograniczenia dotyczące wywoływania usług gRPC Services z usługą gRPC-Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="ab3bd-130">Serwer musi być skonfigurowany do obsługi gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="ab3bd-131">Wywołania przesyłania strumieniowego klientów i połączeń dwukierunkowych nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="ab3bd-132">Przesyłanie strumieniowe serwera jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-132">Server streaming is supported.</span></span>
* <span data-ttu-id="ab3bd-133">Wywoływanie usług gRPC w innej domenie wymaga skonfigurowania [CORS](xref:security/cors) na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="ab3bd-134">JavaScript gRPC — klient sieci Web</span><span class="sxs-lookup"><span data-stu-id="ab3bd-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="ab3bd-135">Istnieje skrypt JavaScript gRPC-Web Client.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="ab3bd-136">Aby uzyskać instrukcje dotyczące korzystania z usługi gRPC-Web w języku JavaScript, zobacz [pisanie kodu klienta JavaScript z gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="ab3bd-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="ab3bd-137">Konfigurowanie gRPC-sieci Web za pomocą klienta .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="ab3bd-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="ab3bd-138">Klienta .NET gRPC można skonfigurować w taki sposób, aby wykonywać wywołania gRPC-sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="ab3bd-139">Jest to przydatne w przypadku aplikacji [Blazor webassembly](xref:blazor/index#blazor-webassembly) , które są hostowane w przeglądarce i mają te same ograniczenia http związane z kodem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="ab3bd-140">Wywołanie gRPC-Web z klientem .NET jest [takie samo jak http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="ab3bd-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="ab3bd-141">Jedyną modyfikacją jest sposób tworzenia kanału.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="ab3bd-142">Aby użyć gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="ab3bd-143">Dodaj odwołanie do pakietu [GRPC .NET. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="ab3bd-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="ab3bd-144">Skonfiguruj kanał, aby używał `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-144">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="ab3bd-145">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-145">The preceding code:</span></span>

* <span data-ttu-id="ab3bd-146">Konfiguruje kanał do korzystania z gRPC-sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-146">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="ab3bd-147">Tworzy klienta i wywołuje połączenie przy użyciu kanału.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-147">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="ab3bd-148">Podczas tworzenia `GrpcWebHandler` są dostępne następujące opcje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="ab3bd-148">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="ab3bd-149">**InnerHandler**: podstawowy <xref:System.Net.Http.HttpMessageHandler>, który wykonuje wywołanie http, na przykład `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-149">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the HTTP call, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="ab3bd-150">**Tryb**: `GrpcWebMode` enum.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-150">**Mode**: `GrpcWebMode` enum.</span></span> <span data-ttu-id="ab3bd-151">`GrpcWebMode.GrpcWebText` konfiguruje zawartość do kodowania base64, która jest wymagana do obsługi wywołań przesyłania strumieniowego serwera.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-151">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded, which is required to support server streaming calls.</span></span>
* <span data-ttu-id="ab3bd-152">**HttpVersion**: protokół http `Version`.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-152">**HttpVersion**: HTTP protocol `Version`.</span></span> <span data-ttu-id="ab3bd-153">gRPC — sieć Web nie wymaga określonego protokołu i nie będzie określać go podczas wysyłania żądania, chyba że zostanie skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="ab3bd-153">gRPC-Web doesn't require a specific protocol and won't specify one when making a request unless configured.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab3bd-154">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ab3bd-154">Additional resources</span></span>

* [<span data-ttu-id="ab3bd-155">gRPC dla klientów sieci Web — projekt GitHub</span><span class="sxs-lookup"><span data-stu-id="ab3bd-155">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
