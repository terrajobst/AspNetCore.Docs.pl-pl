---
title: Tworzenie żądania HTTP w programie ASP.NET Core przy użyciu IHttpClientFactory
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznego wystąpienia klasy HttpClient w programie ASP.NET Core za pomocą interfejsu IHttpClientFactory.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 540f14ad2b290d276436033a94d4c815888e5a95
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536005"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="edb27-103">Tworzenie żądania HTTP w programie ASP.NET Core przy użyciu IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="edb27-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

<span data-ttu-id="edb27-104">Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="edb27-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="edb27-105"><xref:System.Net.Http.IHttpClientFactory> Były rejestrowane i umożliwia konfigurowanie i tworzenie <xref:System.Net.Http.HttpClient> wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb27-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="edb27-106">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="edb27-106">It offers the following benefits:</span></span>

* <span data-ttu-id="edb27-107">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="edb27-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="edb27-108">Na przykład *github* klienta można zarejestrować i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="edb27-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="edb27-109">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="edb27-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="edb27-110">Określa zasady koncepcji wychodzących oprogramowania pośredniczącego za pośrednictwem Delegowanie obsługi w `HttpClient` i zapewnia rozszerzenia na podstawie Polly oprogramowaniu pośredniczącym, aby korzystać z zalet, który.</span><span class="sxs-lookup"><span data-stu-id="edb27-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="edb27-111">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="edb27-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="edb27-112">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="edb27-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="edb27-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="edb27-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="edb27-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="edb27-114">Prerequisites</span></span>

<span data-ttu-id="edb27-115">Projekty przeznaczone dla .NET Framework wymagają instalacji [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="edb27-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="edb27-116">Projekty przeznaczone dla platformy .NET Core i odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) uwzględniają już `Microsoft.Extensions.Http` pakietu.</span><span class="sxs-lookup"><span data-stu-id="edb27-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

::: moniker-end

## <a name="consumption-patterns"></a><span data-ttu-id="edb27-117">Wzorce użycia</span><span class="sxs-lookup"><span data-stu-id="edb27-117">Consumption patterns</span></span>

<span data-ttu-id="edb27-118">Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="edb27-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="edb27-119">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="edb27-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="edb27-120">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="edb27-121">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="edb27-122">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="edb27-123">Żadna z nich są ściśle nadrzędne w stosunku do innego.</span><span class="sxs-lookup"><span data-stu-id="edb27-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="edb27-124">Najlepszym rozwiązaniem, zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb27-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="edb27-125">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="edb27-125">Basic usage</span></span>

<span data-ttu-id="edb27-126">`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` metody rozszerzenia `IServiceCollection`w programie `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="edb27-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="edb27-127">Po zarejestrowaniu kod może akceptować `IHttpClientFactory` dowolnym usług może wprowadzone z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="edb27-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="edb27-128">`IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="edb27-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="edb27-129">Za pomocą `IHttpClientFactory` w ten sposób jest to dobry sposób, w jakie możesz refaktoryzować istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb27-129">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="edb27-130">Nie ma ona wpływu na sposób `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="edb27-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="edb27-131">W miejscach gdzie `HttpClient` aktualnie są tworzone wystąpienia, Zastąp wywołanie w celu te wystąpienia <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="edb27-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="edb27-132">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-132">Named clients</span></span>

<span data-ttu-id="edb27-133">Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**.</span><span class="sxs-lookup"><span data-stu-id="edb27-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="edb27-134">Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="edb27-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="edb27-135">W poprzednim kodzie `AddHttpClient` jest wywoływana, podając nazwę *github*.</span><span class="sxs-lookup"><span data-stu-id="edb27-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="edb27-136">Ten klient ma kilka zastosowano konfigurację domyślną&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="edb27-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="edb27-137">Każdorazowo `CreateClient` jest wywoływana, nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="edb27-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="edb27-138">Korzystanie z klienta o nazwie, mogą być przekazywane jako parametr ciągu `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="edb27-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="edb27-139">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="edb27-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="edb27-140">W poprzednim kodzie żądania nie należy określić nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="edb27-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="edb27-141">Go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.</span><span class="sxs-lookup"><span data-stu-id="edb27-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="edb27-142">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-142">Typed clients</span></span>

<span data-ttu-id="edb27-143">Wpisane klientów zapewniają takie same możliwości jak nazwane klientów bez konieczności używania ciągów jako klucze.</span><span class="sxs-lookup"><span data-stu-id="edb27-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="edb27-144">Podejście klient z typowaniem zawiera funkcję IntelliSense i kompilatora pomoc podczas korzystania z klientów.</span><span class="sxs-lookup"><span data-stu-id="edb27-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="edb27-145">Zapewniają one pojedyncza lokalizacja umożliwiająca Konfigurowanie i wchodzić w interakcje z określonym `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="edb27-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="edb27-146">Na przykład klient z typowaniem pojedynczego mogą być używane dla punktu końcowego pojedynczego zaplecza i hermetyzacji wszystkich logiki radzenia sobie z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="edb27-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="edb27-147">Inną zaletą jest praca z DI i może zostać wprowadzona, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb27-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="edb27-148">Klient z typowaniem akceptuje `HttpClient` parametru w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="edb27-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="edb27-149">W poprzednim kodzie konfiguracji jest przenoszony do klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="edb27-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="edb27-150">`HttpClient` Obiektu jest przedstawiany jako właściwość publiczną.</span><span class="sxs-lookup"><span data-stu-id="edb27-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="edb27-151">Można zdefiniować metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcji.</span><span class="sxs-lookup"><span data-stu-id="edb27-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="edb27-152">`GetAspNetDocsIssues` Metoda umożliwia hermetyzację kodu wymaganego do kwerendy i przeanalizuj najnowsze otwarte problemy z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="edb27-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="edb27-153">Aby zarejestrować klient z typowaniem, ogólnego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> — metoda rozszerzenia mogą być używane w `Startup.ConfigureServices`, określanie klasy klient z typowaniem:</span><span class="sxs-lookup"><span data-stu-id="edb27-153">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="edb27-154">Klient z typowaniem jest zarejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="edb27-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="edb27-155">Klient z typowaniem można wprowadzony i używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="edb27-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="edb27-156">Jeśli preferowane, można określić konfigurację klient z typowaniem podczas rejestracji w `Startup.ConfigureServices`, zamiast w Konstruktorze wpisane klienta:</span><span class="sxs-lookup"><span data-stu-id="edb27-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="edb27-157">Istnieje możliwość całkowicie hermetyzacji `HttpClient` w ramach klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="edb27-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="edb27-158">Zamiast uwidaczniania go jako właściwość, można podać metody publiczne która wywołuje metodę `HttpClient` wystąpienia wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="edb27-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="edb27-159">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="edb27-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="edb27-160">Dostęp do nawiązywania połączeń zewnętrznych przechodzi przez `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="edb27-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="edb27-161">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="edb27-161">Generated clients</span></span>

<span data-ttu-id="edb27-162">`IHttpClientFactory` mogą być używane w połączeniu z innymi bibliotekami innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="edb27-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="edb27-163">Refit jest biblioteką REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="edb27-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="edb27-164">Interfejsów API REST są konwertowane na żywo interfejsów.</span><span class="sxs-lookup"><span data-stu-id="edb27-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="edb27-165">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient` zapewnienie zewnętrznych HTTP wywołuje.</span><span class="sxs-lookup"><span data-stu-id="edb27-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="edb27-166">Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i odpowiedzi przez punkt końcowy:</span><span class="sxs-lookup"><span data-stu-id="edb27-166">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="edb27-167">Klient z typowaniem mogą być dodawane przy użyciu Refit można wygenerować wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="edb27-167">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="edb27-168">Interfejs zdefiniowany mogą być używane w przypadku, gdy jest to konieczne, z implementacją dostarczone przez DI i Refit:</span><span class="sxs-lookup"><span data-stu-id="edb27-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="edb27-169">Wychodzące żądanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="edb27-169">Outgoing request middleware</span></span>

<span data-ttu-id="edb27-170">`HttpClient` ma już pojęcie Delegowanie obsługi, które mogą być połączone ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="edb27-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="edb27-171">`IHttpClientFactory` Ułatwia do definiowania programów obsługi do zastosowania dla każdego klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="edb27-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="edb27-172">Obsługuje rejestrację i tworzenie łańcuchów z wielu obsług do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="edb27-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="edb27-173">Każda z tych programów obsługi jest możliwość wykonywania pracy przed i po nim żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="edb27-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="edb27-174">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="edb27-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="edb27-175">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="edb27-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="edb27-176">Aby utworzyć program obsługi, zdefiniuj Klasa pochodząca od <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="edb27-176">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="edb27-177">Zastąp `SendAsync` metodę, aby wykonać kod przed przekazaniem żądania do następnej procedury obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="edb27-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="edb27-178">Powyższy kod określa podstawowe programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="edb27-179">Sprawdza, czy `X-API-KEY` nagłówka zostało uwzględnione w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="edb27-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="edb27-180">Jeśli brakuje nagłówka, może uniknąć wywołania HTTP i zwracają odpowiedniej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="edb27-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="edb27-181">Podczas rejestracji, jeden lub więcej programów obsługi można dodać do konfiguracji `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="edb27-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="edb27-182">To zadanie jest realizowane za pośrednictwem metody rozszerzenia na <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="edb27-182">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="edb27-183">W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="edb27-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="edb27-184">`IHttpClientFactory` Tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-184">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="edb27-185">Programy obsługi mogą być zależne od usług z dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="edb27-185">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="edb27-186">Usługi, które zależą od obsługi są usuwane po usunięciu programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-186">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="edb27-187">Gdy zarejestrowana, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> może być wywoływana, przekazując typ dla programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-187">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="edb27-188">W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="edb27-188">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="edb27-189">Program obsługi **musi** być zarejestrowany w DI jako usługa przejściowy, nigdy nie o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="edb27-189">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="edb27-190">Jeśli program obsługi jest zarejestrowany jako usługa o określonym zakresie, wszystkie usługi, które zależy od programu obsługi są możliwe do rozporządzania programu obsługi usług można zlikwidowany, zanim program obsługi wykracza poza zakres, co spowoduje, że program obsługi ma zakończyć się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="edb27-190">If the handler is registered as a scoped service and any services that the handler depends upon are disposable, the handler's services could be disposed before the handler goes out of scope, which would cause the handler to fail.</span></span>

<span data-ttu-id="edb27-191">Gdy zarejestrowana, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> może być wywoływana, przekazując typ procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-191">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

::: moniker-end

<span data-ttu-id="edb27-192">Procedury obsługi wielu można zarejestrować w kolejności ich powinien zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="edb27-192">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="edb27-193">Każdy program obsługi kolejna procedura obsługi jest zawijany do momentu końcowe `HttpClientHandler` wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="edb27-193">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="edb27-194">Na udostępnianie stanu danego żądania obsługi komunikatów, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="edb27-194">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="edb27-195">Przekazywanie danych do obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="edb27-195">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="edb27-196">Użyj `IHttpContextAccessor` do dostępu do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="edb27-196">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="edb27-197">Utwórz niestandardową `AsyncLocal` obiekt magazynu, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="edb27-197">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="edb27-198">Użyj obsługi na podstawie Polly</span><span class="sxs-lookup"><span data-stu-id="edb27-198">Use Polly-based handlers</span></span>

<span data-ttu-id="edb27-199">`IHttpClientFactory` integruje się z popularnymi biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="edb27-199">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="edb27-200">Polly jest odporność kompleksowe i przejściowych Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="edb27-200">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="edb27-201">Dzięki niej deweloperzy mogą express zasad, takich jak ponownych prób, wyłącznik, limit czasu, grodziowym izolacji i rezerwowe w sposób fluent i metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="edb27-201">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="edb27-202">Metody rozszerzenia są udostępniane, aby umożliwić korzystanie z zasad w usłudze Polly skonfigurowane `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="edb27-202">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="edb27-203">Polly rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="edb27-203">The Polly extensions:</span></span>

* <span data-ttu-id="edb27-204">Obsługuje dodawanie na podstawie Polly programów obsługi dla klientów.</span><span class="sxs-lookup"><span data-stu-id="edb27-204">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="edb27-205">Mogą być używane po zainstalowaniu [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="edb27-205">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="edb27-206">Pakiet nie jest zawarty w udostępnionej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="edb27-206">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="edb27-207">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="edb27-207">Handle transient faults</span></span>

<span data-ttu-id="edb27-208">Najbardziej typowe błędy występują, gdy zewnętrznych połączeń HTTP jest przejściowy.</span><span class="sxs-lookup"><span data-stu-id="edb27-208">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="edb27-209">Wywołana metoda wygodne rozszerzenie `AddTransientHttpErrorPolicy` jest włączone, co pozwala zasad w celu zdefiniowania do obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="edb27-209">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="edb27-210">Zasady skonfigurowane przy użyciu tego uchwytu — metoda rozszerzenia `HttpRequestException`, odpowiedzi 5xx protokołu HTTP i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="edb27-210">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="edb27-211">`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="edb27-211">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="edb27-212">Rozszerzenie udostępnia `PolicyBuilder` obiektu skonfigurowane do obsługi reprezentujący możliwych błędów przejściowych błędów:</span><span class="sxs-lookup"><span data-stu-id="edb27-212">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="edb27-213">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowano zasad.</span><span class="sxs-lookup"><span data-stu-id="edb27-213">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="edb27-214">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem, 600 ms między próbami.</span><span class="sxs-lookup"><span data-stu-id="edb27-214">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="edb27-215">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="edb27-215">Dynamically select policies</span></span>

<span data-ttu-id="edb27-216">Metody rozszerzające dodatkowe istnieje, który może służyć do dodawania obsługi na podstawie Polly.</span><span class="sxs-lookup"><span data-stu-id="edb27-216">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="edb27-217">Jedno z rozszerzeń jest `AddPolicyHandler`, który ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="edb27-217">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="edb27-218">Jednego przeciążenia umożliwia wysłanie żądania do kontroli podczas definiowania zasad do zastosowania:</span><span class="sxs-lookup"><span data-stu-id="edb27-218">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="edb27-219">W poprzednim kodzie przypadku wychodzące żądanie HTTP GET, 10-sekundowy limit jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="edb27-219">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="edb27-220">Inna metoda HTTP używany jest limit czasu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="edb27-220">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="edb27-221">Dodawanie wielu obsług Polly</span><span class="sxs-lookup"><span data-stu-id="edb27-221">Add multiple Polly handlers</span></span>

<span data-ttu-id="edb27-222">Powszechne jest wprowadzanie zagnieździć Polly zasady, aby zapewnić ulepszone funkcje:</span><span class="sxs-lookup"><span data-stu-id="edb27-222">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="edb27-223">W powyższym przykładzie dwóch metod obsługi są dodawane.</span><span class="sxs-lookup"><span data-stu-id="edb27-223">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="edb27-224">Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania prób.</span><span class="sxs-lookup"><span data-stu-id="edb27-224">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="edb27-225">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy.</span><span class="sxs-lookup"><span data-stu-id="edb27-225">Failed requests are retried up to three times.</span></span> <span data-ttu-id="edb27-226">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="edb27-226">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="edb27-227">Dalsze zewnętrznych żądania są blokowane przez 30 sekund, jeśli pięć nieudanych prób występują po kolei.</span><span class="sxs-lookup"><span data-stu-id="edb27-227">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="edb27-228">Wyłącznik zasady są stanowe.</span><span class="sxs-lookup"><span data-stu-id="edb27-228">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="edb27-229">Wszystkie połączenia za pośrednictwem tego klienta współużytkować ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="edb27-229">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="edb27-230">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="edb27-230">Add policies from the Polly registry</span></span>

<span data-ttu-id="edb27-231">Podejście do zarządzania zasadami regularnie używane dotyczy je jeden raz zdefiniować i zarejestrować ich za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="edb27-231">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="edb27-232">Metody rozszerzenia jest pod warunkiem, co pozwala program obsługi ma zostać dodana za pomocą zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="edb27-232">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="edb27-233">W poprzednim kodzie dwie zasady są rejestrowane podczas `PolicyRegistry` jest dodawany do `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="edb27-233">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="edb27-234">Aby użyć zasad z rejestru, `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="edb27-234">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="edb27-235">Więcej informacji o `IHttpClientFactory` Polly integracji można znaleźć na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="edb27-235">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="edb27-236">Zarządzanie HttpClient i okresu istnienia</span><span class="sxs-lookup"><span data-stu-id="edb27-236">HttpClient and lifetime management</span></span>

<span data-ttu-id="edb27-237">Nowy `HttpClient` wystąpienie jest zwracane każdorazowo `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="edb27-237">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="edb27-238">Brak <xref:System.Net.Http.HttpMessageHandler> na klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="edb27-238">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="edb27-239">Fabryka zarządza okresy istnienia `HttpMessageHandler` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="edb27-239">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="edb27-240">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabryki, aby zmniejszyć wykorzystanie zasobów.</span><span class="sxs-lookup"><span data-stu-id="edb27-240">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="edb27-241">`HttpMessageHandler` Wystąpienia może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli nie upłynął jego okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="edb27-241">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="edb27-242">Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną bazowego połączeń HTTP.</span><span class="sxs-lookup"><span data-stu-id="edb27-242">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="edb27-243">Tworzenie więcej obsługi niż jest to konieczne może spowodować opóźnienia w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="edb27-243">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="edb27-244">Niektóre procedury obsługi również nie zamykaj połączeń przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.</span><span class="sxs-lookup"><span data-stu-id="edb27-244">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="edb27-245">Domyślny okres istnienia obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="edb27-245">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="edb27-246">Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie.</span><span class="sxs-lookup"><span data-stu-id="edb27-246">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="edb27-247">Aby zastąpić go, należy wywołać <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder` , jest zwracana podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="edb27-247">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="edb27-248">Usuwanie klienta nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="edb27-248">Disposal of the client isn't required.</span></span> <span data-ttu-id="edb27-249">Usuwanie anuluje wychodzących żądań i gwarancje danego `HttpClient` wystąpienia nie można używać po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="edb27-249">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="edb27-250">`IHttpClientFactory` śledzi i usuwa zasoby używane przez `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="edb27-250">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="edb27-251">`HttpClient` Wystąpień ogólnie można traktować jako obiektów platformy .NET nie wymaga usunięcia.</span><span class="sxs-lookup"><span data-stu-id="edb27-251">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="edb27-252">Utrzymywanie pojedynczej `HttpClient` wystąpienie aktywności przez długi czas jest typowy wzorzec używany przed powstania `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="edb27-252">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="edb27-253">Ten wzorzec staje się niepotrzebne po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="edb27-253">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="edb27-254">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="edb27-254">Logging</span></span>

<span data-ttu-id="edb27-255">Klientów utworzonych za pomocą `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="edb27-255">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="edb27-256">Włącz poziom odpowiednie informacje w swojej konfiguracji rejestrowania, aby wyświetlić komunikaty dziennika domyślne.</span><span class="sxs-lookup"><span data-stu-id="edb27-256">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="edb27-257">Dodatkowe rejestrowanie, takich jak rejestrowanie nagłówków żądania, jest dostępny tylko na poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="edb27-257">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="edb27-258">Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="edb27-258">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="edb27-259">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="edb27-259">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="edb27-260">Sufiks wiadomości *LogicalHandler* odbywa się poza potoku program obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="edb27-260">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="edb27-261">Na żądanie komunikaty są rejestrowane, zanim innych programów obsługi w potoku zostały przetworzone go.</span><span class="sxs-lookup"><span data-stu-id="edb27-261">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="edb27-262">W odpowiedzi komunikaty są rejestrowane po innych obsługi potoku otrzymane odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="edb27-262">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="edb27-263">Rejestrowanie występuje także w potoku programu obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="edb27-263">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="edb27-264">W *MyNamedClient* przykład te komunikaty są rejestrowane względem kategoria dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="edb27-264">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="edb27-265">Dla żądania dzieje się po wykonaniu innych programów obsługi i od razu, zanim żądanie zostanie wysłane w sieci.</span><span class="sxs-lookup"><span data-stu-id="edb27-265">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="edb27-266">W odpowiedzi rejestrowanie obejmuje stan odpowiedzi przed przekazaniem za pośrednictwem potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="edb27-266">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="edb27-267">Włączanie rejestrowania na zewnątrz i wewnątrz potok umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="edb27-267">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="edb27-268">Może to obejmować zmiany nagłówki, żądań, na przykład lub kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="edb27-268">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="edb27-269">W tym nazwa klienta w kategorii dziennika umożliwia dziennika filtrowania dla określonych klientów o nazwie, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="edb27-269">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="edb27-270">Klasa HttpMessageHandler skonfigurować</span><span class="sxs-lookup"><span data-stu-id="edb27-270">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="edb27-271">Może być konieczne do kontrolowania konfiguracji wewnętrzny `HttpMessageHandler` używany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="edb27-271">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="edb27-272">`IHttpClientBuilder` Jest zwracany, podczas dodawania o nazwie lub wpisane klientów.</span><span class="sxs-lookup"><span data-stu-id="edb27-272">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="edb27-273"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> — Metoda rozszerzenia może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="edb27-273">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="edb27-274">Delegat jest używany do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używane przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="edb27-274">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a><span data-ttu-id="edb27-275">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="edb27-275">Additional resources</span></span>

* [<span data-ttu-id="edb27-276">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="edb27-276">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="edb27-277">Implementowanie ponownych prób wywołania HTTP z wykorzystaniem wykładniczego wycofywania z zasadami dotyczącymi HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="edb27-277">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="edb27-278">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="edb27-278">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)