---
title: Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację z implementacją interfejsu API sieci web z interfejsu API sieci Web programu ASP.NET 4.x ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: ea14b7128582a21b36c70041d59fb638eebdbef0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901096"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="23a2c-103">Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23a2c-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="23a2c-104">Przez [Scott Addie](https://twitter.com/scott_addie) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="23a2c-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="23a2c-105">Interfejsu API sieci Web programu ASP.NET 4.x jest usługą HTTP, która osiąga szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="23a2c-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="23a2c-106">Aplikacja interfejsu API sieci Web modele w prostszy model programowania, znany jako program ASP.NET Core MVC i platformy ASP.NET Core ujednolica MVC ASP.NET 4.x firmy.</span><span class="sxs-lookup"><span data-stu-id="23a2c-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="23a2c-107">W tym artykule przedstawiono kroki wymagane do migracji z interfejsu API sieci Web 4.x ASP.NET do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="23a2c-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="23a2c-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23a2c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23a2c-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="23a2c-109">Prerequisites</span></span>

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="23a2c-110">Przejrzyj projekt interfejsu API sieci Web programu ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="23a2c-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="23a2c-111">Jako punktu wyjścia, w tym artykule wykorzystano *ProductsApp* projektu utworzonego w [wprowadzenie do wzorca ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="23a2c-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="23a2c-112">W tym projekcie prosty projekt interfejsu API sieci Web platformy ASP.NET 4.x jest skonfigurowany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="23a2c-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="23a2c-113">W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="23a2c-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="23a2c-114">`WebApiConfig` Klasa znajduje się w *App_Start* folder i ma statycznych `Register` metody:</span><span class="sxs-lookup"><span data-stu-id="23a2c-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="23a2c-115">Ta klasa umożliwia skonfigurowanie [trasowanie atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż w rzeczywistości nie jest on używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="23a2c-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="23a2c-116">Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="23a2c-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="23a2c-117">W tym przypadku interfejsu API sieci Web programu ASP.NET 4.x oczekuje, że adresy URL służące do być zgodny z formatem `/api/{controller}/{id}`, za pomocą `{id}` opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="23a2c-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="23a2c-118">*ProductsApp* projekt zawiera jeden kontroler.</span><span class="sxs-lookup"><span data-stu-id="23a2c-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="23a2c-119">Kontroler dziedziczy `ApiController` i zawiera dwie akcje:</span><span class="sxs-lookup"><span data-stu-id="23a2c-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="23a2c-120">`Product` Używany przez model `ProductsController` jest prostą klasę:</span><span class="sxs-lookup"><span data-stu-id="23a2c-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="23a2c-121">W poniższych sekcjach przedstawiono migracji projektu interfejsu API sieci Web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="23a2c-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="23a2c-122">Utwórz projekt docelowy</span><span class="sxs-lookup"><span data-stu-id="23a2c-122">Create destination project</span></span>

<span data-ttu-id="23a2c-123">Wykonaj następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="23a2c-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="23a2c-124">Przejdź do **pliku** > **nowe** > **projektu** > **innych typów projektów**  >  **Rozwiązań programu visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="23a2c-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="23a2c-125">Wybierz **puste rozwiązanie**, a rozwiązanie *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="23a2c-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="23a2c-126">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="23a2c-126">Click the **OK** button.</span></span>
* <span data-ttu-id="23a2c-127">Dodaj istniejące *ProductsApp* projektu do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="23a2c-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="23a2c-128">Dodaj nową **aplikacji sieci Web programu ASP.NET Core** projektu do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="23a2c-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="23a2c-129">Wybierz **platformy .NET Core** docelowy framework z listy rozwijanej, a następnie wybierz **API** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="23a2c-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="23a2c-130">Nadaj projektowi nazwę *ProductsCore*i kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="23a2c-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="23a2c-131">Rozwiązanie zawiera teraz dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="23a2c-131">The solution now contains two projects.</span></span> <span data-ttu-id="23a2c-132">W poniższych sekcjach opisano Migrowanie *ProductsApp* zawartość projektu, aby *ProductsCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="23a2c-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="23a2c-133">Migracja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="23a2c-133">Migrate configuration</span></span>

<span data-ttu-id="23a2c-134">Nie korzysta z platformy ASP.NET Core *App_Start* folderu lub *Global.asax* pliku, a *web.config* plik zostanie dodany na czas publikacji.</span><span class="sxs-lookup"><span data-stu-id="23a2c-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="23a2c-135">*Startup.cs* zastępuje *Global.asax* i znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="23a2c-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="23a2c-136">`Startup` Klasa obsługuje wszystkie zadania uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23a2c-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="23a2c-137">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="23a2c-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="23a2c-138">W programie ASP.NET Core MVC trasowanie atrybutów jest domyślnie włączone podczas <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> jest wywoływana w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="23a2c-139">Następujące `UseMvc` wywołać zastępuje *ProductsApp* projektu *App_Start/WebApiConfig.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="23a2c-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="23a2c-140">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="23a2c-140">Migrate models and controllers</span></span>

<span data-ttu-id="23a2c-141">Skopiuj *ProductApp* kontroler projektu i model używa.</span><span class="sxs-lookup"><span data-stu-id="23a2c-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="23a2c-142">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="23a2c-142">Follow these steps:</span></span>

1. <span data-ttu-id="23a2c-143">Kopiuj *Controllers/ProductsController.cs* z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="23a2c-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="23a2c-144">Skopiuj cały *modeli* folderu z oryginalnego projektu na nową.</span><span class="sxs-lookup"><span data-stu-id="23a2c-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="23a2c-145">Zmień obszary nazw kopiowanych plików, aby odpowiadała nowej nazwie projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="23a2c-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="23a2c-146">Dostosuj `using ProductsApp.Models;` instrukcji w *ProductsController.cs* zbyt.</span><span class="sxs-lookup"><span data-stu-id="23a2c-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="23a2c-147">Na tym etapie tworzenia wyniki aplikacji na wiele błędów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="23a2c-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="23a2c-148">Błędy występują, ponieważ następujące składniki nie istnieją w programie ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="23a2c-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="23a2c-149">`ApiController` Klasa</span><span class="sxs-lookup"><span data-stu-id="23a2c-149">`ApiController` class</span></span>
* <span data-ttu-id="23a2c-150">`System.Web.Http` Namespace</span><span class="sxs-lookup"><span data-stu-id="23a2c-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="23a2c-151">`IHttpActionResult` Interfejs</span><span class="sxs-lookup"><span data-stu-id="23a2c-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="23a2c-152">Napraw błędy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="23a2c-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="23a2c-153">Zmiana `ApiController` do <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="23a2c-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="23a2c-154">Dodaj `using Microsoft.AspNetCore.Mvc;` rozpoznać `ControllerBase` odwołania.</span><span class="sxs-lookup"><span data-stu-id="23a2c-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="23a2c-155">Usuń folder `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="23a2c-156">Zmiana `GetProduct` akcja ma typ zwracany z `IHttpActionResult` do `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="23a2c-157">Uprość `GetProduct` akcji `return` instrukcji do następującego:</span><span class="sxs-lookup"><span data-stu-id="23a2c-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="23a2c-158">Konfigurowanie routingu</span><span class="sxs-lookup"><span data-stu-id="23a2c-158">Configure routing</span></span>

<span data-ttu-id="23a2c-159">Skonfiguruj routing w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="23a2c-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="23a2c-160">Dekoracji `ProductsController` klasy z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="23a2c-160">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="23a2c-161">Poprzedni [[trasy]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atrybut konfiguruje wzorzec routingu atrybutów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="23a2c-161">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="23a2c-162">[[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu sprawia, że atrybut routingu wymagana dla wszystkich akcji w tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="23a2c-162">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="23a2c-163">Routing atrybutów obsługuje tokenów, takich jak `[controller]` i `[action]`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="23a2c-164">W czasie wykonywania każdy token jest zastępowana nazwą kontroler lub akcję, odpowiednio, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="23a2c-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="23a2c-165">Tokeny Zmniejsz liczbę magic ciągów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="23a2c-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="23a2c-166">Tokeny upewnij się, trasy ciągle synchronizowana z odpowiednich kontrolerów i akcji po automatyczne nazwy refaktoryzacje są stosowane.</span><span class="sxs-lookup"><span data-stu-id="23a2c-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="23a2c-167">Ustaw tryb zgodności projektu do programu ASP.NET Core w wersji 2.2:</span><span class="sxs-lookup"><span data-stu-id="23a2c-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="23a2c-168">Zmian poprzednim:</span><span class="sxs-lookup"><span data-stu-id="23a2c-168">The preceding change:</span></span>

    * <span data-ttu-id="23a2c-169">Wymagane jest wprowadzenie `[ApiController]` atrybut na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="23a2c-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="23a2c-170">Powoduje do potencjalnie istotne zachowania wprowadzone w programie ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="23a2c-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="23a2c-171">Włącz żądania HTTP Get do `ProductController` akcje:</span><span class="sxs-lookup"><span data-stu-id="23a2c-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="23a2c-172">Zastosuj [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atrybutu `GetAllProducts` akcji.</span><span class="sxs-lookup"><span data-stu-id="23a2c-172">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="23a2c-173">Zastosuj `[HttpGet("{id}")]` atrybutu `GetProduct` akcji.</span><span class="sxs-lookup"><span data-stu-id="23a2c-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="23a2c-174">Po zmian poprzednim i usunięcie nieużywanych `using` instrukcji *ProductsController.cs* pliku wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="23a2c-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="23a2c-175">Uruchom projekt zmigrowane i przejdź do `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="23a2c-176">Zostanie wyświetlona pełna lista trzy produkty.</span><span class="sxs-lookup"><span data-stu-id="23a2c-176">A full list of three products appears.</span></span> <span data-ttu-id="23a2c-177">Przejdź do `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="23a2c-178">Pojawi się pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="23a2c-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="23a2c-179">Podkładki zgodności</span><span class="sxs-lookup"><span data-stu-id="23a2c-179">Compatibility shim</span></span>

<span data-ttu-id="23a2c-180">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteka zawiera poprawkę zgodności, aby przenieść projektach programów ASP.NET 4.x interfejsu API sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23a2c-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="23a2c-181">Podkładki zgodności rozszerza platformy ASP.NET Core, obsługę wielu Konwencji z interfejsu Web API 2 platformy ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="23a2c-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="23a2c-182">Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodność nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="23a2c-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="23a2c-183">W przypadku większych projektów przy użyciu podkładki zgodności może być przydatne do tymczasowo unaoczni API między platformą ASP.NET Core i ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="23a2c-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="23a2c-184">Podkładki zgodności internetowy interfejs API jest przeznaczone do służyć jako tymczasowy środek do obsługi migrowania dużych interfejsu API sieci Web projektach programów ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23a2c-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="23a2c-185">Wraz z upływem czasu projektów powinny zostać uaktualnione do użycia wzorców platformy ASP.NET Core, zamiast polegania na poprawkę zgodności.</span><span class="sxs-lookup"><span data-stu-id="23a2c-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="23a2c-186">Zgodność funkcji dostępnych w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:</span><span class="sxs-lookup"><span data-stu-id="23a2c-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="23a2c-187">Dodaje `ApiController` wpisz, aby kontrolerami typów podstawowych, nie muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="23a2c-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="23a2c-188">Umożliwia powiązanie modelu w stylu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="23a2c-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="23a2c-189">Platforma ASP.NET Core MVC model funkcji wiązania, podobnie jak w przypadku platformy ASP.NET MVC 5 4.x, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="23a2c-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="23a2c-190">Zmiany podkładki zgodności modelu powiązania, które są bardziej podobne do Konwencji powiązanie modelu 4.x Web API 2 platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="23a2c-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="23a2c-191">Na przykład typy złożone są automatycznie powiązany z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="23a2c-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="23a2c-192">Rozszerza wiązania modelu akcji kontrolera można korzystać z parametrami typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="23a2c-193">Dodaje elementy formatujące komunikaty umożliwiając akcji do zwrócenia wyników typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="23a2c-194">Dodaje metody dodatkowe odpowiedzi, które akcje Web API 2 może być używane do udostępniania odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="23a2c-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="23a2c-195">`HttpResponseMessage` generatory:</span><span class="sxs-lookup"><span data-stu-id="23a2c-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="23a2c-196">Metody wynik akcji:</span><span class="sxs-lookup"><span data-stu-id="23a2c-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="23a2c-197">Dodaje wystąpienie `IContentNegotiator` do aplikacji użytkownika kontenera (DI) iniekcji zależności i udostępnia związane z negocjacji typy zawartości z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="23a2c-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="23a2c-198">Przykładami takich typów `DefaultContentNegotiator` i `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="23a2c-199">Aby użyć podkładek zgodności:</span><span class="sxs-lookup"><span data-stu-id="23a2c-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="23a2c-200">Zainstaluj [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="23a2c-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="23a2c-201">Zarejestruj podkładki zgodności usług za pomocą kontenera DI aplikacji przez wywołanie metody `services.AddMvc().AddWebApiConventions()` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="23a2c-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="23a2c-202">Zdefiniuj specyficzne dla interfejsu API tras za pomocą sieci web `MapWebApiRoute` na `IRouteBuilder` aplikacji `IApplicationBuilder.UseMvc` wywołania.</span><span class="sxs-lookup"><span data-stu-id="23a2c-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23a2c-203">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="23a2c-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
