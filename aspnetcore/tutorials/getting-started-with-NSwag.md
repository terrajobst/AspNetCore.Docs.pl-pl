---
title: Rozpoczynanie pracy z NSwag
author: zuckerthoben
description: "Ten samouczek zawiera wskazówki dodawania NSwag do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
keywords: Platformy ASP.NET Core, programu Swagger, NSwag, strony, interfejsu API sieci Web pomocy
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 70923210ef8f5b23ddd3cc74efceb05cbd90a179
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="get-started-with-nswag"></a><span data-ttu-id="5f9a4-104">Rozpoczynanie pracy z NSwag</span><span class="sxs-lookup"><span data-stu-id="5f9a4-104">Get started with NSwag</span></span>

<span data-ttu-id="5f9a4-105">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="5f9a4-105">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="5f9a4-106">Przy użyciu [NSwag](https://github.com/RSuter/NSwag) z platformy ASP.NET Core wymaga oprogramowania pośredniczącego [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-106">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="5f9a4-107">Pakiet składa się z generator Swagger, interfejs użytkownika programu Swagger (v2 i v3), i [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-107">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="5f9a4-108">Zdecydowanie zaleca się korzystać z jego NSwag możliwości generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-108">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="5f9a4-109">Podczas generowania kodu, wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-109">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="5f9a4-110">Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu systemu Windows podczas generowania kodu klienta w języku C# i TypeScript do interfejsu API</span><span class="sxs-lookup"><span data-stu-id="5f9a4-110">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="5f9a4-111">Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet do kodu generowania wewnątrz projektu</span><span class="sxs-lookup"><span data-stu-id="5f9a4-111">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="5f9a4-112">Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="5f9a4-112">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="5f9a4-113">Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet</span><span class="sxs-lookup"><span data-stu-id="5f9a4-113">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="5f9a4-114">Funkcje</span><span class="sxs-lookup"><span data-stu-id="5f9a4-114">Features</span></span>

<span data-ttu-id="5f9a4-115">Głównym celem używania NSwag jest możliwość wprowadzenia nie tylko interfejs użytkownika programu Swagger i struktury Swagger generator, jak używać funkcji generowania kodu elastyczne.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-115">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="5f9a4-116">Nie ma potrzeby istniejącego interfejsu API&mdash;można użyć interfejsów API innych firm, które włączają Swagger i umożliwić NSwag Generowanie implementacja klienta.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-116">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="5f9a4-117">W obu przypadkach jest przyspieszonego cyklu programowanie i łatwiej można dostosować do zmian interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-117">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="5f9a4-118">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="5f9a4-118">Package installation</span></span>

<span data-ttu-id="5f9a4-119">Pakiet NSwag NuGet można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-119">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f9a4-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f9a4-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5f9a4-121">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="5f9a4-122">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-122">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="5f9a4-123">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="5f9a4-124">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="5f9a4-124">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="5f9a4-125">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="5f9a4-125">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="5f9a4-126">Wybierz pakiet "NSwag.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-126">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f9a4-127">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5f9a4-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5f9a4-128">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="5f9a4-129">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="5f9a4-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="5f9a4-130">W polu wyszukiwania wprowadź NSwag.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="5f9a4-130">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="5f9a4-131">Wybierz pakiet NSwag.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-131">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f9a4-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f9a4-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5f9a4-133">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f9a4-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f9a4-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f9a4-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-135">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

# <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="5f9a4-136">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="5f9a4-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="5f9a4-137">Importowanie następujących przestrzeni nazw w `Info` klasy:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-137">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="5f9a4-138">W `Startup.Configure` metody, włącza oprogramowanie pośredniczące dla obsługująca specyfikację struktury Swagger generowane i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-138">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="5f9a4-139">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-139">Launch the app.</span></span> <span data-ttu-id="5f9a4-140">Przejdź do `/swagger` do wyświetlania interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-140">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="5f9a4-141">Przejdź do `/swagger/v1/swagger.json` Aby wyświetlić specyfikację struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-141">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="5f9a4-142">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="5f9a4-142">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="5f9a4-143">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="5f9a4-143">Via NSwagStudio</span></span>

* <span data-ttu-id="5f9a4-144">Zainstaluj `NSwagStudio` z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-144">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="5f9a4-145">Launch NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-145">Launch NSwagStudio.</span></span> <span data-ttu-id="5f9a4-146">Wprowadź lokalizację użytkownika *swagger.json* lub skopiuj go bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-146">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="5f9a4-148">Wskazuje typ danych wyjściowych żądaną klienta.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-148">Indicate the desired client output type.</span></span> <span data-ttu-id="5f9a4-149">Dostępne są następujące opcje **klienta TypeScript**, **klienta CSharp**, lub **Kontroler interfejsu API sieci Web CSharp**.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-149">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="5f9a4-150">Przy użyciu kontrolera interfejsu API sieci Web jest zasadniczo reverse generacji.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-150">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="5f9a4-151">Specyfikacja usługi używa odbudować usługi.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-151">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="5f9a4-152">Kliknij przycisk **Generowanie danych wyjściowych**.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-152">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="5f9a4-153">W tym miejscu wyświetlić implementację klienta pełną *TodoApi.NSwag* przykład w języku C#:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-153">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="5f9a4-155">Umieść plik do projektu klienta (na przykład [platformy Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) aplikacji).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-155">Put the file into a client project (for example, a [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) app).</span></span> <span data-ttu-id="5f9a4-156">Rozpocznij korzystanie z interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-156">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="5f9a4-157">Podstawowy adres URL i/lub klient HTTP można wprowadzić do klienta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-157">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="5f9a4-158">Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-158">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="5f9a4-159">Można teraz uruchomić łatwe Implementowanie interfejsu API do projektów klienckich.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-159">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="5f9a4-160">Inne sposoby generowanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="5f9a4-160">Other ways to generate client code</span></span>

<span data-ttu-id="5f9a4-161">Można wygenerować kod w inny sposób więcej nadaje się do przepływu pracy:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-161">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="5f9a4-162">MSBuild</span><span class="sxs-lookup"><span data-stu-id="5f9a4-162">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="5f9a4-163">W kodzie</span><span class="sxs-lookup"><span data-stu-id="5f9a4-163">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="5f9a4-164">Szablony T4</span><span class="sxs-lookup"><span data-stu-id="5f9a4-164">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="5f9a4-165">Dostosowywanie</span><span class="sxs-lookup"><span data-stu-id="5f9a4-165">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="5f9a4-166">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="5f9a4-166">XML comments</span></span>

<span data-ttu-id="5f9a4-167">Komentarze XML są włączone z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-167">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="5f9a4-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f9a4-168">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="5f9a4-169">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-169">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="5f9a4-170">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-170">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="5f9a4-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5f9a4-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="5f9a4-173">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="5f9a4-173">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="5f9a4-174">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-174">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="5f9a4-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f9a4-176">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="5f9a4-177">Ręcznie dodaj poniższy fragment do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-177">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

---

## <a name="data-annotations"></a><span data-ttu-id="5f9a4-178">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="5f9a4-178">Data annotations</span></span>

<span data-ttu-id="5f9a4-179">Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i jest najlepszym rozwiązaniem dla interfejsu API sieci Web akcji do zwrócenia [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-179">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="5f9a4-180">W rezultacie NSwag nie można wywnioskować czynności akcję i ją zwraca.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-180">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="5f9a4-181">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-181">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="5f9a4-182">Poprzedni zwraca akcji `IActionResult`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-182">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="5f9a4-183">Adnotacje danych służą do Poinformuj klientów odpowiedzi HTTP, które zwraca tej akcji.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-183">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="5f9a4-184">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="5f9a4-184">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="5f9a4-185">Swagger generator teraz można dokładnie opisują tę akcję, a wygenerowanego klienci wiedzieli, co otrzymują podczas wywoływania metody punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-185">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="5f9a4-186">Zdecydowanie zalecane jest dekoracji wszystkie akcje te atrybuty.</span><span class="sxs-lookup"><span data-stu-id="5f9a4-186">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="5f9a4-187">Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, jakie powinna zwrócić operacji interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="5f9a4-187">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
