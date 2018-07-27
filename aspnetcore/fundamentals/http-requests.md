---
title: Inicjowanie żądań HTTP
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznego wystąpienia klasy HttpClient w programie ASP.NET Core za pomocą interfejsu IHttpClientFactory.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 87424eaea499ba7ece1e5ef88649fcbb2e297635
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320658"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="46922-103">Inicjowanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="46922-103">Initiate HTTP requests</span></span>

<span data-ttu-id="46922-104">Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="46922-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="46922-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) były rejestrowane i umożliwia konfigurowanie i tworzenie [HttpClient](/dotnet/api/system.net.http.httpclient) wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="46922-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="46922-106">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="46922-106">It offers the following benefits:</span></span>

* <span data-ttu-id="46922-107">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="46922-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="46922-108">Na przykład klient "github" można zarejestrowane i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="46922-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="46922-109">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="46922-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="46922-110">Określa zasady koncepcji wychodzących oprogramowania pośredniczącego za pośrednictwem Delegowanie obsługi w `HttpClient` i zapewnia rozszerzenia na podstawie Polly oprogramowaniu pośredniczącym, aby korzystać z zalet, który.</span><span class="sxs-lookup"><span data-stu-id="46922-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="46922-111">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="46922-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="46922-112">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="46922-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="46922-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="46922-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46922-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="46922-114">Prerequisites</span></span>

<span data-ttu-id="46922-115">Projekty przeznaczone dla .NET Framework wymagają instalacji [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="46922-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="46922-116">Projekty przeznaczone dla platformy .NET Core i odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) uwzględniają już `Microsoft.Extensions.Http` pakietu.</span><span class="sxs-lookup"><span data-stu-id="46922-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="46922-117">Wzorce użycia</span><span class="sxs-lookup"><span data-stu-id="46922-117">Consumption patterns</span></span>

<span data-ttu-id="46922-118">Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="46922-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="46922-119">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="46922-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="46922-120">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="46922-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="46922-121">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="46922-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="46922-122">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="46922-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="46922-123">Żadna z nich są ściśle nadrzędne w stosunku do innego.</span><span class="sxs-lookup"><span data-stu-id="46922-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="46922-124">Najlepszym rozwiązaniem, zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="46922-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="46922-125">Podstawowe użycia</span><span class="sxs-lookup"><span data-stu-id="46922-125">Basic usage</span></span>

<span data-ttu-id="46922-126">`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` metody rozszerzenia `IServiceCollection`w programie `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="46922-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="46922-127">Po zarejestrowaniu kod może akceptować `IHttpClientFactory` dowolnym usług może wprowadzone z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="46922-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="46922-128">`IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="46922-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="46922-129">Za pomocą `IHttpClientFactory` w ten sposób jest to doskonały sposób, w jakie możesz refaktoryzować istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="46922-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="46922-130">Nie ma ona wpływu na sposób `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="46922-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="46922-131">W miejscach gdzie `HttpClient` aktualnie są tworzone wystąpienia, Zastąp wywołanie w celu te wystąpienia `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="46922-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="46922-132">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="46922-132">Named clients</span></span>

<span data-ttu-id="46922-133">Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**.</span><span class="sxs-lookup"><span data-stu-id="46922-133">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="46922-134">Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="46922-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="46922-135">W poprzednim kodzie `AddHttpClient` jest wywoływana, podając nazwę "github".</span><span class="sxs-lookup"><span data-stu-id="46922-135">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="46922-136">Ten klient ma kilka zastosowano konfigurację domyślną&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="46922-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="46922-137">Każdorazowo `CreateClient` jest wywoływana, nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="46922-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="46922-138">Korzystanie z klienta o nazwie, mogą być przekazywane jako parametr ciągu `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="46922-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="46922-139">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="46922-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="46922-140">W poprzednim kodzie żądania nie należy określić nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="46922-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="46922-141">Go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.</span><span class="sxs-lookup"><span data-stu-id="46922-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="46922-142">Wpisane klientów</span><span class="sxs-lookup"><span data-stu-id="46922-142">Typed clients</span></span>

<span data-ttu-id="46922-143">Wpisane klientów zapewniają takie same możliwości jak nazwane klientów bez konieczności używania ciągów jako klucze.</span><span class="sxs-lookup"><span data-stu-id="46922-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="46922-144">Podejście klient z typowaniem zawiera funkcję IntelliSense i kompilatora pomoc podczas korzystania z klientów.</span><span class="sxs-lookup"><span data-stu-id="46922-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="46922-145">Zapewniają one pojedyncza lokalizacja umożliwiająca Konfigurowanie i wchodzić w interakcje z określonym `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="46922-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="46922-146">Na przykład klient z typowaniem pojedynczego mogą być używane dla punktu końcowego pojedynczego zaplecza i hermetyzacji wszystkich logiki radzenia sobie z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="46922-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="46922-147">Inną zaletą jest praca z DI i może zostać wprowadzona, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="46922-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="46922-148">Klient z typowaniem akceptuje `HttpClient` parametru w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="46922-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="46922-149">W poprzednim kodzie konfiguracji jest przenoszony do klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="46922-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="46922-150">`HttpClient` Obiektu jest przedstawiany jako właściwość publiczną.</span><span class="sxs-lookup"><span data-stu-id="46922-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="46922-151">Można zdefiniować metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcji.</span><span class="sxs-lookup"><span data-stu-id="46922-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="46922-152">`GetAspNetDocsIssues` Metoda umożliwia hermetyzację kodu wymaganego do kwerendy i przeanalizuj najnowsze otwarte problemy z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="46922-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="46922-153">Aby zarejestrować klient z typowaniem, ogólnego `AddHttpClient` — metoda rozszerzenia mogą być używane w `Startup.ConfigureServices`, określanie klasy klient z typowaniem:</span><span class="sxs-lookup"><span data-stu-id="46922-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="46922-154">Klient z typowaniem jest zarejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="46922-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="46922-155">Klient z typowaniem można wprowadzony i używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="46922-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="46922-156">Jeśli preferowane, można określić konfigurację klient z typowaniem podczas rejestracji w `Startup.ConfigureServices`, zamiast w Konstruktorze wpisane klienta:</span><span class="sxs-lookup"><span data-stu-id="46922-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="46922-157">Istnieje możliwość całkowicie hermetyzacji `HttpClient` w ramach klient z typowaniem.</span><span class="sxs-lookup"><span data-stu-id="46922-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="46922-158">Zamiast uwidaczniania go jako właściwość, można podać metody publiczne która wywołuje metodę `HttpClient` wystąpienia wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="46922-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="46922-159">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="46922-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="46922-160">Dostęp do nawiązywania połączeń zewnętrznych przechodzi przez `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="46922-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="46922-161">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="46922-161">Generated clients</span></span>

<span data-ttu-id="46922-162">`IHttpClientFactory` mogą być używane w połączeniu z innymi bibliotekami innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="46922-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="46922-163">Refit jest biblioteką REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="46922-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="46922-164">Interfejsów API REST są konwertowane na żywo interfejsów.</span><span class="sxs-lookup"><span data-stu-id="46922-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="46922-165">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient` zapewnienie zewnętrznych HTTP wywołuje.</span><span class="sxs-lookup"><span data-stu-id="46922-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="46922-166">Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i odpowiedzi przez punkt końcowy:</span><span class="sxs-lookup"><span data-stu-id="46922-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="46922-167">Klient z typowaniem mogą być dodawane przy użyciu Refit można wygenerować wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="46922-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="46922-168">Interfejs zdefiniowany mogą być używane w przypadku, gdy jest to konieczne, z implementacją dostarczone przez DI i Refit:</span><span class="sxs-lookup"><span data-stu-id="46922-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="46922-169">Wychodzące żądanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="46922-169">Outgoing request middleware</span></span>

<span data-ttu-id="46922-170">`HttpClient` ma już pojęcie Delegowanie obsługi, które mogą być połączone ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="46922-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="46922-171">`IHttpClientFactory` Ułatwia do definiowania programów obsługi do zastosowania dla każdego klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="46922-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="46922-172">Obsługuje rejestrację i tworzenie łańcuchów z wielu obsług do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="46922-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="46922-173">Każda z tych programów obsługi jest możliwość wykonywania pracy przed i po nim żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="46922-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="46922-174">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46922-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="46922-175">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="46922-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="46922-176">Aby utworzyć program obsługi, zdefiniuj Klasa pochodząca od `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="46922-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="46922-177">Zastąp `SendAsync` metodę, aby wykonać kod przed przekazaniem żądania do następnej procedury obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="46922-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="46922-178">Powyższy kod określa podstawowe programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="46922-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="46922-179">Sprawdza, jeśli nagłówek X-API-KEY zostało uwzględnione w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="46922-179">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="46922-180">Jeśli brakuje nagłówka, może uniknąć wywołania HTTP i zwracają odpowiedniej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="46922-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="46922-181">Podczas rejestracji, jeden lub więcej programów obsługi można dodać do konfiguracji `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="46922-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="46922-182">To zadanie jest realizowane za pośrednictwem metody rozszerzenia na `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="46922-182">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="46922-183">W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="46922-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="46922-184">Program obsługi **musi** być zarejestrowany w DI jako przejściowe.</span><span class="sxs-lookup"><span data-stu-id="46922-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="46922-185">Gdy zarejestrowana, `AddHttpMessageHandler` może być wywoływana, przekazując typ dla programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="46922-185">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="46922-186">Procedury obsługi wielu można zarejestrować w kolejności ich powinien zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="46922-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="46922-187">Każdy program obsługi kolejna procedura obsługi jest zawijany do momentu końcowe `HttpClientHandler` wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="46922-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="46922-188">Użyj obsługi na podstawie Polly</span><span class="sxs-lookup"><span data-stu-id="46922-188">Use Polly-based handlers</span></span>

<span data-ttu-id="46922-189">`IHttpClientFactory` integruje się z popularnymi biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="46922-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="46922-190">Polly jest odporność kompleksowe i przejściowych Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="46922-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="46922-191">Dzięki niej deweloperzy mogą express zasad, takich jak ponownych prób, wyłącznik, limit czasu, grodziowym izolacji i rezerwowe w sposób fluent i metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="46922-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="46922-192">Metody rozszerzenia są udostępniane, aby umożliwić korzystanie z zasad w usłudze Polly skonfigurowane `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="46922-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="46922-193">W dostępnych rozszerzeń Polly [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="46922-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="46922-194">Ten pakiet nie jest zawarty w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="46922-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="46922-195">Aby korzystać z rozszerzeń, jawnie `<PackageReference />` powinny być uwzględnione w projekcie.</span><span class="sxs-lookup"><span data-stu-id="46922-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="46922-196">Po przywróceniu tego pakietu, metody rozszerzenia są dostępne w celu umożliwienia obsługi dodawania na podstawie Polly programy obsługi dla klientów.</span><span class="sxs-lookup"><span data-stu-id="46922-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="46922-197">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="46922-197">Handle transient faults</span></span>

<span data-ttu-id="46922-198">Najbardziej typowe błędy, oczekiwanych może wystąpić, gdy zewnętrznych wywołań HTTP będzie przejściowy.</span><span class="sxs-lookup"><span data-stu-id="46922-198">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="46922-199">Wywołana metoda wygodne rozszerzenie `AddTransientHttpErrorPolicy` jest włączone, co pozwala zasad w celu zdefiniowania do obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="46922-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="46922-200">Zasady skonfigurowane przy użyciu tego uchwytu — metoda rozszerzenia `HttpRequestException`, odpowiedzi 5xx protokołu HTTP i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="46922-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="46922-201">`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="46922-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="46922-202">Rozszerzenie udostępnia `PolicyBuilder` obiektu skonfigurowane do obsługi reprezentujący możliwych błędów przejściowych błędów:</span><span class="sxs-lookup"><span data-stu-id="46922-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="46922-203">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowano zasad.</span><span class="sxs-lookup"><span data-stu-id="46922-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="46922-204">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem, 600 ms między próbami.</span><span class="sxs-lookup"><span data-stu-id="46922-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="46922-205">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="46922-205">Dynamically select policies</span></span>

<span data-ttu-id="46922-206">Metody rozszerzające dodatkowe istnieje, który może służyć do dodawania obsługi na podstawie Polly.</span><span class="sxs-lookup"><span data-stu-id="46922-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="46922-207">Jedno z rozszerzeń jest `AddPolicyHandler`, który ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="46922-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="46922-208">Jednego przeciążenia umożliwia wysłanie żądania do kontroli podczas definiowania zasad do zastosowania:</span><span class="sxs-lookup"><span data-stu-id="46922-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="46922-209">W poprzednim kodzie Jeśli wychodzące żądanie GET, 10-sekundowy limit jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="46922-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="46922-210">Inna metoda HTTP używany jest limit czasu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="46922-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="46922-211">Dodawanie wielu obsług Polly</span><span class="sxs-lookup"><span data-stu-id="46922-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="46922-212">Powszechne jest wprowadzanie zagnieździć Polly zasady, aby zapewnić ulepszone funkcje:</span><span class="sxs-lookup"><span data-stu-id="46922-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="46922-213">W powyższym przykładzie dwóch metod obsługi są dodawane.</span><span class="sxs-lookup"><span data-stu-id="46922-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="46922-214">Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania prób.</span><span class="sxs-lookup"><span data-stu-id="46922-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="46922-215">Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy.</span><span class="sxs-lookup"><span data-stu-id="46922-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="46922-216">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="46922-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="46922-217">Dalsze zewnętrznych żądania są blokowane przez 30 sekund, jeśli pięć nieudanych prób występują po kolei.</span><span class="sxs-lookup"><span data-stu-id="46922-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="46922-218">Wyłącznik zasady są stanowe.</span><span class="sxs-lookup"><span data-stu-id="46922-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="46922-219">Wszystkie połączenia za pośrednictwem tego klienta współużytkować ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="46922-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="46922-220">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="46922-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="46922-221">Podejście do zarządzania zasadami regularnie używane dotyczy je jeden raz zdefiniować i zarejestrować ich za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="46922-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="46922-222">Metody rozszerzenia jest pod warunkiem, co pozwala program obsługi ma zostać dodana za pomocą zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="46922-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="46922-223">W poprzednim kodzie PolicyRegistry jest dodawany do `ServiceCollection` i dwie zasady są zarejestrowane z nim.</span><span class="sxs-lookup"><span data-stu-id="46922-223">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="46922-224">Aby można było używać zasad z rejestru, `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="46922-224">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="46922-225">Więcej informacji o `IHttpClientFactory` Polly integracji można znaleźć na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="46922-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="46922-226">Zarządzanie HttpClient i okresu istnienia</span><span class="sxs-lookup"><span data-stu-id="46922-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="46922-227">Każdorazowo `CreateClient` jest wywoływana w `IHttpClientFactory`, nowe wystąpienie klasy `HttpClient` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="46922-227">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="46922-228">Będzie `HttpMessageHandler` na klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="46922-228">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="46922-229">`IHttpClientFactory` pula będzie `HttpMessageHandler` wystąpienia utworzone przez fabryki, aby zmniejszyć wykorzystanie zasobów.</span><span class="sxs-lookup"><span data-stu-id="46922-229">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="46922-230">A `HttpMessageHandler` wystąpienia może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli nie upłynął jego okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="46922-230">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="46922-231">Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną podstawowego połączenia HTTP; Tworzenie więcej obsługi niż jest to konieczne może spowodować opóźnienia w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="46922-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="46922-232">Niektóre procedury obsługi również nie zamykaj połączeń przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.</span><span class="sxs-lookup"><span data-stu-id="46922-232">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="46922-233">Domyślny okres istnienia obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="46922-233">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="46922-234">Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie.</span><span class="sxs-lookup"><span data-stu-id="46922-234">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="46922-235">Aby zastąpić go, należy wywołać `SetHandlerLifetime` na `IHttpClientBuilder` , jest zwracana podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="46922-235">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="46922-236">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="46922-236">Logging</span></span>

<span data-ttu-id="46922-237">Klientów utworzonych za pomocą `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="46922-237">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="46922-238">Należy włączyć poziom odpowiednie informacje w swojej konfiguracji rejestrowania, aby wyświetlić komunikaty dziennika domyślne.</span><span class="sxs-lookup"><span data-stu-id="46922-238">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="46922-239">Dodatkowe rejestrowanie, takich jak rejestrowanie nagłówków żądania, jest dostępny tylko na poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="46922-239">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="46922-240">Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="46922-240">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="46922-241">Klient o nazwie "MyNamedClient", na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="46922-241">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="46922-242">Komunikaty z sufiksem "LogicalHandler" odbywa się na zewnątrz potoku program obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="46922-242">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="46922-243">Na żądanie komunikaty są rejestrowane, zanim innych programów obsługi w potoku zostały przetworzone go.</span><span class="sxs-lookup"><span data-stu-id="46922-243">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="46922-244">W odpowiedzi komunikaty są rejestrowane po innych obsługi potoku otrzymane odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="46922-244">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="46922-245">Rejestrowanie występuje również wewnątrz potoku program obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="46922-245">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="46922-246">W przypadku w przykładzie "MyNamedClient" te komunikaty są rejestrowane względem kategoria dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="46922-246">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="46922-247">Dla żądania dzieje się po wykonaniu innych programów obsługi i od razu, zanim żądanie zostanie wysłane w sieci.</span><span class="sxs-lookup"><span data-stu-id="46922-247">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="46922-248">W odpowiedzi rejestrowanie obejmuje stan odpowiedzi przed przekazaniem za pośrednictwem potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="46922-248">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="46922-249">Włączanie rejestrowania na zewnątrz i wewnątrz potok umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="46922-249">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="46922-250">Może to obejmować zmiany nagłówki, żądań, na przykład lub kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="46922-250">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="46922-251">W tym nazwa klienta w kategorii dziennika umożliwia dziennika filtrowania dla określonych klientów o nazwie, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="46922-251">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="46922-252">Klasa HttpMessageHandler skonfigurować</span><span class="sxs-lookup"><span data-stu-id="46922-252">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="46922-253">Może być konieczne do kontrolowania konfiguracji wewnętrzny `HttpMessageHandler` używany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="46922-253">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="46922-254">`IHttpClientBuilder` Jest zwracany, podczas dodawania o nazwie lub wpisane klientów.</span><span class="sxs-lookup"><span data-stu-id="46922-254">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="46922-255">`ConfigurePrimaryHttpMessageHandler` — Metoda rozszerzenia może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="46922-255">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="46922-256">Delegat jest używany do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używane przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="46922-256">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
