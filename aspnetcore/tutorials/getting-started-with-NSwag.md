---
title: Rozpoczynanie pracy z NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak używać NSwag Generowanie dokumentacji do strony sieci Web platformy ASP.NET Core API pomocy.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c0811593609b7d1e3529d5253e8b053f180281f3
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126277"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="6e43b-103">Rozpoczynanie pracy z NSwag i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e43b-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="6e43b-104">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="6e43b-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6e43b-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e43b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6e43b-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e43b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="6e43b-107">Przy użyciu [NSwag](https://github.com/RSuter/NSwag) z platformy ASP.NET Core wymaga oprogramowania pośredniczącego [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e43b-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="6e43b-108">Pakiet składa się z generator Swagger, interfejs użytkownika programu Swagger (v2 i v3), i [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="6e43b-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="6e43b-109">Zdecydowanie zaleca się korzystać z jego NSwag możliwości generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="6e43b-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="6e43b-110">Podczas generowania kodu, wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="6e43b-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="6e43b-111">Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu systemu Windows podczas generowania kodu klienta w języku C# i TypeScript do interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6e43b-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="6e43b-112">Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet do kodu generowania wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="6e43b-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="6e43b-113">Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="6e43b-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="6e43b-114">Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e43b-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="6e43b-115">Funkcje</span><span class="sxs-lookup"><span data-stu-id="6e43b-115">Features</span></span>

<span data-ttu-id="6e43b-116">Głównym celem używania NSwag jest możliwość wprowadzenia nie tylko interfejs użytkownika programu Swagger i struktury Swagger generator, jak używać funkcji generowania kodu elastyczne.</span><span class="sxs-lookup"><span data-stu-id="6e43b-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="6e43b-117">Nie ma potrzeby istniejącego interfejsu API&mdash;można użyć interfejsów API innych firm, które włączają Swagger i umożliwić NSwag Generowanie implementacja klienta.</span><span class="sxs-lookup"><span data-stu-id="6e43b-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="6e43b-118">W obu przypadkach jest przyspieszonego cyklu programowanie i łatwiej można dostosować do zmian interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6e43b-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="6e43b-119">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="6e43b-119">Package installation</span></span>

<span data-ttu-id="6e43b-120">Pakiet NSwag NuGet można dodać z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6e43b-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e43b-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e43b-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e43b-122">Z **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="6e43b-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="6e43b-123">Przejdź do **widoku** > **innych okien** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="6e43b-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="6e43b-124">Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje</span><span class="sxs-lookup"><span data-stu-id="6e43b-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="6e43b-125">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6e43b-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="6e43b-126">Z **Zarządzaj pakietami NuGet** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="6e43b-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="6e43b-127">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="6e43b-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="6e43b-128">Ustaw **źródła pakietu** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="6e43b-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="6e43b-129">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="6e43b-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="6e43b-130">Wybierz pakiet "NSwag.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**</span><span class="sxs-lookup"><span data-stu-id="6e43b-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e43b-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e43b-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e43b-132">Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="6e43b-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="6e43b-133">Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="6e43b-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="6e43b-134">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="6e43b-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="6e43b-135">Wybierz pakiet "NSwag.AspNetCore" w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**</span><span class="sxs-lookup"><span data-stu-id="6e43b-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e43b-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e43b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6e43b-137">Uruchom następujące polecenie z **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="6e43b-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6e43b-138">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6e43b-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6e43b-139">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6e43b-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="6e43b-140">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="6e43b-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="6e43b-141">Importowanie następujących przestrzeni nazw w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="6e43b-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="6e43b-142">W `Startup.Configure` metody, włącza oprogramowanie pośredniczące dla obsługująca specyfikację struktury Swagger generowane i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="6e43b-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="6e43b-143">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6e43b-143">Launch the app.</span></span> <span data-ttu-id="6e43b-144">Przejdź do `http://localhost:<port>/swagger` do wyświetlania interfejsu użytkownika programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="6e43b-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="6e43b-145">Przejdź do `http://localhost:<port>/swagger/v1/swagger.json` Aby wyświetlić specyfikację struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="6e43b-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="6e43b-146">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="6e43b-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="6e43b-147">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="6e43b-147">Via NSwagStudio</span></span>

* <span data-ttu-id="6e43b-148">Zainstaluj NSwagStudio z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="6e43b-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="6e43b-149">Launch NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="6e43b-149">Launch NSwagStudio.</span></span> <span data-ttu-id="6e43b-150">Wprowadź *swagger.json* adres URL w pliku **Swagger URL specyfikacji** polem tekstowym i kliknij przycisk **utworzyć lokalną kopię** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6e43b-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="6e43b-151">Wybierz **klienta CSharp** typ danych wyjściowych klienta.</span><span class="sxs-lookup"><span data-stu-id="6e43b-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="6e43b-152">Inne opcje obejmują **klienta TypeScript** i **Kontroler interfejsu API sieci Web CSharp**.</span><span class="sxs-lookup"><span data-stu-id="6e43b-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="6e43b-153">Przy użyciu kontrolera interfejsu API sieci Web jest zasadniczo reverse generacji.</span><span class="sxs-lookup"><span data-stu-id="6e43b-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="6e43b-154">Specyfikacja usługi używa odbudować usługi.</span><span class="sxs-lookup"><span data-stu-id="6e43b-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="6e43b-155">Kliknij przycisk **Generowanie danych wyjściowych** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6e43b-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="6e43b-156">Pełna C# klienta implementacja elementu *TodoApi.NSwag* projektu jest generowany.</span><span class="sxs-lookup"><span data-stu-id="6e43b-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="6e43b-157">Kliknij przycisk **klienta CSharp** karcie **dane wyjściowe** sekcję, aby wyświetlić kod wygenerowanego klienta:</span><span class="sxs-lookup"><span data-stu-id="6e43b-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="6e43b-158">Kod klienta C# jest generowany na podstawie ustawień zdefiniowanych w **ustawienia** karcie **klienta CSharp** kartę. Zmodyfikuj ustawienia w celu wykonywania zadań takich jak zmiana nazwy przestrzeni nazw domyślne i generowania metoda synchroniczna.</span><span class="sxs-lookup"><span data-stu-id="6e43b-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="6e43b-159">Skopiuj wygenerowany kod C# do pliku w projekcie klienta (na przykład [platformy Xamarin.Forms](/xamarin/xamarin-forms/) aplikacji).</span><span class="sxs-lookup"><span data-stu-id="6e43b-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="6e43b-160">Rozpocznij korzystanie z interfejsu API sieci web:</span><span class="sxs-lookup"><span data-stu-id="6e43b-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="6e43b-161">Podstawowy adres URL i/lub klient HTTP można wprowadzić do klienta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6e43b-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="6e43b-162">Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="6e43b-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="6e43b-163">Inne sposoby generowanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="6e43b-163">Other ways to generate client code</span></span>

<span data-ttu-id="6e43b-164">Istnieje możliwość wygenerowania kodu klienta w inny sposób więcej, nadaje się do przepływu pracy:</span><span class="sxs-lookup"><span data-stu-id="6e43b-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="6e43b-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="6e43b-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="6e43b-166">W kodzie</span><span class="sxs-lookup"><span data-stu-id="6e43b-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="6e43b-167">Szablony T4</span><span class="sxs-lookup"><span data-stu-id="6e43b-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="6e43b-168">Dostosuj</span><span class="sxs-lookup"><span data-stu-id="6e43b-168">Customize</span></span>

<span data-ttu-id="6e43b-169">Struktury swagger udostępnia opcje dla dokumentowanie model obiektów do jej obsługi ułatwiają przez interfejs API sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e43b-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="6e43b-170">Informacje o interfejsu API i opis</span><span class="sxs-lookup"><span data-stu-id="6e43b-170">API info and description</span></span>

<span data-ttu-id="6e43b-171">W `Startup.Configure` metody akcji konfiguracji przekazany do `UseSwagger` metoda dodaje informacje, takie jak autor, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="6e43b-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="6e43b-172">Interfejs użytkownika programu Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="6e43b-172">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="6e43b-174">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="6e43b-174">XML comments</span></span>

<span data-ttu-id="6e43b-175">Komentarze XML są włączone z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6e43b-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="6e43b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e43b-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="6e43b-177">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **.csproj < nazwa_projektu > Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="6e43b-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="6e43b-178">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="6e43b-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="6e43b-179">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="6e43b-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="6e43b-180">Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** kartę</span><span class="sxs-lookup"><span data-stu-id="6e43b-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="6e43b-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e43b-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="6e43b-182">Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="6e43b-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="6e43b-183">Przejdź do **narzędzia** > **Przeprowadź edycję pliku**.</span><span class="sxs-lookup"><span data-stu-id="6e43b-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="6e43b-184">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="6e43b-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="6e43b-185">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="6e43b-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="6e43b-186">Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji</span><span class="sxs-lookup"><span data-stu-id="6e43b-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="6e43b-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e43b-187">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="6e43b-188">Ręcznie Dodaj wyróżnione wiersze do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="6e43b-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="6e43b-189">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="6e43b-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6e43b-190">Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji interfejsu API sieci web jest [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="6e43b-190">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="6e43b-191">W rezultacie NSwag nie można wywnioskować czynności akcję i ją zwraca.</span><span class="sxs-lookup"><span data-stu-id="6e43b-191">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="6e43b-192">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="6e43b-192">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="6e43b-193">Poprzedni zwraca akcji `IActionResult`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="6e43b-193">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="6e43b-194">Adnotacje danych służą do Poinformuj klientów kodów stanu HTTP ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="6e43b-194">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="6e43b-195">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="6e43b-195">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6e43b-196">Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji interfejsu API sieci web jest [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="6e43b-196">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="6e43b-197">W rezultacie NSwag można tylko wywnioskować zwracanego typu określone przez `T`.</span><span class="sxs-lookup"><span data-stu-id="6e43b-197">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="6e43b-198">Nie można wywnioskować innych możliwych typów zwracanych w akcji.</span><span class="sxs-lookup"><span data-stu-id="6e43b-198">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="6e43b-199">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="6e43b-199">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="6e43b-200">Poprzedni zwraca akcji `ActionResult<T>`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="6e43b-200">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="6e43b-201">Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu, [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) zbyt odpowiedzi jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="6e43b-201">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="6e43b-202">Zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="6e43b-202">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="6e43b-203">Adnotacje danych służą do Poinformuj klientów kodów stanu HTTP ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="6e43b-203">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="6e43b-204">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="6e43b-204">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

<span data-ttu-id="6e43b-205">Swagger generator teraz można dokładnie opisują tę akcję, a wygenerowanego klienci wiedzieli, co otrzymują podczas wywoływania metody punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6e43b-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="6e43b-206">Zdecydowanie zalecane jest dekoracji wszystkie akcje te atrybuty.</span><span class="sxs-lookup"><span data-stu-id="6e43b-206">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="6e43b-207">Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, jakie powinna zwrócić operacji interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="6e43b-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
