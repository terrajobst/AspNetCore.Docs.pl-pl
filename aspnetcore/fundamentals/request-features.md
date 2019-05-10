---
title: Funkcje w programie ASP.NET Core na żądanie
author: ardalis
description: Więcej informacji na temat szczegółów implementacji serwera sieci web związanych z żądań HTTP i odpowiedzi, które są zdefiniowane w interfejsach dla platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: fundamentals/request-features
ms.openlocfilehash: d0f3ae521d1f314dd04cb581d9a921da4719273d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087035"
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="ccf0a-103">Funkcje w programie ASP.NET Core na żądanie</span><span class="sxs-lookup"><span data-stu-id="ccf0a-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="ccf0a-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ccf0a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ccf0a-105">Szczegóły implementacji serwera sieci Web związanych z żądań HTTP i odpowiedzi są zdefiniowane w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="ccf0a-106">Te interfejsy są używane przez implementacje serwera i oprogramowania pośredniczącego do tworzenia i modyfikowania potoku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="ccf0a-107">Interfejsy funkcji</span><span class="sxs-lookup"><span data-stu-id="ccf0a-107">Feature interfaces</span></span>

<span data-ttu-id="ccf0a-108">Platforma ASP.NET Core definiuje kilka interfejsów funkcji HTTP w `Microsoft.AspNetCore.Http.Features` które są używane przez serwery do identyfikowania funkcje, które obsługują.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="ccf0a-109">Następujące interfejsy funkcji obsługi żądań i zwracania odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="ccf0a-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="ccf0a-110">`IHttpRequestFeature` Definiuje strukturę żądania HTTP, łącznie z protokołem, ścieżki, ciąg zapytania, nagłówki i treść.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="ccf0a-111">`IHttpResponseFeature` Definiuje strukturę odpowiedź HTTP, w tym kod stanu, nagłówki i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="ccf0a-112">`IHttpAuthenticationFeature` Definiuje obsługę identyfikowanie użytkowników na podstawie `ClaimsPrincipal` i określając program obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="ccf0a-113">`IHttpUpgradeFeature` Definiuje obsługę [uaktualnień HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), które umożliwiają klienta określić dodatkowe protokoły go chcesz użyć, jeśli serwer zamierza Przełącz protokołów.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="ccf0a-114">`IHttpBufferingFeature` Definiuje metody wyłączenie buforowania żądań i/lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="ccf0a-115">`IHttpConnectionFeature` Definiuje właściwości lokalne i zdalne adresy i porty.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="ccf0a-116">`IHttpRequestLifetimeFeature` Definiuje obsługę przerywanie połączenia lub wykrywania, jeśli żądanie został zakończony przedwcześnie, takie jak przez rozłączenia klienta.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="ccf0a-117">`IHttpSendFileFeature` Definiuje metodę wysyłania plików asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="ccf0a-118">`IHttpWebSocketFeature` Definiuje interfejs API do obsługi gniazda sieci web.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="ccf0a-119">`IHttpRequestIdentifierFeature` Dodaje właściwość, którą można implementować do unikatowego identyfikowania żądań.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="ccf0a-120">`ISessionFeature` Definiuje `ISessionFactory` i `ISession` elementy abstrakcji do obsługi sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="ccf0a-121">`ITlsConnectionFeature` Definiuje interfejs API pobierania certyfikatów klienta.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="ccf0a-122">`ITlsTokenBindingFeature` Definiuje metody do pracy z parametrami tworzenia powiązań tokenu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="ccf0a-123">`ISessionFeature` nie jest funkcją serwera, ale jest implementowany przez `SessionMiddleware` (zobacz [stan aplikacji Zarządzanie](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="ccf0a-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="ccf0a-124">Funkcja kolekcji</span><span class="sxs-lookup"><span data-stu-id="ccf0a-124">Feature collections</span></span>

<span data-ttu-id="ccf0a-125">`Features` Właściwość `HttpContext` udostępnia interfejs dla pobierania i ustawiania dostępne funkcje protokołu HTTP dla bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="ccf0a-126">Ponieważ kolekcji funkcji jest modyfikowalna, nawet w kontekście żądania, oprogramowanie pośredniczące może służyć do modyfikowania kolekcji i dodanie obsługi dodatkowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="ccf0a-127">Funkcje oprogramowania pośredniczącego i żądania</span><span class="sxs-lookup"><span data-stu-id="ccf0a-127">Middleware and request features</span></span>

<span data-ttu-id="ccf0a-128">Mimo że serwery odpowiedzialnych za tworzenie kolekcji funkcji, oprogramowanie pośredniczące mogą dodać do tej kolekcji i używanie funkcji z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="ccf0a-129">Na przykład `StaticFileMiddleware` uzyskuje dostęp do `IHttpSendFileFeature` funkcji.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="ccf0a-130">Jeśli funkcja znajduje się, służy do wysyłania żądanych plików statycznych z jego ścieżka fizyczna.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="ccf0a-131">W przeciwnym razie wolniejsze alternatywną metodą jest używany do wysłania pliku.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="ccf0a-132">Jeśli są dostępne, `IHttpSendFileFeature` umożliwia systemowi operacyjnemu, otwórz plik i wykonywać kopię trybu jądra bezpośrednich do karty sieciowej.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="ccf0a-133">Ponadto oprogramowanie pośredniczące można dodać do kolekcji funkcji ustanowione przez serwer.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="ccf0a-134">Istniejące funkcje można zastąpić nawet przez oprogramowanie pośredniczące, dzięki czemu oprogramowania pośredniczącego, aby rozszerzyć funkcjonalność serwera.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="ccf0a-135">Funkcje dodane do kolekcji mogą natychmiast innym oprogramowaniu pośredniczącym lub podstawowej aplikacji w dalszej części Potok żądań.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="ccf0a-136">Łącząc implementacji niestandardowego serwera i udoskonaleń dotyczących określonego oprogramowania pośredniczącego, można konstruować dokładny zestaw funkcji, których wymaga aplikacja.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="ccf0a-137">Dzięki temu brakujące funkcje do dodania bez konieczności zmiany na serwerze i zapewnia minimalnej ilości funkcje są widoczne, co ogranicza ataki powierzchni obszaru i poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="ccf0a-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ccf0a-138">Summary</span></span>

<span data-ttu-id="ccf0a-139">Funkcja interfejsy definiują określone funkcje HTTP, które mogą obsługiwać danego żądania.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="ccf0a-140">Serwery zdefiniować kolekcji funkcji i początkowego zestawu funkcji obsługiwanych przez ten serwer, ale oprogramowanie pośredniczące może służyć do zwiększenia tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="ccf0a-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccf0a-141">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ccf0a-141">Additional resources</span></span>

* [<span data-ttu-id="ccf0a-142">Serwery</span><span class="sxs-lookup"><span data-stu-id="ccf0a-142">Servers</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="ccf0a-143">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="ccf0a-143">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ccf0a-144">Otwarty interfejs internetowy dla platformy .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="ccf0a-144">Open Web Interface for .NET (OWIN)</span></span>](xref:fundamentals/owin)
