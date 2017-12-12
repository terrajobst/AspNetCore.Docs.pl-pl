---
title: "ASP.NET Core stron sieci Web interfejsu API pomocy przy użyciu programu Swagger"
author: spboyer
description: "Ten samouczek zawiera wskazówki dodawania programu Swagger do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
keywords: Platformy ASP.NET Core, programu Swagger, Swashbuckle, strony, interfejsu API sieci Web pomocy
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 8a87972a7394246ece2af3485d93739975ba5383
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="0e20a-104">ASP.NET Core stron sieci Web interfejsu API pomocy przy użyciu programu Swagger</span><span class="sxs-lookup"><span data-stu-id="0e20a-104">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="0e20a-105">Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="0e20a-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="0e20a-106">Opis różnych metod interfejsu API może być wyzwaniem dla dewelopera podczas kompilowania odbierającą aplikację.</span><span class="sxs-lookup"><span data-stu-id="0e20a-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="0e20a-107">Generowanie dobrej stron pomocy i dokumentacja dla interfejsu API sieci Web, przy użyciu [Swagger](https://swagger.io/) z implementacją .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), wystarczy kilka pakietów NuGet Dodawanie i modyfikowanie *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0e20a-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="0e20a-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) to projekt open source służący do generowania dokumentów programu Swagger dla interfejsów API platformy ASP.NET Core sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0e20a-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="0e20a-109">[Swagger](https://swagger.io/) czytelną reprezentacja interfejsu API RESTful, umożliwiającą obsługę interakcyjne dokumentacji, generowanie zestawów SDK klientów i odnajdywania.</span><span class="sxs-lookup"><span data-stu-id="0e20a-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="0e20a-110">W tym samouczku opiera się na przykład [budynku swój pierwszy interfejsu API sieci Web platformy ASP.NET Core MVC i Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="0e20a-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="0e20a-111">Jeśli chcesz z niego skorzystać, Pobierz przykład na [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="0e20a-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="0e20a-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="0e20a-112">Getting Started</span></span>

<span data-ttu-id="0e20a-113">Istnieją trzy główne składniki do Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="0e20a-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="0e20a-114">`Swashbuckle.AspNetCore.Swagger`: model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.</span><span class="sxs-lookup"><span data-stu-id="0e20a-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="0e20a-115">`Swashbuckle.AspNetCore.SwaggerGen`: generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="0e20a-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="0e20a-116">Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="0e20a-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="0e20a-117">`Swashbuckle.AspNetCore.SwaggerUI`: wbudowana wersja narzędzia interfejs użytkownika programu Swagger, które będą interpretowane przez JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0e20a-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="0e20a-118">Obejmuje on przewodów wbudowanych testu dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="0e20a-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="0e20a-119">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="0e20a-119">NuGet Packages</span></span>

<span data-ttu-id="0e20a-120">Pakiet Swashbuckle można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="0e20a-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e20a-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e20a-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0e20a-122">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="0e20a-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="0e20a-123">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="0e20a-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="0e20a-124">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="0e20a-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="0e20a-125">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="0e20a-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="0e20a-126">W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="0e20a-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="0e20a-127">Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="0e20a-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0e20a-128">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="0e20a-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0e20a-129">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="0e20a-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="0e20a-130">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="0e20a-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="0e20a-131">W polu wyszukiwania wprowadź Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="0e20a-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="0e20a-132">Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="0e20a-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e20a-133">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e20a-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0e20a-134">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="0e20a-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0e20a-135">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0e20a-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0e20a-136">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0e20a-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="0e20a-137">Dodawanie i konfigurowanie programu Swagger do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="0e20a-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="0e20a-138">Dodaj Swagger generator kolekcji usług w `ConfigureServices` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e20a-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="0e20a-139">Dodaj następujący kod za pomocą instrukcji dla `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="0e20a-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="0e20a-140">W `Configure` metody *Startup.cs*, włącza oprogramowanie pośredniczące dla obsługująca wygenerowanego dokument JSON i SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="0e20a-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="0e20a-141">Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="0e20a-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="0e20a-142">Pojawi się wygenerowanym opisujący punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="0e20a-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="0e20a-143">**Uwaga:** Microsoft Edge, Google Chrome i Firefox wyświetlanie dokumentów JSON natywnie.</span><span class="sxs-lookup"><span data-stu-id="0e20a-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="0e20a-144">Brak rozszerzenia dla programu Chrome, które formatowania dokumentu, aby ułatwić czytanie.</span><span class="sxs-lookup"><span data-stu-id="0e20a-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="0e20a-145">*Poniższy przykład, zostanie zmniejszona do skrócenia.*</span><span class="sxs-lookup"><span data-stu-id="0e20a-145">*The following example is reduced for brevity.*</span></span>

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

<span data-ttu-id="0e20a-146">Ten dokument dyski interfejsu użytkownika programu Swagger, który można wyświetlić, przechodząc do `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="0e20a-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="0e20a-148">Każda metoda akcji publicznego w `TodoController` można przetestować w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e20a-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="0e20a-149">Kliknij nazwę metody, aby rozwinąć sekcję.</span><span class="sxs-lookup"><span data-stu-id="0e20a-149">Click a method name to expand the section.</span></span> <span data-ttu-id="0e20a-150">Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk "Wypróbuj ją!".</span><span class="sxs-lookup"><span data-stu-id="0e20a-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Przykład struktury Swagger UZYSKAĆ testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="0e20a-152">Dostosowywanie & rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="0e20a-152">Customization & Extensibility</span></span>

<span data-ttu-id="0e20a-153">Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.</span><span class="sxs-lookup"><span data-stu-id="0e20a-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="0e20a-154">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="0e20a-154">API Info and Description</span></span>

<span data-ttu-id="0e20a-155">Akcja konfiguracji przekazany do `AddSwaggerGen` metody można użyć do dodawania informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="0e20a-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="0e20a-156">Poniższa ilustracja przedstawia interfejsu użytkownika programu Swagger, wyświetlane informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="0e20a-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="0e20a-158">Komentarze XML</span><span class="sxs-lookup"><span data-stu-id="0e20a-158">XML Comments</span></span>

<span data-ttu-id="0e20a-159">Komentarze XML można włączyć z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="0e20a-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e20a-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e20a-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0e20a-161">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="0e20a-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="0e20a-162">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:</span><span class="sxs-lookup"><span data-stu-id="0e20a-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0e20a-164">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="0e20a-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0e20a-165">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="0e20a-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="0e20a-166">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:</span><span class="sxs-lookup"><span data-stu-id="0e20a-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e20a-168">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e20a-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0e20a-169">Ręcznie dodaj poniższy fragment do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="0e20a-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

<span data-ttu-id="0e20a-170">Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="0e20a-170">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="0e20a-171">Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0e20a-171">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="0e20a-172">Na przykład *ToDoApi.XML* będzie można znaleźć pliku w systemie Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="0e20a-172">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="0e20a-173">W powyższym kodzie `ApplicationBasePath` pobiera podstawowa ścieżka aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e20a-173">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="0e20a-174">Podstawowa ścieżka jest używana do lokalizowania komentarze pliku XML.</span><span class="sxs-lookup"><span data-stu-id="0e20a-174">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="0e20a-175">*TodoApi.xml* działa tylko w tym przykładzie, ponieważ nazwa wygenerowanego kodu XML komentarze pliku opiera się na nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e20a-175">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="0e20a-176">Dodawanie komentarzy potrójnym ukośnikiem do metody podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do Nagłówek sekcji:</span><span class="sxs-lookup"><span data-stu-id="0e20a-176">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="0e20a-179">Interfejs użytkownika jest wymuszany przez wygenerowanego pliku JSON, który zawiera również te komentarze:</span><span class="sxs-lookup"><span data-stu-id="0e20a-179">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="0e20a-180">Dodaj [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) znacznika `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0e20a-180">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="0e20a-181">Uzupełnia on informacje określone w `<summary>` tagu i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="0e20a-181">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="0e20a-182">`<remarks>` Zawartości tagu może składać się tekst JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="0e20a-182">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="0e20a-183">Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe komentarze.</span><span class="sxs-lookup"><span data-stu-id="0e20a-183">Notice the UI enhancements with these additional comments.</span></span>

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="0e20a-185">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="0e20a-185">Data Annotations</span></span>

<span data-ttu-id="0e20a-186">Dekoracji modelu z atrybutami, znalezione w `System.ComponentModel.DataAnnotations`, aby pomóc dysków składniki interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="0e20a-186">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="0e20a-187">Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="0e20a-187">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="0e20a-188">Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:</span><span class="sxs-lookup"><span data-stu-id="0e20a-188">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="0e20a-189">Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="0e20a-189">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="0e20a-190">Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zwracany typ zawartości *application/json*:</span><span class="sxs-lookup"><span data-stu-id="0e20a-190">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="0e20a-191">**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:</span><span class="sxs-lookup"><span data-stu-id="0e20a-191">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="0e20a-193">Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="0e20a-193">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="0e20a-194">Opisujący typy odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="0e20a-194">Describing Response Types</span></span>

<span data-ttu-id="0e20a-195">Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą &mdash; w szczególności typy odpowiedzi i kody błędów (o ile nie standard).</span><span class="sxs-lookup"><span data-stu-id="0e20a-195">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="0e20a-196">Te są obsługiwane w adnotacjach komentarzy i danych XML.</span><span class="sxs-lookup"><span data-stu-id="0e20a-196">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="0e20a-197">`Create` Akcji zwraca `201 Created` w przypadku powodzenia lub `400 Bad Request` gdy treść żądania przesłanego ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="0e20a-197">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="0e20a-198">Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="0e20a-198">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="0e20a-199">Ten problem został rozwiązany przez dodanie wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0e20a-199">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="0e20a-200">Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="0e20a-200">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="0e20a-202">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="0e20a-202">Customizing the UI</span></span>

<span data-ttu-id="0e20a-203">Zasoby interfejsu użytkownika jest funkcjonalności i presentable; Jednak podczas kompilowania stron z dokumentacją do interfejsu API, ma się reprezentować motywu lub marki, na których.</span><span class="sxs-lookup"><span data-stu-id="0e20a-203">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="0e20a-204">Wykonywanie zadań tego zadania ze składnikami Swashbuckle wymaga Dodawanie zasobów do obsługi plików statycznych, a następnie tworzenie struktury folderów do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="0e20a-204">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="0e20a-205">Jeśli przeznaczonych dla platformy .NET Framework, Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="0e20a-205">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="0e20a-206">Włącza oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="0e20a-206">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="0e20a-207">Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="0e20a-207">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="0e20a-208">Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="0e20a-208">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="0e20a-209">Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="0e20a-209">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="0e20a-210">Utwórz *wwwroot/swagger/ui/css/custom.css* pliku z następujących CSS, aby dostosować nagłówka strony:</span><span class="sxs-lookup"><span data-stu-id="0e20a-210">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="0e20a-211">Odwołanie *custome.CSS* w *index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="0e20a-211">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="0e20a-212">Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="0e20a-212">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="0e20a-213">Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="0e20a-213">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="0e20a-214">Wynikowa strona wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="0e20a-214">The resulting page looks as follows:</span></span>

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="0e20a-216">Jest znacznie więcej można zrobić za pomocą strony.</span><span class="sxs-lookup"><span data-stu-id="0e20a-216">There is much more you can do with the page.</span></span> <span data-ttu-id="0e20a-217">Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="0e20a-217">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
