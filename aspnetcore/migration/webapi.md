---
title: Migrowanie z interfejsu API sieci Web ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak migrować implementację internetowego interfejsu API z ASP.NET 4. x sieci Web API do ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: 7f61b78c589fc9d01061b50554e5a639e372c3d8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661848"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="83fd9-103">Migrowanie z interfejsu API sieci Web ASP.NET do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83fd9-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="83fd9-104">Przez [Scott Addie](https://twitter.com/scott_addie) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="83fd9-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="83fd9-105">Internetowy interfejs API ASP.NET 4. x to usługa HTTP, która dociera do szerokiego zakresu klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="83fd9-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="83fd9-106">ASP.NET Core modele aplikacji MVC i Web API łączy ASP.NET 4. x w prostszy model programowania znany jako ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="83fd9-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="83fd9-107">W tym artykule przedstawiono kroki wymagane do przeprowadzenia migracji z interfejsu API sieci Web ASP.NET 4. x do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="83fd9-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="83fd9-108">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="83fd9-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83fd9-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="83fd9-109">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="83fd9-110">Przegląd projektu interfejsu API sieci Web ASP.NET 4. x</span><span class="sxs-lookup"><span data-stu-id="83fd9-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="83fd9-111">Jako punkt początkowy, w tym artykule używany jest projekt *ProductsApp* utworzony w [wprowadzenie z ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="83fd9-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="83fd9-112">W tym projekcie jest skonfigurowany prosty projekt interfejsu API sieci Web ASP.NET 4. x w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="83fd9-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="83fd9-113">W *Global.asax.cs*, wykonano wywołanie do `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="83fd9-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="83fd9-114">Klasa `WebApiConfig` znajduje się w folderze *App_Start* i ma statyczną metodę `Register`:</span><span class="sxs-lookup"><span data-stu-id="83fd9-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="83fd9-115">Ta klasa konfiguruje [Routing atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż nie jest faktycznie używana w projekcie.</span><span class="sxs-lookup"><span data-stu-id="83fd9-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="83fd9-116">Konfiguruje również tabelę routingu, która jest używana przez interfejs API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83fd9-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="83fd9-117">W takim przypadku interfejs API sieci Web ASP.NET 4. x oczekuje, że adresy URL są zgodne z formatem `/api/{controller}/{id}`, z opcjonalną `{id}`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="83fd9-118">Projekt *ProductsApp* zawiera jeden kontroler.</span><span class="sxs-lookup"><span data-stu-id="83fd9-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="83fd9-119">Kontroler dziedziczy po `ApiController` i zawiera dwie akcje:</span><span class="sxs-lookup"><span data-stu-id="83fd9-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="83fd9-120">Model `Product` używany przez `ProductsController` jest prostą klasą:</span><span class="sxs-lookup"><span data-stu-id="83fd9-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="83fd9-121">W poniższych sekcjach przedstawiono migrację projektu interfejsu API sieci Web do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="83fd9-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="83fd9-122">Utwórz projekt docelowy</span><span class="sxs-lookup"><span data-stu-id="83fd9-122">Create destination project</span></span>

<span data-ttu-id="83fd9-123">Wykonaj następujące kroki w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="83fd9-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="83fd9-124">Przejdź do **pliku** > **nowy** > **project** > **innych typów projektów** > **rozwiązania programu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="83fd9-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="83fd9-125">Wybierz pozycję **puste rozwiązanie**i nazwij rozwiązanie *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="83fd9-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="83fd9-126">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="83fd9-126">Click the **OK** button.</span></span>
* <span data-ttu-id="83fd9-127">Dodaj istniejący projekt *ProductsApp* do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="83fd9-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="83fd9-128">Dodaj nowy projekt **aplikacji sieci Web ASP.NET Core** do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="83fd9-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="83fd9-129">Wybierz platformę docelową **platformy .NET Core** z listy rozwijanej i wybierz szablon projektu **interfejsu API** .</span><span class="sxs-lookup"><span data-stu-id="83fd9-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="83fd9-130">Nazwij projekt *ProductsCore*, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="83fd9-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="83fd9-131">Rozwiązanie zawiera teraz dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="83fd9-131">The solution now contains two projects.</span></span> <span data-ttu-id="83fd9-132">W poniższych sekcjach opisano Migrowanie zawartości projektu *ProductsApp* do projektu *ProductsCore* .</span><span class="sxs-lookup"><span data-stu-id="83fd9-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="83fd9-133">Migruj konfigurację</span><span class="sxs-lookup"><span data-stu-id="83fd9-133">Migrate configuration</span></span>

<span data-ttu-id="83fd9-134">ASP.NET Core nie używa folderu *App_Start* ani pliku *Global. asax* , a plik *Web. config* jest dodawany w czasie publikacji.</span><span class="sxs-lookup"><span data-stu-id="83fd9-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="83fd9-135">*Startup.cs* jest zamiennikiem elementu *Global. asax* i znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="83fd9-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="83fd9-136">Klasa `Startup` obsługuje wszystkie zadania uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83fd9-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="83fd9-137">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="83fd9-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="83fd9-138">W ASP.NET Core MVC, routing atrybutu jest uwzględniany domyślnie, gdy <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> jest wywoływana w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="83fd9-139">Następujące wywołanie `UseMvc` zastępuje plik */webapiconfig.cs App_Start* projektu *ProductsApp* :</span><span class="sxs-lookup"><span data-stu-id="83fd9-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="83fd9-140">Migrowanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="83fd9-140">Migrate models and controllers</span></span>

<span data-ttu-id="83fd9-141">Kopiuj przez kontroler projektu *ProductApp* oraz model, którego używa.</span><span class="sxs-lookup"><span data-stu-id="83fd9-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="83fd9-142">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="83fd9-142">Follow these steps:</span></span>

1. <span data-ttu-id="83fd9-143">Skopiuj *Kontrolery/ProductsController. cs* z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="83fd9-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="83fd9-144">Skopiuj cały folder *modele* z oryginalnego projektu do nowego.</span><span class="sxs-lookup"><span data-stu-id="83fd9-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="83fd9-145">Zmień przestrzenie nazw skopiowane pliki na pasujące do nowej nazwy projektu (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="83fd9-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="83fd9-146">Dostosuj również instrukcję `using ProductsApp.Models;` w *ProductsController.cs* .</span><span class="sxs-lookup"><span data-stu-id="83fd9-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="83fd9-147">W tym momencie Kompilowanie aplikacji powoduje szereg błędów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="83fd9-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="83fd9-148">Błędy występują, ponieważ następujące składniki nie istnieją w ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="83fd9-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="83fd9-149">Klasa `ApiController`</span><span class="sxs-lookup"><span data-stu-id="83fd9-149">`ApiController` class</span></span>
* <span data-ttu-id="83fd9-150">`System.Web.Http` przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="83fd9-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="83fd9-151">Interfejs `IHttpActionResult`</span><span class="sxs-lookup"><span data-stu-id="83fd9-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="83fd9-152">Usuń błędy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="83fd9-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="83fd9-153">Zmień `ApiController`, aby <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="83fd9-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="83fd9-154">Dodaj `using Microsoft.AspNetCore.Mvc;`, aby rozwiązać `ControllerBase` odwołanie.</span><span class="sxs-lookup"><span data-stu-id="83fd9-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="83fd9-155">Usuń folder `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="83fd9-156">Zmień typ zwracany akcji `GetProduct` z `IHttpActionResult` na `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="83fd9-157">Uprość instrukcję `return` akcji `GetProduct` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="83fd9-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="83fd9-158">Configure routing (Konfigurowanie routingu)</span><span class="sxs-lookup"><span data-stu-id="83fd9-158">Configure routing</span></span>

<span data-ttu-id="83fd9-159">Skonfiguruj Routing w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="83fd9-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="83fd9-160">Oznacz klasę `ProductsController` następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="83fd9-160">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="83fd9-161">Poprzedni atrybut [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) konfiguruje wzorzec routingu atrybutu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="83fd9-161">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="83fd9-162">Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) powoduje, że atrybut routingu wymaga dla wszystkich akcji w tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="83fd9-162">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="83fd9-163">Routing atrybutów obsługuje tokeny, takie jak `[controller]` i `[action]`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="83fd9-164">W czasie wykonywania każdy token jest zastępowany odpowiednio nazwą kontrolera lub akcji, do którego zastosowano atrybut.</span><span class="sxs-lookup"><span data-stu-id="83fd9-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="83fd9-165">Tokeny zmniejszają liczbę ciągów magicznych w projekcie.</span><span class="sxs-lookup"><span data-stu-id="83fd9-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="83fd9-166">Tokeny zapewniają również, że trasy pozostają zsynchronizowane z odpowiednimi kontrolerami i akcjami, gdy stosowane są automatyczne refaktoryzacje zmiany nazwy.</span><span class="sxs-lookup"><span data-stu-id="83fd9-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="83fd9-167">Ustaw tryb zgodności projektu na ASP.NET Core 2,2:</span><span class="sxs-lookup"><span data-stu-id="83fd9-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="83fd9-168">Poprzednia zmiana:</span><span class="sxs-lookup"><span data-stu-id="83fd9-168">The preceding change:</span></span>

    * <span data-ttu-id="83fd9-169">Jest wymagany do użycia atrybutu `[ApiController]` na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="83fd9-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="83fd9-170">Umożliwia potencjalne uszkodzenie zachowań wprowadzonych w ASP.NET Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="83fd9-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="83fd9-171">Włącz żądania HTTP GET do akcji `ProductController`:</span><span class="sxs-lookup"><span data-stu-id="83fd9-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="83fd9-172">Zastosuj atrybut [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) do akcji `GetAllProducts`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-172">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="83fd9-173">Zastosuj atrybut `[HttpGet("{id}")]` do akcji `GetProduct`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="83fd9-174">Po powyższych zmianach i usunięciu nieużywanych instrukcji `using` plik *ProductsController.cs* wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="83fd9-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="83fd9-175">Uruchom migrowany projekt i przejdź do `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="83fd9-176">Zostanie wyświetlona pełna lista trzech produktów.</span><span class="sxs-lookup"><span data-stu-id="83fd9-176">A full list of three products appears.</span></span> <span data-ttu-id="83fd9-177">Przejdź do `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="83fd9-178">Zostanie wyświetlony pierwszy produkt.</span><span class="sxs-lookup"><span data-stu-id="83fd9-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="83fd9-179">Podkładka zgodności</span><span class="sxs-lookup"><span data-stu-id="83fd9-179">Compatibility shim</span></span>

<span data-ttu-id="83fd9-180">Biblioteka [Microsoft. AspNetCore. MVC. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) zawiera podkładkę zgodności do przenoszenia projektów interfejsu API sieci Web ASP.NET 4. x do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83fd9-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="83fd9-181">Podkładka zgodności rozszerza ASP.NET Core do obsługi wielu konwencji z ASP.NET 4. x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="83fd9-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="83fd9-182">Przykładowy port podany wcześniej w tym dokumencie jest wystarczająco mały, że podkładka zgodności była niepotrzebna.</span><span class="sxs-lookup"><span data-stu-id="83fd9-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="83fd9-183">W przypadku większych projektów użycie podkładki zgodności może być przydatne do tymczasowego mostkowania przerwy między ASP.NET Core i ASP.NET 4. x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="83fd9-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="83fd9-184">Podkładka zgodności dla interfejsu API sieci Web ma służyć jako tymczasowa miara do obsługi migrowania dużych ASP.NETych projektów interfejsu API sieci Web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83fd9-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="83fd9-185">W miarę upływu czasu projekty należy zaktualizować tak, aby korzystały z wzorców ASP.NET Core zamiast polegać na podkładki zgodności.</span><span class="sxs-lookup"><span data-stu-id="83fd9-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="83fd9-186">Funkcje zgodności zawarte w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:</span><span class="sxs-lookup"><span data-stu-id="83fd9-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="83fd9-187">Dodaje typ `ApiController`, aby nie trzeba było aktualizować typów podstawowych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="83fd9-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="83fd9-188">Włącza powiązanie modelu internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="83fd9-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="83fd9-189">ASP.NET Core funkcji powiązania modelu MVC podobnie jak w przypadku ASP.NET 4. x MVC 5, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="83fd9-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="83fd9-190">W ramach podkładki zgodności zmiany powiązania modelu są bardziej podobne do Konwencji powiązań modelu ASP.NET 4. x sieci Web.</span><span class="sxs-lookup"><span data-stu-id="83fd9-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="83fd9-191">Na przykład typy złożone są automatycznie powiązane z treścią żądania.</span><span class="sxs-lookup"><span data-stu-id="83fd9-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="83fd9-192">Rozszerza powiązanie modelu, aby akcje kontrolera mogły przyjmować parametry typu `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="83fd9-193">Dodaje elementy formatujące komunikaty umożliwiające działanie zwracające wyniki typu `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="83fd9-194">Dodaje dodatkowe metody odpowiedzi, które mogą być używane przez działania Web API 2 do obsłużenia odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="83fd9-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="83fd9-195">Generatory `HttpResponseMessage`:</span><span class="sxs-lookup"><span data-stu-id="83fd9-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="83fd9-196">Metody wyniku akcji:</span><span class="sxs-lookup"><span data-stu-id="83fd9-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="83fd9-197">Dodaje wystąpienie `IContentNegotiator` do kontenera iniekcji zależności aplikacji i udostępnia typy powiązane z negocjowaniem zawartości z [Microsoft. ASPNET. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="83fd9-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="83fd9-198">Przykłady takich typów obejmują `DefaultContentNegotiator` i `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="83fd9-199">Aby użyć podkładek zgodności:</span><span class="sxs-lookup"><span data-stu-id="83fd9-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="83fd9-200">Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .</span><span class="sxs-lookup"><span data-stu-id="83fd9-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="83fd9-201">Zarejestruj usługi podkładki zgodności z kontenerem DI aplikacji, wywołując `services.AddMvc().AddWebApiConventions()` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="83fd9-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="83fd9-202">Zdefiniuj trasy specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` w wywołaniu `IApplicationBuilder.UseMvc` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83fd9-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83fd9-203">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="83fd9-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
