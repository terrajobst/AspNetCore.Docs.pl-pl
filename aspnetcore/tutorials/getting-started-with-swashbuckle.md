---
title: Rozpoczynanie pracy z Swashbuckle
author: zuckerthoben
description: "Ten samouczek zawiera wskazówki dodawania Swashbuckle do projektu do integracji interfejsu użytkownika programu Swagger."
keywords: Platformy ASP.NET Core, programu Swagger, Swashbuckle, strony, interfejsu API sieci Web pomocy
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: aaeaf9b776a5fd98900f2adf04468becec216da0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-swashbuckle"></a><span data-ttu-id="dacbb-104">Rozpoczynanie pracy z Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="dacbb-104">Get started with Swashbuckle</span></span>

<span data-ttu-id="dacbb-105">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dacbb-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dacbb-106">Istnieją trzy główne składniki do Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="dacbb-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="dacbb-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.</span><span class="sxs-lookup"><span data-stu-id="dacbb-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="dacbb-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="dacbb-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="dacbb-109">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="dacbb-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="dacbb-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika programu Swagger, które będą interpretowane przez JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dacbb-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="dacbb-111">Obejmuje on przewodów wbudowanych testu dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="dacbb-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="dacbb-112">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="dacbb-112">Package installation</span></span>

<span data-ttu-id="dacbb-113">Pakiet Swashbuckle można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="dacbb-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dacbb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dacbb-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dacbb-115">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="dacbb-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="dacbb-116">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="dacbb-116">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="dacbb-117">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="dacbb-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="dacbb-118">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="dacbb-118">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="dacbb-119">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="dacbb-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="dacbb-120">Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="dacbb-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dacbb-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dacbb-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dacbb-122">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="dacbb-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="dacbb-123">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="dacbb-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="dacbb-124">W polu wyszukiwania wprowadź Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="dacbb-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="dacbb-125">Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="dacbb-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dacbb-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dacbb-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dacbb-127">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="dacbb-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dacbb-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="dacbb-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dacbb-129">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dacbb-129">Run the following command:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="dacbb-130">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="dacbb-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="dacbb-131">Dodaj Swagger generator kolekcji usług w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="dacbb-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="dacbb-132">Importowanie następujących przestrzeni nazw w `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="dacbb-132">Import the following namespaces in the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="dacbb-133">W `Startup.Configure` metody, włącza oprogramowanie pośredniczące do obsługi interfejsu użytkownika programu Swagger i wygenerowanego dokumentów JSON:</span><span class="sxs-lookup"><span data-stu-id="dacbb-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="dacbb-134">Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="dacbb-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="dacbb-135">Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano w [Swagger specyfikacji (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="dacbb-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="dacbb-136">Interfejs użytkownika programu Swagger można znaleźć w folderze `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="dacbb-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="dacbb-137">Teraz można eksplorować przez interfejs użytkownika programu Swagger interfejsu API i uwzględnić go w innych programów.</span><span class="sxs-lookup"><span data-stu-id="dacbb-137">Now you can explore the API via Swagger UI and incorporate it in other programs.</span></span>

## <a name="customize--extend"></a><span data-ttu-id="dacbb-138">Dostosowywanie i rozszerzanie</span><span class="sxs-lookup"><span data-stu-id="dacbb-138">Customize & extend</span></span>

<span data-ttu-id="dacbb-139">Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.</span><span class="sxs-lookup"><span data-stu-id="dacbb-139">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="dacbb-140">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="dacbb-140">API info and description</span></span>

<span data-ttu-id="dacbb-141">Akcja konfiguracji przekazany do `AddSwaggerGen` metody można użyć do dodawania informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="dacbb-141">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?range=20-30,36)]

<span data-ttu-id="dacbb-142">Poniższa ilustracja przedstawia interfejsu użytkownika programu Swagger, wyświetlane informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="dacbb-142">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="dacbb-144">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="dacbb-144">XML comments</span></span>

<span data-ttu-id="dacbb-145">Komentarze XML można włączyć z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="dacbb-145">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="dacbb-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dacbb-146">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="dacbb-147">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="dacbb-147">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="dacbb-148">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:</span><span class="sxs-lookup"><span data-stu-id="dacbb-148">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="dacbb-150">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dacbb-150">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="dacbb-151">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="dacbb-151">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="dacbb-152">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:</span><span class="sxs-lookup"><span data-stu-id="dacbb-152">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="dacbb-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dacbb-154">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="dacbb-155">Ręcznie dodaj poniższy fragment do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="dacbb-155">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?range=7-9)]

---

<span data-ttu-id="dacbb-156">Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="dacbb-156">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="dacbb-157">Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="dacbb-157">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="dacbb-158">Na przykład następujący komunikat wskazuje naruszenie kod ostrzeżenia 1591:</span><span class="sxs-lookup"><span data-stu-id="dacbb-158">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="dacbb-159">Pomijanie ostrzeżeń, definiując rozdzielaną średnikami listę kodów Ostrzeżenie można zignorować w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="dacbb-159">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="dacbb-160">Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="dacbb-160">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="dacbb-161">Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="dacbb-161">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="dacbb-162">Na przykład *TodoApi.Swashbuckle.XML* pliku jest prawidłowy dla systemu Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="dacbb-162">For example, a *TodoApi.Swashbuckle.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="dacbb-163">W powyższym kodzie `ApplicationBasePath` pobiera podstawowa ścieżka aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dacbb-163">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="dacbb-164">Podstawowa ścieżka jest używana do lokalizowania komentarze pliku XML.</span><span class="sxs-lookup"><span data-stu-id="dacbb-164">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="dacbb-165">*TodoApi.Swashbuckle.xml* działa tylko w tym przykładzie, ponieważ nazwa wygenerowanego kodu XML komentarze pliku opiera się na nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dacbb-165">*TodoApi.Swashbuckle.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="dacbb-166">Dodawanie komentarzy potrójnym ukośnikiem do metody podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do Nagłówek sekcji:</span><span class="sxs-lookup"><span data-stu-id="dacbb-166">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="dacbb-169">Interfejs użytkownika jest wymuszany przez wygenerowanego pliku JSON, który zawiera również te komentarze:</span><span class="sxs-lookup"><span data-stu-id="dacbb-169">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="dacbb-170">Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) znacznika `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="dacbb-170">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="dacbb-171">Uzupełnia on informacje określone w [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) tagu i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="dacbb-171">It supplements information specified in the [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="dacbb-172">`<remarks>` Zawartości tagu może składać się tekst JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="dacbb-172">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="dacbb-173">Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe komentarze.</span><span class="sxs-lookup"><span data-stu-id="dacbb-173">Notice the UI enhancements with these additional comments.</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="dacbb-175">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="dacbb-175">Data annotations</span></span>

<span data-ttu-id="dacbb-176">Dekoracji modelu z atrybutami, znalezione w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, ułatwiające dysków składniki interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="dacbb-176">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="dacbb-177">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="dacbb-177">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="dacbb-178">Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="dacbb-178">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="dacbb-179">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="dacbb-179">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="dacbb-180">Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zawartości odpowiedzi *application/json*:</span><span class="sxs-lookup"><span data-stu-id="dacbb-180">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="dacbb-181">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="dacbb-181">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="dacbb-183">Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="dacbb-183">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="dacbb-184">Opis typów odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="dacbb-184">Describe response types</span></span>

<span data-ttu-id="dacbb-185">Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą&mdash;w szczególności typy odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="dacbb-185">Consuming developers are most concerned with what is returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="dacbb-186">Te są obsługiwane w adnotacjach komentarzy i danych XML.</span><span class="sxs-lookup"><span data-stu-id="dacbb-186">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="dacbb-187">`Create` Akcji zwraca `201 Created` w przypadku powodzenia lub `400 Bad Request` gdy treść żądania przesłanego ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="dacbb-187">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="dacbb-188">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="dacbb-188">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="dacbb-189">Ten problem został rozwiązany przez dodanie wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="dacbb-189">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="dacbb-190">Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="dacbb-190">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="dacbb-192">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="dacbb-192">Customize the UI</span></span>

<span data-ttu-id="dacbb-193">Zasoby interfejsu użytkownika jest funkcjonalności i presentable; Jednak podczas kompilowania stron z dokumentacją do interfejsu API, ma się reprezentować motywu lub marki, na których.</span><span class="sxs-lookup"><span data-stu-id="dacbb-193">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="dacbb-194">Wykonywanie zadań tego zadania ze składnikami Swashbuckle wymaga Dodawanie zasobów do obsługi plików statycznych, a następnie tworzenie struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="dacbb-194">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="dacbb-195">Jeśli przeznaczonych dla platformy .NET Framework lub .NET Core 1.x, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="dacbb-195">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="dacbb-196">Poprzedni pakiet NuGet jest już zainstalowana, jeśli przeznaczonych dla platformy .NET Core 2.x i przy użyciu [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="dacbb-196">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="dacbb-197">Włącza oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="dacbb-197">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="dacbb-198">Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="dacbb-198">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="dacbb-199">Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="dacbb-199">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="dacbb-200">Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="dacbb-200">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="dacbb-201">Utwórz *wwwroot/swagger/ui/css/custom.css* pliku z następujących CSS, aby dostosować nagłówka strony:</span><span class="sxs-lookup"><span data-stu-id="dacbb-201">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="dacbb-202">Odwołanie *custome.CSS* w *index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="dacbb-202">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="dacbb-203">Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="dacbb-203">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="dacbb-204">Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="dacbb-204">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="dacbb-205">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="dacbb-205">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="dacbb-207">Jest znacznie więcej można zrobić za pomocą strony.</span><span class="sxs-lookup"><span data-stu-id="dacbb-207">There's much more you can do with the page.</span></span> <span data-ttu-id="dacbb-208">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="dacbb-208">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
