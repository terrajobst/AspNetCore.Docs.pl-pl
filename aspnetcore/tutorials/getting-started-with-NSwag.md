---
title: Wprowadzenie do NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak używać NSwag do generowania dokumentacji i stron pomocy dla ASP.NET Core internetowego interfejsu API.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 676e911a14bd128998c987b3f955c40e19af98d1
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881305"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="74b84-103">Wprowadzenie do NSwag i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74b84-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="74b84-104">[Christoph Nienaber](https://twitter.com/zuckerthoben), [Suter](https://rsuter.com)i [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="74b84-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="74b84-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74b84-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="74b84-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74b84-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="74b84-107">NSwag oferuje następujące możliwości:</span><span class="sxs-lookup"><span data-stu-id="74b84-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="74b84-108">Możliwość korzystania z interfejsu użytkownika programu Swagger i generatora Swagger.</span><span class="sxs-lookup"><span data-stu-id="74b84-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="74b84-109">Elastyczne możliwości generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="74b84-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="74b84-110">W przypadku usługi NSwag nie jest potrzebny istniejący interfejs API&mdash;można używać interfejsów API innych firm, które zawierają strukturę Swagger i generować implementację klienta.</span><span class="sxs-lookup"><span data-stu-id="74b84-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="74b84-111">NSwag umożliwia przyspieszenie cyklu programowania i łatwe dostosowanie do zmian interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="74b84-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="74b84-112">Rejestrowanie oprogramowania pośredniczącego NSwag</span><span class="sxs-lookup"><span data-stu-id="74b84-112">Register the NSwag middleware</span></span>

<span data-ttu-id="74b84-113">Zarejestruj oprogramowanie pośredniczące NSwag w programie:</span><span class="sxs-lookup"><span data-stu-id="74b84-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="74b84-114">Generuj specyfikację Swagger dla zaimplementowanego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="74b84-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="74b84-115">Zaoferują interfejs użytkownika struktury Swagger, aby przeglądać i testować internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="74b84-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="74b84-116">Aby użyć oprogramowania [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core pośredniczącego, zainstaluj pakiet NuGet [NSwag. AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) .</span><span class="sxs-lookup"><span data-stu-id="74b84-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="74b84-117">Ten pakiet zawiera oprogramowanie pośredniczące do generowania i obsługiwania specyfikacji struktury Swagger, interfejsu użytkownika struktury Swagger (v2 i V3) oraz [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="74b84-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="74b84-118">Aby zainstalować pakiet NuGet NSwag, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="74b84-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="74b84-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74b84-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="74b84-120">W oknie **konsola Menedżera pakietów** :</span><span class="sxs-lookup"><span data-stu-id="74b84-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="74b84-121">Przejdź do **widoku** > **innej** **konsoli Menedżera pakietów** > Windows</span><span class="sxs-lookup"><span data-stu-id="74b84-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="74b84-122">Przejdź do katalogu, w którym znajduje się plik *TodoApi. csproj*</span><span class="sxs-lookup"><span data-stu-id="74b84-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="74b84-123">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="74b84-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="74b84-124">W oknie dialogowym **Zarządzanie pakietami NuGet** :</span><span class="sxs-lookup"><span data-stu-id="74b84-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="74b84-125">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="74b84-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="74b84-126">Ustaw **Źródło pakietu** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="74b84-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="74b84-127">Wprowadź ciąg "NSwag. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="74b84-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="74b84-128">Wybierz pakiet "NSwag. AspNetCore" z karty **Przeglądaj** , a następnie kliknij przycisk **Instaluj** .</span><span class="sxs-lookup"><span data-stu-id="74b84-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="74b84-129">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="74b84-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="74b84-130">Kliknij prawym przyciskiem myszy folder *pakiety* w **okienko rozwiązania** > **Dodaj pakiety...**</span><span class="sxs-lookup"><span data-stu-id="74b84-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="74b84-131">Ustaw listę rozwijaną **źródła** okna **Dodaj pakiety** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="74b84-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="74b84-132">Wprowadź ciąg "NSwag. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="74b84-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="74b84-133">Wybierz pakiet "NSwag. AspNetCore" z okienka wyników, a następnie kliknij pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="74b84-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="74b84-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="74b84-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="74b84-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="74b84-135">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="74b84-136">Dodawanie i Konfigurowanie oprogramowania pośredniczącego programu Swagger</span><span class="sxs-lookup"><span data-stu-id="74b84-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="74b84-137">Dodaj i skonfiguruj strukturę Swagger w aplikacji ASP.NET Core, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="74b84-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="74b84-138">W metodzie `Startup.ConfigureServices` Zarejestruj wymagane usługi Swagger:</span><span class="sxs-lookup"><span data-stu-id="74b84-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="74b84-139">W metodzie `Startup.Configure` Włącz oprogramowanie pośredniczące do obsługi wygenerowanej specyfikacji struktury Swagger i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="74b84-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="74b84-140">Uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="74b84-140">Launch the app.</span></span> <span data-ttu-id="74b84-141">Przejdź do:</span><span class="sxs-lookup"><span data-stu-id="74b84-141">Navigate to:</span></span>
  * <span data-ttu-id="74b84-142">`http://localhost:<port>/swagger` wyświetlić interfejsu użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="74b84-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="74b84-143">`http://localhost:<port>/swagger/v1/swagger.json` wyświetlić specyfikacji struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="74b84-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="74b84-144">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="74b84-144">Code generation</span></span>

<span data-ttu-id="74b84-145">Aby skorzystać z możliwości generowania kodu w programie NSwag, należy wybrać jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="74b84-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="74b84-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; aplikacji klasycznej dla systemu Windows w celu wygenerowania C# kodu klienta interfejsu API w języku lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="74b84-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="74b84-147">Pakiety NuGet [NSwag. CodeGeneration. CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag. CodeGeneration. TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) na potrzeby generowania kodu w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="74b84-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="74b84-148">NSwag z [wiersza polecenia](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="74b84-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="74b84-149">Pakiet NuGet [NSwag. MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) .</span><span class="sxs-lookup"><span data-stu-id="74b84-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/NSwag.MSBuild) NuGet package.</span></span>
* <span data-ttu-id="74b84-150">[Połączona usługa openapi (Swagger)](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) nie &ndash; połączonej usługi programu Visual Studio w celu wygenerowania kodu klienta C# interfejsu API w języku lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="74b84-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="74b84-151">Program generuje C# również kontrolery dla usług openapi Services z NSwag.</span><span class="sxs-lookup"><span data-stu-id="74b84-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="74b84-152">Generuj kod przy użyciu NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="74b84-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="74b84-153">Zainstaluj program NSwagStudio, postępując zgodnie z instrukcjami w [repozytorium GitHub NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="74b84-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="74b84-154">Uruchom NSwagStudio i wprowadź adres URL pliku *Swagger. JSON* w polu tekstowym **adres URL specyfikacji struktury Swagger** .</span><span class="sxs-lookup"><span data-stu-id="74b84-154">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="74b84-155">Przykładowy adres URL to *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="74b84-155">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="74b84-156">Kliknij przycisk **Utwórz kopię lokalną** , aby wygenerować reprezentację JSON specyfikacji struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="74b84-156">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Utwórz lokalną kopię specyfikacji struktury Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="74b84-158">W obszarze dane **wyjściowe** kliknij pole wyboru **Klient CSharp** .</span><span class="sxs-lookup"><span data-stu-id="74b84-158">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="74b84-159">W zależności od projektu można również wybrać **klienta TypeScript** lub **kontroler internetowego interfejsu API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="74b84-159">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="74b84-160">W przypadku wybrania opcji **kontroler internetowego interfejsu API CSharp**Specyfikacja usługi ponownie kompiluje usługę, służącą jako generacja odwrotna.</span><span class="sxs-lookup"><span data-stu-id="74b84-160">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="74b84-161">Kliknij pozycję **Generuj dane wyjściowe** , aby C# utworzyć kompletną implementację projektu *TodoApi. NSwag* .</span><span class="sxs-lookup"><span data-stu-id="74b84-161">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="74b84-162">Aby wyświetlić wygenerowany kod klienta, kliknij kartę **Klient CSharp** :</span><span class="sxs-lookup"><span data-stu-id="74b84-162">To see the generated client code, click the **CSharp Client** tab:</span></span>

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
> <span data-ttu-id="74b84-163">Kod C# klienta jest generowany na podstawie opcji wybranych na karcie **Ustawienia** . zmodyfikuj ustawienia, aby wykonać zadania, takie jak domyślna zmiana nazw obszaru nazw i generowanie metody synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="74b84-163">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="74b84-164">Skopiuj wygenerowany C# kod do pliku w projekcie klienta, który będzie korzystał z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="74b84-164">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="74b84-165">Rozpocznij korzystanie z internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="74b84-165">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="74b84-166">Dostosuj dokumentację interfejsu API</span><span class="sxs-lookup"><span data-stu-id="74b84-166">Customize API documentation</span></span>

<span data-ttu-id="74b84-167">Struktura Swagger zawiera opcje dokumentujące model obiektów, co ułatwia użycie internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="74b84-167">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="74b84-168">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="74b84-168">API info and description</span></span>

<span data-ttu-id="74b84-169">W metodzie `Startup.ConfigureServices` akcja konfiguracyjna przeniesiona do metody `AddSwaggerDocument` dodaje takie informacje, jak autor, licencja i opis:</span><span class="sxs-lookup"><span data-stu-id="74b84-169">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="74b84-170">Interfejs użytkownika struktury Swagger wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="74b84-170">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury Swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="74b84-172">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="74b84-172">XML comments</span></span>

<span data-ttu-id="74b84-173">Aby włączyć Komentarze XML, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="74b84-173">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="74b84-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74b84-174">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="74b84-175">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie wybierz polecenie **edytuj < Project_Name >. csproj**.</span><span class="sxs-lookup"><span data-stu-id="74b84-175">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="74b84-176">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="74b84-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="74b84-177">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Właściwości**</span><span class="sxs-lookup"><span data-stu-id="74b84-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="74b84-178">Sprawdź pole **plik dokumentacji XML** w sekcji **dane wyjściowe** na karcie **kompilacja**</span><span class="sxs-lookup"><span data-stu-id="74b84-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="74b84-179">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="74b84-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="74b84-180">W *okienko rozwiązania*naciśnij klawisz **Control** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="74b84-180">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="74b84-181">Przejdź do **menu narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="74b84-181">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="74b84-182">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="74b84-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="74b84-183">Otwórz okno dialogowe **Opcje projektu** > **kompilacja** > **kompilator**</span><span class="sxs-lookup"><span data-stu-id="74b84-183">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="74b84-184">Zaznacz pole **Generuj dokumentację XML** w sekcji **Opcje ogólne** .</span><span class="sxs-lookup"><span data-stu-id="74b84-184">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="74b84-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="74b84-185">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="74b84-186">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="74b84-186">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="74b84-187">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="74b84-187">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="74b84-188">Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecanym typem zwracanym dla akcji interfejsu API sieci Web jest [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), nie może wnioskować o to, co działa i co zwraca.</span><span class="sxs-lookup"><span data-stu-id="74b84-188">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="74b84-189">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="74b84-189">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="74b84-190">Poprzednia akcja zwraca `IActionResult`, ale wewnątrz akcji, zwraca albo [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) lub [nieprawidłowego żądania](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="74b84-190">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="74b84-191">Użyj adnotacji danych, aby poinformować klientów, których kodów stanu HTTP Ta akcja jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="74b84-191">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="74b84-192">Oznacz akcję następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="74b84-192">Mark the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="74b84-193">Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecanym typem zwracanym dla akcji interfejsu API sieci Web jest [ActionResult\<t >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), może on tylko wnioskować o typie zwracanym zdefiniowanym przez `T`.</span><span class="sxs-lookup"><span data-stu-id="74b84-193">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="74b84-194">Nie można automatycznie wywnioskować innych możliwych typów zwracanych.</span><span class="sxs-lookup"><span data-stu-id="74b84-194">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="74b84-195">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="74b84-195">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="74b84-196">Poprzednia akcja zwraca `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="74b84-196">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="74b84-197">Wewnątrz akcji zwraca [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="74b84-197">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="74b84-198">Ponieważ kontroler ma atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) , istnieje również odpowiedź [nieprawidłowego żądania](xref:System.Web.Http.ApiController.BadRequest*) .</span><span class="sxs-lookup"><span data-stu-id="74b84-198">Since the controller has the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="74b84-199">Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="74b84-199">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="74b84-200">Użyj adnotacji danych, aby poinformować klientów, których kodów stanu HTTP Ta akcja jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="74b84-200">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="74b84-201">Oznacz akcję następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="74b84-201">Mark the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="74b84-202">W ASP.NET Core 2,2 lub nowszych można używać konwencji zamiast jawnie dekorowania nazwy poszczególnych akcji z `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="74b84-202">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="74b84-203">Aby uzyskać więcej informacji, zobacz temat <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="74b84-203">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="74b84-204">Generator Swagger może teraz prawidłowo opisać tę akcję, a wygenerowane klienci wiedzą, co otrzymują podczas wywoływania punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="74b84-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="74b84-205">Zgodnie z zaleceniem Oznacz wszystkie akcje z tymi atrybutami.</span><span class="sxs-lookup"><span data-stu-id="74b84-205">As a recommendation, mark all actions with these attributes.</span></span>

<span data-ttu-id="74b84-206">Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, które powinny być zwracane przez działania interfejsu API, zobacz [specyfikację RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="74b84-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
