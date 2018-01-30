---
title: "Funkcje platformy ASP.NET Core żądania"
author: ardalis
description: "Więcej informacji na temat szczegóły implementacji serwera sieci web związanych z żądaniami HTTP i odpowiedzi, które są zdefiniowane w interfejsach dla platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/request-features
ms.openlocfilehash: 11644dc646f2c0e749f0f64e80ee00ea8b671302
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="60e97-103">Funkcje platformy ASP.NET Core żądania</span><span class="sxs-lookup"><span data-stu-id="60e97-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="60e97-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="60e97-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="60e97-105">Szczegóły implementacji serwera sieci Web związanych z żądaniami HTTP i odpowiedzi są zdefiniowane w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="60e97-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="60e97-106">Te interfejsy są używane przez implementacje serwera i oprogramowanie pośredniczące do tworzenia i modyfikowania procesu hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60e97-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="60e97-107">Funkcja interfejsy</span><span class="sxs-lookup"><span data-stu-id="60e97-107">Feature interfaces</span></span>

<span data-ttu-id="60e97-108">Platformy ASP.NET Core definiuje kilka interfejsów funkcji HTTP w `Microsoft.AspNetCore.Http.Features` używane przez serwery do identyfikować funkcje, które obsługują.</span><span class="sxs-lookup"><span data-stu-id="60e97-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="60e97-109">Następujące interfejsy funkcji obsługi żądań i zwracać odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="60e97-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="60e97-110">`IHttpRequestFeature`Definiuje strukturę żądania HTTP, łącznie z protokołem, ścieżka, ciąg zapytania, nagłówki i treść.</span><span class="sxs-lookup"><span data-stu-id="60e97-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="60e97-111">`IHttpResponseFeature`Definiuje strukturę odpowiedzi HTTP, w tym kod stanu, nagłówki i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="60e97-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="60e97-112">`IHttpAuthenticationFeature`Definiuje pomocy technicznej do identyfikacji użytkowników na podstawie `ClaimsPrincipal` i określając program obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="60e97-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="60e97-113">`IHttpUpgradeFeature`Określa obsługę [uaktualnień HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), umożliwiające klienta określić dodatkowe protokoły go chcesz użyć, jeśli serwer chce przełącznika protokołów.</span><span class="sxs-lookup"><span data-stu-id="60e97-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="60e97-114">`IHttpBufferingFeature`Definiuje metody wyłączenie buforowania żądań i/lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="60e97-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="60e97-115">`IHttpConnectionFeature`Definiuje właściwości dla adresów lokalnych i zdalnych i portów.</span><span class="sxs-lookup"><span data-stu-id="60e97-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="60e97-116">`IHttpRequestLifetimeFeature`Określa obsługę przerywania połączenia lub wykrywania, jeśli żądanie został zakończony przedwcześnie, takie jak przez rozłączeniu klienta.</span><span class="sxs-lookup"><span data-stu-id="60e97-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="60e97-117">`IHttpSendFileFeature`Definiuje metoda asynchronicznego przesyłania plików.</span><span class="sxs-lookup"><span data-stu-id="60e97-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="60e97-118">`IHttpWebSocketFeature`Definiuje interfejs API do obsługi gniazda sieci web.</span><span class="sxs-lookup"><span data-stu-id="60e97-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="60e97-119">`IHttpRequestIdentifierFeature`Dodaje właściwość, która może być używane do unikatowego identyfikowania żądań.</span><span class="sxs-lookup"><span data-stu-id="60e97-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="60e97-120">`ISessionFeature`Definiuje `ISessionFactory` i `ISession` obiektów abstrakcyjnych odpowiadających Obsługa sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="60e97-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="60e97-121">`ITlsConnectionFeature`Definiuje interfejs API pobieranie certyfikatów klienta.</span><span class="sxs-lookup"><span data-stu-id="60e97-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="60e97-122">`ITlsTokenBindingFeature`Definiuje metody do pracy z parametrami tokenu powiązania protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="60e97-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="60e97-123">`ISessionFeature`nie jest funkcją serwera, ale jest implementowany przez `SessionMiddleware` (zobacz [stan aplikacji Zarządzanie](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="60e97-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="60e97-124">Funkcja kolekcji</span><span class="sxs-lookup"><span data-stu-id="60e97-124">Feature collections</span></span>

<span data-ttu-id="60e97-125">`Features` Właściwość `HttpContext` udostępnia interfejs dla pobieranie i ustawianie dostępnych funkcji HTTP dla bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="60e97-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="60e97-126">Ponieważ funkcja kolekcji jest modyfikowalna, nawet w kontekście żądania, oprogramowanie pośredniczące może służyć do modyfikowania kolekcji i dodać obsługę dodatkowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="60e97-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="60e97-127">Funkcje oprogramowania pośredniczącego i żądania</span><span class="sxs-lookup"><span data-stu-id="60e97-127">Middleware and request features</span></span>

<span data-ttu-id="60e97-128">Serwery są odpowiedzialne za tworzenie kolekcji funkcji, oprogramowanie pośredniczące zarówno dodać do tej kolekcji i korzystać z funkcji z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="60e97-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="60e97-129">Na przykład `StaticFileMiddleware` uzyskuje dostęp do `IHttpSendFileFeature` funkcji.</span><span class="sxs-lookup"><span data-stu-id="60e97-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="60e97-130">Jeśli funkcja istnieje, służy do wysyłania żądany plik statyczny z jego ścieżka fizyczna.</span><span class="sxs-lookup"><span data-stu-id="60e97-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="60e97-131">W przeciwnym razie wolniej alternatywną metodą jest używany do wysyłania pliku.</span><span class="sxs-lookup"><span data-stu-id="60e97-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="60e97-132">Jeśli jest dostępna, `IHttpSendFileFeature` umożliwia systemowi operacyjnemu Otwórz plik i wykonania kopii trybu jądra bezpośrednio do karty sieciowej.</span><span class="sxs-lookup"><span data-stu-id="60e97-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="60e97-133">Ponadto oprogramowanie pośredniczące można dodać do kolekcji funkcji ustanowionych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="60e97-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="60e97-134">Oprogramowanie pośredniczące, dzięki czemu oprogramowaniu pośredniczącym, aby rozszerzyć funkcjonalność serwera nawet można zastąpić istniejące funkcje.</span><span class="sxs-lookup"><span data-stu-id="60e97-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="60e97-135">Funkcje dodane do kolekcji są dostępne natychmiast na inne oprogramowanie pośredniczące lub podstawowej aplikacji później w potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="60e97-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="60e97-136">Połączenie implementacji niestandardowego serwera z określonym oprogramowaniem pośredniczącym ulepszenia, można skonstruować dokładne zestaw funkcji, które wymaga aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60e97-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="60e97-137">Dzięki temu Brak funkcji do dodania bez konieczności wprowadzania zmian na serwerze i zapewnia są widoczne tylko skraca funkcji, co ogranicza ataku powierzchni obszaru i poprawia wydajność.</span><span class="sxs-lookup"><span data-stu-id="60e97-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="60e97-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="60e97-138">Summary</span></span>

<span data-ttu-id="60e97-139">Funkcja interfejsy zdefiniuj określonych funkcji HTTP, które mogą obsługiwać danego żądania.</span><span class="sxs-lookup"><span data-stu-id="60e97-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="60e97-140">Serwery zdefiniować kolekcje funkcji i początkowego zestawu funkcji obsługiwanych przez ten serwer, ale oprogramowanie pośredniczące może służyć do rozszerzają te funkcje.</span><span class="sxs-lookup"><span data-stu-id="60e97-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60e97-141">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60e97-141">Additional resources</span></span>

* [<span data-ttu-id="60e97-142">Serwery</span><span class="sxs-lookup"><span data-stu-id="60e97-142">Servers</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="60e97-143">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="60e97-143">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="60e97-144">Otwarty interfejs internetowy dla platformy .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="60e97-144">Open Web Interface for .NET (OWIN)</span></span>](xref:fundamentals/owin)
