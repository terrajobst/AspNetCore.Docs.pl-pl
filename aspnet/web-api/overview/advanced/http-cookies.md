---
uid: web-api/overview/advanced/http-cookies
title: Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368408"
---
<a name="http-cookies-in-aspnet-web-api"></a>Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym temacie opisano, jak wysyłać i odbierać pliki cookie protokołu HTTP w interfejsie API sieci Web.

## <a name="background-on-http-cookies"></a>Podstawowe informacje dotyczące plików cookie protokołu HTTP

Ta sekcja zawiera krótkie omówienie sposobu implementacji plików cookie na poziomie protokołu HTTP. Aby uzyskać szczegółowe informacje, zapoznaj się z [RFC 6265](http://tools.ietf.org/html/rfc6265).

Plik cookie jest element danych, który serwer wysyła w odpowiedzi HTTP. Klient (opcjonalnie) są przechowywane pliki cookie i zwraca go subsequet żądań. Dzięki temu klientowi i serwerowi udostępnianie stanu. Aby ustawić plik cookie, na serwerze znajduje się nagłówka Set-Cookie odpowiedzi. Format pliku cookie jest pary nazwa wartość, za pomocą opcjonalnych atrybutów. Na przykład:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Oto przykład z atrybutami:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Aby przywrócić serwer pliku cookie, klient dołącza nagłówek Cookie w nowszych żądań.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Odpowiedź HTTP może obejmować wiele nagłówków Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Wiele plików cookie, za pomocą pojedynczego nagłówek Cookie zwracane przez klienta.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Zakresu i czasu trwania w pliku cookie są kontrolowane przez następujące atrybuty do nagłówka Set-Cookie:

- **Domeny**: informuje klienta, które domeny powinien zostać wyświetlony plik cookie. Na przykład jeśli domena jest "example.com", klient zwraca plik cookie co poddomeny example.com. Jeśli nie zostanie określony, domena jest serwer pochodzenia.
- **Ścieżka**: ogranicza pliku cookie do określonej ścieżki w domenie. Jeśli nie zostanie określony, używany jest ścieżka identyfikatora URI żądania.
- **Wygasa**: ustawia datę wygaśnięcia pliku cookie. Klient usuwa plik cookie po jego wygaśnięciu.
- **MAX-Age**: Ustawia maksymalny wiek pliku cookie. Klient usuwa plik cookie, gdy osiągnie maksymalny wiek.

Jeśli oba `Expires` i `Max-Age` są ustawione, `Max-Age` ma pierwszeństwo. Jeśli nie jest ustawiona, klient usuwa plik cookie po zakończeniu bieżącej sesji. (Dokładnie znaczenie "sesja" jest określany przez agenta użytkownika).

Należy jednak pamiętać, że klienci zignorować plików cookie. Na przykład użytkownik może wyłączyć plików cookie w celu zachowania prywatności. Klienci mogą usunąć pliki cookie, zanim wygaśnie lub ograniczyć liczbę tych plików cookie przechowywane. Ze względu na zasady zachowania poufności klienci często odrzucić pliki cookie "third party", gdzie domena jest niezgodny serwer pochodzenia. Krótko mówiąc serwer nie należy polegać na pliki cookie, które ustawia powrót.

## <a name="cookies-in-web-api"></a>Pliki cookie w interfejsie Web API

Aby dodać plik cookie do odpowiedzi HTTP, Utwórz **CookieHeaderValue** wystąpienia, która reprezentuje plik cookie. Następnie wywołaj **AddCookies** metodę rozszerzenia, która jest zdefiniowana w **System.Net.Http. HttpResponseHeadersExtensions** klasy, aby dodać plik cookie.

Na przykład poniższy kod dodaje plik cookie w ramach akcji kontrolera:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Należy zauważyć, że **AddCookies** pobiera tablicę **CookieHeaderValue** wystąpień.

Aby wyodrębnić pliki cookie z żądania klienta, należy wywołać **GetCookies** metody:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** zawiera zbiór **CookieState** wystąpień. Każdy **CookieState** reprezentuje jednego pliku cookie. Pobieranie przy użyciu metody indeksatora **CookieState** według nazwy, jak pokazano.

## <a name="structured-cookie-data"></a>Dane ze strukturą pliku Cookie

Wiele przeglądarek limit liczby plików cookie, będą one przechowywane&#8212;łączną liczbą i numer w każdej domenie. W związku z tym może być przydatne umieścić ustrukturyzowanych danych w pojedynczym pliku cookie, zamiast ustawiać wiele plików cookie.

> [!NOTE]
> RFC 6265 nie definiuje strukturę dane pliku cookie.


Za pomocą **CookieHeaderValue** klasy, można przekazać listę par nazwa wartość, aby uzyskać dane pliku cookie. Te pary nazwa wartość są zakodowane jako dane zakodowane jako adres URL formularza do nagłówka Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Powyższy kod generuje następujący nagłówek Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** klasa udostępnia metodę indeksatora odczytać podrzędnych wartości z pliku cookie w komunikacie żądania:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Przykład: Ustawianie i pobieranie plików cookie programu obsługi wiadomości

W poprzednich przykładach pokazano, jak używać plików cookie z wewnątrz kontrolera interfejsu API sieci Web. Innym rozwiązaniem jest użycie [programy obsługi komunikatów](http-message-handlers.md). Programy obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolerów. Program obsługi komunikatów można odczytać plików cookie z żądania, zanim żądanie osiąga kontrolera lub dodania plików cookie do odpowiedzi, gdy kontroler wygeneruje odpowiedzi.

![](http-cookies/_static/image2.png)

Poniższy kod przedstawia obsługi wiadomości, do tworzenia identyfikatorów sesji. Identyfikator sesji jest przechowywany w pliku cookie. Program obsługi sprawdza, czy żądanie dla pliku cookie sesji. Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje identyfikator nowej sesji. W obu przypadkach procedura obsługi przechowuje identyfikator sesji w **HttpRequestMessage.Properties** zbioru właściwości. Dodaje także plik cookie sesji do odpowiedzi HTTP.

Ta implementacja nie weryfikuje, czy identyfikator sesji z klienta rzeczywiście został wystawiony przez serwer. Nie jest używany jako formę uwierzytelniania! Punkt przykładu jest pokazanie zarządzania pliku cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Kontroler można uzyskać Identyfikatora sesji z **HttpRequestMessage.Properties** zbioru właściwości.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
