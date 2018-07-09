---
title: Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację z implementacją interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894195"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="b380e-103">Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b380e-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="b380e-104">Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b380e-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b380e-105">Interfejsy API sieci Web są usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="b380e-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="b380e-106">Platforma ASP.NET Core MVC obejmuje obsługę tworzenia interfejsów API sieci Web, dzięki czemu powinny być zawsze w procesie tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="b380e-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="b380e-107">W tym artykule pokażemy, kroki wymagane do migracji z implementacją interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b380e-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="b380e-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b380e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="b380e-109">Przegląd projektu Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b380e-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="b380e-110">W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonego w artykule [wprowadzenie do wzorca ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego.</span><span class="sxs-lookup"><span data-stu-id="b380e-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="b380e-111">W tym projekcie prosty projekt ASP.NET Web API jest skonfigurowany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="b380e-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="b380e-112">W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="b380e-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="b380e-113">`WebApiConfig` jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="b380e-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="b380e-114">Ta klasa umożliwia skonfigurowanie [trasowanie atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż w rzeczywistości nie jest on używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="b380e-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="b380e-115">Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b380e-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="b380e-116">W tym przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL służące do być zgodny z formatem */api/ {controller} / {id}*, za pomocą *{id}* opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b380e-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="b380e-117">*ProductsApp* projekt zawiera tylko jeden kontroler prostą, która dziedziczy z `ApiController` i udostępnia dwie metody:</span><span class="sxs-lookup"><span data-stu-id="b380e-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="b380e-118">Na koniec modelu *produktu*, który jest używany przez *ProductsApp*, jest prostą klasę:</span><span class="sxs-lookup"><span data-stu-id="b380e-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="b380e-119">Teraz, gdy mamy już prostego projektu, który ma wyznaczać początek, firma Microsoft pokazują, jak przeprowadzić migrację tego projektu interfejsu API sieci Web do programu ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b380e-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="b380e-120">Utwórz projekt docelowy</span><span class="sxs-lookup"><span data-stu-id="b380e-120">Create the Destination Project</span></span>

<span data-ttu-id="b380e-121">W programie Visual Studio Utwórz nową, pustą rozwiązanie i nadaj mu nazwę *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="b380e-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="b380e-122">Dodaj istniejące *ProductsApp* projektu do niego, a następnie, Dodaj nowy projekt aplikacji sieci Web platformy ASP.NET Core z rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="b380e-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="b380e-123">Nazwa nowego projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="b380e-123">Name the new project *ProductsCore*.</span></span>

![Otwórz okno dialogowe Nowy projekt, aby szablonów sieci Web](webapi/_static/add-web-project.png)

<span data-ttu-id="b380e-125">Następnie wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b380e-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="b380e-126">Będziemy migrować *ProductsApp* zawartość do tego nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="b380e-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Okno dialogowe Nowy aplikacji sieci Web za pomocą szablonu projektu składnika Web API zaznaczony na liście szablonów platformy ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="b380e-128">Usuń `Project_Readme.html` plik z nowym projektem.</span><span class="sxs-lookup"><span data-stu-id="b380e-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="b380e-129">Rozwiązanie powinno teraz wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b380e-129">Your solution should now look like this:</span></span>

![Otwórz w Eksploratorze rozwiązań, wyświetlanie plików i folderów projektów ProductsApp i ProductsCore rozwiązanie aplikacji](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="b380e-131">Migracja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b380e-131">Migrate Configuration</span></span>

<span data-ttu-id="b380e-132">Platforma ASP.NET Core nie używa już *Global.asax*, *web.config*, lub *App_Start* folderów.</span><span class="sxs-lookup"><span data-stu-id="b380e-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="b380e-133">Zamiast tego wszystkie zadania uruchamiania są wykonywane w *Startup.cs* w katalogu głównym projektu (zobacz [uruchamiania aplikacji](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="b380e-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="b380e-134">W aplikacji ASP.NET Core MVC, routing oparty na atrybut jest teraz domyślnie uwzględniana podczas `UseMvc()` nosi nazwę; i, jest to zalecane podejście do konfigurowania tras interfejsu API sieci Web (a jak projekt startowy internetowy interfejs API obsługuje routing).</span><span class="sxs-lookup"><span data-stu-id="b380e-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="b380e-135">Przy założeniu, że chcesz użyć trasowanie atrybutów w projekcie idąc dalej, dodatkowa konfiguracja nie jest potrzebna.</span><span class="sxs-lookup"><span data-stu-id="b380e-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="b380e-136">Wystarczy zastosować atrybutów zgodnie z potrzebami do kontrolerów i akcji, jak odbywa się w próbce `ValuesController` klasę, która znajduje się w projekcie starter interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="b380e-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="b380e-137">Należy zwrócić uwagę na obecność *[controller]* w wierszu 8.</span><span class="sxs-lookup"><span data-stu-id="b380e-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="b380e-138">Routing oparty na atrybut teraz obsługuje niektóre tokeny, takie jak *[controller]* i *[action]*.</span><span class="sxs-lookup"><span data-stu-id="b380e-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="b380e-139">Tokeny te są zastępowane w czasie wykonywania nazwę kontroler lub akcję, odpowiednio, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="b380e-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="b380e-140">Dzięki temu można zmniejszyć liczbę magic ciągów w projekcie i gwarantuje, że trasy zostaną zachowane synchronizowana z ich odpowiednich kontrolerów i akcji po zastosowaniu refaktoryzacji automatyczna zmiana nazwy.</span><span class="sxs-lookup"><span data-stu-id="b380e-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="b380e-141">Aby przeprowadzić migrację Kontroler interfejsu API produktów, możemy najpierw skopiować *ProductsController* do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="b380e-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="b380e-142">Następnie po prostu Dołącz atrybut trasy na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="b380e-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="b380e-143">Musisz również dodać `[HttpGet]` atrybutu dwie metody, ponieważ oba powinna być wywoływana za pośrednictwem HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="b380e-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="b380e-144">Obejmują oczekuje parametru "id" w atrybucie dla `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="b380e-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="b380e-145">W tym momencie routing jest prawidłowo skonfigurowany; Jednak firma Microsoft nie może jeszcze ją przetestować.</span><span class="sxs-lookup"><span data-stu-id="b380e-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="b380e-146">Dodatkowe zmiany muszą zostać wprowadzone przed *ProductsController* zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="b380e-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="b380e-147">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="b380e-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="b380e-148">Ostatnim krokiem w procesie migracji dla tego prostego projektu interfejsu API sieci Web jest do skopiowania kontrolerów i żadnych modeli, które używają.</span><span class="sxs-lookup"><span data-stu-id="b380e-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="b380e-149">W takim przypadku po prostu skopiować *Controllers/ProductsController.cs* z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="b380e-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="b380e-150">Następnie skopiuj cały folder modeli z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="b380e-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="b380e-151">Dostosuj przestrzenie nazw, aby dopasować nową nazwę projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="b380e-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="b380e-152">W tym momencie można skompilować aplikację i znajdzie się liczba błędów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b380e-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="b380e-153">Te powinny ogólnie można podzielić na następujące kategorie:</span><span class="sxs-lookup"><span data-stu-id="b380e-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="b380e-154">*Klasy ApiController* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="b380e-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="b380e-155">*System.Web.Http* przestrzeni nazw nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="b380e-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="b380e-156">*IHttpActionResult* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="b380e-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="b380e-157">Na szczęście są wszystkie można łatwo rozwiązać:</span><span class="sxs-lookup"><span data-stu-id="b380e-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="b380e-158">Zmiana *klasy ApiController* do *kontrolera* (może być konieczne dodanie *przy użyciu Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="b380e-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="b380e-159">Usuń wszystkie za pomocą instrukcji odnoszące się do *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="b380e-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="b380e-160">Zmień wszystkie metody, zwracając *IHttpActionResult* do zwrócenia *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="b380e-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="b380e-161">Gdy te zmiany zostały dokonane i nieużywanych za pomocą instrukcji usunięciu zmigrowanych *ProductsController* klasy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="b380e-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="b380e-162">Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do */api/produktów*; i przeczytaj pełną listę produktów 3.</span><span class="sxs-lookup"><span data-stu-id="b380e-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="b380e-163">Przejdź do */api/products/1* powinien zostać wyświetlony pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="b380e-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="b380e-164">Podkładki zgodności programu ASP.NET 4.x Web API 2</span><span class="sxs-lookup"><span data-stu-id="b380e-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="b380e-165">Jest przydatne narzędzie podczas migrowania ASP.NET Web API projektów ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteki.</span><span class="sxs-lookup"><span data-stu-id="b380e-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="b380e-166">Podkładki zgodności rozszerza platformy ASP.NET Core, aby zezwolić na wiele różnych konwencji Web API 2 ma być używany.</span><span class="sxs-lookup"><span data-stu-id="b380e-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="b380e-167">Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodność nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="b380e-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="b380e-168">W przypadku większych projektów przy użyciu podkładki zgodności może być przydatne do tymczasowo unaoczni API między platformą ASP.NET Core i ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="b380e-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="b380e-169">Podkładki zgodności internetowy interfejs API jest przeznaczone do służyć jako tymczasowy środek do ułatwienia migrowania dużych projektów interfejsu API sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b380e-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="b380e-170">Wraz z upływem czasu projektów powinny zostać uaktualnione do użycia wzorców platformy ASP.NET Core, zamiast polegania na poprawkę zgodności.</span><span class="sxs-lookup"><span data-stu-id="b380e-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="b380e-171">Zgodność funkcji dostępnych w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:</span><span class="sxs-lookup"><span data-stu-id="b380e-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="b380e-172">Dodaje `ApiController` wpisz, aby kontrolerami typów podstawowych, nie muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="b380e-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="b380e-173">Umożliwia powiązanie modelu w stylu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b380e-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="b380e-174">Platforma ASP.NET Core MVC model funkcji wiązania, podobnie jak MVC 5, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b380e-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="b380e-175">Zmiany podkładki zgodności modelu powiązania, które są bardziej podobne do Konwencji powiązanie modelu Web API 2.</span><span class="sxs-lookup"><span data-stu-id="b380e-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="b380e-176">Na przykład typy złożone są automatycznie powiązany z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="b380e-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="b380e-177">Rozszerza wiązania modelu akcji kontrolera można korzystać z parametrami typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="b380e-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="b380e-178">Dodaje elementy formatujące komunikaty umożliwiając akcji do zwrócenia wyników typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="b380e-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="b380e-179">Dodaje metody dodatkowe odpowiedzi, które akcje Web API 2 może być używane do udostępniania odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="b380e-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="b380e-180">Generatory obiektu HttpResponseMessage:</span><span class="sxs-lookup"><span data-stu-id="b380e-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="b380e-181">Metody wynik akcji:</span><span class="sxs-lookup"><span data-stu-id="b380e-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="b380e-182">Dodaje wystąpienie `IContentNegotiator` do kontenera DI aplikacji i sprawia, że zawartość powiązane negocjacji typów z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) dostępne.</span><span class="sxs-lookup"><span data-stu-id="b380e-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="b380e-183">Obejmuje to typy, takie jak `DefaultContentNegotiator`, `MediaTypeFormatter`itp.</span><span class="sxs-lookup"><span data-stu-id="b380e-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="b380e-184">Aby użyć podkładek zgodności, należy:</span><span class="sxs-lookup"><span data-stu-id="b380e-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="b380e-185">Zainstaluj [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="b380e-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="b380e-186">Zarejestruj podkładki zgodności usług za pomocą kontenera DI aplikacji przez wywołanie metody `services.AddMvc().AddWebApiConventions()` aplikacji `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="b380e-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="b380e-187">Definiowanie tras specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` aplikacji `IApplicationBuilder.UseMvc` wywołania.</span><span class="sxs-lookup"><span data-stu-id="b380e-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="b380e-188">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b380e-188">Summary</span></span>

<span data-ttu-id="b380e-189">Migrowanie prosty projekt interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC jest dość prosta, Dziękujemy za wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b380e-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="b380e-190">Każdy projekt interfejsu API sieci Web platformy ASP.NET, należy przeprowadzić migrację wybrane elementy główne są tras, kontrolerów i modeli, wraz z aktualizacjami typy używane przez kontrolerów i akcji.</span><span class="sxs-lookup"><span data-stu-id="b380e-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
