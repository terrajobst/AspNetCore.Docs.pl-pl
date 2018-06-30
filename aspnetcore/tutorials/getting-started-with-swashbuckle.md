---
title: Rozpoczynanie pracy z Swashbuckle i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak dodać pakiet Swashbuckle do projektu interfejsu API sieci web platformy ASP.NET Core integracja interfejsu użytkownika programu Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 70a1503a1ddbfe7f569d12b0034d967b220c9c44
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126251"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="d0604-103">Rozpoczynanie pracy z Swashbuckle i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0604-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="d0604-104">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d0604-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d0604-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0604-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d0604-106">Istnieją trzy główne składniki do Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="d0604-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="d0604-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.</span><span class="sxs-lookup"><span data-stu-id="d0604-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="d0604-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="d0604-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="d0604-109">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="d0604-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="d0604-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="d0604-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="d0604-111">Interpretuje ją JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0604-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="d0604-112">Obejmuje on przewodów wbudowanych testu dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="d0604-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="d0604-113">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="d0604-113">Package installation</span></span>

<span data-ttu-id="d0604-114">Pakiet Swashbuckle można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d0604-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0604-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0604-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d0604-116">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="d0604-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="d0604-117">Przejdź do **widoku** > **innych okien** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="d0604-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="d0604-118">Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje</span><span class="sxs-lookup"><span data-stu-id="d0604-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="d0604-119">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d0604-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="d0604-120">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="d0604-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="d0604-121">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="d0604-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="d0604-122">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="d0604-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="d0604-123">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="d0604-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="d0604-124">Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="d0604-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0604-125">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d0604-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d0604-126">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="d0604-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="d0604-127">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="d0604-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="d0604-128">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="d0604-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="d0604-129">Wybierz pakiet "Swashbuckle.AspNetCore" w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="d0604-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0604-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0604-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d0604-131">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="d0604-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d0604-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d0604-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d0604-133">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d0604-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="d0604-134">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="d0604-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="d0604-135">Dodaj Swagger generator kolekcji usług w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="d0604-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="d0604-136">Zaimportuj następujące przestrzeń nazw za pomocą `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="d0604-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="d0604-137">W `Startup.Configure` metody, włącza oprogramowanie pośredniczące do obsługi interfejsu użytkownika programu Swagger i wygenerowanego dokumentów JSON:</span><span class="sxs-lookup"><span data-stu-id="d0604-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="d0604-138">Uruchom aplikację i przejdź do `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="d0604-138">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="d0604-139">Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano w [Swagger specyfikacji (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="d0604-139">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="d0604-140">Interfejs użytkownika programu Swagger można znaleźć w folderze `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="d0604-140">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="d0604-141">Eksploruj interfejsu API przez interfejs użytkownika programu Swagger i uwzględnić go w innych programów.</span><span class="sxs-lookup"><span data-stu-id="d0604-141">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="d0604-142">Do obsługi interfejsu użytkownika programu Swagger w katalogu głównym aplikacji (`http://localhost:<port>/`) ustaw `RoutePrefix` właściwości na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="d0604-142">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a><span data-ttu-id="d0604-143">Dostosowywanie i rozszerzanie</span><span class="sxs-lookup"><span data-stu-id="d0604-143">Customize and extend</span></span>

<span data-ttu-id="d0604-144">Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.</span><span class="sxs-lookup"><span data-stu-id="d0604-144">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="d0604-145">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="d0604-145">API info and description</span></span>

<span data-ttu-id="d0604-146">Akcja konfiguracji przekazany do `AddSwaggerGen` metoda dodaje informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="d0604-146">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="d0604-147">Interfejs użytkownika programu Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="d0604-147">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="d0604-149">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="d0604-149">XML comments</span></span>

<span data-ttu-id="d0604-150">Komentarze XML można włączyć z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d0604-150">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="d0604-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0604-151">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="d0604-152">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **.csproj < nazwa_projektu > Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="d0604-152">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="d0604-153">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="d0604-153">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="d0604-154">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="d0604-154">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="d0604-155">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** kartę.</span><span class="sxs-lookup"><span data-stu-id="d0604-155">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="d0604-156">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d0604-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="d0604-157">Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="d0604-157">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="d0604-158">Przejdź do **narzędzia** > **Przeprowadź edycję pliku**.</span><span class="sxs-lookup"><span data-stu-id="d0604-158">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="d0604-159">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="d0604-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="d0604-160">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="d0604-160">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="d0604-161">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji</span><span class="sxs-lookup"><span data-stu-id="d0604-161">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="d0604-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0604-162">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="d0604-163">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="d0604-163">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="d0604-164">Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="d0604-164">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="d0604-165">Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="d0604-165">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="d0604-166">Na przykład następujący komunikat wskazuje naruszenie kod ostrzeżenia 1591:</span><span class="sxs-lookup"><span data-stu-id="d0604-166">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="d0604-167">Pomijanie ostrzeżeń, definiując rozdzielaną średnikami listę kodów Ostrzeżenie można zignorować w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="d0604-167">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file.</span></span> <span data-ttu-id="d0604-168">Dołączanie kody ostrzeżenie `$(NoWarn);` zbyt stosuje ustawienia domyślne języka C#.</span><span class="sxs-lookup"><span data-stu-id="d0604-168">Appending the warning codes to `$(NoWarn);` applies the C# default values too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="d0604-169">Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="d0604-169">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="d0604-170">Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="d0604-170">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="d0604-171">Na przykład *TodoApi.XML* pliku jest prawidłowy dla systemu Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="d0604-171">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="d0604-172">W powyższym kodzie [odbicia](/dotnet/csharp/programming-guide/concepts/reflection) jest używany do tworzenia nazwę pliku XML odpowiadającym, który projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0604-172">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="d0604-173">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) właściwość jest używana do skonstruowania ścieżki do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="d0604-173">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="d0604-174">Dodawanie komentarzy potrójnym ukośnikiem na akcję podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do nagłówku sekcji.</span><span class="sxs-lookup"><span data-stu-id="d0604-174">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="d0604-175">Dodaj [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) element powyżej `Delete` akcji:</span><span class="sxs-lookup"><span data-stu-id="d0604-175">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="d0604-176">Interfejs użytkownika programu Swagger wyświetlany jest tekst wewnętrzny poprzedni kod `<summary>` elementu:</span><span class="sxs-lookup"><span data-stu-id="d0604-176">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="d0604-179">Interfejs użytkownika jest wymuszany przez wygenerowane schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="d0604-179">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="d0604-180">Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="d0604-180">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="d0604-181">Uzupełnia on informacje określone w `<summary>` element i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="d0604-181">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="d0604-182">`<remarks>` Zawartości elementu może składać się tekst JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="d0604-182">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="d0604-183">Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe uwagi:</span><span class="sxs-lookup"><span data-stu-id="d0604-183">Notice the UI enhancements with these additional comments:</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="d0604-185">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="d0604-185">Data annotations</span></span>

<span data-ttu-id="d0604-186">Dekoracji modelu z atrybutami, znalezione w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, ułatwiające dysków składniki interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="d0604-186">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="d0604-187">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="d0604-187">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="d0604-188">Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="d0604-188">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="d0604-189">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d0604-189">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="d0604-190">Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zawartości odpowiedzi *application/json*:</span><span class="sxs-lookup"><span data-stu-id="d0604-190">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="d0604-191">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="d0604-191">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="d0604-193">Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d0604-193">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="d0604-194">Opis typów odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d0604-194">Describe response types</span></span>

<span data-ttu-id="d0604-195">Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą&mdash;w szczególności typy odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="d0604-195">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="d0604-196">Kody błędów i typów odpowiedzi są wskazywane w adnotacjach komentarzy i danych XML.</span><span class="sxs-lookup"><span data-stu-id="d0604-196">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="d0604-197">`Create` Akcji zwraca kod stanu 201 protokołu HTTP w przypadku powodzenia.</span><span class="sxs-lookup"><span data-stu-id="d0604-197">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="d0604-198">Kod stanu HTTP 400 jest zwracany, gdy treść żądania przesłanego ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="d0604-198">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="d0604-199">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="d0604-199">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="d0604-200">Rozwiązać ten problem, dodając wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d0604-200">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="d0604-201">Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="d0604-201">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="d0604-203">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="d0604-203">Customize the UI</span></span>

<span data-ttu-id="d0604-204">Zasoby interfejsu użytkownika jest funkcjonalności i presentable.</span><span class="sxs-lookup"><span data-stu-id="d0604-204">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="d0604-205">Jednak stron z dokumentacją interfejsu API powinno reprezentować Twojej markę lub motywu.</span><span class="sxs-lookup"><span data-stu-id="d0604-205">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="d0604-206">Składniki pakietu Swashbuckle znakowania wymaga Dodawanie zasobów do obsługi plików statycznych i tworzenia struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="d0604-206">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="d0604-207">Jeśli przeznaczonych dla platformy .NET Framework lub .NET Core 1.x, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="d0604-207">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="d0604-208">Poprzedni pakiet NuGet jest już zainstalowana, jeśli przeznaczonych dla platformy .NET Core 2.x i przy użyciu [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d0604-208">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="d0604-209">Włącza oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="d0604-209">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="d0604-210">Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="d0604-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="d0604-211">Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="d0604-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="d0604-212">Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="d0604-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="d0604-213">Utwórz *custome.CSS* pliku w *wwwroot/swagger/interfejsu użytkownika*, z następujących CSS, aby dostosować nagłówka strony:</span><span class="sxs-lookup"><span data-stu-id="d0604-213">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="d0604-214">Odwołanie *custome.CSS* w *index.html* pliku po innych plików CSS:</span><span class="sxs-lookup"><span data-stu-id="d0604-214">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="d0604-215">Przejdź do *index.html* pod `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="d0604-215">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="d0604-216">Wprowadź `http://localhost:<port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d0604-216">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="d0604-217">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="d0604-217">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="d0604-219">Jest znacznie więcej można zrobić za pomocą strony.</span><span class="sxs-lookup"><span data-stu-id="d0604-219">There's much more you can do with the page.</span></span> <span data-ttu-id="d0604-220">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="d0604-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
