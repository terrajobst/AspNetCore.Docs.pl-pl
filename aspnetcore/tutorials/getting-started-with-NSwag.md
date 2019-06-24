---
title: Rozpoczynanie pracy z usługą NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak za pomocą NSwag generować dokumentację i Pomóż strony dla internetowego interfejsu API platformy ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c5b2dc47328d6d3c271a87579fa8c300109bd734
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316553"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="02c38-103">Rozpoczynanie pracy z usługą NSwag i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02c38-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="02c38-104">Przez [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), i [firmy Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="02c38-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02c38-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02c38-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="02c38-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02c38-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="02c38-107">NSwag oferuje następujące możliwości:</span><span class="sxs-lookup"><span data-stu-id="02c38-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="02c38-108">Umożliwia korzystanie z programu Swagger interfejsu użytkownika i programu Swagger generator.</span><span class="sxs-lookup"><span data-stu-id="02c38-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="02c38-109">Możliwości generowania kodu elastyczne.</span><span class="sxs-lookup"><span data-stu-id="02c38-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="02c38-110">Nswag, nie ma potrzeby istniejącego interfejsu API&mdash;możesz użyć interfejsów API innych firm, które włączają struktury Swagger i wygenerować implementacji klienta.</span><span class="sxs-lookup"><span data-stu-id="02c38-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="02c38-111">NSwag pozwala przyspieszyć cykl tworzenia oprogramowania i łatwo przystosować do zmiany interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="02c38-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="02c38-112">Zarejestruj oprogramowanie pośredniczące NSwag</span><span class="sxs-lookup"><span data-stu-id="02c38-112">Register the NSwag middleware</span></span>

<span data-ttu-id="02c38-113">Zarejestruj NSwag oprogramowaniu pośredniczącym, aby:</span><span class="sxs-lookup"><span data-stu-id="02c38-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="02c38-114">Generowanie specyfikacją struktury Swagger dla interfejsu API wdrożony w sieci web.</span><span class="sxs-lookup"><span data-stu-id="02c38-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="02c38-115">Obsługiwać interfejs użytkownika struktury Swagger do przeglądania i testowanie interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="02c38-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="02c38-116">Aby użyć [NSwag](https://github.com/RicoSuter/NSwag) oprogramowanie pośredniczące platformy ASP.NET Core, zainstaluj [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="02c38-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="02c38-117">Ten pakiet zawiera oprogramowaniu pośredniczącym, aby wygenerować i obsługiwać specyfikacją struktury Swagger interfejs użytkownika struktury Swagger (v2 i v3), a [ReDoc UI](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="02c38-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="02c38-118">Aby zainstalować pakiet NSwag NuGet, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="02c38-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="02c38-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02c38-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="02c38-120">Z **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="02c38-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="02c38-121">Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="02c38-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="02c38-122">Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje</span><span class="sxs-lookup"><span data-stu-id="02c38-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="02c38-123">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="02c38-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="02c38-124">Z **Zarządzaj pakietami NuGet** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="02c38-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="02c38-125">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="02c38-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="02c38-126">Ustaw **źródła pakietu** na stronie "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="02c38-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="02c38-127">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="02c38-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="02c38-128">Wybierz pakiet "NSwag.AspNetCore" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="02c38-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="02c38-129">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="02c38-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="02c38-130">Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**</span><span class="sxs-lookup"><span data-stu-id="02c38-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="02c38-131">Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="02c38-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="02c38-132">W polu wyszukiwania wprowadź "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="02c38-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="02c38-133">Wybierz pakiet "NSwag.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="02c38-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="02c38-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="02c38-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="02c38-135">Uruchom następujące polecenie z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="02c38-135">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="02c38-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="02c38-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="02c38-137">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="02c38-137">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="02c38-138">Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger</span><span class="sxs-lookup"><span data-stu-id="02c38-138">Add and configure Swagger middleware</span></span>

<span data-ttu-id="02c38-139">Dodawanie i konfigurowanie programu Swagger w aplikacji platformy ASP.NET Core, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="02c38-139">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="02c38-140">W `Startup.ConfigureServices` metodę rejestrowania wymagane usługi struktury Swagger:</span><span class="sxs-lookup"><span data-stu-id="02c38-140">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="02c38-141">W `Startup.Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane Specyfikacja Swagger i interfejs użytkownika struktury Swagger:</span><span class="sxs-lookup"><span data-stu-id="02c38-141">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="02c38-142">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="02c38-142">Launch the app.</span></span> <span data-ttu-id="02c38-143">Przejdź do:</span><span class="sxs-lookup"><span data-stu-id="02c38-143">Navigate to:</span></span>
  * <span data-ttu-id="02c38-144">`http://localhost:<port>/swagger` Aby wyświetlić interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="02c38-144">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="02c38-145">`http://localhost:<port>/swagger/v1/swagger.json` Aby zapoznać się ze specyfikacją struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="02c38-145">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="02c38-146">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="02c38-146">Code generation</span></span>

<span data-ttu-id="02c38-147">Korzystać z zalet możliwości generowania kodu NSwag firmy, wybierając jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="02c38-147">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="02c38-148">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; podczas generowania kodu klienta interfejsu API w aplikacji pulpitu Windows C# lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="02c38-148">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="02c38-149">[NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet dla generowania kodu wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="02c38-149">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="02c38-150">NSwag z [wiersza polecenia](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="02c38-150">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="02c38-151">[NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="02c38-151">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet package.</span></span>
* <span data-ttu-id="02c38-152">[Unchase OpenAPI (Swagger) podłączonej usługi](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; Visual Studio Connected Service for generowanie kodu klienta interfejsu API w C# lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="02c38-152">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="02c38-153">Generuje również C# kontrolerów, usług interfejsu OpenAPI nswag.</span><span class="sxs-lookup"><span data-stu-id="02c38-153">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="02c38-154">Generowanie kodu za pomocą NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="02c38-154">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="02c38-155">Zainstaluj NSwagStudio, postępując zgodnie z instrukcjami w artykule [repozytorium NSwagStudio GitHub](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="02c38-155">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="02c38-156">Uruchom NSwagStudio, a następnie wprowadź *swagger.json* adresu URL w pliku **Swagger URL specyfikacji** pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="02c38-156">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="02c38-157">Na przykład *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="02c38-157">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="02c38-158">Kliknij przycisk **utworzyć lokalną kopię** przycisk, aby wygenerować reprezentacja JSON usługi specyfikacją struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="02c38-158">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Utwórz lokalną kopię specyfikacją struktury Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="02c38-160">W **dane wyjściowe** obszaru, kliknij przycisk **klienta CSharp** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="02c38-160">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="02c38-161">W zależności od projektu, można także **klienta TypeScript** lub **Kontroler interfejsu API sieci Web języka CSharp**.</span><span class="sxs-lookup"><span data-stu-id="02c38-161">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="02c38-162">Jeśli wybierzesz **Kontroler interfejsu API sieci Web języka CSharp**, Specyfikacja usługi odtwarza usługi, która służy jako zwrotny generacji.</span><span class="sxs-lookup"><span data-stu-id="02c38-162">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="02c38-163">Kliknij przycisk **Generowanie danych wyjściowych** do produkcji kompletna C# implementacji klienta *TodoApi.NSwag* projektu.</span><span class="sxs-lookup"><span data-stu-id="02c38-163">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="02c38-164">Aby wyświetlić kod wygenerowanego klienta, kliknij przycisk **klienta CSharp** karty:</span><span class="sxs-lookup"><span data-stu-id="02c38-164">To see the generated client code, click the **CSharp Client** tab:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> <span data-ttu-id="02c38-165">C# Kod klienta jest generowany na podstawie wyborów w **ustawienia** kartę. Zmodyfikuj ustawienia, aby wykonywać zadania takie jak zmiana nazwy przestrzeni nazw domyślne i generowanie metody synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="02c38-165">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="02c38-166">Skopiuj wygenerowany C# kod do pliku w projekcie klienta, które będą korzystać z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="02c38-166">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="02c38-167">Rozpocznij korzystanie z interfejsu API sieci web:</span><span class="sxs-lookup"><span data-stu-id="02c38-167">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="02c38-168">Dostosowywanie dokumentacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="02c38-168">Customize API documentation</span></span>

<span data-ttu-id="02c38-169">Struktury swagger zawiera opcje dokumentowanie modelu obiektów do jej obsługi ułatwiają realizację użycia interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="02c38-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="02c38-170">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="02c38-170">API info and description</span></span>

<span data-ttu-id="02c38-171">W `Startup.ConfigureServices` metody akcji konfiguracji przekazywane do `AddSwaggerDocument` metoda dodaje informacje, takie jak tworzenie, licencji i opis:</span><span class="sxs-lookup"><span data-stu-id="02c38-171">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="02c38-172">Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="02c38-172">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="02c38-174">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="02c38-174">XML comments</span></span>

<span data-ttu-id="02c38-175">Aby włączyć komentarze XML, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="02c38-175">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="02c38-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02c38-176">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="02c38-177">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >** .</span><span class="sxs-lookup"><span data-stu-id="02c38-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="02c38-178">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="02c38-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="02c38-179">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="02c38-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="02c38-180">Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę</span><span class="sxs-lookup"><span data-stu-id="02c38-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="02c38-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="02c38-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="02c38-182">Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="02c38-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="02c38-183">Przejdź do **narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="02c38-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="02c38-184">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="02c38-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="02c38-185">Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**</span><span class="sxs-lookup"><span data-stu-id="02c38-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="02c38-186">Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji</span><span class="sxs-lookup"><span data-stu-id="02c38-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="02c38-187">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="02c38-187">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="02c38-188">Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="02c38-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="02c38-189">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="02c38-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="02c38-190">Ponieważ używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji internetowego interfejsu API jest [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), nie można wywnioskować, co robi Twoja Akcja i ją zwraca.</span><span class="sxs-lookup"><span data-stu-id="02c38-190">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="02c38-191">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="02c38-191">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="02c38-192">Poprzedni zwraca akcji `IActionResult`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) lub [element BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="02c38-192">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="02c38-193">Adnotacje danych umożliwia Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="02c38-193">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="02c38-194">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="02c38-194">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="02c38-195">Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [ActionResult\<T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), tylko można go wywnioskować zwracany typ zdefiniowany przez `T`.</span><span class="sxs-lookup"><span data-stu-id="02c38-195">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="02c38-196">Nie można automatycznie wywnioskować inne możliwe zwracane typy.</span><span class="sxs-lookup"><span data-stu-id="02c38-196">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="02c38-197">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="02c38-197">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="02c38-198">Poprzedni zwraca akcji `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="02c38-198">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="02c38-199">W akcji, zwraca [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="02c38-199">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="02c38-200">Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu [element BadRequest](xref:System.Web.Http.ApiController.BadRequest*) odpowiedzi jest to możliwe, zbyt.</span><span class="sxs-lookup"><span data-stu-id="02c38-200">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="02c38-201">Aby uzyskać więcej informacji, zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="02c38-201">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="02c38-202">Adnotacje danych umożliwia Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="02c38-202">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="02c38-203">Dekoracji działanie z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="02c38-203">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="02c38-204">W programie ASP.NET Core 2.2 lub nowszej, można użyć konwencji zamiast jawnie urządzanie poszczególne akcje za pomocą `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="02c38-204">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="02c38-205">Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="02c38-205">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="02c38-206">Generatora struktury Swagger teraz można dokładnie opisują tej akcji i wygenerowanego klienci wiedzieli, czego otrzymują podczas wywoływania punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="02c38-206">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="02c38-207">Jako zalecenie dekoracji wszystkie akcje za pomocą tych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="02c38-207">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="02c38-208">Aby uzyskać wskazówki, jakie odpowiedzi HTTP, powinien zwrócić swoje działania interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="02c38-208">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
