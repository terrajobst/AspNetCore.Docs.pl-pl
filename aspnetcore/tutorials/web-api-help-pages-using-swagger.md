---
title: "Platformy ASP.NET Core interfejsu API sieci Web pomocy strony za pomocą programu Swagger"
author: spboyer
description: "Ten samouczek zawiera wskazówki dodawania programu Swagger do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
manager: wpickett
ms.author: spboyer
ms.date: 02/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 95dc89fbd9aa3038b7ec500121db4345de9765c6
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="acc18-103">Platformy ASP.NET Core interfejsu API sieci Web pomocy strony za pomocą programu Swagger</span><span class="sxs-lookup"><span data-stu-id="acc18-103">ASP.NET Core Web API help pages using Swagger</span></span>

<span data-ttu-id="acc18-104">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="acc18-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="acc18-105">Opis różnych metod interfejsu API może być wyzwaniem dla dewelopera podczas kompilowania odbierającą aplikację.</span><span class="sxs-lookup"><span data-stu-id="acc18-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming app.</span></span>

<span data-ttu-id="acc18-106">Generowanie dobrej stron pomocy i dokumentacja dla interfejsu API sieci Web, przy użyciu [Swagger](https://swagger.io/) z implementacją .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), wystarczy kilka pakietów NuGet Dodawanie i modyfikowanie *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="acc18-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="acc18-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) to projekt open source służący do generowania dokumentów programu Swagger dla interfejsów API platformy ASP.NET Core sieci Web.</span><span class="sxs-lookup"><span data-stu-id="acc18-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="acc18-108">[Swagger](https://swagger.io/) czytelną reprezentacja interfejsu API RESTful, umożliwiającą obsługę interakcyjne dokumentacji, generowanie zestawów SDK klientów i odnajdywania.</span><span class="sxs-lookup"><span data-stu-id="acc18-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="acc18-109">W tym samouczku opiera się na przykład [budynku swój pierwszy interfejsu API sieci Web platformy ASP.NET Core MVC i Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="acc18-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="acc18-110">Jeśli chcesz z niego skorzystać, Pobierz przykład na [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="acc18-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="get-started"></a><span data-ttu-id="acc18-111">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="acc18-111">Get started</span></span>

<span data-ttu-id="acc18-112">Istnieją trzy główne składniki do Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="acc18-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="acc18-113">`Swashbuckle.AspNetCore.Swagger`: model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.</span><span class="sxs-lookup"><span data-stu-id="acc18-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="acc18-114">`Swashbuckle.AspNetCore.SwaggerGen`: generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="acc18-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="acc18-115">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="acc18-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="acc18-116">`Swashbuckle.AspNetCore.SwaggerUI`: wbudowana wersja narzędzia interfejs użytkownika programu Swagger, które będą interpretowane przez JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="acc18-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="acc18-117">Obejmuje on przewodów wbudowanych testu dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="acc18-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="acc18-118">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="acc18-118">NuGet packages</span></span>

<span data-ttu-id="acc18-119">Pakiet Swashbuckle można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="acc18-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="acc18-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acc18-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="acc18-121">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="acc18-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="acc18-122">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="acc18-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="acc18-123">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="acc18-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="acc18-124">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="acc18-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="acc18-125">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="acc18-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="acc18-126">Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="acc18-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="acc18-127">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="acc18-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="acc18-128">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="acc18-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="acc18-129">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="acc18-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="acc18-130">W polu wyszukiwania wprowadź Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="acc18-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="acc18-131">Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="acc18-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="acc18-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="acc18-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="acc18-133">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="acc18-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="acc18-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="acc18-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="acc18-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="acc18-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="acc18-136">Dodawanie i konfigurowanie programu Swagger do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="acc18-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="acc18-137">Dodaj Swagger generator kolekcji usług w `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="acc18-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="acc18-138">Dodaj następujący kod za pomocą instrukcji dla `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="acc18-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="acc18-139">W `Configure` metody *Startup.cs*, włącza oprogramowanie pośredniczące dla obsługująca wygenerowanego dokument JSON i SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="acc18-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="acc18-140">Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="acc18-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="acc18-141">Pojawi się wygenerowanym opisujący punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="acc18-141">The generated document describing the endpoints appears.</span></span>

> [!NOTE]
> <span data-ttu-id="acc18-142">Microsoft Edge, Google Chrome i Firefox natywnie wyświetlanie dokumentów JSON.</span><span class="sxs-lookup"><span data-stu-id="acc18-142">Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="acc18-143">Brak rozszerzenia dla programu Chrome, które formatowania dokumentu, aby ułatwić czytanie.</span><span class="sxs-lookup"><span data-stu-id="acc18-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="acc18-144">*Poniższy przykład, zostanie zmniejszona do skrócenia.*</span><span class="sxs-lookup"><span data-stu-id="acc18-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="acc18-145">Ten dokument dyski interfejsu użytkownika programu Swagger, który można wyświetlić, przechodząc do `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="acc18-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="acc18-147">Każda metoda akcji publicznego w `TodoController` można przetestować w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="acc18-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="acc18-148">Kliknij nazwę metody, aby rozwinąć sekcję.</span><span class="sxs-lookup"><span data-stu-id="acc18-148">Click a method name to expand the section.</span></span> <span data-ttu-id="acc18-149">Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk "Wypróbuj ją!".</span><span class="sxs-lookup"><span data-stu-id="acc18-149">Add any necessary parameters, and click "Try it out!".</span></span>

![Przykład struktury Swagger UZYSKAĆ testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customize--extend"></a><span data-ttu-id="acc18-151">Dostosowywanie i rozszerzanie</span><span class="sxs-lookup"><span data-stu-id="acc18-151">Customize & extend</span></span>

<span data-ttu-id="acc18-152">Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.</span><span class="sxs-lookup"><span data-stu-id="acc18-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="acc18-153">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="acc18-153">API info and description</span></span>

<span data-ttu-id="acc18-154">Akcja konfiguracji przekazany do `AddSwaggerGen` metody można użyć do dodawania informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="acc18-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="acc18-155">Poniższa ilustracja przedstawia interfejsu użytkownika programu Swagger, wyświetlane informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="acc18-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="acc18-157">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="acc18-157">XML comments</span></span>

<span data-ttu-id="acc18-158">Komentarze XML można włączyć z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="acc18-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="acc18-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acc18-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="acc18-160">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="acc18-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="acc18-161">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:</span><span class="sxs-lookup"><span data-stu-id="acc18-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="acc18-163">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="acc18-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="acc18-164">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="acc18-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="acc18-165">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:</span><span class="sxs-lookup"><span data-stu-id="acc18-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="acc18-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="acc18-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="acc18-168">Ręcznie dodaj poniższy fragment do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="acc18-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=8-9,11)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="acc18-169">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="acc18-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="acc18-170">Zobacz kod programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acc18-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="acc18-171">Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="acc18-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="acc18-172">Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="acc18-172">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="acc18-173">Na przykład następujący komunikat wskazuje naruszenie kod ostrzeżenia 1591:</span><span class="sxs-lookup"><span data-stu-id="acc18-173">For example, the following message indicates a violation of warning code 1591:</span></span>

```
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="acc18-174">Pomijanie ostrzeżeń, definiując rozdzielaną średnikami listę kodów Ostrzeżenie można zignorować w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="acc18-174">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="acc18-175">Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="acc18-175">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="acc18-176">Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="acc18-176">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="acc18-177">Na przykład *ToDoApi.XML* będzie można znaleźć pliku w systemie Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="acc18-177">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="acc18-178">W powyższym kodzie `ApplicationBasePath` pobiera podstawowa ścieżka aplikacji.</span><span class="sxs-lookup"><span data-stu-id="acc18-178">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="acc18-179">Podstawowa ścieżka jest używana do lokalizowania komentarze pliku XML.</span><span class="sxs-lookup"><span data-stu-id="acc18-179">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="acc18-180">*TodoApi.xml* działa tylko w tym przykładzie, ponieważ nazwa wygenerowanego kodu XML komentarze pliku opiera się na nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="acc18-180">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the app name.</span></span>

<span data-ttu-id="acc18-181">Dodawanie komentarzy potrójnym ukośnikiem do metody podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do Nagłówek sekcji:</span><span class="sxs-lookup"><span data-stu-id="acc18-181">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="acc18-184">Interfejs użytkownika jest wymuszany przez wygenerowanego pliku JSON, który zawiera również te komentarze:</span><span class="sxs-lookup"><span data-stu-id="acc18-184">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="acc18-185">Dodaj [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) znacznika `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="acc18-185">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="acc18-186">Uzupełnia on informacje określone w `<summary>` tagu i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="acc18-186">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="acc18-187">`<remarks>` Zawartości tagu może składać się tekst JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="acc18-187">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="acc18-188">Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe komentarze.</span><span class="sxs-lookup"><span data-stu-id="acc18-188">Notice the UI enhancements with these additional comments.</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="acc18-190">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="acc18-190">Data annotations</span></span>

<span data-ttu-id="acc18-191">Dekoracji modelu z atrybutami, znalezione w `System.ComponentModel.DataAnnotations`, aby pomóc dysków składniki interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="acc18-191">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="acc18-192">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="acc18-192">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="acc18-193">Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="acc18-193">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="acc18-194">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="acc18-194">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="acc18-195">Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zwracany typ zawartości *application/json*:</span><span class="sxs-lookup"><span data-stu-id="acc18-195">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="acc18-196">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="acc18-196">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="acc18-198">Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="acc18-198">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="acc18-199">Opis typów odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="acc18-199">Describe response types</span></span>

<span data-ttu-id="acc18-200">Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą &mdash; w szczególności typy odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="acc18-200">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="acc18-201">Te są obsługiwane w adnotacjach komentarzy i danych XML.</span><span class="sxs-lookup"><span data-stu-id="acc18-201">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="acc18-202">`Create` Akcji zwraca `201 Created` w przypadku powodzenia lub `400 Bad Request` gdy treść żądania przesłanego ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="acc18-202">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="acc18-203">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="acc18-203">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="acc18-204">Ten problem został rozwiązany przez dodanie wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="acc18-204">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="acc18-205">Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="acc18-205">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="acc18-207">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="acc18-207">Customize the UI</span></span>

<span data-ttu-id="acc18-208">Zasoby interfejsu użytkownika jest funkcjonalności i presentable; Jednak podczas kompilowania stron z dokumentacją do interfejsu API, ma się reprezentować motywu lub marki, na których.</span><span class="sxs-lookup"><span data-stu-id="acc18-208">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="acc18-209">Wykonywanie zadań tego zadania ze składnikami Swashbuckle wymaga Dodawanie zasobów do obsługi plików statycznych, a następnie tworzenie struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="acc18-209">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="acc18-210">Jeśli przeznaczonych dla platformy .NET Framework, Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="acc18-210">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="acc18-211">Włącza oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="acc18-211">Enable the static files middleware:</span></span>

[!code-csharp[](web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="acc18-212">Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="acc18-212">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="acc18-213">Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="acc18-213">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="acc18-214">Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="acc18-214">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="acc18-215">Utwórz *wwwroot/swagger/ui/css/custom.css* pliku z następujących CSS, aby dostosować nagłówka strony:</span><span class="sxs-lookup"><span data-stu-id="acc18-215">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[](web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="acc18-216">Odwołanie *custome.CSS* w *index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="acc18-216">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[](web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="acc18-217">Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="acc18-217">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="acc18-218">Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="acc18-218">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="acc18-219">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="acc18-219">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="acc18-221">Jest znacznie więcej można zrobić za pomocą strony.</span><span class="sxs-lookup"><span data-stu-id="acc18-221">There's much more you can do with the page.</span></span> <span data-ttu-id="acc18-222">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="acc18-222">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
