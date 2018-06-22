---
title: Funkcje platformy ASP.NET Core żądania
author: ardalis
description: Więcej informacji na temat szczegóły implementacji serwera sieci web związanych z żądaniami HTTP i odpowiedzi, które są zdefiniowane w interfejsach dla platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: fundamentals/request-features
ms.openlocfilehash: d0f3ae521d1f314dd04cb581d9a921da4719273d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279496"
---
# <a name="request-features-in-aspnet-core"></a>Funkcje platformy ASP.NET Core żądania

Przez [Steve Smith](https://ardalis.com/)

Szczegóły implementacji serwera sieci Web związanych z żądaniami HTTP i odpowiedzi są zdefiniowane w interfejsach. Te interfejsy są używane przez implementacje serwera i oprogramowanie pośredniczące do tworzenia i modyfikowania procesu hostingu aplikacji.

## <a name="feature-interfaces"></a>Funkcja interfejsy

Platformy ASP.NET Core definiuje kilka interfejsów funkcji HTTP w `Microsoft.AspNetCore.Http.Features` używane przez serwery do identyfikować funkcje, które obsługują. Następujące interfejsy funkcji obsługi żądań i zwracać odpowiedzi:

`IHttpRequestFeature` Definiuje strukturę żądania HTTP, łącznie z protokołem, ścieżka, ciąg zapytania, nagłówki i treść.

`IHttpResponseFeature` Definiuje strukturę odpowiedzi HTTP, w tym kod stanu, nagłówki i treść odpowiedzi.

`IHttpAuthenticationFeature` Definiuje pomocy technicznej do identyfikacji użytkowników na podstawie `ClaimsPrincipal` i określając program obsługi uwierzytelniania.

`IHttpUpgradeFeature` Określa obsługę [uaktualnień HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), umożliwiające klienta określić dodatkowe protokoły go chcesz użyć, jeśli serwer chce przełącznika protokołów.

`IHttpBufferingFeature` Definiuje metody wyłączenie buforowania żądań i/lub odpowiedzi.

`IHttpConnectionFeature` Definiuje właściwości dla adresów lokalnych i zdalnych i portów.

`IHttpRequestLifetimeFeature` Określa obsługę przerywania połączenia lub wykrywania, jeśli żądanie został zakończony przedwcześnie, takie jak przez rozłączeniu klienta.

`IHttpSendFileFeature` Definiuje metoda asynchronicznego przesyłania plików.

`IHttpWebSocketFeature` Definiuje interfejs API do obsługi gniazda sieci web.

`IHttpRequestIdentifierFeature` Dodaje właściwość, która może być używane do unikatowego identyfikowania żądań.

`ISessionFeature` Definiuje `ISessionFactory` i `ISession` obiektów abstrakcyjnych odpowiadających Obsługa sesji użytkownika.

`ITlsConnectionFeature` Definiuje interfejs API pobieranie certyfikatów klienta.

`ITlsTokenBindingFeature` Definiuje metody do pracy z parametrami tokenu powiązania protokołu TLS.

> [!NOTE]
> `ISessionFeature` nie jest funkcją serwera, ale jest implementowany przez `SessionMiddleware` (zobacz [stan aplikacji Zarządzanie](app-state.md)).

## <a name="feature-collections"></a>Funkcja kolekcji

`Features` Właściwość `HttpContext` udostępnia interfejs dla pobieranie i ustawianie dostępnych funkcji HTTP dla bieżącego żądania. Ponieważ funkcja kolekcji jest modyfikowalna, nawet w kontekście żądania, oprogramowanie pośredniczące może służyć do modyfikowania kolekcji i dodać obsługę dodatkowych funkcji.

## <a name="middleware-and-request-features"></a>Funkcje oprogramowania pośredniczącego i żądania

Serwery są odpowiedzialne za tworzenie kolekcji funkcji, oprogramowanie pośredniczące zarówno dodać do tej kolekcji i korzystać z funkcji z kolekcji. Na przykład `StaticFileMiddleware` uzyskuje dostęp do `IHttpSendFileFeature` funkcji. Jeśli funkcja istnieje, służy do wysyłania żądany plik statyczny z jego ścieżka fizyczna. W przeciwnym razie wolniej alternatywną metodą jest używany do wysyłania pliku. Jeśli jest dostępna, `IHttpSendFileFeature` umożliwia systemowi operacyjnemu Otwórz plik i wykonania kopii trybu jądra bezpośrednio do karty sieciowej.

Ponadto oprogramowanie pośredniczące można dodać do kolekcji funkcji ustanowionych przez serwer. Oprogramowanie pośredniczące, dzięki czemu oprogramowaniu pośredniczącym, aby rozszerzyć funkcjonalność serwera nawet można zastąpić istniejące funkcje. Funkcje dodane do kolekcji są dostępne natychmiast na inne oprogramowanie pośredniczące lub podstawowej aplikacji później w potoku żądania.

Połączenie implementacji niestandardowego serwera z określonym oprogramowaniem pośredniczącym ulepszenia, można skonstruować dokładne zestaw funkcji, które wymaga aplikacji. Dzięki temu Brak funkcji do dodania bez konieczności wprowadzania zmian na serwerze i zapewnia są widoczne tylko skraca funkcji, co ogranicza ataku powierzchni obszaru i poprawia wydajność.

## <a name="summary"></a>Podsumowanie

Funkcja interfejsy zdefiniuj określonych funkcji HTTP, które mogą obsługiwać danego żądania. Serwery zdefiniować kolekcje funkcji i początkowego zestawu funkcji obsługiwanych przez ten serwer, ale oprogramowanie pośredniczące może służyć do rozszerzają te funkcje.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Serwery](xref:fundamentals/servers/index)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Otwarty interfejs internetowy dla platformy .NET (OWIN)](xref:fundamentals/owin)
