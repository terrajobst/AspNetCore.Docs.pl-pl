---
title: Rozpoczynanie pracy z usługą NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak za pomocą NSwag generować dokumentację i Pomóż strony dla internetowego interfejsu API platformy ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: b9266e2df75563be6bad1a1f464cef788c333d4c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028171"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="4a162-103">Rozpoczynanie pracy z usługą NSwag i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a162-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="4a162-104">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="4a162-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4a162-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a162-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="4a162-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a162-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="4a162-107">Zarejestruj middlewares NSwag do:</span><span class="sxs-lookup"><span data-stu-id="4a162-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="4a162-108">Generowanie specyfikacją struktury Swagger dla interfejsu API wdrożony w sieci web.</span><span class="sxs-lookup"><span data-stu-id="4a162-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="4a162-109">Obsługiwać interfejs użytkownika struktury Swagger do przeglądania i testowanie interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="4a162-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="4a162-110">Aby użyć [NSwag](https://github.com/RSuter/NSwag) middlewares platformy ASP.NET Core, zainstaluj [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="4a162-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="4a162-111">Ten pakiet zawiera middlewares do generowania i obsługiwać specyfikacją struktury Swagger interfejs użytkownika struktury Swagger (v2 i v3), a [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="4a162-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="4a162-112">Ponadto zdecydowanie zaleca się korzystania z tych firmy NSwag możliwości generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="4a162-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="4a162-113">Wybierz jedną z poniższych opcji, aby korzystać z funkcji generowania kodu:</span><span class="sxs-lookup"><span data-stu-id="4a162-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="4a162-114">Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu Windows podczas generowania kodu klienta w języku C# i TypeScript dla interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4a162-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="4a162-115">Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakiety NuGet umożliwiające code generation wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="4a162-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="4a162-116">Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="4a162-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="4a162-117">Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="4a162-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="4a162-118">Funkcje</span><span class="sxs-lookup"><span data-stu-id="4a162-118">Features</span></span>

<span data-ttu-id="4a162-119">Głównym celem użycia NSwag jest możliwość nie tylko interfejs użytkownika struktury Swagger i generatora struktury Swagger, ale również upewnij wykorzystanie możliwości generowania kodu elastyczne.</span><span class="sxs-lookup"><span data-stu-id="4a162-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="4a162-120">Nie ma potrzeby istniejącego interfejsu API&mdash;możesz użyć interfejsów API innych firm, które włączają struktury Swagger i umożliwić NSwag Generowanie implementacji klienta.</span><span class="sxs-lookup"><span data-stu-id="4a162-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="4a162-121">W obu przypadkach cyklu tworzenia oprogramowania jest przyspieszona i łatwiej można dostosować do zmian interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="4a162-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="4a162-122">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="4a162-122">Package installation</span></span>

<span data-ttu-id="4a162-123">Pakiet NSwag NuGet mogą być dodawane przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4a162-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a162-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a162-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a162-125">Z **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="4a162-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="4a162-126">Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="4a162-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="4a162-127">Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje</span><span class="sxs-lookup"><span data-stu-id="4a162-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="4a162-128">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4a162-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="4a162-129">Z **Zarządzaj pakietami NuGet** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="4a162-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="4a162-130">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="4a162-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="4a162-131">Ustaw **źródła pakietu** na stronie "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="4a162-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="4a162-132">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="4a162-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="4a162-133">Wybierz pakiet "NSwag.AspNetCore" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="4a162-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4a162-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4a162-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4a162-135">Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="4a162-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="4a162-136">Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="4a162-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="4a162-137">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="4a162-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="4a162-138">Wybierz pakiet "NSwag.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="4a162-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a162-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a162-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a162-140">Uruchom następujące polecenie z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="4a162-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4a162-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4a162-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4a162-142">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4a162-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="4a162-143">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="4a162-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="4a162-144">Zaimportuj następujące przestrzenie nazw w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="4a162-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="4a162-145">W `Startup.ConfigureServices` metodę rejestrowania wymagane usługi struktury Swagger:</span><span class="sxs-lookup"><span data-stu-id="4a162-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="4a162-146">W `Startup.Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane Specyfikacja Swagger i v3 interfejs użytkownika struktury Swagger:</span><span class="sxs-lookup"><span data-stu-id="4a162-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="4a162-147">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="4a162-147">Launch the app.</span></span> <span data-ttu-id="4a162-148">Przejdź do `http://localhost:<port>/swagger` Aby wyświetlić interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="4a162-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="4a162-149">Przejdź do `http://localhost:<port>/swagger/v1/swagger.json` można zapoznać się ze specyfikacją struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="4a162-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="4a162-150">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="4a162-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="4a162-151">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="4a162-151">Via NSwagStudio</span></span>

* <span data-ttu-id="4a162-152">Zainstaluj NSwagStudio z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="4a162-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="4a162-153">Launch NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="4a162-153">Launch NSwagStudio.</span></span> <span data-ttu-id="4a162-154">Wprowadź *swagger.json* adresu URL w pliku **URL Specyfikacja Swagger** polu tekstowym i kliknij przycisk **utworzyć lokalną kopię** przycisku.</span><span class="sxs-lookup"><span data-stu-id="4a162-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="4a162-155">Wybierz **klienta CSharp** klienta, typ danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="4a162-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="4a162-156">Inne opcje obejmują **klienta TypeScript** i **Kontroler interfejsu API sieci Web języka CSharp**.</span><span class="sxs-lookup"><span data-stu-id="4a162-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="4a162-157">Za pomocą kontrolera interfejsu API sieci Web jest zasadniczo odwróconej generacji.</span><span class="sxs-lookup"><span data-stu-id="4a162-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="4a162-158">Specyfikacja usługi używa odbudować usługi.</span><span class="sxs-lookup"><span data-stu-id="4a162-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="4a162-159">Kliknij przycisk **Generowanie danych wyjściowych** przycisku.</span><span class="sxs-lookup"><span data-stu-id="4a162-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="4a162-160">Pełne C# klienta wdrożenie *TodoApi.NSwag* projektu jest generowany.</span><span class="sxs-lookup"><span data-stu-id="4a162-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="4a162-161">Kliknij przycisk **klienta CSharp** karcie **dane wyjściowe** sekcję, aby wyświetlić wygenerowanego kodu klienta:</span><span class="sxs-lookup"><span data-stu-id="4a162-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="4a162-162">Kod klienta języka C# jest generowany na podstawie ustawień zdefiniowanych w **ustawienia** karcie **klienta CSharp** kartę. Zmodyfikuj ustawienia, aby wykonywać zadania takie jak zmiana nazwy przestrzeni nazw domyślne i generowanie metody synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="4a162-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="4a162-163">Skopiuj wygenerowany kod C# do pliku w projekcie klienta (na przykład [Xamarin.Forms](/xamarin/xamarin-forms/) aplikacji).</span><span class="sxs-lookup"><span data-stu-id="4a162-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="4a162-164">Rozpocznij korzystanie z interfejsu API sieci web:</span><span class="sxs-lookup"><span data-stu-id="4a162-164">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="4a162-165">Podstawowy adres URL i/lub klienta HTTP można wstawić do klienta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4a162-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="4a162-166">Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="4a162-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="4a162-167">Inne sposoby generowania kodu klienta</span><span class="sxs-lookup"><span data-stu-id="4a162-167">Other ways to generate client code</span></span>

<span data-ttu-id="4a162-168">Można wygenerować kod klienta w inny sposób więcej dostosowane do przepływu pracy:</span><span class="sxs-lookup"><span data-stu-id="4a162-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="4a162-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="4a162-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="4a162-170">W kodzie</span><span class="sxs-lookup"><span data-stu-id="4a162-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="4a162-171">Szablony T4</span><span class="sxs-lookup"><span data-stu-id="4a162-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="4a162-172">Dostosuj</span><span class="sxs-lookup"><span data-stu-id="4a162-172">Customize</span></span>

<span data-ttu-id="4a162-173">Struktury swagger zawiera opcje dokumentowanie modelu obiektów do jej obsługi ułatwiają realizację użycia interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="4a162-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="4a162-174">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="4a162-174">API info and description</span></span>

<span data-ttu-id="4a162-175">W `Startup.Configure` metody akcji konfiguracji przekazywane do `UseSwagger` metoda dodaje informacje, takie jak tworzenie, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="4a162-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="4a162-176">Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="4a162-176">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="4a162-178">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="4a162-178">XML comments</span></span>

<span data-ttu-id="4a162-179">Komentarze XML są włączane wraz z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4a162-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="4a162-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a162-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="4a162-181">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >**.</span><span class="sxs-lookup"><span data-stu-id="4a162-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="4a162-182">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="4a162-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="4a162-183">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="4a162-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="4a162-184">Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę</span><span class="sxs-lookup"><span data-stu-id="4a162-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="4a162-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4a162-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="4a162-186">Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="4a162-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="4a162-187">Przejdź do **narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="4a162-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="4a162-188">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="4a162-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="4a162-189">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="4a162-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="4a162-190">Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji</span><span class="sxs-lookup"><span data-stu-id="4a162-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="4a162-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a162-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="4a162-192">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="4a162-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="4a162-193">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="4a162-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="4a162-194">Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="4a162-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="4a162-195">W związku z tym nie można wywnioskować NSwag, co robi Twoja Akcja i ją zwraca.</span><span class="sxs-lookup"><span data-stu-id="4a162-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="4a162-196">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="4a162-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="4a162-197">Poprzedni zwraca akcji `IActionResult`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="4a162-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="4a162-198">Adnotacje danych są używane do Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="4a162-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="4a162-199">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="4a162-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4a162-200">Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="4a162-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="4a162-201">W związku z tym, NSwag tylko może wywnioskować typ zwracany zdefiniowane przez `T`.</span><span class="sxs-lookup"><span data-stu-id="4a162-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="4a162-202">Nie można wywnioskować inne możliwe zwracane typy akcji.</span><span class="sxs-lookup"><span data-stu-id="4a162-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="4a162-203">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="4a162-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="4a162-204">Poprzedni zwraca akcji `ActionResult<T>`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="4a162-204">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="4a162-205">Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) odpowiedzi możliwe jest również.</span><span class="sxs-lookup"><span data-stu-id="4a162-205">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="4a162-206">Zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4a162-206">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="4a162-207">Adnotacje danych są używane do Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="4a162-207">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="4a162-208">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="4a162-208">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

<span data-ttu-id="4a162-209">Generatora struktury Swagger teraz można dokładnie opisują tej akcji i wygenerowanego klienci wiedzieli, czego otrzymują podczas wywoływania punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4a162-209">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="4a162-210">Zdecydowanie zaleca się urządzanie wszystkie akcje za pomocą tych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4a162-210">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="4a162-211">Aby uzyskać wskazówki, jakie odpowiedzi HTTP, powinien zwrócić swoje działania interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="4a162-211">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
