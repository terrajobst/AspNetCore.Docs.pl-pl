---
title: Migracja z interfejsu API sieci Web ASP.NET do platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 9385805d548bc87f4a50b87f2c06aa74abdaf8af
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272533"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="2bc1b-103">Migracja z interfejsu API sieci Web ASP.NET do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2bc1b-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="2bc1b-104">Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2bc1b-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2bc1b-105">Interfejsy API sieci Web są usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="2bc1b-106">Podstawowe ASP.NET MVC obsługuje tworzenie interfejsów API sieci Web, dzięki czemu powinny być zawsze tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="2bc1b-107">W tym artykule przedstawiony kroki wymagane do migracji implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="2bc1b-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2bc1b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="2bc1b-109">Projekt interfejsu API sieci Web ASP.NET przeglądu</span><span class="sxs-lookup"><span data-stu-id="2bc1b-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="2bc1b-110">W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonych w artykule [wprowadzenie do korzystania z programu ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="2bc1b-111">W tym projekcie prostego projektu interfejsu API sieci Web platformy ASP.NET jest skonfigurowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="2bc1b-112">W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="2bc1b-113">`WebApiConfig` jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="2bc1b-114">Ta klasa konfiguruje [trasami atrybutów](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), mimo że faktycznie nie jest on używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="2bc1b-115">Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="2bc1b-116">W takim przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL zgodne z formatem */api/ {controller} / {id}*, z *{id}* opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="2bc1b-117">*ProductsApp* projekt zawiera tylko jeden proste kontrolera, który dziedziczy `ApiController` i udostępnia dwie metody:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="2bc1b-118">Na koniec modelu *produktu*, używana przez *ProductsApp*, jest prostą klasę:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="2bc1b-119">Teraz, gdy mamy prostego projektu, z którego chcesz uruchomić, firma Microsoft pokazują, jak przeprowadzić migrację tego projektu interfejsu API sieci Web do platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="2bc1b-120">Tworzenie projektu docelowego</span><span class="sxs-lookup"><span data-stu-id="2bc1b-120">Create the Destination Project</span></span>

<span data-ttu-id="2bc1b-121">Za pomocą programu Visual Studio, Utwórz nowy, pusty rozwiązanie i nadaj mu nazwę *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="2bc1b-122">Dodaj istniejące *ProductsApp* projektu do niego, a następnie, Dodaj nowy projekt aplikacji sieci Web platformy ASP.NET Core do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="2bc1b-123">Nazwa nowego projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-123">Name the new project *ProductsCore*.</span></span>

![Otwórz okno dialogowe nowego projektu do szablonów sieci Web](webapi/_static/add-web-project.png)

<span data-ttu-id="2bc1b-125">Następnie wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="2bc1b-126">Będziemy migrować *ProductsApp* zawartość do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Okno dialogowe nowego aplikacji sieci Web z wybranych z listy szablonów platformy ASP.NET Core szablonu projektu interfejsu API sieci Web](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="2bc1b-128">Usuń `Project_Readme.html` pliku z nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="2bc1b-129">Rozwiązania powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-129">Your solution should now look like this:</span></span>

![Otwórz w Eksploratorze rozwiązań przedstawiający plików i folderów projektów ProductsApp i ProductsCore rozwiązanie aplikacji](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="2bc1b-131">Migruj konfigurację</span><span class="sxs-lookup"><span data-stu-id="2bc1b-131">Migrate Configuration</span></span>

<span data-ttu-id="2bc1b-132">Już korzysta z platformy ASP.NET Core *Global.asax*, *web.config*, lub *App_Start* folderów.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="2bc1b-133">Zamiast tego, wszystkie zadania uruchamiania są wykonywane w *Startup.cs* w katalogu głównym projektu (zobacz [uruchamiania aplikacji](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="2bc1b-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="2bc1b-134">W programie ASP.NET MVC Core, opartych na atrybutach routingu jest teraz zawarta domyślnie podczas `UseMvc()` nosi nazwę; i jest to zalecane podejście do konfigurowania tras interfejsu API sieci Web (, jak projekt starter interfejsu API sieci Web obsługuje routing).</span><span class="sxs-lookup"><span data-stu-id="2bc1b-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="2bc1b-135">Zakładając, że chcesz używać atrybutu routingu w projekcie w przyszłości, dodatkowa konfiguracja nie jest potrzebna.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="2bc1b-136">Po prostu stosowanie atrybutów w razie potrzeby do kontrolerów i akcji, co jest wykonywane w próbce `ValuesController` klasy, która znajduje się w projekcie starter interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="2bc1b-137">Należy zwrócić uwagę na obecność *[kontrolera]* w wierszu 8.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="2bc1b-138">Na podstawie atrybutów routingu teraz obsługuje niektórych tokenów, takich jak *[kontrolera]* i *[action]*.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="2bc1b-139">Tokeny te są zamieniane w czasie wykonywania o nazwie kontroler lub akcję, odpowiednio, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="2bc1b-140">Dzięki temu można zmniejszyć liczbę magic ciągów w projekcie i gwarantuje, że trasy zostaną zachowane synchronizowana z ich odpowiednich kontrolerów i akcji po zastosowaniu refaktoryzacje automatycznej zmiany nazwy.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="2bc1b-141">Aby przeprowadzić migrację Kontroler interfejsu API produktów, możemy należy najpierw skopiować *ProductsController* do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="2bc1b-142">Następnie wystarczy dołączyć atrybut trasy na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="2bc1b-143">Należy również dodać `[HttpGet]` atrybutu dwie metody, ponieważ obie powinna być wywoływana za pośrednictwem HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="2bc1b-144">Obejmują oczekuje parametru "id" w atrybucie dla `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="2bc1b-145">W tym momencie routingu jest poprawnie skonfigurowana; Jednak firma Microsoft nie może jeszcze przetestować.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="2bc1b-146">Dodatkowe zmiany, musi być dokonana przed *ProductsController* zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="2bc1b-147">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="2bc1b-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="2bc1b-148">Ostatni etap procesu migracji dla tego prostego projektu interfejsu API sieci Web jest kopiować kontrolery i żadnych modeli korzystają.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="2bc1b-149">W takim przypadku wystarczy skopiować *Controllers/ProductsController.cs* z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="2bc1b-150">Następnie skopiuj cały folder modeli z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="2bc1b-151">Dostosuj przestrzenie nazw, aby odpowiadała nowej nazwie projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="2bc1b-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="2bc1b-152">W tym momencie można skompilować aplikację i dostępne są różne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="2bc1b-153">Te zazwyczaj powinno należeć do następujących kategorii:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="2bc1b-154">*Klasy ApiController* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="2bc1b-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="2bc1b-155">*System.Web.Http* przestrzeni nazw nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="2bc1b-156">*IHttpActionResult* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="2bc1b-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="2bc1b-157">Na szczęście są bardzo łatwo można poprawić:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="2bc1b-158">Zmień *klasy ApiController* do *kontrolera* (może być konieczne dodanie *przy użyciu Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="2bc1b-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="2bc1b-159">Usuń wszystkie za pomocą instrukcji odwołujących się do *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="2bc1b-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="2bc1b-160">Zmień wszystkie zwracanie — metoda *IHttpActionResult* do zwrócenia *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="2bc1b-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="2bc1b-161">Po te zmiany zostały dokonane i nieużywanych instrukcje using usunięty, zmigrowane *ProductsController* klasy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="2bc1b-162">Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do *produkty/api/*; i będzie widoczna z pełną listą produktów 3.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="2bc1b-163">Przejdź do */api/products/1* i powinien zostać wyświetlony pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="2bc1b-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="2bc1b-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="2bc1b-165">Jest przydatne narzędzie podczas migrowania składnika ASP.NET Web API projekty do platformy ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteki.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="2bc1b-166">Podkładki zgodności rozszerza platformy ASP.NET Core, aby umożliwić szereg różnych konwencji 2 interfejsu API sieci Web do użycia.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="2bc1b-167">Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodności nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="2bc1b-168">Dla większych projektów przy użyciu podkładki zgodności może być przydatne mostkowania tymczasowo interfejsu API odstęp między platformy ASP.NET Core i ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="2bc1b-169">Oznacza, że podkładki zgodności interfejsu API sieci Web można użyć jako środek tymczasowy w celu ułatwienia migrowania dużych projektów interfejsu API sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="2bc1b-170">Wraz z upływem czasu projekty powinny zostać uaktualnione do korzystania z platformy ASP.NET Core wzorce zamiast polegania na podkładki zgodności.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="2bc1b-171">Funkcje zgodności objęte Microsoft.AspNetCore.Mvc.WebApiCompatShim:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="2bc1b-172">Dodaje `ApiController` wpisz, aby kontrolery typów podstawowych nie muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="2bc1b-173">Umożliwia powiązanie modelu stylu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="2bc1b-174">ASP.NET MVC model powiązania funkcji podstawowych podobnie jak MVC 5, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="2bc1b-175">Zmiany podkładki zgodności modelu powiązania wyglądać mniej więcej konwencje powiązanie modelu 2 interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="2bc1b-176">Na przykład typy złożone automatycznie są powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="2bc1b-177">Rozszerza wiązania modelu tak, aby kontroler akcji może zająć parametrów typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="2bc1b-178">Dodaje elementy formatujące komunikaty stosowanie akcji do zwracania wyników typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="2bc1b-179">Dodaje metody odpowiedzi dodatkowe, które akcje 2 interfejsu API sieci Web mógł zostać użyty do obsługi odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="2bc1b-180">Generatory HttpResponseMessage:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="2bc1b-181">Metody wynik akcji:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-181">Action result methods:</span></span>
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="2bc1b-182">Dodaje wystąpienie `IContentNegotiator` do aplikacji kontenera Podpisane i sprawia, że zawartość związanych z negocjacji typów z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) dostępne.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="2bc1b-183">Dotyczy to również wykresach `DefaultContentNegotiator`, `MediaTypeFormatter`itp.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="2bc1b-184">Aby użyć podkładek zgodności, musisz:</span><span class="sxs-lookup"><span data-stu-id="2bc1b-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="2bc1b-185">Odwołanie [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="2bc1b-186">Zarejestruj usługi podkładki zgodności aplikacji kontenera Podpisane przez wywołanie metody `services.AddWebApiConventions()` w aplikacji `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="2bc1b-187">Definiowanie tras specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` w aplikacji `IApplicationBuilder.UseMvc` wywołania.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="2bc1b-188">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2bc1b-188">Summary</span></span>

<span data-ttu-id="2bc1b-189">Migrowanie prostego projektu interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC jest bardzo prosta dzięki wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="2bc1b-190">Główne elementy, które mają być każdy projekt interfejsu API sieci Web platformy ASP.NET, aby przeprowadzić migrację są tras, kontrolerów i modeli, wraz z aktualizacji na typy używane przez kontrolerów i akcji.</span><span class="sxs-lookup"><span data-stu-id="2bc1b-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
