---
title: 'Samouczek: Wywoływanie interfejsu API sieci Web za pomocą platformy jQuery przy użyciu ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak wywołać interfejs API sieci Web ASP.NET Core za pomocą platformy jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602586"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="45386-103">Samouczek: Wywoływanie ASP.NET Core internetowego interfejsu API za pomocą platformy jQuery</span><span class="sxs-lookup"><span data-stu-id="45386-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="45386-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="45386-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="45386-105">W tym samouczku pokazano, jak wywołać interfejs API sieci Web ASP.NET Core za pomocą platformy jQuery</span><span class="sxs-lookup"><span data-stu-id="45386-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="45386-106">Aby uzyskać ASP.NET Core 2,2, zobacz wersja 2,2 [wywołania interfejsu API sieci Web za pomocą platformy jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span><span class="sxs-lookup"><span data-stu-id="45386-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="45386-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="45386-107">Prerequisites</span></span>

<span data-ttu-id="45386-108">Pełny [samouczek: Tworzenie internetowego interfejsu API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="45386-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="45386-109">Wywoływanie interfejsu API przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="45386-109">Call the API with jQuery</span></span>

<span data-ttu-id="45386-110">W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api.</span><span class="sxs-lookup"><span data-stu-id="45386-110">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="45386-111">jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="45386-111">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="45386-112">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="45386-112">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="45386-113">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="45386-113">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="45386-114">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="45386-114">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="45386-115">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="45386-115">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="45386-116">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="45386-116">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="45386-117">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="45386-117">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="45386-118">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="45386-118">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="45386-119">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="45386-119">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="45386-120">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="45386-120">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="45386-121">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="45386-121">There are several ways to get jQuery.</span></span> <span data-ttu-id="45386-122">W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.</span><span class="sxs-lookup"><span data-stu-id="45386-122">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="45386-123">Ten przykład wywołuje wszystkie metody CRUD interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="45386-123">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="45386-124">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="45386-124">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="45386-125">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="45386-125">Get a list of to-do items</span></span>

<span data-ttu-id="45386-126">JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="45386-126">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="45386-127">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="45386-127">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="45386-128">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="45386-128">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="45386-129">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="45386-129">Add a to-do item</span></span>

<span data-ttu-id="45386-130">[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="45386-130">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="45386-131">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="45386-131">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="45386-132">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="45386-132">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="45386-133">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="45386-133">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="45386-134">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="45386-134">Update a to-do item</span></span>

<span data-ttu-id="45386-135">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="45386-135">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="45386-136">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="45386-136">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="45386-137">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="45386-137">Delete a to-do item</span></span>

<span data-ttu-id="45386-138">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="45386-138">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="45386-139">Przejdź do następnego samouczka, aby dowiedzieć się, jak można wygenerować stron pomocy interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="45386-139">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end