---
title: Inicjowanie żądań HTTP
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznego wystąpienia klasy HttpClient w programie ASP.NET Core za pomocą interfejsu IHttpClientFactory.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 0aca2b260e787f9b8aa0846bcccef2b33f372ee6
ms.sourcegitcommit: 6425baa92cec4537368705f8d27f3d0e958e43cd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39220589"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="3075e-103">Inicjowanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="3075e-103">Initiate HTTP requests</span></span>

<span data-ttu-id="3075e-104">Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3075e-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="3075e-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) były rejestrowane i umożliwia konfigurowanie i tworzenie [HttpClient](/dotnet/api/system.net.http.httpclient) wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3075e-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="3075e-106">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="3075e-106">It offers the following benefits:</span></span>

* <span data-ttu-id="3075e-107">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="3075e-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3075e-108">Na przykład klient "github" można zarejestrowane i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="3075e-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="3075e-109">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="3075e-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3075e-110">Określa zasady koncepcji wychodzących oprogramowania pośredniczącego za pośrednictwem Delegowanie obsługi w `HttpClient` i zapewnia rozszerzenia na podstawie Polly oprogramowaniu pośredniczącym, aby korzystać z zalet, który.</span><span class="sxs-lookup"><span data-stu-id="3075e-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3075e-111">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="3075e-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3075e-112">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="3075e-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3075e-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3075e-113">Prerequisites</span></span>

<span data-ttu-id="3075e-114">Projekty przeznaczone dla .NET Framework wymagają instalacji [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3075e-114">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="3075e-115">Projekty przeznaczone dla platformy .NET Core i odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) uwzględniają już `Microsoft.Extensions.Http` pakietu.</span><span class="sxs-lookup"><span data-stu-id="3075e-115">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="3075e-116">Wzorce użycia</span><span class="sxs-lookup"><span data-stu-id="3075e-116">Consumption patterns</span></span>

<span data-ttu-id="3075e-117">Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3075e-117">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3075e-118">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="3075e-118">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3075e-119">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-119">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3075e-120">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-120">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3075e-121">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-121">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3075e-122">Żadna z nich są ściśle nadrzędne w stosunku do innego.</span><span class="sxs-lookup"><span data-stu-id="3075e-122">None of them are strictly superior to another.</span></span> <span data-ttu-id="3075e-123">Najlepszym rozwiązaniem, zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3075e-123">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3075e-124">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="3075e-124">Basic usage</span></span>

<span data-ttu-id="3075e-125">`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` metody rozszerzenia `IServiceCollection`w programie `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="3075e-125">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="3075e-126">Po zarejestrowaniu kod może akceptować `IHttpClientFactory` dowolnym usług może wprowadzone z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="3075e-126">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="3075e-127">`IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="3075e-127">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="3075e-128">Za pomocą `IHttpClientFactory` w ten sposób jest to doskonały sposób, w jakie możesz refaktoryzować istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3075e-128">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="3075e-129">Nie ma ona wpływu na sposób `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="3075e-129">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3075e-130">W miejscach gdzie `HttpClient` aktualnie są tworzone wystąpienia, Zastąp wywołanie w celu te wystąpienia `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3075e-130">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3075e-131">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-131">Named clients</span></span>

<span data-ttu-id="3075e-132">Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**.</span><span class="sxs-lookup"><span data-stu-id="3075e-132">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3075e-133">Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3075e-133">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="3075e-134">W poprzednim kodzie `AddHttpClient` jest wywoływana, podając nazwę "github".</span><span class="sxs-lookup"><span data-stu-id="3075e-134">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="3075e-135">Ten klient ma kilka zastosowano konfigurację domyślną&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="3075e-135">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3075e-136">Każdorazowo `CreateClient` jest wywoływana, nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3075e-136">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3075e-137">Korzystanie z klienta o nazwie, mogą być przekazywane jako parametr ciągu `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3075e-137">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3075e-138">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="3075e-138">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="3075e-139">W poprzednim kodzie żądania nie należy określić nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="3075e-139">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3075e-140">Go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3075e-140">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3075e-141">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-141">Typed clients</span></span>

<span data-ttu-id="3075e-142">Wpisane klientów zapewniają takie same możliwości jak nazwane klientów bez konieczności używania ciągów jako klucze.</span><span class="sxs-lookup"><span data-stu-id="3075e-142">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="3075e-143">Podejście klient z typowaniem zawiera funkcję IntelliSense i kompilatora pomoc podczas korzystania z klientów.</span><span class="sxs-lookup"><span data-stu-id="3075e-143">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="3075e-144">Zapewniają one pojedyncza lokalizacja umożliwiająca Konfigurowanie i wchodzić w interakcje z określonym `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3075e-144">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3075e-145">Na przykład klient z typowaniem pojedynczego mogą być używane dla punktu końcowego pojedynczego zaplecza i hermetyzacji wszystkich logiki radzenia sobie z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="3075e-145">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="3075e-146">Inną zaletą jest praca z DI i może zostać wprowadzona, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3075e-146">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3075e-147">Klient z typowaniem akceptuje `HttpClient` parametru w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="3075e-147">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3075e-148">W poprzednim kodzie konfiguracji jest przenoszony do klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="3075e-148">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3075e-149">`HttpClient` Obiektu jest przedstawiany jako właściwość publiczną.</span><span class="sxs-lookup"><span data-stu-id="3075e-149">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3075e-150">Można zdefiniować metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcji.</span><span class="sxs-lookup"><span data-stu-id="3075e-150">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3075e-151">`GetAspNetDocsIssues` Metoda umożliwia hermetyzację kodu wymaganego do kwerendy i przeanalizuj najnowsze otwarte problemy z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="3075e-151">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3075e-152">Aby zarejestrować klient z typowaniem, ogólnego `AddHttpClient` — metoda rozszerzenia mogą być używane w `Startup.ConfigureServices`, określanie klasy klient z typowaniem:</span><span class="sxs-lookup"><span data-stu-id="3075e-152">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="3075e-153">Klient z typowaniem jest zarejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="3075e-153">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3075e-154">Klient z typowaniem można wprowadzony i używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3075e-154">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3075e-155">Jeśli preferowane, można określić konfigurację klient z typowaniem podczas rejestracji w `Startup.ConfigureServices`, zamiast w Konstruktorze wpisane klienta:</span><span class="sxs-lookup"><span data-stu-id="3075e-155">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="3075e-156">Istnieje możliwość całkowicie hermetyzacji `HttpClient` w ramach klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="3075e-156">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3075e-157">Zamiast uwidaczniania go jako właściwość, można podać metody publiczne która wywołuje metodę `HttpClient` wystąpienia wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="3075e-157">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="3075e-158">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="3075e-158">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3075e-159">Dostęp do nawiązywania połączeń zewnętrznych przechodzi przez `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="3075e-159">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3075e-160">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="3075e-160">Generated clients</span></span>

<span data-ttu-id="3075e-161">`IHttpClientFactory` mogą być używane w połączeniu z innymi bibliotekami innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="3075e-161">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3075e-162">Refit jest biblioteką REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3075e-162">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3075e-163">Interfejsów API REST są konwertowane na żywo interfejsów.</span><span class="sxs-lookup"><span data-stu-id="3075e-163">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3075e-164">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient` zapewnienie zewnętrznych HTTP wywołuje.</span><span class="sxs-lookup"><span data-stu-id="3075e-164">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3075e-165">Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i odpowiedzi przez punkt końcowy:</span><span class="sxs-lookup"><span data-stu-id="3075e-165">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="3075e-166">Klient z typowaniem mogą być dodawane przy użyciu Refit można wygenerować wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="3075e-166">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="3075e-167">Interfejs zdefiniowany mogą być używane w przypadku, gdy jest to konieczne, z implementacją dostarczone przez DI i Refit:</span><span class="sxs-lookup"><span data-stu-id="3075e-167">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3075e-168">Wychodzące żądanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="3075e-168">Outgoing request middleware</span></span>

<span data-ttu-id="3075e-169">`HttpClient` ma już pojęcie Delegowanie obsługi, które mogą być połączone ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3075e-169">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3075e-170">`IHttpClientFactory` Ułatwia do definiowania programów obsługi do zastosowania dla każdego klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="3075e-170">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3075e-171">Obsługuje rejestrację i tworzenie łańcuchów z wielu obsług do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="3075e-171">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3075e-172">Każda z tych programów obsługi jest możliwość wykonywania pracy przed i po nim żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="3075e-172">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3075e-173">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3075e-173">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3075e-174">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="3075e-174">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3075e-175">Aby utworzyć program obsługi, zdefiniuj Klasa pochodząca od `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="3075e-175">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="3075e-176">Zastąp `SendAsync` metodę, aby wykonać kod przed przekazaniem żądania do następnej procedury obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="3075e-176">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3075e-177">Powyższy kod określa podstawowe programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3075e-177">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3075e-178">Sprawdza, jeśli nagłówek X-API-KEY zostało uwzględnione w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3075e-178">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="3075e-179">Jeśli brakuje nagłówka, może uniknąć wywołania HTTP i zwracają odpowiedniej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3075e-179">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3075e-180">Podczas rejestracji, jeden lub więcej programów obsługi można dodać do konfiguracji `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3075e-180">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3075e-181">To zadanie jest realizowane za pośrednictwem metody rozszerzenia na `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3075e-181">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="3075e-182">W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="3075e-182">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3075e-183">Program obsługi **musi** być zarejestrowany w DI jako przejściowe.</span><span class="sxs-lookup"><span data-stu-id="3075e-183">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="3075e-184">Gdy zarejestrowana, `AddHttpMessageHandler` może być wywoływana, przekazując typ dla programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3075e-184">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="3075e-185">Procedury obsługi wielu można zarejestrować w kolejności ich powinien zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="3075e-185">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3075e-186">Każdy program obsługi kolejna procedura obsługi jest zawijany do momentu końcowe `HttpClientHandler` wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="3075e-186">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3075e-187">Użyj obsługi na podstawie Polly</span><span class="sxs-lookup"><span data-stu-id="3075e-187">Use Polly-based handlers</span></span>

<span data-ttu-id="3075e-188">`IHttpClientFactory` integruje się z popularnymi biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="3075e-188">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3075e-189">Polly jest odporność kompleksowe i przejściowych Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3075e-189">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3075e-190">Dzięki niej deweloperzy mogą express zasad, takich jak ponownych prób, wyłącznik, limit czasu, grodziowym izolacji i rezerwowe w sposób fluent i metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="3075e-190">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3075e-191">Metody rozszerzenia są udostępniane, aby umożliwić korzystanie z zasad w usłudze Polly skonfigurowane `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="3075e-191">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3075e-192">W dostępnych rozszerzeń Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3075e-192">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3075e-193">Ten pakiet nie jest zawarty w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3075e-193">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="3075e-194">Aby korzystać z rozszerzeń, jawnie `<PackageReference />` powinny być uwzględnione w projekcie.</span><span class="sxs-lookup"><span data-stu-id="3075e-194">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="3075e-195">Po przywróceniu tego pakietu, metody rozszerzenia są dostępne w celu umożliwienia obsługi dodawania na podstawie Polly programy obsługi dla klientów.</span><span class="sxs-lookup"><span data-stu-id="3075e-195">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3075e-196">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="3075e-196">Handle transient faults</span></span>

<span data-ttu-id="3075e-197">Najbardziej typowe błędy, oczekiwanych może wystąpić, gdy zewnętrznych wywołań HTTP będzie przejściowy.</span><span class="sxs-lookup"><span data-stu-id="3075e-197">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="3075e-198">Wywołana metoda wygodne rozszerzenie `AddTransientHttpErrorPolicy` jest włączone, co pozwala zasad w celu zdefiniowania do obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="3075e-198">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3075e-199">Zasady skonfigurowane przy użyciu tego uchwytu — metoda rozszerzenia `HttpRequestException`, odpowiedzi 5xx protokołu HTTP i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="3075e-199">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3075e-200">`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3075e-200">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3075e-201">Rozszerzenie udostępnia `PolicyBuilder` obiektu skonfigurowane do obsługi reprezentujący możliwych błędów przejściowych błędów:</span><span class="sxs-lookup"><span data-stu-id="3075e-201">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="3075e-202">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowano zasad.</span><span class="sxs-lookup"><span data-stu-id="3075e-202">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3075e-203">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem, 600 ms między próbami.</span><span class="sxs-lookup"><span data-stu-id="3075e-203">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3075e-204">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="3075e-204">Dynamically select policies</span></span>

<span data-ttu-id="3075e-205">Metody rozszerzające dodatkowe istnieje, który może służyć do dodawania obsługi na podstawie Polly.</span><span class="sxs-lookup"><span data-stu-id="3075e-205">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3075e-206">Jedno z rozszerzeń jest `AddPolicyHandler`, który ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="3075e-206">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3075e-207">Jednego przeciążenia umożliwia wysłanie żądania do kontroli podczas definiowania zasad do zastosowania:</span><span class="sxs-lookup"><span data-stu-id="3075e-207">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="3075e-208">W poprzednim kodzie Jeśli wychodzące żądanie GET, 10-sekundowy limit jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="3075e-208">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3075e-209">Inna metoda HTTP używany jest limit czasu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="3075e-209">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3075e-210">Dodawanie wielu obsług Polly</span><span class="sxs-lookup"><span data-stu-id="3075e-210">Add multiple Polly handlers</span></span>

<span data-ttu-id="3075e-211">Powszechne jest wprowadzanie zagnieździć Polly zasady, aby zapewnić ulepszone funkcje:</span><span class="sxs-lookup"><span data-stu-id="3075e-211">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="3075e-212">W powyższym przykładzie dwóch metod obsługi są dodawane.</span><span class="sxs-lookup"><span data-stu-id="3075e-212">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3075e-213">Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania prób.</span><span class="sxs-lookup"><span data-stu-id="3075e-213">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3075e-214">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy.</span><span class="sxs-lookup"><span data-stu-id="3075e-214">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3075e-215">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="3075e-215">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3075e-216">Dalsze zewnętrznych żądania są blokowane przez 30 sekund, jeśli pięć nieudanych prób występują po kolei.</span><span class="sxs-lookup"><span data-stu-id="3075e-216">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3075e-217">Wyłącznik zasady są stanowe.</span><span class="sxs-lookup"><span data-stu-id="3075e-217">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3075e-218">Wszystkie połączenia za pośrednictwem tego klienta współużytkować ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="3075e-218">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3075e-219">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="3075e-219">Add policies from the Polly registry</span></span>

<span data-ttu-id="3075e-220">Podejście do zarządzania zasadami regularnie używane dotyczy je jeden raz zdefiniować i zarejestrować ich za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="3075e-220">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3075e-221">Metody rozszerzenia jest pod warunkiem, co pozwala program obsługi ma zostać dodana za pomocą zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="3075e-221">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="3075e-222">W poprzednim kodzie PolicyRegistry jest dodawany do `ServiceCollection` i dwie zasady są zarejestrowane z nim.</span><span class="sxs-lookup"><span data-stu-id="3075e-222">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="3075e-223">Aby można było używać zasad z rejestru, `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3075e-223">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3075e-224">Więcej informacji o `IHttpClientFactory` Polly integracji można znaleźć na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="3075e-224">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3075e-225">Zarządzanie HttpClient i okresu istnienia</span><span class="sxs-lookup"><span data-stu-id="3075e-225">HttpClient and lifetime management</span></span>

<span data-ttu-id="3075e-226">Każdorazowo `CreateClient` jest wywoływana w `IHttpClientFactory`, nowe wystąpienie klasy `HttpClient` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="3075e-226">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="3075e-227">Będzie `HttpMessageHandler` na klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="3075e-227">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="3075e-228">`IHttpClientFactory` pula będzie `HttpMessageHandler` wystąpienia utworzone przez fabryki, aby zmniejszyć wykorzystanie zasobów.</span><span class="sxs-lookup"><span data-stu-id="3075e-228">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3075e-229">A `HttpMessageHandler` wystąpienia może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli nie upłynął jego okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="3075e-229">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="3075e-230">Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną podstawowego połączenia HTTP; Tworzenie więcej obsługi niż jest to konieczne może spowodować opóźnienia w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="3075e-230">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3075e-231">Niektóre procedury obsługi również nie zamykaj połączeń przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.</span><span class="sxs-lookup"><span data-stu-id="3075e-231">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3075e-232">Domyślny okres istnienia obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3075e-232">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3075e-233">Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie.</span><span class="sxs-lookup"><span data-stu-id="3075e-233">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3075e-234">Aby zastąpić go, należy wywołać `SetHandlerLifetime` na `IHttpClientBuilder` , jest zwracana podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="3075e-234">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="3075e-235">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3075e-235">Logging</span></span>

<span data-ttu-id="3075e-236">Klientów utworzonych za pomocą `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="3075e-236">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3075e-237">Należy włączyć poziom odpowiednie informacje w swojej konfiguracji rejestrowania, aby wyświetlić komunikaty dziennika domyślne.</span><span class="sxs-lookup"><span data-stu-id="3075e-237">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3075e-238">Dodatkowe rejestrowanie, takich jak rejestrowanie nagłówków żądania, jest dostępny tylko na poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3075e-238">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3075e-239">Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="3075e-239">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3075e-240">Klient o nazwie "MyNamedClient", na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="3075e-240">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3075e-241">Komunikaty z sufiksem "LogicalHandler" odbywa się na zewnątrz potoku program obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="3075e-241">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="3075e-242">Na żądanie komunikaty są rejestrowane, zanim innych programów obsługi w potoku zostały przetworzone go.</span><span class="sxs-lookup"><span data-stu-id="3075e-242">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3075e-243">W odpowiedzi komunikaty są rejestrowane po innych obsługi potoku otrzymane odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3075e-243">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3075e-244">Rejestrowanie występuje również wewnątrz potoku program obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="3075e-244">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="3075e-245">W przypadku w przykładzie "MyNamedClient" te komunikaty są rejestrowane względem kategoria dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="3075e-245">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3075e-246">Dla żądania dzieje się po wykonaniu innych programów obsługi i od razu, zanim żądanie zostanie wysłane w sieci.</span><span class="sxs-lookup"><span data-stu-id="3075e-246">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3075e-247">W odpowiedzi rejestrowanie obejmuje stan odpowiedzi przed przekazaniem za pośrednictwem potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3075e-247">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3075e-248">Włączanie rejestrowania na zewnątrz i wewnątrz potok umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="3075e-248">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3075e-249">Może to obejmować zmiany nagłówki, żądań, na przykład lub kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3075e-249">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3075e-250">W tym nazwa klienta w kategorii dziennika umożliwia dziennika filtrowania dla określonych klientów o nazwie, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3075e-250">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3075e-251">Klasa HttpMessageHandler skonfigurować</span><span class="sxs-lookup"><span data-stu-id="3075e-251">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3075e-252">Może być konieczne do kontrolowania konfiguracji wewnętrzny `HttpMessageHandler` używany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3075e-252">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3075e-253">`IHttpClientBuilder` Jest zwracany, podczas dodawania o nazwie lub wpisane klientów.</span><span class="sxs-lookup"><span data-stu-id="3075e-253">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3075e-254">`ConfigurePrimaryHttpMessageHandler` — Metoda rozszerzenia może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="3075e-254">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="3075e-255">Delegat jest używany do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używane przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="3075e-255">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
