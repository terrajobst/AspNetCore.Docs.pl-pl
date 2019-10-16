---
title: 'Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript'
author: rick-anderson
description: Dowiedz się, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu języka JavaScript.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: bbe261307f6f68af002cb98cc4895888ade7f61c
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378700"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API).

## <a name="prerequisites"></a>Wymagania wstępne

* Kompletny [Samouczek: Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)
* Znajomość stylów CSS, HTML i JavaScript

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

W tej sekcji dodasz stronę HTML zawierającą formularze do tworzenia elementów do wykonania i zarządzania nimi. Programy obsługi zdarzeń są dołączone do elementów na stronie. Programy obsługi zdarzeń powodują żądania HTTP do metod akcji internetowego interfejsu API. Funkcja `fetch` interfejsu API pobierania inicjuje każde żądanie HTTP.

Funkcja `fetch` zwraca obiekt `Promise`, który zawiera odpowiedź HTTP reprezentowane jako obiekt `Response`. Typowym wzorcem jest wyodrębnienie treści odpowiedzi JSON przez wywołanie funkcji `json` w obiekcie `Response`. Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.

Najprostszym wywołaniem `fetch` akceptuje pojedynczy parametr reprezentujący trasę. Drugi parametr, znany jako obiekt `init`, jest opcjonalny. `init` służy do skonfigurowania żądania HTTP.

1. Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_). Następujący wyróżniony kod jest wymagany w metodzie `Configure` *Startup.cs*:

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. Utwórz katalog *wwwroot* w katalogu głównym projektu.

1. Dodaj plik HTML o nazwie *index. html* do katalogu *wwwroot* . Zastąp jego zawartość następującym znacznikiem:

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. Dodaj plik języka JavaScript o nazwie *site. js* do katalogu *wwwroot* . Zastąp jego zawartość następującym kodem:

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

Zmiana ustawień uruchamiania projektu ASP.NET Core może być wymagana do lokalnego przetestowania strony HTML:

1. Otwórz *Properties\launchSettings.JSON*.
1. Usuń właściwość `launchUrl`, aby wymusić, że aplikacja zostanie otwarta w pliku *index. html*@no__t — domyślny plik projektu 2the.

Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API. Poniżej znajdują się wyjaśnienia żądań interfejsu API sieci Web.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

W poniższym kodzie żądanie HTTP GET jest wysyłane do trasy *API/TodoItems* :

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

Gdy internetowy interfejs API zwraca kod stanu pomyślnego, wywoływana jest funkcja `_displayItems`. Każdy element do wykonania w parametrze tablicy akceptowanym przez `_displayItems` jest dodawany do tabeli za pomocą przycisków **Edytuj** i **Usuń** . Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

W poniższym kodzie:

* Zmienna `item` jest zadeklarowana w celu skonstruowania literału obiektu reprezentacji elementu do wykonania.
* Żądanie pobrania jest konfigurowane z następującymi opcjami:
  * `method` @ no__t-1specifies zlecenie akcji POST protokołu HTTP.
  * `body` @ no__t-1specifies reprezentację treści żądania w formacie JSON. KOD JSON jest tworzony przez przekazanie literału obiektu przechowywanego w `item` do funkcji [JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) .
  * `headers` @ no__t-1specifies nagłówki żądania HTTP `Accept` i `Content-Type`. Oba nagłówki są ustawione na `application/json`, aby określić typ nośnika, który jest odbierany i wysyłany odpowiednio.
* Żądanie HTTP POST jest wysyłane do trasy *API/TodoItems* .

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

Gdy internetowy interfejs API zwraca kod stanu pomyślnego, funkcja `getItems` jest wywoływana w celu zaktualizowania tabeli HTML. Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.

### <a name="update-a-to-do-item"></a>Aktualizowanie elementu do wykonania

Aktualizowanie elementu do wykonania jest podobne do dodawania jednego z nich; Istnieją jednak dwie znaczące różnice:

* Trasa ma sufiks z unikatowym identyfikatorem elementu do zaktualizowania. Na przykład *API/TodoItems/1*.
* Zlecenie akcji HTTP jest UMIESZCZAne w sposób wskazany przez opcję `method`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Aby usunąć element do wykonania, należy ustawić opcję `method` dla żądania na `DELETE` i określić unikatowy identyfikator elementu w adresie URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

Przejdź do następnego samouczka, aby dowiedzieć się, jak generować strony pomocy interfejsu API sieci Web:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
