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
# <a name="request-features-in-aspnet-core"></a>Funkcje w programie ASP.NET Core na żądanie

Przez [Steve Smith](https://ardalis.com/)

Szczegóły implementacji serwera sieci Web związanych z żądań HTTP i odpowiedzi są zdefiniowane w interfejsach. Te interfejsy są używane przez implementacje serwera i oprogramowania pośredniczącego do tworzenia i modyfikowania potoku hostingu aplikacji.

## <a name="feature-interfaces"></a>Interfejsy funkcji

Platforma ASP.NET Core definiuje kilka interfejsów funkcji HTTP w `Microsoft.AspNetCore.Http.Features` które są używane przez serwery do identyfikowania funkcje, które obsługują. Następujące interfejsy funkcji obsługi żądań i zwracania odpowiedzi:

`IHttpRequestFeature` Definiuje strukturę żądania HTTP, łącznie z protokołem, ścieżki, ciąg zapytania, nagłówki i treść.

`IHttpResponseFeature` Definiuje strukturę odpowiedź HTTP, w tym kod stanu, nagłówki i treść odpowiedzi.

`IHttpAuthenticationFeature` Definiuje obsługę identyfikowanie użytkowników na podstawie `ClaimsPrincipal` i określając program obsługi uwierzytelniania.

`IHttpUpgradeFeature` Definiuje obsługę [uaktualnień HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), które umożliwiają klienta określić dodatkowe protokoły go chcesz użyć, jeśli serwer zamierza Przełącz protokołów.

`IHttpBufferingFeature` Definiuje metody wyłączenie buforowania żądań i/lub odpowiedzi.

`IHttpConnectionFeature` Definiuje właściwości lokalne i zdalne adresy i porty.

`IHttpRequestLifetimeFeature` Definiuje obsługę przerywanie połączenia lub wykrywania, jeśli żądanie został zakończony przedwcześnie, takie jak przez rozłączenia klienta.

`IHttpSendFileFeature` Definiuje metodę wysyłania plików asynchronicznie.

`IHttpWebSocketFeature` Definiuje interfejs API do obsługi gniazda sieci web.

`IHttpRequestIdentifierFeature` Dodaje właściwość, którą można implementować do unikatowego identyfikowania żądań.

`ISessionFeature` Definiuje `ISessionFactory` i `ISession` elementy abstrakcji do obsługi sesji użytkownika.

`ITlsConnectionFeature` Definiuje interfejs API pobierania certyfikatów klienta.

`ITlsTokenBindingFeature` Definiuje metody do pracy z parametrami tworzenia powiązań tokenu protokołu TLS.

> [!NOTE]
> `ISessionFeature` nie jest funkcją serwera, ale jest implementowany przez `SessionMiddleware` (zobacz [stan aplikacji Zarządzanie](app-state.md)).

## <a name="feature-collections"></a>Funkcja kolekcji

`Features` Właściwość `HttpContext` udostępnia interfejs dla pobierania i ustawiania dostępne funkcje protokołu HTTP dla bieżącego żądania. Ponieważ kolekcji funkcji jest modyfikowalna, nawet w kontekście żądania, oprogramowanie pośredniczące może służyć do modyfikowania kolekcji i dodanie obsługi dodatkowych funkcji.

## <a name="middleware-and-request-features"></a>Funkcje oprogramowania pośredniczącego i żądania

Mimo że serwery odpowiedzialnych za tworzenie kolekcji funkcji, oprogramowanie pośredniczące mogą dodać do tej kolekcji i używanie funkcji z kolekcji. Na przykład `StaticFileMiddleware` uzyskuje dostęp do `IHttpSendFileFeature` funkcji. Jeśli funkcja znajduje się, służy do wysyłania żądanych plików statycznych z jego ścieżka fizyczna. W przeciwnym razie wolniejsze alternatywną metodą jest używany do wysłania pliku. Jeśli są dostępne, `IHttpSendFileFeature` umożliwia systemowi operacyjnemu, otwórz plik i wykonywać kopię trybu jądra bezpośrednich do karty sieciowej.

Ponadto oprogramowanie pośredniczące można dodać do kolekcji funkcji ustanowione przez serwer. Istniejące funkcje można zastąpić nawet przez oprogramowanie pośredniczące, dzięki czemu oprogramowania pośredniczącego, aby rozszerzyć funkcjonalność serwera. Funkcje dodane do kolekcji mogą natychmiast innym oprogramowaniu pośredniczącym lub podstawowej aplikacji w dalszej części Potok żądań.

Łącząc implementacji niestandardowego serwera i udoskonaleń dotyczących określonego oprogramowania pośredniczącego, można konstruować dokładny zestaw funkcji, których wymaga aplikacja. Dzięki temu brakujące funkcje do dodania bez konieczności zmiany na serwerze i zapewnia minimalnej ilości funkcje są widoczne, co ogranicza ataki powierzchni obszaru i poprawa wydajności.

## <a name="summary"></a>Podsumowanie

Funkcja interfejsy definiują określone funkcje HTTP, które mogą obsługiwać danego żądania. Serwery zdefiniować kolekcji funkcji i początkowego zestawu funkcji obsługiwanych przez ten serwer, ale oprogramowanie pośredniczące może służyć do zwiększenia tych funkcji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Serwery](xref:fundamentals/servers/index)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Otwarty interfejs internetowy dla platformy .NET (OWIN)](xref:fundamentals/owin)
