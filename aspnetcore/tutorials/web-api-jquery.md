---
title: 'Samouczek: Wywoływanie interfejsu API sieci Web za pomocą platformy jQuery przy użyciu ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak wywołać interfejs API sieci Web ASP.NET Core za pomocą platformy jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: a319e4b4ce09e9b09afeaff065d5740276deb115
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022561"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Samouczek: Wywoływanie ASP.NET Core internetowego interfejsu API za pomocą platformy jQuery

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak wywołać interfejs API sieci Web ASP.NET Core za pomocą platformy jQuery

::: moniker range="< aspnetcore-3.0"

Aby uzyskać ASP.NET Core 2,2, zobacz wersja 2,2 [wywołania interfejsu API sieci Web za pomocą platformy jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Wymagania wstępne

* Pełny [samouczek: Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)
* Znajomość stylów CSS, HTML, JavaScript i jQuery

## <a name="call-the-api-with-jquery"></a>Wywoływanie interfejsu API przy użyciu jQuery

W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api. jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.

Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Tworzenie *wwwroot* folder w katalogu projektu.

Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:

* Otwórz *Properties\launchSettings.json*.
* Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.

Istnieje kilka sposobów uzyskania biblioteki jQuery. W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.

Ten przykład wywołuje wszystkie metody CRUD interfejsu API. Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania. `success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem. Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania. `accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych. Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Zaktualizuj element do wykonania

Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego. `url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

Przejdź do następnego samouczka, aby dowiedzieć się, jak można wygenerować stron pomocy interfejsu API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end
