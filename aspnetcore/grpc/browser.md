---
title: Używanie gRPC w aplikacjach przeglądarki
author: jamesnk
description: Dowiedz się, jak skonfigurować usługi gRPC na ASP.NET Core, aby możliwe było wywoływanie z aplikacji przeglądarki za pomocą gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172279"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="1e141-103">Używanie gRPC w aplikacjach przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1e141-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="1e141-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="1e141-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e141-105">**gRPC — obsługa sieci Web w programie .NET jest eksperymentalna**</span><span class="sxs-lookup"><span data-stu-id="1e141-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="1e141-106">gRPC-Web for .NET to eksperymentalny projekt, a nie produkt zatwierdzony.</span><span class="sxs-lookup"><span data-stu-id="1e141-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="1e141-107">Chcemy:</span><span class="sxs-lookup"><span data-stu-id="1e141-107">We want to:</span></span>
>
> * <span data-ttu-id="1e141-108">Przetestuj, że nasze podejście do implementacji gRPC-Web działa.</span><span class="sxs-lookup"><span data-stu-id="1e141-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="1e141-109">Uzyskaj opinię na temat tego, czy takie podejście jest przydatne dla deweloperów platformy .NET w porównaniu do tradycyjnego sposobu konfigurowania gRPC-sieci Web za pośrednictwem serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="1e141-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="1e141-110">Wystaw opinię w [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) , aby upewnić się, że kompilujemy coś, co deweloperzy są podobne i wydajne.</span><span class="sxs-lookup"><span data-stu-id="1e141-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="1e141-111">Nie można wywołać usługi gRPC protokołu HTTP/2 z poziomu aplikacji opartej na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1e141-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="1e141-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) to protokół, który umożliwia aplikacjom JavaScript i Blazor w przeglądarce wywoływanie usług gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="1e141-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="1e141-113">W tym artykule wyjaśniono, jak używać gRPC-Web w programie .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e141-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="1e141-114">Konfigurowanie gRPC-sieci Web w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e141-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="1e141-115">usługi gRPC hostowane w ASP.NET Core można skonfigurować do obsługi gRPC-Web, a nie gRPC protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="1e141-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="1e141-116">gRPC — sieć Web nie wymaga żadnych zmian w usługach.</span><span class="sxs-lookup"><span data-stu-id="1e141-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="1e141-117">Jedyną modyfikacją jest konfiguracja uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1e141-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="1e141-118">Aby włączyć usługę gRPC-Web za pomocą usługi gRPC ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1e141-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="1e141-119">Dodaj odwołanie do pakietu [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="1e141-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="1e141-120">Skonfiguruj aplikację do używania gRPC-Web, dodając `AddGrpcWeb` i `UseGrpcWeb` do *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1e141-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="1e141-121">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1e141-121">The preceding code:</span></span>

* <span data-ttu-id="1e141-122">Dodaje oprogramowanie pośredniczące gRPC-Web, `UseGrpcWeb`po stronie routingu i przed punktami końcowymi.</span><span class="sxs-lookup"><span data-stu-id="1e141-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="1e141-123">Określa, `endpoints.MapGrpcService<GreeterService>()` Metoda obsługuje gRPC-Web z `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="1e141-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="1e141-124">Alternatywnie można skonfigurować wszystkie usługi do obsługi gRPC-Web, dodając `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` do ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="1e141-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="1e141-125">Do wywołania gRPC-Web z przeglądarki może być wymagana dodatkowa konfiguracja, taka jak konfigurowanie ASP.NET Core do obsługi mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="1e141-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="1e141-126">Aby uzyskać więcej informacji, zobacz temat [Obsługa mechanizmu CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="1e141-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="1e141-127">Wywołaj gRPC-Web z przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1e141-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="1e141-128">Aplikacje przeglądarki mogą używać gRPC-Web do wywoływania usług gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="1e141-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="1e141-129">Istnieją pewne wymagania i ograniczenia dotyczące wywoływania usług gRPC Services z usługą gRPC-Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="1e141-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="1e141-130">Serwer musi być skonfigurowany do obsługi gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="1e141-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="1e141-131">Wywołania przesyłania strumieniowego klientów i połączeń dwukierunkowych nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1e141-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="1e141-132">Przesyłanie strumieniowe serwera jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1e141-132">Server streaming is supported.</span></span>
* <span data-ttu-id="1e141-133">Wywoływanie usług gRPC w innej domenie wymaga skonfigurowania [CORS](xref:security/cors) na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1e141-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="1e141-134">JavaScript gRPC — klient sieci Web</span><span class="sxs-lookup"><span data-stu-id="1e141-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="1e141-135">Istnieje skrypt JavaScript gRPC-Web Client.</span><span class="sxs-lookup"><span data-stu-id="1e141-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="1e141-136">Aby uzyskać instrukcje dotyczące korzystania z usługi gRPC-Web w języku JavaScript, zobacz [pisanie kodu klienta JavaScript z gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="1e141-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="1e141-137">Konfigurowanie gRPC-sieci Web za pomocą klienta .NET gRPC</span><span class="sxs-lookup"><span data-stu-id="1e141-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="1e141-138">Klienta .NET gRPC można skonfigurować w taki sposób, aby wykonywać wywołania gRPC-sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1e141-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="1e141-139">Jest to przydatne w przypadku aplikacji [Blazor webassembly](xref:blazor/index#blazor-webassembly) , które są hostowane w przeglądarce i mają te same ograniczenia http związane z kodem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e141-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="1e141-140">Wywołanie gRPC-Web z klientem .NET jest [takie samo jak http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="1e141-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="1e141-141">Jedyną modyfikacją jest sposób tworzenia kanału.</span><span class="sxs-lookup"><span data-stu-id="1e141-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="1e141-142">Aby użyć gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="1e141-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="1e141-143">Dodaj odwołanie do pakietu [GRPC .NET. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="1e141-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="1e141-144">Upewnij się, że odwołanie do [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) Package ma wartość 2.27.0 lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="1e141-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="1e141-145">Skonfiguruj kanał, aby używał `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="1e141-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="1e141-146">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1e141-146">The preceding code:</span></span>

* <span data-ttu-id="1e141-147">Konfiguruje kanał do korzystania z gRPC-sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1e141-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="1e141-148">Tworzy klienta i wywołuje połączenie przy użyciu kanału.</span><span class="sxs-lookup"><span data-stu-id="1e141-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="1e141-149">Podczas tworzenia `GrpcWebHandler` są dostępne następujące opcje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1e141-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="1e141-150">**InnerHandler**: podstawowy <xref:System.Net.Http.HttpMessageHandler>, który wysyła żądanie HTTP gRPC, na przykład `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="1e141-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="1e141-151">**Tryb**: typ wyliczeniowy, który określa, czy `Content-Type` żądanie HTTP żądania gRPC jest `application/grpc-web` lub `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="1e141-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="1e141-152">`GrpcWebMode.GrpcWeb` konfiguruje zawartość do wysłania bez kodowania.</span><span class="sxs-lookup"><span data-stu-id="1e141-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="1e141-153">Wartość domyślna.</span><span class="sxs-lookup"><span data-stu-id="1e141-153">Default value.</span></span>
    * <span data-ttu-id="1e141-154">`GrpcWebMode.GrpcWebText` konfiguruje zawartość do kodowania base64.</span><span class="sxs-lookup"><span data-stu-id="1e141-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="1e141-155">Wymagane dla wywołań przesyłania strumieniowego serwera w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="1e141-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="1e141-156">**HttpVersion**: protokół http `Version` używany do ustawiania [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) w źródłowym żądaniu HTTP gRPC.</span><span class="sxs-lookup"><span data-stu-id="1e141-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="1e141-157">gRPC — sieć Web nie wymaga określonej wersji i nie przesłania jej domyślnie, chyba że zostanie określona.</span><span class="sxs-lookup"><span data-stu-id="1e141-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e141-158">Wygenerowane gRPC klienci mają metody synchronizacji i asynchroniczne do wywoływania metod jednoargumentowych.</span><span class="sxs-lookup"><span data-stu-id="1e141-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="1e141-159">Na przykład `SayHello` jest synchronizowana i `SayHelloAsync` jest Async.</span><span class="sxs-lookup"><span data-stu-id="1e141-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="1e141-160">Wywołanie metody synchronizacji w aplikacji Blazor webassembly spowoduje, że aplikacja przestanie odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="1e141-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="1e141-161">Metody asynchroniczne muszą być zawsze używane w zestawie webassembly Blazor.</span><span class="sxs-lookup"><span data-stu-id="1e141-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e141-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1e141-162">Additional resources</span></span>

* [<span data-ttu-id="1e141-163">gRPC dla klientów sieci Web — projekt GitHub</span><span class="sxs-lookup"><span data-stu-id="1e141-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
