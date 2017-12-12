---
uid: web-api/overview/advanced/http-cookies
title: "Pliki cookie protokołu HTTP w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a>Pliki cookie protokołu HTTP w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym temacie opisano sposób wysyłania i odbierania plików cookie protokołu HTTP w interfejsie API sieci Web.

## <a name="background-on-http-cookies"></a>Podstawowe informacje dotyczące plików cookie protokołu HTTP

Ta sekcja zawiera krótki przegląd sposobu implementacji plików cookie na poziomie protokołu HTTP. Aby uzyskać więcej informacji, zapoznaj się [RFC 6265](http://tools.ietf.org/html/rfc6265).

Plik cookie jest element danych, który serwer wysyła w odpowiedzi HTTP. Klient (opcjonalnie) są przechowywane w pliku cookie i zwraca go na subsequet żądań. Dzięki temu klient i serwer udostępniania stanu. Aby ustawić plik cookie, na serwerze znajduje się nagłówka Set-Cookie odpowiedzi. Format pliku cookie jest pary nazwa wartość, przy użyciu atrybutów opcjonalnych. Na przykład:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Oto przykład z atrybutami:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Aby uzyskać informacje pliku cookie do serwera zawiera klienta nagłówek Cookie nowszych żądań.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Odpowiedź HTTP może obejmować wiele nagłówków Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Klient zwraca wiele plików cookie przy użyciu pojedynczego nagłówka pliku Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Zakresu i czasu trwania pliku cookie są kontrolowane przez następujące atrybuty nagłówka Set-Cookie:

- **Domeny**: informuje klienta, która domena powinien zostać wyświetlony plik cookie. Na przykład jeśli domena "example.com", klient zwraca plik cookie co poddomeny example.com. Jeśli nie zostanie określony, domena jest serwera pochodzenia.
- **Ścieżka**: ogranicza pliku cookie do określonej ścieżki w ramach domeny. Jeśli nie zostanie określony, używana jest ścieżka identyfikatora URI żądania.
- **Wygasa**: ustawia datę wygaśnięcia pliku cookie. Klient usuwa plik cookie po jego wygaśnięciu.
- **Maksymalny wiek**: Ustawia maksymalny wiek dla pliku cookie. Klient usuwa plik cookie, gdy osiągnie maksymalny wiek.

Jeśli oba `Expires` i `Max-Age` są ustawione, `Max-Age` ma pierwszeństwo. Jeśli nie jest ustawiona, klient usuwa plik cookie po zakończeniu bieżącej sesji. (Dokładne znaczenie "session" jest określony przez agenta użytkownika).

Należy jednak pamiętać, że klienci zignorować plików cookie. Na przykład użytkownik może wyłączyć pliki cookie dotyczące prywatności powodów. Klientów można usunąć pliki cookie, zanim wygaśnie lub Ogranicz liczbę przechowywanych plików cookie. Ze względu na zasady ochrony prywatności klienci często odrzucić pliki cookie "third party", gdzie domena nie odpowiada serwera pochodzenia. Krótko mówiąc serwer nie należy polegać na odzyskać pliki cookie, które ustawia.

## <a name="cookies-in-web-api"></a>Pliki cookie w składniku Web API

Aby dodać plik cookie do odpowiedzi HTTP, Utwórz **CookieHeaderValue** wystąpienia, który reprezentuje plik cookie. Następnie wywołaj **AddCookies** metodę rozszerzenia, która jest zdefiniowana w **System.Net.Http. HttpResponseHeadersExtensions** klasy, aby dodać plik cookie.

Na przykład następujący kod dodaje plik cookie w akcji kontrolera:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Zwróć uwagę, że **AddCookies** pobiera tablicę **CookieHeaderValue** wystąpień.

Aby wyodrębnić pliki cookie z żądania klienta, należy wywołać **GetCookies** metody:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** zawiera kolekcję **CookieState** wystąpień. Każdy **CookieState** reprezentuje jednego pliku cookie. Użyj metody indeksatora, aby pobrać **CookieState** według nazwy, jak pokazano.

## <a name="structured-cookie-data"></a>Dane strukturalne pliku Cookie

Wiele przeglądarek ograniczyć liczbę plików cookie, będą one przechowywane &#8212; zarówno całkowitą liczbę i numer w każdej domenie. W związku z tym może być przydatne do umieszczanie danych strukturalnych w pojedynczym pliku cookie, zamiast ustawienie wiele plików cookie.

> [!NOTE]
> RFC 6265 nie definiuje struktury danych pliku cookie.


Przy użyciu **CookieHeaderValue** klasy, można przekazać listę par nazwa wartość dla danych pliku cookie. Te pary nazwa wartość są zakodowane jako dane formularza kodowane za pomocą adresu URL nagłówka Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Poprzedni kod generuje następujący nagłówek Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** klasa dostarcza metody indeksatora do odczytu z pliku cookie w komunikacie żądania podrzędnego wartości:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Przykład: Ustawiania i pobierania plików cookie programu obsługi wiadomości

W poprzednich przykładach pokazano, jak używać plików cookie z wewnątrz kontrolera interfejsu API sieci Web. Inną opcją jest użycie [programy obsługi komunikatów](http-message-handlers.md). Programy obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolerów. Program obsługi komunikatów można odczytywania plików cookie z żądania, zanim żądanie dotrze kontrolera lub dodać pliki cookie do odpowiedzi po kontrolera generuje odpowiedzi.

![](http-cookies/_static/image2.png)

Poniższy kod przedstawia program obsługi komunikatów do tworzenia identyfikatorów sesji. Identyfikator sesji jest przechowywany w pliku cookie. Program obsługi sprawdza, czy żądanie dla pliku cookie sesji. Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje identyfikator nowej sesji W obu przypadkach program obsługi przechowuje identyfikator sesji w **HttpRequestMessage.Properties** zbioru właściwości. Dodaje również pliku cookie sesji do odpowiedzi HTTP.

Ta implementacja nie sprawdza, czy identyfikator sesji z klienta faktycznie został wystawiony przez serwer. Nie jest używany jako formy uwierzytelniania! Punkt przykładu jest pokazanie zarządzania pliku cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Kontroler może pobrać Identyfikatora sesji z **HttpRequestMessage.Properties** zbioru właściwości.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
