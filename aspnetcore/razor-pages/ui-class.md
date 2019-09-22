---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/21/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 1b544e208be049f02d01e35daa6eb3bfba94265a
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176478"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="9312f-103">Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9312f-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="9312f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9312f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9312f-105">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="9312f-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="9312f-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="9312f-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="9312f-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="9312f-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="9312f-108">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="9312f-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9312f-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="9312f-110">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="9312f-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9312f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9312f-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9312f-112">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="9312f-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9312f-113">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9312f-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9312f-114">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="9312f-115">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="9312f-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="9312f-116">Sprawdź, czy wybrano **ASP.NET Core 3,0** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="9312f-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="9312f-117">Wybierz pozycję > **Biblioteka klas Razor** **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="9312f-118">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="9312f-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="9312f-119">Opcja szablonu w programie Visual Studio zapewnia obsługę szablonów dla stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="9312f-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9312f-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9312f-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9312f-121">W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="9312f-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="9312f-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9312f-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="9312f-123">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="9312f-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="9312f-124">Aby zapewnić obsługę stron`dotnet new razorclasslib -support-pages-and-views`i widoków, należy przekazać opcję().`-support-pages-and-views`</span><span class="sxs-lookup"><span data-stu-id="9312f-124">Pass the `-support-pages-and-views` option (`dotnet new razorclasslib -support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="9312f-125">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9312f-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="9312f-126">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="9312f-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="9312f-127">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="9312f-128">Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="9312f-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="9312f-129">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość `~/Pages` zamiast. `~/Areas/Pages`</span><span class="sxs-lookup"><span data-stu-id="9312f-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="9312f-130">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="9312f-130">Reference RCL content</span></span>

<span data-ttu-id="9312f-131">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="9312f-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="9312f-132">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="9312f-132">NuGet package.</span></span> <span data-ttu-id="9312f-133">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="9312f-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="9312f-134">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="9312f-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="9312f-135">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="9312f-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="9312f-136">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="9312f-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="9312f-137">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="9312f-138">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="9312f-139">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="9312f-140">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9312f-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="9312f-141">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9312f-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="9312f-142">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="9312f-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="9312f-143">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="9312f-143">RCL Pages layout</span></span>

<span data-ttu-id="9312f-144">Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:</span><span class="sxs-lookup"><span data-stu-id="9312f-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="9312f-145">*RazorUIClassLib/stron*</span><span class="sxs-lookup"><span data-stu-id="9312f-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="9312f-146">*RazorUIClassLib/stron/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="9312f-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="9312f-147">Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9312f-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="9312f-148">`<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="9312f-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="9312f-149">Tworzenie RCL przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="9312f-149">Create an RCL with static assets</span></span>

<span data-ttu-id="9312f-150">RCL może wymagać pomocnika zasobów statycznych, do których może odwoływać się aplikacja zużywana przez RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="9312f-151">ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="9312f-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="9312f-152">Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="9312f-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="9312f-153">Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.</span><span class="sxs-lookup"><span data-stu-id="9312f-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="9312f-154">Wyklucz zasoby statyczne</span><span class="sxs-lookup"><span data-stu-id="9312f-154">Exclude static assets</span></span>

<span data-ttu-id="9312f-155">Aby wykluczyć statyczne elementy zawartości, Dodaj żądaną ścieżkę wykluczenia do `$(DefaultItemExcludes)` grupy właściwości w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="9312f-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="9312f-156">Oddzielaj wpisy średnikami (`;`).</span><span class="sxs-lookup"><span data-stu-id="9312f-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="9312f-157">W poniższym przykładzie arkusz stylów *lib. css* w folderze *wwwroot* nie jest traktowany jako statyczny zasób i nie jest uwzględniony w opublikowanym RCL:</span><span class="sxs-lookup"><span data-stu-id="9312f-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="9312f-158">Integracja języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="9312f-158">Typescript integration</span></span>

<span data-ttu-id="9312f-159">Aby dołączyć pliki TypeScript do RCL:</span><span class="sxs-lookup"><span data-stu-id="9312f-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="9312f-160">Umieść pliki TypeScript ( *. TS*) poza folderem *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="9312f-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="9312f-161">Na przykład Umieść pliki w folderze *Client* .</span><span class="sxs-lookup"><span data-stu-id="9312f-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="9312f-162">Skonfiguruj dane wyjściowe kompilacji TypeScript dla folderu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="9312f-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="9312f-163">Ustaw właściwość wewnątrz elementu `PropertyGroup` w pliku projektu: `TypescriptOutDir`</span><span class="sxs-lookup"><span data-stu-id="9312f-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="9312f-164">Dołącz obiekt docelowy TypeScript jako zależność `ResolveCurrentProjectStaticWebAssets` obiektu docelowego, dodając następujący element docelowy wewnątrz `PropertyGroup` pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="9312f-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="9312f-165">Korzystanie z zawartości z RCL, do którego istnieje odwołanie</span><span class="sxs-lookup"><span data-stu-id="9312f-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="9312f-166">Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione dla aplikacji zużywanej pod prefiksem `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="9312f-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="9312f-167">Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej pod adresem `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="9312f-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="9312f-168">Aplikacja, która zużywa odwołania `<script>` `<img>`, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z, `<style>`, i innymi tagami HTML.</span><span class="sxs-lookup"><span data-stu-id="9312f-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="9312f-169">Aplikacja zużywana musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="9312f-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="9312f-170">W przypadku uruchamiania aplikacji zużywającej dane wyjściowe kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="9312f-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="9312f-171">Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych `UseStaticWebAssets` kompilacji, należy wywołać konstruktora hosta w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9312f-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="9312f-172">Wywołanie `UseStaticWebAssets` nie jest wymagane w przypadku uruchamiania aplikacji z opublikowanych danych`dotnet publish`wyjściowych ().</span><span class="sxs-lookup"><span data-stu-id="9312f-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="9312f-173">Przepływ programistyczny dla wieloprojektowych</span><span class="sxs-lookup"><span data-stu-id="9312f-173">Multi-project development flow</span></span>

<span data-ttu-id="9312f-174">Po uruchomieniu aplikacji zużywanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="9312f-174">When the consuming app runs:</span></span>

* <span data-ttu-id="9312f-175">Zasoby w RCL pozostają w swoich oryginalnych folderach.</span><span class="sxs-lookup"><span data-stu-id="9312f-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="9312f-176">Elementy zawartości nie są przenoszone do aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="9312f-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="9312f-177">Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9312f-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="9312f-178">Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9312f-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="9312f-179">Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów.</span><span class="sxs-lookup"><span data-stu-id="9312f-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="9312f-180">Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="9312f-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="9312f-181">Publikowanie</span><span class="sxs-lookup"><span data-stu-id="9312f-181">Publish</span></span>

<span data-ttu-id="9312f-182">Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="9312f-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9312f-183">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="9312f-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="9312f-184">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="9312f-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="9312f-185">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="9312f-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="9312f-186">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="9312f-187">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9312f-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="9312f-188">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="9312f-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9312f-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9312f-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9312f-190">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="9312f-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9312f-191">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9312f-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9312f-192">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="9312f-193">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="9312f-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="9312f-194">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="9312f-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9312f-195">Wybierz pozycję > **Biblioteka klas Razor** **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="9312f-196">RCL ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="9312f-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9312f-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9312f-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9312f-198">W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="9312f-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="9312f-199">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9312f-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="9312f-200">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9312f-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="9312f-201">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="9312f-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="9312f-202">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="9312f-203">Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="9312f-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="9312f-204">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość `~/Pages` zamiast. `~/Areas/Pages`</span><span class="sxs-lookup"><span data-stu-id="9312f-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="9312f-205">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="9312f-205">Reference RCL content</span></span>

<span data-ttu-id="9312f-206">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="9312f-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="9312f-207">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="9312f-207">NuGet package.</span></span> <span data-ttu-id="9312f-208">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="9312f-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="9312f-209">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="9312f-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="9312f-210">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="9312f-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="9312f-211">Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9312f-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="9312f-212">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="9312f-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="9312f-213">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="9312f-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="9312f-214">Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="9312f-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="9312f-215">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="9312f-215">Test the download app</span></span>

<span data-ttu-id="9312f-216">Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="9312f-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9312f-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9312f-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9312f-218">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9312f-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="9312f-219">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="9312f-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9312f-220">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9312f-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9312f-221">Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.</span><span class="sxs-lookup"><span data-stu-id="9312f-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="9312f-222">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="9312f-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="9312f-223">Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="9312f-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="9312f-224">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="9312f-224">Create an RCL</span></span>

<span data-ttu-id="9312f-225">W tej sekcji jest tworzony RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-225">In this section, an RCL is created.</span></span> <span data-ttu-id="9312f-226">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9312f-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9312f-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9312f-228">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="9312f-228">Create the RCL project:</span></span>

* <span data-ttu-id="9312f-229">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="9312f-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9312f-230">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9312f-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9312f-231">Nadaj aplikacji nazwę **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="9312f-232">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="9312f-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9312f-233">Wybierz pozycję > **Biblioteka klas Razor** **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="9312f-234">Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9312f-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9312f-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9312f-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9312f-236">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9312f-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="9312f-237">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="9312f-237">The preceding commands:</span></span>

* <span data-ttu-id="9312f-238">`RazorUIClassLib` Tworzy RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="9312f-239">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="9312f-240">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="9312f-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="9312f-241">Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="9312f-242">*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="9312f-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="9312f-243">Dodawanie Razor plików i folderów do projektu</span><span class="sxs-lookup"><span data-stu-id="9312f-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="9312f-244">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9312f-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="9312f-245">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9312f-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="9312f-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="9312f-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="9312f-247">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="9312f-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="9312f-248">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9312f-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="9312f-249">Aby uzyskać więcej informacji na temat *_ViewImports.cshtml*, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="9312f-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="9312f-250">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="9312f-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="9312f-251">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="9312f-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="9312f-252">*RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.</span><span class="sxs-lookup"><span data-stu-id="9312f-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="9312f-253">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="9312f-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9312f-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9312f-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9312f-255">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="9312f-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="9312f-256">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** > **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="9312f-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="9312f-257">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9312f-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9312f-258">Określanie nazwy aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="9312f-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="9312f-259">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="9312f-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="9312f-260">Wybierz pozycję > **aplikacja sieci Web** **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="9312f-261">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="9312f-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="9312f-262">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **kompilacja zależności** > **projektu zależności**.</span><span class="sxs-lookup"><span data-stu-id="9312f-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="9312f-263">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="9312f-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="9312f-264">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Dodaj** > **odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="9312f-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="9312f-265">W oknie dialogowym **Menedżer odwołań** Sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="9312f-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="9312f-266">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="9312f-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9312f-267">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9312f-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9312f-268">Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:</span><span class="sxs-lookup"><span data-stu-id="9312f-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="9312f-269">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="9312f-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="9312f-270">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="9312f-270">Test WebApp1</span></span>

<span data-ttu-id="9312f-271">Przejdź do `/MyFeature/Page1` strony, aby sprawdzić, czy biblioteka klas interfejsu użytkownika Razor jest używana.</span><span class="sxs-lookup"><span data-stu-id="9312f-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="9312f-272">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="9312f-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="9312f-273">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="9312f-274">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="9312f-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="9312f-275">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="9312f-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="9312f-276">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9312f-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="9312f-277">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9312f-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="9312f-278">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="9312f-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="9312f-279">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="9312f-279">RCL Pages layout</span></span>

<span data-ttu-id="9312f-280">Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:</span><span class="sxs-lookup"><span data-stu-id="9312f-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="9312f-281">*RazorUIClassLib/stron*</span><span class="sxs-lookup"><span data-stu-id="9312f-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="9312f-282">*RazorUIClassLib/stron/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="9312f-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="9312f-283">Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9312f-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="9312f-284">`<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="9312f-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
