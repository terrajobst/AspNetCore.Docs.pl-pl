---
title: Migrowanie z interfejsu API sieci Web ASP.NET
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 6325bdf602485b42d8193a05ede00ae275bf0a90
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="fb35c-102">Migrowanie z interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fb35c-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="fb35c-103">Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="fb35c-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="fb35c-104">Interfejsy API sieci Web są usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.</span><span class="sxs-lookup"><span data-stu-id="fb35c-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="fb35c-105">Podstawowe ASP.NET MVC obsługuje tworzenie interfejsów API sieci Web, dzięki czemu powinny być zawsze tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="fb35c-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="fb35c-106">W tym artykule przedstawiony kroki wymagane do migracji implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="fb35c-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="fb35c-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb35c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="fb35c-108">Projekt interfejsu API sieci Web ASP.NET przeglądu</span><span class="sxs-lookup"><span data-stu-id="fb35c-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="fb35c-109">W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonych w artykule [wprowadzenie do składnika ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego.</span><span class="sxs-lookup"><span data-stu-id="fb35c-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="fb35c-110">W tym projekcie prostego projektu interfejsu API sieci Web platformy ASP.NET jest skonfigurowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="fb35c-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="fb35c-111">W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="fb35c-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="fb35c-112">`WebApiConfig`jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="fb35c-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="fb35c-113">Ta klasa konfiguruje [trasami atrybutów](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), mimo że faktycznie nie jest on używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="fb35c-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="fb35c-114">Konfiguruje również tabeli routingu, który jest używany przez interfejs API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb35c-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="fb35c-115">W takim przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL zgodne z formatem */api/ {controller} / {id}*, z *{id}* opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="fb35c-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="fb35c-116">*ProductsApp* projekt zawiera tylko jeden proste kontrolera, który dziedziczy `ApiController` i udostępnia dwie metody:</span><span class="sxs-lookup"><span data-stu-id="fb35c-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="fb35c-117">Na koniec modelu *produktu*, używana przez *ProductsApp*, jest prostą klasę:</span><span class="sxs-lookup"><span data-stu-id="fb35c-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="fb35c-118">Teraz, gdy mamy prostego projektu, z którego chcesz uruchomić, firma Microsoft pokazują, jak przeprowadzić migrację tego projektu interfejsu API sieci Web do platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="fb35c-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="fb35c-119">Tworzenie projektu docelowego</span><span class="sxs-lookup"><span data-stu-id="fb35c-119">Create the Destination Project</span></span>

<span data-ttu-id="fb35c-120">Za pomocą programu Visual Studio, Utwórz nowy, pusty rozwiązanie i nadaj mu nazwę *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="fb35c-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="fb35c-121">Dodaj istniejące *ProductsApp* projektu do niego, a następnie, Dodaj nowy projekt aplikacji sieci Web platformy ASP.NET Core do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="fb35c-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="fb35c-122">Nazwa nowego projektu *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="fb35c-122">Name the new project *ProductsCore*.</span></span>

![Otwórz okno dialogowe nowego projektu do szablonów sieci Web](webapi/_static/add-web-project.png)

<span data-ttu-id="fb35c-124">Następnie wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fb35c-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="fb35c-125">Będziemy migrować *ProductsApp* zawartość do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="fb35c-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Okno dialogowe nowego aplikacji sieci Web z wybranych z listy szablonów platformy ASP.NET Core szablonu projektu interfejsu API sieci Web](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="fb35c-127">Usuń `Project_Readme.html` pliku z nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="fb35c-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="fb35c-128">Rozwiązania powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="fb35c-128">Your solution should now look like this:</span></span>

![Otwórz w Eksploratorze rozwiązań przedstawiający plików i folderów projektów ProductsApp i ProductsCore rozwiązanie aplikacji](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="fb35c-130">Migruj konfigurację</span><span class="sxs-lookup"><span data-stu-id="fb35c-130">Migrate Configuration</span></span>

<span data-ttu-id="fb35c-131">Już korzysta z platformy ASP.NET Core *Global.asax*, *web.config*, lub *App_Start* folderów.</span><span class="sxs-lookup"><span data-stu-id="fb35c-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="fb35c-132">Zamiast tego, wszystkie zadania uruchamiania są wykonywane w *Startup.cs* w katalogu głównym projektu (zobacz [uruchamiania aplikacji](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="fb35c-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="fb35c-133">W programie ASP.NET MVC Core, opartych na atrybutach routingu jest teraz zawarta domyślnie podczas `UseMvc()` nosi nazwę; i jest to zalecane podejście do konfigurowania tras interfejsu API sieci Web (, jak projekt starter interfejsu API sieci Web obsługuje routing).</span><span class="sxs-lookup"><span data-stu-id="fb35c-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="fb35c-134">Zakładając, że chcesz używać atrybutu routingu w projekcie w przyszłości, dodatkowa konfiguracja nie jest potrzebna.</span><span class="sxs-lookup"><span data-stu-id="fb35c-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="fb35c-135">Po prostu stosowanie atrybutów w razie potrzeby do kontrolerów i akcji, co jest wykonywane w próbce `ValuesController` klasy, która znajduje się w projekcie starter interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="fb35c-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="fb35c-136">Należy zwrócić uwagę na obecność *[kontrolera]* w wierszu 8.</span><span class="sxs-lookup"><span data-stu-id="fb35c-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="fb35c-137">Na podstawie atrybutów routingu teraz obsługuje niektórych tokenów, takich jak *[kontrolera]* i *[action]*.</span><span class="sxs-lookup"><span data-stu-id="fb35c-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="fb35c-138">Tokeny te są zamieniane w czasie wykonywania o nazwie kontroler lub akcję, odpowiednio, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="fb35c-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="fb35c-139">Dzięki temu można zmniejszyć liczbę magic ciągów w projekcie i gwarantuje, że trasy zostaną zachowane synchronizowana z ich odpowiednich kontrolerów i akcji po zastosowaniu refaktoryzacje automatycznej zmiany nazwy.</span><span class="sxs-lookup"><span data-stu-id="fb35c-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="fb35c-140">Aby przeprowadzić migrację Kontroler interfejsu API produktów, możemy należy najpierw skopiować *ProductsController* do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="fb35c-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="fb35c-141">Następnie wystarczy dołączyć atrybut trasy na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="fb35c-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="fb35c-142">Należy również dodać `[HttpGet]` atrybutu dwie metody, ponieważ obie powinna być wywoływana za pośrednictwem HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="fb35c-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="fb35c-143">Obejmują oczekuje parametru "id" w atrybucie dla `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="fb35c-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="fb35c-144">W tym momencie routingu jest poprawnie skonfigurowana; Jednak firma Microsoft nie może jeszcze przetestować.</span><span class="sxs-lookup"><span data-stu-id="fb35c-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="fb35c-145">Dodatkowe zmiany, musi być dokonana przed *ProductsController* zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="fb35c-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="fb35c-146">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="fb35c-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="fb35c-147">Ostatni etap procesu migracji dla tego prostego projektu interfejsu API sieci Web jest kopiować kontrolery i żadnych modeli korzystają.</span><span class="sxs-lookup"><span data-stu-id="fb35c-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="fb35c-148">W takim przypadku wystarczy skopiować *Controllers/ProductsController.cs* z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="fb35c-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="fb35c-149">Następnie skopiuj cały folder modeli z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="fb35c-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="fb35c-150">Dostosuj przestrzenie nazw, aby odpowiadała nowej nazwie projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="fb35c-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="fb35c-151">W tym momencie można skompilować aplikację i dostępne są różne błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb35c-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="fb35c-152">Te zazwyczaj powinno należeć do następujących kategorii:</span><span class="sxs-lookup"><span data-stu-id="fb35c-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="fb35c-153">*Klasy ApiController* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="fb35c-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="fb35c-154">*System.Web.Http* przestrzeni nazw nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="fb35c-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="fb35c-155">*IHttpActionResult* nie istnieje</span><span class="sxs-lookup"><span data-stu-id="fb35c-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="fb35c-156">Na szczęście są bardzo łatwo można poprawić:</span><span class="sxs-lookup"><span data-stu-id="fb35c-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="fb35c-157">Zmień *klasy ApiController* do *kontrolera* (może być konieczne dodanie *przy użyciu Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="fb35c-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="fb35c-158">Usuń wszystkie za pomocą instrukcji odwołujących się do *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="fb35c-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="fb35c-159">Zmień wszystkie zwracanie — metoda *IHttpActionResult* do zwrócenia *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="fb35c-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="fb35c-160">Po te zmiany zostały dokonane i nieużywanych instrukcje using usunięty, zmigrowane *ProductsController* klasy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="fb35c-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="fb35c-161">Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do *produkty/api/*; i będzie widoczna z pełną listą produktów 3.</span><span class="sxs-lookup"><span data-stu-id="fb35c-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="fb35c-162">Przejdź do */api/products/1* i powinien zostać wyświetlony pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="fb35c-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="fb35c-163">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="fb35c-163">Summary</span></span>

<span data-ttu-id="fb35c-164">Migrowanie prostego projektu interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC jest bardzo prosta dzięki wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="fb35c-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="fb35c-165">Główne elementy, które mają być każdy projekt interfejsu API sieci Web platformy ASP.NET, aby przeprowadzić migrację są tras, kontrolerów i modeli, wraz z aktualizacji na typy używane przez kontrolerów i akcji.</span><span class="sxs-lookup"><span data-stu-id="fb35c-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
