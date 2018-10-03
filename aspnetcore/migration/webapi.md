---
title: Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację z implementacją interfejsu API sieci web z interfejsu API sieci Web programu ASP.NET 4.x ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: 3c4ded874de2700e1290022a535c08f30dce9490
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861021"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="619a8-103">Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="619a8-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="619a8-104">Przez [Scott Addie](https://twitter.com/scott_addie) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="619a8-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="619a8-105">Interfejsu API sieci Web programu ASP.NET 4.x jest usługą HTTP, która osiąga szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="619a8-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="619a8-106">Aplikacja interfejsu API sieci Web modele w prostszy model programowania, znany jako program ASP.NET Core MVC i platformy ASP.NET Core ujednolica MVC ASP.NET 4.x firmy.</span><span class="sxs-lookup"><span data-stu-id="619a8-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="619a8-107">W tym artykule przedstawiono kroki wymagane do migracji z interfejsu API sieci Web 4.x ASP.NET do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="619a8-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="619a8-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="619a8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="619a8-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="619a8-109">Prerequisites</span></span>

* [<span data-ttu-id="619a8-110">Zestaw SDK programu .NET core 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="619a8-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="619a8-111">[Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="619a8-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="619a8-112">Przejrzyj projekt interfejsu API sieci Web programu ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="619a8-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="619a8-113">Jako punktu wyjścia, w tym artykule wykorzystano *ProductsApp* projektu utworzonego w [wprowadzenie do wzorca ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="619a8-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="619a8-114">W tym projekcie prosty projekt interfejsu API sieci Web platformy ASP.NET 4.x jest skonfigurowany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="619a8-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="619a8-115">W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="619a8-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="619a8-116">`WebApiConfig` jest zdefiniowany w *App_Start* folderu.</span><span class="sxs-lookup"><span data-stu-id="619a8-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="619a8-117">Ma ona tylko jedną statyczną `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="619a8-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="619a8-118">Ta klasa umożliwia skonfigurowanie [trasowanie atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż w rzeczywistości nie jest on używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="619a8-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="619a8-119">Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="619a8-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="619a8-120">W tym przypadku interfejsu API sieci Web programu ASP.NET 4.x oczekuje, że adresy URL służące do być zgodny z formatem `/api/{controller}/{id}`, za pomocą `{id}` opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="619a8-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="619a8-121">*ProductsApp* projekt zawiera jeden kontroler.</span><span class="sxs-lookup"><span data-stu-id="619a8-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="619a8-122">Kontroler dziedziczy `ApiController` i udostępnia dwie metody:</span><span class="sxs-lookup"><span data-stu-id="619a8-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="619a8-123">`Product` Używany przez model `ProductsController` jest prostą klasę:</span><span class="sxs-lookup"><span data-stu-id="619a8-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="619a8-124">W poniższych sekcjach przedstawiono migracji projektu interfejsu API sieci Web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="619a8-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="619a8-125">Utwórz projekt docelowy</span><span class="sxs-lookup"><span data-stu-id="619a8-125">Create destination project</span></span>

<span data-ttu-id="619a8-126">Wykonaj następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="619a8-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="619a8-127">Przejdź do **pliku** > **nowe** > **projektu** > **innych typów projektów**  >  **Rozwiązań programu visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="619a8-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="619a8-128">Wybierz **puste rozwiązanie**, a rozwiązanie *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="619a8-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="619a8-129">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="619a8-129">Click the **OK** button.</span></span>
* <span data-ttu-id="619a8-130">Dodaj istniejące *ProductsApp* projektu do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="619a8-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="619a8-131">Dodaj nową **aplikacji sieci Web programu ASP.NET Core** projektu do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="619a8-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="619a8-132">Wybierz **platformy .NET Core** docelowy framework z listy rozwijanej, a następnie wybierz **API** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="619a8-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="619a8-133">Nadaj projektowi nazwę *ProductsCore*i kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="619a8-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="619a8-134">Rozwiązanie zawiera teraz dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="619a8-134">The solution now contains two projects.</span></span> <span data-ttu-id="619a8-135">W poniższych sekcjach opisano Migrowanie *ProductsApp* zawartość projektu, aby *ProductsCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="619a8-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="619a8-136">Migracja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="619a8-136">Migrate configuration</span></span>

<span data-ttu-id="619a8-137">Nie korzysta z platformy ASP.NET Core *App_Start* folderu lub *Global.asax* pliku, a *web.config* plik zostanie dodany na czas publikacji.</span><span class="sxs-lookup"><span data-stu-id="619a8-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="619a8-138">*Startup.cs* zastępuje *Global.asax* i znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="619a8-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="619a8-139">`Startup` Klasa obsługuje wszystkie zadania uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="619a8-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="619a8-140">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="619a8-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="619a8-141">W programie ASP.NET Core MVC trasowanie atrybutów jest domyślnie włączone podczas <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> jest wywoływana w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="619a8-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="619a8-142">Następujące `UseMvc` wywołać zastępuje *ProductsApp* projektu *App_Start/WebApiConfig.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="619a8-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="619a8-143">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="619a8-143">Migrate models and controllers</span></span>

<span data-ttu-id="619a8-144">Skopiuj *ProductApp* kontroler projektu i model używa.</span><span class="sxs-lookup"><span data-stu-id="619a8-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="619a8-145">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="619a8-145">Follow these steps:</span></span>

1. <span data-ttu-id="619a8-146">Kopiuj *Controllers/ProductsController.cs* z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="619a8-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="619a8-147">Skopiuj cały *modeli* folderu z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="619a8-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="619a8-148">Zmień obszary nazw kopiowanych plików, aby odpowiadała nowej nazwie projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="619a8-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="619a8-149">Dostosuj `using ProductsApp.Models;` instrukcji w *ProductsController.cs* zbyt.</span><span class="sxs-lookup"><span data-stu-id="619a8-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="619a8-150">Na tym etapie tworzenia wyniki aplikacji na wiele błędów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="619a8-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="619a8-151">Błędy występują, ponieważ następujące składniki nie istnieją w programie ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="619a8-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="619a8-152">`ApiController` Klasa</span><span class="sxs-lookup"><span data-stu-id="619a8-152">`ApiController` class</span></span>
* <span data-ttu-id="619a8-153">`System.Web.Http` Namespace</span><span class="sxs-lookup"><span data-stu-id="619a8-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="619a8-154">`IHttpActionResult` Interfejs</span><span class="sxs-lookup"><span data-stu-id="619a8-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="619a8-155">Napraw błędy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="619a8-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="619a8-156">Zmiana `ApiController` do <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="619a8-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="619a8-157">Dodaj `using Microsoft.AspNetCore.Mvc;` rozpoznać `ControllerBase` odwołania.</span><span class="sxs-lookup"><span data-stu-id="619a8-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="619a8-158">Usuń folder `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="619a8-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="619a8-159">Zmiana `GetProduct` akcja ma typ zwracany z `IHttpActionResult` do `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="619a8-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="619a8-160">Konfigurowanie routingu</span><span class="sxs-lookup"><span data-stu-id="619a8-160">Configure routing</span></span>

<span data-ttu-id="619a8-161">Skonfiguruj routing w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="619a8-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="619a8-162">Dekoracji `ProductsController` klasy z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="619a8-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="619a8-163">Poprzedni [[trasy]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atrybut konfiguruje wzorzec routingu atrybutów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="619a8-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="619a8-164">[[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu sprawia, że atrybut routingu wymagana dla wszystkich akcji w tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="619a8-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="619a8-165">Routing atrybutów obsługuje tokenów, takich jak `[controller]` i `[action]`.</span><span class="sxs-lookup"><span data-stu-id="619a8-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="619a8-166">W czasie wykonywania każdy token jest zastępowana nazwą kontroler lub akcję, odpowiednio, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="619a8-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="619a8-167">Tokeny Zmniejsz liczbę magic ciągów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="619a8-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="619a8-168">Tokeny upewnij się, trasy ciągle synchronizowana z odpowiednich kontrolerów i akcji po automatyczne nazwy refaktoryzacje są stosowane.</span><span class="sxs-lookup"><span data-stu-id="619a8-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="619a8-169">Włącz żądania HTTP Get do `ProductController` akcje:</span><span class="sxs-lookup"><span data-stu-id="619a8-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="619a8-170">Zastosuj [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atrybutu `GetAllProducts` akcji.</span><span class="sxs-lookup"><span data-stu-id="619a8-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="619a8-171">Zastosuj `[HttpGet("{id}")]` atrybutu `GetProduct` akcji.</span><span class="sxs-lookup"><span data-stu-id="619a8-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="619a8-172">Po tych zmian i usuwania nieużywanych `using` instrukcji *ProductsController.cs* pliku wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="619a8-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="619a8-173">Uruchom projekt zmigrowane i przejdź do `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="619a8-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="619a8-174">Zostanie wyświetlona pełna lista trzy produkty.</span><span class="sxs-lookup"><span data-stu-id="619a8-174">A full list of three products appears.</span></span> <span data-ttu-id="619a8-175">Przejdź do `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="619a8-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="619a8-176">Pojawi się pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="619a8-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="619a8-177">Podkładki zgodności</span><span class="sxs-lookup"><span data-stu-id="619a8-177">Compatibility shim</span></span>

<span data-ttu-id="619a8-178">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteka zawiera poprawkę zgodności, aby przenieść projektach programów ASP.NET 4.x interfejsu API sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="619a8-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="619a8-179">Podkładki zgodności rozszerza platformy ASP.NET Core, obsługę wielu Konwencji z interfejsu Web API 2 platformy ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="619a8-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="619a8-180">Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodność nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="619a8-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="619a8-181">W przypadku większych projektów przy użyciu podkładki zgodności może być przydatne do tymczasowo unaoczni API między platformą ASP.NET Core i ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="619a8-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="619a8-182">Podkładki zgodności internetowy interfejs API jest przeznaczone do służyć jako tymczasowy środek do obsługi migrowania dużych interfejsu API sieci Web projektach programów ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="619a8-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="619a8-183">Wraz z upływem czasu projektów powinny zostać uaktualnione do użycia wzorców platformy ASP.NET Core, zamiast polegania na poprawkę zgodności.</span><span class="sxs-lookup"><span data-stu-id="619a8-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="619a8-184">Zgodność funkcji dostępnych w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:</span><span class="sxs-lookup"><span data-stu-id="619a8-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="619a8-185">Dodaje `ApiController` wpisz, aby kontrolerami typów podstawowych, nie muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="619a8-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="619a8-186">Umożliwia powiązanie modelu w stylu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="619a8-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="619a8-187">Platforma ASP.NET Core MVC model funkcji wiązania, podobnie jak w przypadku platformy ASP.NET MVC 5 4.x, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="619a8-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="619a8-188">Zmiany podkładki zgodności modelu powiązania, które są bardziej podobne do Konwencji powiązanie modelu 4.x Web API 2 platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="619a8-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="619a8-189">Na przykład typy złożone są automatycznie powiązany z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="619a8-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="619a8-190">Rozszerza wiązania modelu akcji kontrolera można korzystać z parametrami typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="619a8-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="619a8-191">Dodaje elementy formatujące komunikaty umożliwiając akcji do zwrócenia wyników typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="619a8-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="619a8-192">Dodaje metody dodatkowe odpowiedzi, które akcje Web API 2 może być używane do udostępniania odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="619a8-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="619a8-193">`HttpResponseMessage` generatory:</span><span class="sxs-lookup"><span data-stu-id="619a8-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="619a8-194">Metody wynik akcji:</span><span class="sxs-lookup"><span data-stu-id="619a8-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="619a8-195">Dodaje wystąpienie `IContentNegotiator` do aplikacji użytkownika kontenera (DI) iniekcji zależności i udostępnia związane z negocjacji typy zawartości z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="619a8-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="619a8-196">Przykładami takich typów `DefaultContentNegotiator` i `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="619a8-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="619a8-197">Aby użyć podkładek zgodności:</span><span class="sxs-lookup"><span data-stu-id="619a8-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="619a8-198">Zainstaluj [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="619a8-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="619a8-199">Zarejestruj podkładki zgodności usług za pomocą kontenera DI aplikacji przez wywołanie metody `services.AddMvc().AddWebApiConventions()` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="619a8-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="619a8-200">Zdefiniuj specyficzne dla interfejsu API tras za pomocą sieci web `MapWebApiRoute` na `IRouteBuilder` aplikacji `IApplicationBuilder.UseMvc` wywołania.</span><span class="sxs-lookup"><span data-stu-id="619a8-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="619a8-201">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="619a8-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
