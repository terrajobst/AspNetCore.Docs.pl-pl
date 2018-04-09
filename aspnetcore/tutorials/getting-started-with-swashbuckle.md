---
title: Rozpoczynanie pracy z Swashbuckle i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak dodać pakiet Swashbuckle do projektu platformy ASP.NET Core integracja interfejsu użytkownika programu Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="41342-103">Rozpoczynanie pracy z Swashbuckle i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41342-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="41342-104">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="41342-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="41342-105">Istnieją trzy główne składniki do Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="41342-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="41342-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.</span><span class="sxs-lookup"><span data-stu-id="41342-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="41342-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="41342-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="41342-108">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="41342-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="41342-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="41342-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="41342-110">Interpretuje ją JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="41342-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="41342-111">Obejmuje on przewodów wbudowanych testu dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="41342-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="41342-112">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="41342-112">Package installation</span></span>

<span data-ttu-id="41342-113">Pakiet Swashbuckle można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="41342-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41342-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41342-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="41342-115">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="41342-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="41342-116">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="41342-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="41342-117">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="41342-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="41342-118">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="41342-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="41342-119">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="41342-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="41342-120">Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="41342-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="41342-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41342-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="41342-122">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="41342-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="41342-123">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="41342-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="41342-124">W polu wyszukiwania wprowadź Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="41342-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="41342-125">Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="41342-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="41342-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41342-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="41342-127">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="41342-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="41342-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="41342-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="41342-129">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="41342-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="41342-130">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="41342-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="41342-131">Dodaj Swagger generator kolekcji usług w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="41342-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="41342-132">Zaimportuj następujące przestrzeń nazw za pomocą `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="41342-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="41342-133">W `Startup.Configure` metody, włącza oprogramowanie pośredniczące do obsługi interfejsu użytkownika programu Swagger i wygenerowanego dokumentów JSON:</span><span class="sxs-lookup"><span data-stu-id="41342-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="41342-134">Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="41342-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="41342-135">Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano w [Swagger specyfikacji (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="41342-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="41342-136">Interfejs użytkownika programu Swagger można znaleźć w folderze `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="41342-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="41342-137">Eksploruj interfejsu API przez interfejs użytkownika programu Swagger i uwzględnić go w innych programów.</span><span class="sxs-lookup"><span data-stu-id="41342-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="41342-138">Do obsługi interfejsu użytkownika programu Swagger w katalogu głównym aplikacji (`http://localhost:<random_port>/`) ustaw `RoutePrefix` właściwości na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="41342-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="41342-139">Dostosowywanie i rozszerzanie</span><span class="sxs-lookup"><span data-stu-id="41342-139">Customize & extend</span></span>

<span data-ttu-id="41342-140">Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.</span><span class="sxs-lookup"><span data-stu-id="41342-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="41342-141">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="41342-141">API info and description</span></span>

<span data-ttu-id="41342-142">Akcja konfiguracji przekazany do `AddSwaggerGen` metoda dodaje informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="41342-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="41342-143">Interfejs użytkownika programu Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="41342-143">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="41342-145">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="41342-145">XML comments</span></span>

<span data-ttu-id="41342-146">Komentarze XML można włączyć z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="41342-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="41342-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41342-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="41342-148">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="41342-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="41342-149">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:</span><span class="sxs-lookup"><span data-stu-id="41342-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="41342-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41342-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="41342-152">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="41342-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="41342-153">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:</span><span class="sxs-lookup"><span data-stu-id="41342-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="41342-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41342-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="41342-156">Ręcznie dodaj poniższy fragment do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="41342-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="41342-157">Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="41342-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="41342-158">Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="41342-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="41342-159">Na przykład następujący komunikat wskazuje naruszenie kod ostrzeżenia 1591:</span><span class="sxs-lookup"><span data-stu-id="41342-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="41342-160">Pomijanie ostrzeżeń, definiując rozdzielaną średnikami listę kodów Ostrzeżenie można zignorować w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="41342-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="41342-161">Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="41342-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="41342-162">Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="41342-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="41342-163">Na przykład *TodoApi.XML* pliku jest prawidłowy dla systemu Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="41342-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="41342-164">W powyższym kodzie [odbicia](/dotnet/csharp/programming-guide/concepts/reflection) jest używany do tworzenia nazwę pliku XML odpowiadającym, który projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="41342-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="41342-165">To podejście zapewnia zgodność wygenerowana nazwa pliku XML nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="41342-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="41342-166">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) właściwość jest używana do skonstruowania ścieżki do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="41342-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="41342-167">Dodawanie komentarzy potrójnym ukośnikiem na akcję podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do nagłówku sekcji.</span><span class="sxs-lookup"><span data-stu-id="41342-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="41342-168">Dodaj [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) element powyżej `Delete` akcji:</span><span class="sxs-lookup"><span data-stu-id="41342-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="41342-169">Interfejs użytkownika programu Swagger wyświetlany jest tekst wewnętrzny poprzedni kod `<summary>` elementu:</span><span class="sxs-lookup"><span data-stu-id="41342-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="41342-172">Interfejs użytkownika jest wymuszany przez wygenerowane schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="41342-172">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="41342-173">Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="41342-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="41342-174">Uzupełnia on informacje określone w `<summary>` element i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="41342-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="41342-175">`<remarks>` Zawartości elementu może składać się tekst JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="41342-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="41342-176">Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe uwagi:</span><span class="sxs-lookup"><span data-stu-id="41342-176">Notice the UI enhancements with these additional comments:</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="41342-178">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="41342-178">Data annotations</span></span>

<span data-ttu-id="41342-179">Dekoracji modelu z atrybutami, znalezione w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, ułatwiające dysków składniki interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="41342-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="41342-180">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="41342-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="41342-181">Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="41342-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="41342-182">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="41342-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="41342-183">Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zawartości odpowiedzi *application/json*:</span><span class="sxs-lookup"><span data-stu-id="41342-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="41342-184">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="41342-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="41342-186">Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="41342-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="41342-187">Opis typów odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="41342-187">Describe response types</span></span>

<span data-ttu-id="41342-188">Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą&mdash;w szczególności typy odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="41342-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="41342-189">Kody błędów i typów odpowiedzi są wskazywane w adnotacjach komentarzy i danych XML.</span><span class="sxs-lookup"><span data-stu-id="41342-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="41342-190">`Create` Akcji zwraca kod stanu 201 protokołu HTTP w przypadku powodzenia.</span><span class="sxs-lookup"><span data-stu-id="41342-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="41342-191">Kod stanu HTTP 400 jest zwracany, gdy treść żądania przesłanego ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="41342-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="41342-192">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="41342-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="41342-193">Rozwiązać ten problem, dodając wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="41342-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="41342-194">Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="41342-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="41342-196">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="41342-196">Customize the UI</span></span>

<span data-ttu-id="41342-197">Zasoby interfejsu użytkownika jest funkcjonalności i presentable.</span><span class="sxs-lookup"><span data-stu-id="41342-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="41342-198">Jednak stron z dokumentacją interfejsu API powinno reprezentować Twojej markę lub motywu.</span><span class="sxs-lookup"><span data-stu-id="41342-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="41342-199">Składniki pakietu Swashbuckle znakowania wymaga Dodawanie zasobów do obsługi plików statycznych i tworzenia struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="41342-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="41342-200">Jeśli przeznaczonych dla platformy .NET Framework lub .NET Core 1.x, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="41342-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="41342-201">Poprzedni pakiet NuGet jest już zainstalowana, jeśli przeznaczonych dla platformy .NET Core 2.x i przy użyciu [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="41342-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="41342-202">Włącza oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="41342-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="41342-203">Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="41342-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="41342-204">Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="41342-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="41342-205">Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="41342-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="41342-206">Utwórz *custome.CSS* pliku w *wwwroot/swagger/interfejsu użytkownika*, z następujących CSS, aby dostosować nagłówka strony:</span><span class="sxs-lookup"><span data-stu-id="41342-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="41342-207">Odwołanie *custome.CSS* w *index.html* pliku po innych plików CSS:</span><span class="sxs-lookup"><span data-stu-id="41342-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="41342-208">Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="41342-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="41342-209">Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="41342-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="41342-210">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="41342-210">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="41342-212">Jest znacznie więcej można zrobić za pomocą strony.</span><span class="sxs-lookup"><span data-stu-id="41342-212">There's much more you can do with the page.</span></span> <span data-ttu-id="41342-213">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="41342-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
