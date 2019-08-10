---
title: Wprowadzenie do NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak używać NSwag do generowania dokumentacji i stron pomocy dla ASP.NET Core internetowego interfejsu API.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: af8e2a266e54364857f0b49cc78a54683dff9de4
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68915093"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="099d1-103">Wprowadzenie do NSwag i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="099d1-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="099d1-104">[Christoph Nienaber](https://twitter.com/zuckerthoben), [Suter](https://rsuter.com)i [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="099d1-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="099d1-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="099d1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="099d1-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="099d1-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="099d1-107">NSwag oferuje następujące możliwości:</span><span class="sxs-lookup"><span data-stu-id="099d1-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="099d1-108">Możliwość korzystania z interfejsu użytkownika programu Swagger i generatora Swagger.</span><span class="sxs-lookup"><span data-stu-id="099d1-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="099d1-109">Elastyczne możliwości generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="099d1-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="099d1-110">W przypadku korzystania z usługi NSwag nie jest potrzebny&mdash;istniejący interfejs API, który zawiera interfejsy API innych firm, które zawierają strukturę Swagger i generują implementację klienta.</span><span class="sxs-lookup"><span data-stu-id="099d1-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="099d1-111">NSwag umożliwia przyspieszenie cyklu programowania i łatwe dostosowanie do zmian interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="099d1-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="099d1-112">Rejestrowanie oprogramowania pośredniczącego NSwag</span><span class="sxs-lookup"><span data-stu-id="099d1-112">Register the NSwag middleware</span></span>

<span data-ttu-id="099d1-113">Zarejestruj oprogramowanie pośredniczące NSwag w programie:</span><span class="sxs-lookup"><span data-stu-id="099d1-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="099d1-114">Generuj specyfikację Swagger dla zaimplementowanego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="099d1-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="099d1-115">Zaoferują interfejs użytkownika struktury Swagger, aby przeglądać i testować internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="099d1-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="099d1-116">Aby użyć oprogramowania [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core pośredniczącego, zainstaluj pakiet NuGet [NSwag. AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) .</span><span class="sxs-lookup"><span data-stu-id="099d1-116">To use the [NSwag](https://github.com/RicoSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="099d1-117">Ten pakiet zawiera oprogramowanie pośredniczące do generowania i obsługiwania specyfikacji struktury Swagger, interfejsu użytkownika struktury Swagger (v2 i V3) oraz [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="099d1-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="099d1-118">Aby zainstalować pakiet NuGet NSwag, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="099d1-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="099d1-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="099d1-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="099d1-120">W oknie **konsola Menedżera pakietów** :</span><span class="sxs-lookup"><span data-stu-id="099d1-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="099d1-121">Przejdź do **wyświetlania** > innych**konsoli Menedżera pakietów** **systemu Windows** > </span><span class="sxs-lookup"><span data-stu-id="099d1-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="099d1-122">Przejdź do katalogu, w którym znajduje się plik *TodoApi. csproj*</span><span class="sxs-lookup"><span data-stu-id="099d1-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="099d1-123">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="099d1-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="099d1-124">W oknie dialogowym **Zarządzanie pakietami NuGet** :</span><span class="sxs-lookup"><span data-stu-id="099d1-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="099d1-125">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="099d1-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="099d1-126">Ustaw **Źródło pakietu** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="099d1-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="099d1-127">Wprowadź ciąg "NSwag. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="099d1-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="099d1-128">Wybierz pakiet "NSwag. AspNetCore" z karty **Przeglądaj** , a następnie kliknij przycisk **Instaluj** .</span><span class="sxs-lookup"><span data-stu-id="099d1-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="099d1-129">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="099d1-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="099d1-130">Kliknij prawym przyciskiem myszy folder *pakiety* w **okienko rozwiązania** > **Dodaj pakiety...**</span><span class="sxs-lookup"><span data-stu-id="099d1-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="099d1-131">Ustaw listę rozwijaną **źródła** okna **Dodaj pakiety** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="099d1-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="099d1-132">Wprowadź ciąg "NSwag. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="099d1-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="099d1-133">Wybierz pakiet "NSwag. AspNetCore" z okienka wyników, a następnie kliknij pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="099d1-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="099d1-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="099d1-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="099d1-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="099d1-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="099d1-136">Dodawanie i Konfigurowanie oprogramowania pośredniczącego programu Swagger</span><span class="sxs-lookup"><span data-stu-id="099d1-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="099d1-137">Dodaj i skonfiguruj strukturę Swagger w aplikacji ASP.NET Core, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="099d1-137">Add and configure Swagger in your ASP.NET Core app by performing the following steps:</span></span>

* <span data-ttu-id="099d1-138">`Startup.ConfigureServices` W metodzie Zarejestruj wymagane usługi Swagger:</span><span class="sxs-lookup"><span data-stu-id="099d1-138">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="099d1-139">`Startup.Configure` W metodzie Włącz oprogramowanie pośredniczące do obsługi wygenerowanej specyfikacji struktury Swagger i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="099d1-139">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="099d1-140">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="099d1-140">Launch the app.</span></span> <span data-ttu-id="099d1-141">Przejdź do:</span><span class="sxs-lookup"><span data-stu-id="099d1-141">Navigate to:</span></span>
  * <span data-ttu-id="099d1-142">`http://localhost:<port>/swagger`Aby wyświetlić interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="099d1-142">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="099d1-143">`http://localhost:<port>/swagger/v1/swagger.json`Aby wyświetlić specyfikację struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="099d1-143">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="099d1-144">Generowanie kodu</span><span class="sxs-lookup"><span data-stu-id="099d1-144">Code generation</span></span>

<span data-ttu-id="099d1-145">Aby skorzystać z możliwości generowania kodu w programie NSwag, należy wybrać jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="099d1-145">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="099d1-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) Aplikacja klasyczna systemu Windows do generowania kodu klienta interfejsu C# API w języku lub TypeScript. &ndash;</span><span class="sxs-lookup"><span data-stu-id="099d1-146">[NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="099d1-147">Pakiety NuGet [NSwag. CodeGeneration. CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag. CodeGeneration. TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) na potrzeby generowania kodu w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="099d1-147">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="099d1-148">NSwag z [wiersza polecenia](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="099d1-148">NSwag from the [command line](https://github.com/RicoSuter/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="099d1-149">Pakiet NuGet [NSwag. MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) .</span><span class="sxs-lookup"><span data-stu-id="099d1-149">The [NSwag.MSBuild](https://github.com/RicoSuter/NSwag/wiki/MSBuild) NuGet package.</span></span>
* <span data-ttu-id="099d1-150">&ndash; [Połączona usługa openapi (Swagger)](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) jest połączoną usługą programu Visual Studio do generowania kodu klienta interfejsu API C# w języku lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="099d1-150">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="099d1-151">Program generuje C# również kontrolery dla usług openapi Services z NSwag.</span><span class="sxs-lookup"><span data-stu-id="099d1-151">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="099d1-152">Generuj kod przy użyciu NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="099d1-152">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="099d1-153">Zainstaluj program NSwagStudio, postępując zgodnie z instrukcjami w [repozytorium GitHub NSwagStudio](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="099d1-153">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RicoSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="099d1-154">Uruchom NSwagStudio i wprowadź adres URL pliku *Swagger. JSON* w polu tekstowym **adres URL specyfikacji struktury Swagger** .</span><span class="sxs-lookup"><span data-stu-id="099d1-154">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="099d1-155">Na przykład *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="099d1-155">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="099d1-156">Kliknij przycisk **Utwórz kopię lokalną** , aby wygenerować reprezentację JSON specyfikacji struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="099d1-156">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Utwórz lokalną kopię specyfikacji struktury Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="099d1-158">W obszarze dane **wyjściowe** kliknij pole wyboru **Klient CSharp** .</span><span class="sxs-lookup"><span data-stu-id="099d1-158">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="099d1-159">W zależności od projektu można również wybrać **klienta TypeScript** lub **kontroler internetowego interfejsu API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="099d1-159">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="099d1-160">W przypadku wybrania opcji **kontroler internetowego interfejsu API CSharp**Specyfikacja usługi ponownie kompiluje usługę, służącą jako generacja odwrotna.</span><span class="sxs-lookup"><span data-stu-id="099d1-160">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="099d1-161">Kliknij pozycję **Generuj dane wyjściowe** , aby C# utworzyć kompletną implementację projektu *TodoApi. NSwag* .</span><span class="sxs-lookup"><span data-stu-id="099d1-161">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="099d1-162">Aby wyświetlić wygenerowany kod klienta, kliknij kartę **Klient CSharp** :</span><span class="sxs-lookup"><span data-stu-id="099d1-162">To see the generated client code, click the **CSharp Client** tab:</span></span>

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
> <span data-ttu-id="099d1-163">Kod C# klienta jest generowany na podstawie opcji wybranych na karcie **Ustawienia** . Zmodyfikuj ustawienia, aby wykonać zadania, takie jak domyślna zmiana nazw obszaru nazw i generowanie metody synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="099d1-163">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="099d1-164">Skopiuj wygenerowany C# kod do pliku w projekcie klienta, który będzie korzystał z interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="099d1-164">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="099d1-165">Rozpocznij korzystanie z internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="099d1-165">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="099d1-166">Dostosuj dokumentację interfejsu API</span><span class="sxs-lookup"><span data-stu-id="099d1-166">Customize API documentation</span></span>

<span data-ttu-id="099d1-167">Struktura Swagger zawiera opcje dokumentujące model obiektów, co ułatwia użycie internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="099d1-167">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="099d1-168">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="099d1-168">API info and description</span></span>

<span data-ttu-id="099d1-169">W metodzie akcja konfiguracji przeniesiona `AddSwaggerDocument` do metody powoduje dodanie informacji, takich jak autor, licencja i opis: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="099d1-169">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="099d1-170">Interfejs użytkownika struktury Swagger wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="099d1-170">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury Swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="099d1-172">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="099d1-172">XML comments</span></span>

<span data-ttu-id="099d1-173">Aby włączyć Komentarze XML, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="099d1-173">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="099d1-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="099d1-174">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="099d1-175">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie wybierz polecenie **edytuj < Project_Name >. csproj**.</span><span class="sxs-lookup"><span data-stu-id="099d1-175">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="099d1-176">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="099d1-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="099d1-177">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Właściwości**</span><span class="sxs-lookup"><span data-stu-id="099d1-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="099d1-178">Sprawdź pole **plik dokumentacji XML** w sekcji **dane wyjściowe** na karcie **kompilacja**</span><span class="sxs-lookup"><span data-stu-id="099d1-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="099d1-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="099d1-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="099d1-180">W *okienko rozwiązania*naciśnij klawisz **Control** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="099d1-180">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="099d1-181">Przejdź do **menu Narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="099d1-181">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="099d1-182">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="099d1-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="099d1-183">Otwórz okno dialogowe **Opcje projektu** > **kompilator** **kompilacji** ></span><span class="sxs-lookup"><span data-stu-id="099d1-183">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="099d1-184">Zaznacz pole **Generuj dokumentację XML** w sekcji **Opcje ogólne** .</span><span class="sxs-lookup"><span data-stu-id="099d1-184">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="099d1-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="099d1-185">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="099d1-186">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="099d1-186">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="099d1-187">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="099d1-187">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="099d1-188">Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecanym typem zwracanym dla akcji interfejsu API sieci Web jest [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), nie może wnioskować o to, co działa i co zwraca.</span><span class="sxs-lookup"><span data-stu-id="099d1-188">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="099d1-189">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="099d1-189">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="099d1-190">Poprzednia Akcja `IActionResult`zwróci wartość, ale wewnątrz akcji, która zwraca albo [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) lub [nieprawidłowego żądania](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="099d1-190">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="099d1-191">Użyj adnotacji danych, aby poinformować klientów, których kodów stanu HTTP Ta akcja jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="099d1-191">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="099d1-192">Dekorować akcję z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="099d1-192">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="099d1-193">Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecanym typem zwracanym dla akcji interfejsu API sieci Web jest [\<ActionResult T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), można tylko wnioskować o typie `T`zwracanym zdefiniowanym przez.</span><span class="sxs-lookup"><span data-stu-id="099d1-193">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="099d1-194">Nie można automatycznie wywnioskować innych możliwych typów zwracanych.</span><span class="sxs-lookup"><span data-stu-id="099d1-194">You can't automatically infer other possible return types.</span></span>

<span data-ttu-id="099d1-195">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="099d1-195">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="099d1-196">Poprzednia Akcja `ActionResult<T>`zwraca wartość.</span><span class="sxs-lookup"><span data-stu-id="099d1-196">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="099d1-197">Wewnątrz akcji zwraca [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="099d1-197">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="099d1-198">Ponieważ kontroler ma atrybut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) , istnieje również odpowiedź [nieprawidłowego żądania](xref:System.Web.Http.ApiController.BadRequest*) .</span><span class="sxs-lookup"><span data-stu-id="099d1-198">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="099d1-199">Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="099d1-199">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="099d1-200">Użyj adnotacji danych, aby poinformować klientów, których kodów stanu HTTP Ta akcja jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="099d1-200">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="099d1-201">Dekorować akcję z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="099d1-201">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="099d1-202">W ASP.NET Core 2,2 lub nowszych można używać konwencji zamiast jawnie dekorowania nazwy poszczególne akcje z `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="099d1-202">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="099d1-203">Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="099d1-203">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="099d1-204">Generator Swagger może teraz prawidłowo opisać tę akcję, a wygenerowane klienci wiedzą, co otrzymują podczas wywoływania punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="099d1-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="099d1-205">Zgodnie z zaleceniem dekorować wszystkie akcje z tymi atrybutami.</span><span class="sxs-lookup"><span data-stu-id="099d1-205">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="099d1-206">Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, które powinny być zwracane przez działania interfejsu API, zobacz [specyfikację RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="099d1-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
