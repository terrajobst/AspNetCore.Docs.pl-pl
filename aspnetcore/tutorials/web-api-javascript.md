---
title: 'Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript'
author: rick-anderson
description: Dowiedz się, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu języka JavaScript.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 2a19a7d16ca8b8f5d6ac8eb99ad919b89f1e368b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655254"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a><span data-ttu-id="63fa0-103">Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="63fa0-103">Tutorial: Call an ASP.NET Core web API with JavaScript</span></span>

<span data-ttu-id="63fa0-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63fa0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63fa0-105">W tym samouczku pokazano, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API).</span><span class="sxs-lookup"><span data-stu-id="63fa0-105">This tutorial shows how to call an ASP.NET Core web API with JavaScript, using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="63fa0-106">Aby uzyskać ASP.NET Core 2,2, zobacz wersja 2,2 [wywołania interfejsu API sieci Web przy użyciu języka JavaScript](xref:tutorials/first-web-api#call-the-web-api-with-javascript).</span><span class="sxs-lookup"><span data-stu-id="63fa0-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the web API with JavaScript](xref:tutorials/first-web-api#call-the-web-api-with-javascript).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="63fa0-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="63fa0-107">Prerequisites</span></span>

* <span data-ttu-id="63fa0-108">Kompletny [Samouczek: Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="63fa0-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="63fa0-109">Znajomość stylów CSS, HTML i JavaScript</span><span class="sxs-lookup"><span data-stu-id="63fa0-109">Familiarity with CSS, HTML, and JavaScript</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="63fa0-110">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="63fa0-110">Call the web API with JavaScript</span></span>

<span data-ttu-id="63fa0-111">W tej sekcji dodasz stronę HTML zawierającą formularze do tworzenia elementów do wykonania i zarządzania nimi.</span><span class="sxs-lookup"><span data-stu-id="63fa0-111">In this section, you'll add an HTML page containing forms for creating and managing to-do items.</span></span> <span data-ttu-id="63fa0-112">Programy obsługi zdarzeń są dołączone do elementów na stronie.</span><span class="sxs-lookup"><span data-stu-id="63fa0-112">Event handlers are attached to elements on the page.</span></span> <span data-ttu-id="63fa0-113">Programy obsługi zdarzeń powodują żądania HTTP do metod akcji internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="63fa0-113">The event handlers result in HTTP requests to the web API's action methods.</span></span> <span data-ttu-id="63fa0-114">Funkcja `fetch` interfejsu API pobierania inicjuje każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="63fa0-114">The Fetch API's `fetch` function initiates each HTTP request.</span></span>

<span data-ttu-id="63fa0-115">Funkcja `fetch` zwraca obiekt [obietnicy](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) , który zawiera odpowiedź HTTP reprezentowane jako obiekt `Response`.</span><span class="sxs-lookup"><span data-stu-id="63fa0-115">The `fetch` function returns a [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) object, which contains an HTTP response represented as a `Response` object.</span></span> <span data-ttu-id="63fa0-116">Typowym wzorcem jest wyodrębnienie treści odpowiedzi JSON przez wywołanie funkcji `json` na obiekcie `Response`.</span><span class="sxs-lookup"><span data-stu-id="63fa0-116">A common pattern is to extract the JSON response body by invoking the `json` function on the `Response` object.</span></span> <span data-ttu-id="63fa0-117">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="63fa0-117">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="63fa0-118">Najprostszym wywołaniem `fetch` akceptuje pojedynczy parametr reprezentujący trasę.</span><span class="sxs-lookup"><span data-stu-id="63fa0-118">The simplest `fetch` call accepts a single parameter representing the route.</span></span> <span data-ttu-id="63fa0-119">Drugi parametr, znany jako obiekt `init`, jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="63fa0-119">A second parameter, known as the `init` object, is optional.</span></span> <span data-ttu-id="63fa0-120">`init` służy do konfigurowania żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="63fa0-120">`init` is used to configure the HTTP request.</span></span>

1. <span data-ttu-id="63fa0-121">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="63fa0-121">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="63fa0-122">Następujący wyróżniony kod jest wymagany w metodzie `Configure` *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63fa0-122">The following highlighted code is needed in the `Configure` method of *Startup.cs*:</span></span>

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. <span data-ttu-id="63fa0-123">Utwórz folder *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="63fa0-123">Create a *wwwroot* folder in the project root.</span></span>

1. <span data-ttu-id="63fa0-124">Utwórz folder *js* w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="63fa0-124">Create a *js* folder inside of the *wwwroot* folder.</span></span>

1. <span data-ttu-id="63fa0-125">Dodaj plik HTML o nazwie *index. html* do folderu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="63fa0-125">Add an HTML file named *index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="63fa0-126">Zastąp zawartość *index. html* następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="63fa0-126">Replace the contents of *index.html* with the following markup:</span></span>

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. <span data-ttu-id="63fa0-127">Dodaj plik języka JavaScript o nazwie *site. js* do folderu *wwwroot/js* .</span><span class="sxs-lookup"><span data-stu-id="63fa0-127">Add a JavaScript file named *site.js* to the *wwwroot/js* folder.</span></span> <span data-ttu-id="63fa0-128">Zastąp zawartość środowiska *site. js* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="63fa0-128">Replace the contents of *site.js* with the following code:</span></span>

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

<span data-ttu-id="63fa0-129">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="63fa0-129">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

1. <span data-ttu-id="63fa0-130">Otwórz *Properties\launchSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="63fa0-130">Open *Properties\launchSettings.json*.</span></span>
1. <span data-ttu-id="63fa0-131">Usuń właściwość `launchUrl`, aby wymusić, że aplikacja zostanie otwarta w pliku *index. html*&mdash;domyślnym plikiem projektu.</span><span class="sxs-lookup"><span data-stu-id="63fa0-131">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="63fa0-132">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="63fa0-132">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="63fa0-133">Poniżej znajdują się wyjaśnienia żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="63fa0-133">Following are explanations of the web API requests.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="63fa0-134">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="63fa0-134">Get a list of to-do items</span></span>

<span data-ttu-id="63fa0-135">W poniższym kodzie żądanie HTTP GET jest wysyłane do trasy *API/TodoItems* :</span><span class="sxs-lookup"><span data-stu-id="63fa0-135">In the following code, an HTTP GET request is sent to the *api/TodoItems* route:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

<span data-ttu-id="63fa0-136">Gdy internetowy interfejs API zwraca kod stanu pomyślnego, wywoływana jest funkcja `_displayItems`.</span><span class="sxs-lookup"><span data-stu-id="63fa0-136">When the web API returns a successful status code, the `_displayItems` function is invoked.</span></span> <span data-ttu-id="63fa0-137">Każdy element do wykonania w parametrze tablicy akceptowanym przez `_displayItems` jest dodawany do tabeli za pomocą przycisków **Edytuj** i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="63fa0-137">Each to-do item in the array parameter accepted by `_displayItems` is added to a table with **Edit** and **Delete** buttons.</span></span> <span data-ttu-id="63fa0-138">Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="63fa0-138">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="add-a-to-do-item"></a><span data-ttu-id="63fa0-139">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="63fa0-139">Add a to-do item</span></span>

<span data-ttu-id="63fa0-140">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="63fa0-140">In the following code:</span></span>

* <span data-ttu-id="63fa0-141">Zmienna `item` jest zadeklarowana w celu skonstruowania literału obiektu reprezentacji elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="63fa0-141">An `item` variable is declared to construct an object literal representation of the to-do item.</span></span>
* <span data-ttu-id="63fa0-142">Żądanie pobrania jest konfigurowane z następującymi opcjami:</span><span class="sxs-lookup"><span data-stu-id="63fa0-142">A Fetch request is configured with the following options:</span></span>
  * <span data-ttu-id="63fa0-143">`method`&mdash;określa zlecenie akcji POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="63fa0-143">`method`&mdash;specifies the POST HTTP action verb.</span></span>
  * <span data-ttu-id="63fa0-144">`body`&mdash;określa reprezentację treści żądania w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="63fa0-144">`body`&mdash;specifies the JSON representation of the request body.</span></span> <span data-ttu-id="63fa0-145">KOD JSON jest tworzony przez przekazanie literału obiektu przechowywanego w `item` do funkcji [JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) .</span><span class="sxs-lookup"><span data-stu-id="63fa0-145">The JSON is produced by passing the object literal stored in `item` to the [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) function.</span></span>
  * <span data-ttu-id="63fa0-146">`headers`&mdash;Określa nagłówki żądania HTTP `Accept` i `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="63fa0-146">`headers`&mdash;specifies the `Accept` and `Content-Type` HTTP request headers.</span></span> <span data-ttu-id="63fa0-147">Oba nagłówki są ustawione na `application/json`, aby określić typ nośnika, który jest odbierany i wysyłany odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="63fa0-147">Both headers are set to `application/json` to specify the media type being received and sent, respectively.</span></span>
* <span data-ttu-id="63fa0-148">Żądanie HTTP POST jest wysyłane do trasy *API/TodoItems* .</span><span class="sxs-lookup"><span data-stu-id="63fa0-148">An HTTP POST request is sent to the *api/TodoItems* route.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

<span data-ttu-id="63fa0-149">Gdy internetowy interfejs API zwraca kod stanu pomyślnego, funkcja `getItems` jest wywoływana w celu zaktualizowania tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="63fa0-149">When the web API returns a successful status code, the `getItems` function is invoked to update the HTML table.</span></span> <span data-ttu-id="63fa0-150">Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="63fa0-150">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="update-a-to-do-item"></a><span data-ttu-id="63fa0-151">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="63fa0-151">Update a to-do item</span></span>

<span data-ttu-id="63fa0-152">Aktualizowanie elementu do wykonania jest podobne do dodawania jednego z nich; Istnieją jednak dwie znaczące różnice:</span><span class="sxs-lookup"><span data-stu-id="63fa0-152">Updating a to-do item is similar to adding one; however, there are two significant differences:</span></span>

* <span data-ttu-id="63fa0-153">Trasa ma sufiks z unikatowym identyfikatorem elementu do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="63fa0-153">The route is suffixed with the unique identifier of the item to update.</span></span> <span data-ttu-id="63fa0-154">Na przykład *API/TodoItems/1*.</span><span class="sxs-lookup"><span data-stu-id="63fa0-154">For example, *api/TodoItems/1*.</span></span>
* <span data-ttu-id="63fa0-155">Zlecenie akcji HTTP jest UMIESZCZAne w sposób wskazany przez opcję `method`.</span><span class="sxs-lookup"><span data-stu-id="63fa0-155">The HTTP action verb is PUT, as indicated by the `method` option.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="63fa0-156">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="63fa0-156">Delete a to-do item</span></span>

<span data-ttu-id="63fa0-157">Aby usunąć element do wykonania, ustaw opcję `method` żądania, aby `DELETE` i określić unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="63fa0-157">To delete a to-do item, set the request's `method` option to `DELETE` and specify the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

<span data-ttu-id="63fa0-158">Przejdź do następnego samouczka, aby dowiedzieć się, jak generować strony pomocy interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="63fa0-158">Advance to the next tutorial to learn how to generate web API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
