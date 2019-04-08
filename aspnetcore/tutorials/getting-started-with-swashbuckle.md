---
title: Wprowadzenie do pakietu Swashbuckle i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak dodać pakiet Swashbuckle do swojego projektu interfejsu API sieci web platformy ASP.NET Core, aby zintegrować interfejs użytkownika struktury Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/04/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 31d45eaa684118ab78d1b3ecac594e95712f631f
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068352"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="53c42-103">Wprowadzenie do pakietu Swashbuckle i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53c42-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="53c42-104">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="53c42-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="53c42-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53c42-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="53c42-106">Istnieją trzy główne składniki do narzędzia Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="53c42-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="53c42-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby udostępnić `SwaggerDocument` obiektów jako punkty końcowe w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="53c42-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="53c42-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generatora struktury Swagger, który tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="53c42-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="53c42-109">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="53c42-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="53c42-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="53c42-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="53c42-111">Interpretuje JSON programu Swagger do tworzenia rozbudowanych, możliwych do dostosowania środowisko do opisywania funkcje interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="53c42-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="53c42-112">Obejmuje ona wiązka testów wbudowanych dla metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="53c42-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="53c42-113">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="53c42-113">Package installation</span></span>

<span data-ttu-id="53c42-114">Pakiet Swashbuckle mogą być dodawane przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="53c42-114">Swashbuckle can be added with the following approaches:</span></span>

### [<a name="visual-studio"></a><span data-ttu-id="53c42-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53c42-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="53c42-116">Z **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="53c42-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="53c42-117">Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="53c42-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="53c42-118">Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje</span><span class="sxs-lookup"><span data-stu-id="53c42-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="53c42-119">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="53c42-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="53c42-120">Z **Zarządzaj pakietami NuGet** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="53c42-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="53c42-121">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="53c42-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="53c42-122">Ustaw **źródła pakietu** na stronie "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="53c42-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="53c42-123">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="53c42-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="53c42-124">Wybierz pakiet "Swashbuckle.AspNetCore" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="53c42-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### [<a name="visual-studio-for-mac"></a><span data-ttu-id="53c42-125">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="53c42-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="53c42-126">Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="53c42-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="53c42-127">Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="53c42-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="53c42-128">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="53c42-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="53c42-129">Wybierz pakiet "Swashbuckle.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="53c42-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### [<a name="visual-studio-code"></a><span data-ttu-id="53c42-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53c42-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="53c42-131">Uruchom następujące polecenie z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="53c42-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### [<a name="net-core-cli"></a><span data-ttu-id="53c42-132">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="53c42-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="53c42-133">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="53c42-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="53c42-134">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="53c42-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="53c42-135">Dodaj generatora struktury Swagger do kolekcji usługi w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="53c42-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="53c42-136">Importowanie następująca przestrzeń nazw do użycia `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="53c42-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="53c42-137">W `Startup.Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane dokumentów JSON i interfejs użytkownika struktury Swagger:</span><span class="sxs-lookup"><span data-stu-id="53c42-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="53c42-138">Poprzedni `UseSwaggerUI` umożliwia wywołanie metody [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="53c42-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="53c42-139">Jeśli przeznaczony dla .NET Framework lub .NET Core 1.x, należy dodać [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="53c42-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="53c42-140">Uruchom aplikację i przejdź do `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="53c42-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="53c42-141">Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano na [(swagger.json) specyfikacjami struktury Swagger](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="53c42-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="53c42-142">Interfejs użytkownika struktury Swagger, można znaleźć w folderze `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="53c42-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="53c42-143">Poznaj interfejs API poprzez interfejs użytkownika struktury Swagger, a następnie Uwzględnij ją w innych programach.</span><span class="sxs-lookup"><span data-stu-id="53c42-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="53c42-144">Aby obsługiwać interfejs użytkownika struktury Swagger w katalogu głównym aplikacji (`http://localhost:<port>/`) ustaw `RoutePrefix` właściwości na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="53c42-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="53c42-145">Jeśli przy użyciu katalogów w usługach IIS lub zwrotny serwer proxy punktu końcowego struktury Swagger za pomocą ścieżki względnej `./` prefiks.</span><span class="sxs-lookup"><span data-stu-id="53c42-145">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="53c42-146">Na przykład `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="53c42-146">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="53c42-147">Za pomocą `/swagger/v1/swagger.json` instruuje aplikację, aby wyszukać plik JSON w katalogu głównym PRAWDA adres URL (plus prefiks trasy, jeśli używane).</span><span class="sxs-lookup"><span data-stu-id="53c42-147">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="53c42-148">Na przykład użyć `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` zamiast `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="53c42-148">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="53c42-149">Dostosowywanie i rozszerzanie</span><span class="sxs-lookup"><span data-stu-id="53c42-149">Customize and extend</span></span>

<span data-ttu-id="53c42-150">Struktury swagger zawiera opcje dokumentowanie model obiektu i dostosowywanie interfejsu użytkownika do dopasowania Twój wybrany motyw.</span><span class="sxs-lookup"><span data-stu-id="53c42-150">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="53c42-151">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="53c42-151">API info and description</span></span>

<span data-ttu-id="53c42-152">Akcja konfiguracji są przekazywane do `AddSwaggerGen` metoda dodaje informacje, takie jak tworzenie, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="53c42-152">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="53c42-153">Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="53c42-153">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autorem i zobacz link więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="53c42-155">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="53c42-155">XML comments</span></span>

<span data-ttu-id="53c42-156">Komentarze XML można włączyć za pomocą następujących metod:</span><span class="sxs-lookup"><span data-stu-id="53c42-156">XML comments can be enabled with the following approaches:</span></span>

#### [<a name="visual-studio"></a><span data-ttu-id="53c42-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53c42-157">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="53c42-158">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >**.</span><span class="sxs-lookup"><span data-stu-id="53c42-158">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="53c42-159">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="53c42-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="53c42-160">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="53c42-160">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="53c42-161">Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę.</span><span class="sxs-lookup"><span data-stu-id="53c42-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### [<a name="visual-studio-for-mac"></a><span data-ttu-id="53c42-162">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="53c42-162">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="53c42-163">Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="53c42-163">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="53c42-164">Przejdź do **narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="53c42-164">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="53c42-165">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="53c42-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="53c42-166">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="53c42-166">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="53c42-167">Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji</span><span class="sxs-lookup"><span data-stu-id="53c42-167">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### [<a name="visual-studio-code"></a><span data-ttu-id="53c42-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53c42-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="53c42-169">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="53c42-169">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### [<a name="net-core-cli"></a><span data-ttu-id="53c42-170">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="53c42-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="53c42-171">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="53c42-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="53c42-172">Włączanie komentarzy XML zawiera informacje o debugowaniu nieudokumentowane typy publiczne i elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="53c42-172">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="53c42-173">Nieudokumentowany typy i elementy członkowskie są wskazywane przez komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="53c42-173">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="53c42-174">Na przykład następujący komunikat wskazuje naruszenie kod ostrzegawczy 1591:</span><span class="sxs-lookup"><span data-stu-id="53c42-174">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="53c42-175">Aby pominąć ostrzeżenia całego projektu, należy zdefiniować rozdzieloną średnikami listę kodów błędów do zignorowania w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="53c42-175">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="53c42-176">Dołączanie kodów `$(NoWarn);` stosuje [ C# wartości domyślne](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) zbyt.</span><span class="sxs-lookup"><span data-stu-id="53c42-176">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="53c42-177">Aby pominąć ostrzeżenia tylko dla określonych członków, należy wpisać kod w [ostrzeżenie #pragma](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) dyrektywy preprocesora.</span><span class="sxs-lookup"><span data-stu-id="53c42-177">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="53c42-178">To podejście jest przydatne w przypadku kodu, który nie powinien być udostępniane za pośrednictwem dokumentacji interfejsu API. W poniższym przykładzie, kod ostrzegawczy CS1591 jest ignorowany dla całego `Program` klasy.</span><span class="sxs-lookup"><span data-stu-id="53c42-178">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="53c42-179">Wymuszanie kod ostrzegawczy zostanie przywrócony na koniec definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="53c42-179">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="53c42-180">Określ wiele kodów ostrzeżenie listę rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="53c42-180">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="53c42-181">Skonfiguruj strukturę Swagger, aby użyć pliku XML, który jest generowany przy użyciu poprzednich instrukcji.</span><span class="sxs-lookup"><span data-stu-id="53c42-181">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="53c42-182">Dla systemu Linux lub w systemach operacyjnych innych niż Windows może być uwzględniana wielkość liter nazw plików oraz ścieżek.</span><span class="sxs-lookup"><span data-stu-id="53c42-182">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="53c42-183">Na przykład *TodoApi.XML* plik jest prawidłowy w Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="53c42-183">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="53c42-184">W poprzednim kodzie [odbicia](/dotnet/csharp/programming-guide/concepts/reflection) jest używany do tworzenia nazwę pliku XML dopasowania, projekt interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="53c42-184">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="53c42-185">[AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) właściwość jest używana do konstruowania ścieżkę do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="53c42-185">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="53c42-186">Niektóre funkcje programu Swagger (na przykład wypełniana parametrów wejściowych lub metody HTTP i kodów odpowiedzi z odpowiednich atrybutów) działa bez użycia pliku dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="53c42-186">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="53c42-187">W przypadku większości funkcji, mianowicie podsumowania metody i opisy parametrów i kodów odpowiedzi korzystanie z pliku XML jest obowiązkowe.</span><span class="sxs-lookup"><span data-stu-id="53c42-187">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="53c42-188">Dodawanie komentarze z potrójnym ukośnikiem akcję zwiększa interfejsu użytkownika programu Swagger, dodając opis do nagłówku sekcji.</span><span class="sxs-lookup"><span data-stu-id="53c42-188">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="53c42-189">Dodaj [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) element powyżej `Delete` akcji:</span><span class="sxs-lookup"><span data-stu-id="53c42-189">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="53c42-190">Interfejs użytkownika struktury Swagger zawiera tekst wewnętrzny dla poprzedniego kodu `<summary>` elementu:</span><span class="sxs-lookup"><span data-stu-id="53c42-190">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Wyświetlanie komentarza XML "Usuwa określone TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="53c42-193">Interfejs użytkownika jest wymuszany przez wygenerowany schemat JSON:</span><span class="sxs-lookup"><span data-stu-id="53c42-193">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="53c42-194">Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="53c42-194">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="53c42-195">Uzupełnia artykuł informacji o określonych w `<summary>` elementu i zapewnia bardziej niezawodny interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="53c42-195">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="53c42-196">`<remarks>` Element zawartości może zawierać tekstu, JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="53c42-196">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="53c42-197">Zwróć uwagę, ulepszenia interfejsu użytkownika za pomocą te dodatkowe uwagi:</span><span class="sxs-lookup"><span data-stu-id="53c42-197">Notice the UI enhancements with these additional comments:</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="53c42-199">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="53c42-199">Data annotations</span></span>

<span data-ttu-id="53c42-200">Dekoracji modelu z atrybutów, w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, oferuje składniki interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="53c42-200">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="53c42-201">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="53c42-201">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="53c42-202">Obecność tego atrybutu zmienia zachowanie interfejsu użytkownika i zmienia schemat JSON źródłowy:</span><span class="sxs-lookup"><span data-stu-id="53c42-202">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="53c42-203">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="53c42-203">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="53c42-204">Jej celem jest, aby zadeklarować, że akcji kontrolera obsługuje typ zawartości odpowiedzi *application/json*:</span><span class="sxs-lookup"><span data-stu-id="53c42-204">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="53c42-205">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako wartość domyślna dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="53c42-205">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="53c42-207">W miarę zwiększania użycia adnotacje danych w interfejsie API sieci web interfejsu API i interfejsu użytkownika strony, stają się bardziej opisowy i przydatne pomocy.</span><span class="sxs-lookup"><span data-stu-id="53c42-207">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="53c42-208">Opis typów odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="53c42-208">Describe response types</span></span>

<span data-ttu-id="53c42-209">Deweloperom korzystanie z interfejsu API sieci web są najbardziej interesujących co to jest zwracany&mdash;specjalnie typów odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="53c42-209">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="53c42-210">Typy odpowiedzi i kody błędów są wskazywane w adnotacjach komentarze i dane XML.</span><span class="sxs-lookup"><span data-stu-id="53c42-210">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="53c42-211">`Create` Akcji zwraca kod stanu 201 protokołu HTTP w przypadku powodzenia.</span><span class="sxs-lookup"><span data-stu-id="53c42-211">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="53c42-212">Treść żądania przesłane ma wartość null zwracany jest kod stanu HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="53c42-212">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="53c42-213">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="53c42-213">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="53c42-214">Rozwiązać ten problem, dodając wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="53c42-214">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="53c42-215">Teraz interfejs użytkownika struktury Swagger dokumenty wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="53c42-215">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Wyświetlanie opis klasy odpowiedzi WPIS "Zwraca nowo utworzony element Todo" interfejs użytkownika struktury swagger i "400 - Jeśli element ma wartość null" dla kodu stan i przyczyna w komunikatach odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="53c42-217">W programie ASP.NET Core 2.2 lub nowszej, konwencje mogą być używane jako alternatywa jawnie urządzanie poszczególne akcje za pomocą `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="53c42-217">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="53c42-218">Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="53c42-218">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="53c42-219">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="53c42-219">Customize the UI</span></span>

<span data-ttu-id="53c42-220">Zasoby interfejsu użytkownika jest funkcjonalności i zawartości.</span><span class="sxs-lookup"><span data-stu-id="53c42-220">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="53c42-221">Jednak strony dokumentacji interfejsu API powinno reprezentować Twojej marki lub motywu.</span><span class="sxs-lookup"><span data-stu-id="53c42-221">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="53c42-222">Składniki pakietu Swashbuckle znakowania wymaga dodawania zasobów do obsługi plików statycznych i tworzenia struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="53c42-222">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="53c42-223">Jeśli przeznaczony dla .NET Framework lub .NET Core 1.x, należy dodać [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="53c42-223">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="53c42-224">Poprzedni pakiet NuGet jest już zainstalowany, jeśli przeznaczony dla platformy .NET Core 2.x i przy użyciu [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="53c42-224">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="53c42-225">Włącz oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="53c42-225">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="53c42-226">Uzyskiwanie zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="53c42-226">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="53c42-227">Ten folder zawiera zasoby wymagane dla strony interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="53c42-227">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="53c42-228">Tworzenie *wwwroot/swagger/ui* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="53c42-228">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="53c42-229">Tworzenie *custome.CSS* pliku w *wwwroot/swagger/ui*, za pomocą następujących CSS, aby dostosować nagłówek strony:</span><span class="sxs-lookup"><span data-stu-id="53c42-229">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="53c42-230">Odwołanie *custome.CSS* w *index.html* pliku po wszystkich innych plików CSS:</span><span class="sxs-lookup"><span data-stu-id="53c42-230">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="53c42-231">Przejdź do *index.html* pod `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="53c42-231">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="53c42-232">Wprowadź `http://localhost:<port>/swagger/v1/swagger.json` w polu tekstowym w nagłówku i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="53c42-232">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="53c42-233">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="53c42-233">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="53c42-235">Jest znacznie więcej można zrobić ze stroną.</span><span class="sxs-lookup"><span data-stu-id="53c42-235">There's much more you can do with the page.</span></span> <span data-ttu-id="53c42-236">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="53c42-236">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
