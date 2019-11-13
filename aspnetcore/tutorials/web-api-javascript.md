---
title: 'Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript'
author: rick-anderson
description: Dowiedz się, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu języka JavaScript.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 0070816149d64fc1d71d453eb0f135050c78597a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2019
ms.locfileid: "72378700"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a><span data-ttu-id="70e47-103">Samouczek: wywoływanie interfejsu API sieci Web ASP.NET Core przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="70e47-103">Tutorial: Call an ASP.NET Core web API with JavaScript</span></span>

<span data-ttu-id="70e47-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="70e47-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="70e47-105">W tym samouczku pokazano, jak wywołać interfejs API sieci Web ASP.NET Core przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API).</span><span class="sxs-lookup"><span data-stu-id="70e47-105">This tutorial shows how to call an ASP.NET Core web API with JavaScript, using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="70e47-106">Aby uzyskać ASP.NET Core 2,2, zobacz wersja 2,2 [wywołania interfejsu API sieci Web przy użyciu języka JavaScript](xref:tutorials/first-web-api#call-the-web-api-with-javascript).</span><span class="sxs-lookup"><span data-stu-id="70e47-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the web API with JavaScript](xref:tutorials/first-web-api#call-the-web-api-with-javascript).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="70e47-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="70e47-107">Prerequisites</span></span>

* <span data-ttu-id="70e47-108">Kompletny [Samouczek: Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="70e47-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="70e47-109">Znajomość stylów CSS, HTML i JavaScript</span><span class="sxs-lookup"><span data-stu-id="70e47-109">Familiarity with CSS, HTML, and JavaScript</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="70e47-110">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="70e47-110">Call the web API with JavaScript</span></span>

<span data-ttu-id="70e47-111">W tej sekcji dodasz stronę HTML zawierającą formularze do tworzenia elementów do wykonania i zarządzania nimi.</span><span class="sxs-lookup"><span data-stu-id="70e47-111">In this section, you'll add an HTML page containing forms for creating and managing to-do items.</span></span> <span data-ttu-id="70e47-112">Programy obsługi zdarzeń są dołączone do elementów na stronie.</span><span class="sxs-lookup"><span data-stu-id="70e47-112">Event handlers are attached to elements on the page.</span></span> <span data-ttu-id="70e47-113">Programy obsługi zdarzeń powodują żądania HTTP do metod akcji internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70e47-113">The event handlers result in HTTP requests to the web API's action methods.</span></span> <span data-ttu-id="70e47-114">Funkcja `fetch` interfejsu API pobierania inicjuje każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="70e47-114">The Fetch API's `fetch` function initiates each HTTP request.</span></span>

<span data-ttu-id="70e47-115">Funkcja `fetch` zwraca obiekt `Promise`, który zawiera odpowiedź HTTP reprezentowane jako obiekt `Response`.</span><span class="sxs-lookup"><span data-stu-id="70e47-115">The `fetch` function returns a `Promise` object, which contains an HTTP response represented as a `Response` object.</span></span> <span data-ttu-id="70e47-116">Typowym wzorcem jest wyodrębnienie treści odpowiedzi JSON przez wywołanie funkcji `json` w obiekcie `Response`.</span><span class="sxs-lookup"><span data-stu-id="70e47-116">A common pattern is to extract the JSON response body by invoking the `json` function on the `Response` object.</span></span> <span data-ttu-id="70e47-117">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70e47-117">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="70e47-118">Najprostszym wywołaniem `fetch` akceptuje pojedynczy parametr reprezentujący trasę.</span><span class="sxs-lookup"><span data-stu-id="70e47-118">The simplest `fetch` call accepts a single parameter representing the route.</span></span> <span data-ttu-id="70e47-119">Drugi parametr, znany jako obiekt `init`, jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="70e47-119">A second parameter, known as the `init` object, is optional.</span></span> <span data-ttu-id="70e47-120">`init` służy do skonfigurowania żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="70e47-120">`init` is used to configure the HTTP request.</span></span>

1. <span data-ttu-id="70e47-121">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="70e47-121">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="70e47-122">Następujący wyróżniony kod jest wymagany w metodzie `Configure` *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="70e47-122">The following highlighted code is needed in the `Configure` method of *Startup.cs*:</span></span>

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. <span data-ttu-id="70e47-123">Utwórz katalog *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="70e47-123">Create a *wwwroot* directory in the project root.</span></span>

1. <span data-ttu-id="70e47-124">Dodaj plik HTML o nazwie *index. html* do katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="70e47-124">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="70e47-125">Zastąp jego zawartość następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="70e47-125">Replace its contents with the following markup:</span></span>

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. <span data-ttu-id="70e47-126">Dodaj plik języka JavaScript o nazwie *site. js* do katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="70e47-126">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="70e47-127">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="70e47-127">Replace its contents with the following code:</span></span>

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

<span data-ttu-id="70e47-128">Zmiana ustawień uruchamiania projektu ASP.NET Core może być wymagana do lokalnego przetestowania strony HTML:</span><span class="sxs-lookup"><span data-stu-id="70e47-128">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

1. <span data-ttu-id="70e47-129">Otwórz *Properties\launchSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="70e47-129">Open *Properties\launchSettings.json*.</span></span>
1. <span data-ttu-id="70e47-130">Usuń właściwość `launchUrl`, aby wymusić, że aplikacja zostanie otwarta w pliku *index. html* &mdash;the domyślny plik projektu.</span><span class="sxs-lookup"><span data-stu-id="70e47-130">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="70e47-131">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70e47-131">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="70e47-132">Poniżej znajdują się wyjaśnienia żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="70e47-132">Following are explanations of the web API requests.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="70e47-133">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="70e47-133">Get a list of to-do items</span></span>

<span data-ttu-id="70e47-134">W poniższym kodzie żądanie HTTP GET jest wysyłane do trasy *API/TodoItems* :</span><span class="sxs-lookup"><span data-stu-id="70e47-134">In the following code, an HTTP GET request is sent to the *api/TodoItems* route:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

<span data-ttu-id="70e47-135">Gdy internetowy interfejs API zwraca kod stanu pomyślnego, wywoływana jest funkcja `_displayItems`.</span><span class="sxs-lookup"><span data-stu-id="70e47-135">When the web API returns a successful status code, the `_displayItems` function is invoked.</span></span> <span data-ttu-id="70e47-136">Każdy element do wykonania w parametrze tablicy akceptowanym przez `_displayItems` jest dodawany do tabeli za pomocą przycisków **Edytuj** i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="70e47-136">Each to-do item in the array parameter accepted by `_displayItems` is added to a table with **Edit** and **Delete** buttons.</span></span> <span data-ttu-id="70e47-137">Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="70e47-137">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="add-a-to-do-item"></a><span data-ttu-id="70e47-138">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="70e47-138">Add a to-do item</span></span>

<span data-ttu-id="70e47-139">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="70e47-139">In the following code:</span></span>

* <span data-ttu-id="70e47-140">Zmienna `item` jest zadeklarowana w celu skonstruowania literału obiektu reprezentacji elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="70e47-140">An `item` variable is declared to construct an object literal representation of the to-do item.</span></span>
* <span data-ttu-id="70e47-141">Żądanie pobrania jest konfigurowane z następującymi opcjami:</span><span class="sxs-lookup"><span data-stu-id="70e47-141">A Fetch request is configured with the following options:</span></span>
    * <span data-ttu-id="70e47-142">`method`&mdash;określa zlecenie akcji POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="70e47-142">`method`&mdash;specifies the POST HTTP action verb.</span></span>
    * <span data-ttu-id="70e47-143">`body`&mdash;określa reprezentację treści żądania w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="70e47-143">`body`&mdash;specifies the JSON representation of the request body.</span></span> <span data-ttu-id="70e47-144">KOD JSON jest tworzony przez przekazanie literału obiektu przechowywanego w `item` do funkcji [JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) .</span><span class="sxs-lookup"><span data-stu-id="70e47-144">The JSON is produced by passing the object literal stored in `item` to the [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) function.</span></span>
    * <span data-ttu-id="70e47-145">`headers`&mdash;Określa nagłówki żądania HTTP `Accept` i `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="70e47-145">`headers`&mdash;specifies the `Accept` and `Content-Type` HTTP request headers.</span></span> <span data-ttu-id="70e47-146">Oba nagłówki są ustawione na `application/json`, aby określić typ nośnika, który jest odbierany i wysyłany odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="70e47-146">Both headers are set to `application/json` to specify the media type being received and sent, respectively.</span></span>
* <span data-ttu-id="70e47-147">Żądanie HTTP POST jest wysyłane do trasy *API/TodoItems* .</span><span class="sxs-lookup"><span data-stu-id="70e47-147">An HTTP POST request is sent to the *api/TodoItems* route.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

<span data-ttu-id="70e47-148">Gdy internetowy interfejs API zwraca kod stanu pomyślnego, funkcja `getItems` jest wywoływana w celu zaktualizowania tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="70e47-148">When the web API returns a successful status code, the `getItems` function is invoked to update the HTML table.</span></span> <span data-ttu-id="70e47-149">Jeśli żądanie internetowego interfejsu API nie powiedzie się, zostanie zarejestrowany błąd w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="70e47-149">If the web API request fails, an error is logged to the browser's console.</span></span>

### <a name="update-a-to-do-item"></a><span data-ttu-id="70e47-150">Aktualizowanie elementu do wykonania</span><span class="sxs-lookup"><span data-stu-id="70e47-150">Update a to-do item</span></span>

<span data-ttu-id="70e47-151">Aktualizowanie elementu do wykonania jest podobne do dodawania jednego z nich; Istnieją jednak dwie znaczące różnice:</span><span class="sxs-lookup"><span data-stu-id="70e47-151">Updating a to-do item is similar to adding one; however, there are two significant differences:</span></span>

* <span data-ttu-id="70e47-152">Trasa ma sufiks z unikatowym identyfikatorem elementu do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="70e47-152">The route is suffixed with the unique identifier of the item to update.</span></span> <span data-ttu-id="70e47-153">Na przykład *API/TodoItems/1*.</span><span class="sxs-lookup"><span data-stu-id="70e47-153">For example, *api/TodoItems/1*.</span></span>
* <span data-ttu-id="70e47-154">Zlecenie akcji HTTP jest UMIESZCZAne w sposób wskazany przez opcję `method`.</span><span class="sxs-lookup"><span data-stu-id="70e47-154">The HTTP action verb is PUT, as indicated by the `method` option.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="70e47-155">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="70e47-155">Delete a to-do item</span></span>

<span data-ttu-id="70e47-156">Aby usunąć element do wykonania, należy ustawić opcję `method` dla żądania na `DELETE` i określić unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="70e47-156">To delete a to-do item, set the request's `method` option to `DELETE` and specify the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

<span data-ttu-id="70e47-157">Przejdź do następnego samouczka, aby dowiedzieć się, jak generować strony pomocy interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="70e47-157">Advance to the next tutorial to learn how to generate web API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
