---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 8813244ea6d00b170d9f95d12743e9fee38bf810
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172644"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="b4866-103">Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4866-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="b4866-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b4866-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b4866-105">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b4866-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="b4866-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="b4866-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b4866-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="b4866-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b4866-108">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b4866-109">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b4866-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b4866-110">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="b4866-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4866-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4866-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b4866-112">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b4866-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="b4866-113">Wybierz pozycję **Biblioteka klas Razor** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b4866-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="b4866-114">Nazwij bibliotekę (na przykład "RazorClassLib"), > **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b4866-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="b4866-115">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b4866-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="b4866-116">Wybierz **strony i widoki pomocy technicznej** , jeśli chcesz obsługiwać widoki.</span><span class="sxs-lookup"><span data-stu-id="b4866-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="b4866-117">Domyślnie obsługiwane są tylko Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b4866-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="b4866-118">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b4866-118">Select **Create**.</span></span>

<span data-ttu-id="b4866-119">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="b4866-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="b4866-120">Opcja **strony i widoki pomocy technicznej** obsługuje strony i widoki.</span><span class="sxs-lookup"><span data-stu-id="b4866-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4866-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4866-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4866-122">W wierszu polecenia Uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b4866-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b4866-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b4866-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b4866-124">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="b4866-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="b4866-125">Przekaż opcję `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`), aby zapewnić obsługę stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="b4866-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="b4866-126">Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b4866-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="b4866-127">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b4866-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="b4866-128">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b4866-129">Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* .</span><span class="sxs-lookup"><span data-stu-id="b4866-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="b4866-130">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="b4866-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="b4866-131">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="b4866-131">Reference RCL content</span></span>

<span data-ttu-id="b4866-132">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="b4866-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b4866-133">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b4866-133">NuGet package.</span></span> <span data-ttu-id="b4866-134">Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b4866-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b4866-135">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="b4866-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b4866-136">Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b4866-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b4866-137">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="b4866-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="b4866-138">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b4866-139">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="b4866-140">W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.</span><span class="sxs-lookup"><span data-stu-id="b4866-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b4866-141">Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* częściowo do *WebApp1/Areas/Webfeature/Pages/Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4866-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b4866-142">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b4866-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b4866-143">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="b4866-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="b4866-144">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="b4866-144">RCL Pages layout</span></span>

<span data-ttu-id="b4866-145">Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:</span><span class="sxs-lookup"><span data-stu-id="b4866-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="b4866-146">*RazorUIClassLib/strony*</span><span class="sxs-lookup"><span data-stu-id="b4866-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="b4866-147">*RazorUIClassLib/strony/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="b4866-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="b4866-148">Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4866-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="b4866-149">Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b4866-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="b4866-150">Tworzenie RCL przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="b4866-150">Create an RCL with static assets</span></span>

<span data-ttu-id="b4866-151">RCL może wymagać pomocnika zasobów statycznych, do których można odwoływać się za pomocą RCL lub aplikacji zużywanej przez RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="b4866-152">ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="b4866-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="b4866-153">Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="b4866-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="b4866-154">Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.</span><span class="sxs-lookup"><span data-stu-id="b4866-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="b4866-155">Wyklucz zasoby statyczne</span><span class="sxs-lookup"><span data-stu-id="b4866-155">Exclude static assets</span></span>

<span data-ttu-id="b4866-156">Aby wykluczyć statyczne elementy zawartości, Dodaj żądaną ścieżkę wykluczenia do grupy właściwości `$(DefaultItemExcludes)` w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b4866-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="b4866-157">Oddzielaj wpisy średnikami (`;`).</span><span class="sxs-lookup"><span data-stu-id="b4866-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="b4866-158">W poniższym przykładzie arkusz stylów *lib. css* w folderze *wwwroot* nie jest traktowany jako statyczny zasób i nie jest uwzględniony w opublikowanym RCL:</span><span class="sxs-lookup"><span data-stu-id="b4866-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="b4866-159">Integracja języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="b4866-159">Typescript integration</span></span>

<span data-ttu-id="b4866-160">Aby dołączyć pliki TypeScript do RCL:</span><span class="sxs-lookup"><span data-stu-id="b4866-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="b4866-161">Umieść pliki TypeScript ( *. TS*) poza folderem *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="b4866-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="b4866-162">Na przykład Umieść pliki w folderze *Client* .</span><span class="sxs-lookup"><span data-stu-id="b4866-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="b4866-163">Skonfiguruj dane wyjściowe kompilacji TypeScript dla folderu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="b4866-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="b4866-164">Ustaw właściwość `TypescriptOutDir` wewnątrz `PropertyGroup` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="b4866-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="b4866-165">Dołącz obiekt docelowy TypeScript jako zależność `ResolveCurrentProjectStaticWebAssets` celu, dodając następujący element docelowy wewnątrz `PropertyGroup` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="b4866-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="b4866-166">Korzystanie z zawartości z RCL, do którego istnieje odwołanie</span><span class="sxs-lookup"><span data-stu-id="b4866-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="b4866-167">Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione jako RCL lub aplikację, która korzysta z prefiksu `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="b4866-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="b4866-168">Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej w `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="b4866-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="b4866-169">Podczas tworzenia pakietu NuGet, a nazwa zestawu nie jest taka sama jak identyfikator pakietu, użyj identyfikatora pakietu dla `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b4866-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="b4866-170">Aplikacja, której używa, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z `<script>`, `<style>`, `<img>`i innych tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="b4866-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="b4866-171">Aplikacja, która korzysta z aplikacji, musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b4866-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="b4866-172">W przypadku uruchamiania aplikacji zużywającej dane wyjściowe kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="b4866-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="b4866-173">Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych kompilacji, wywołaj `UseStaticWebAssets` na konstruktorze hosta w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b4866-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="b4866-174">Wywoływanie `UseStaticWebAssets` nie jest wymagane w przypadku uruchamiania aplikacji z opublikowanych danych wyjściowych (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="b4866-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="b4866-175">Przepływ programistyczny dla wieloprojektowych</span><span class="sxs-lookup"><span data-stu-id="b4866-175">Multi-project development flow</span></span>

<span data-ttu-id="b4866-176">Po uruchomieniu aplikacji zużywanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="b4866-176">When the consuming app runs:</span></span>

* <span data-ttu-id="b4866-177">Zasoby w RCL pozostają w swoich oryginalnych folderach.</span><span class="sxs-lookup"><span data-stu-id="b4866-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="b4866-178">Elementy zawartości nie są przenoszone do aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="b4866-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="b4866-179">Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b4866-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="b4866-180">Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="b4866-181">Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów.</span><span class="sxs-lookup"><span data-stu-id="b4866-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="b4866-182">Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="b4866-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="b4866-183">Publikowanie</span><span class="sxs-lookup"><span data-stu-id="b4866-183">Publish</span></span>

<span data-ttu-id="b4866-184">Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="b4866-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b4866-185">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b4866-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="b4866-186">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="b4866-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b4866-187">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="b4866-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b4866-188">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b4866-189">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b4866-189">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b4866-190">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="b4866-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4866-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4866-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b4866-192">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="b4866-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b4866-193">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b4866-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b4866-194">Nazwij bibliotekę (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="b4866-195">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b4866-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="b4866-196">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="b4866-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b4866-197">Wybierz **bibliotekę klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="b4866-198">RCL ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="b4866-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4866-199">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4866-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4866-200">W wierszu polecenia Uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b4866-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b4866-201">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b4866-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b4866-202">Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b4866-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="b4866-203">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b4866-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="b4866-204">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b4866-205">Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* .</span><span class="sxs-lookup"><span data-stu-id="b4866-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="b4866-206">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="b4866-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="b4866-207">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="b4866-207">Reference RCL content</span></span>

<span data-ttu-id="b4866-208">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="b4866-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b4866-209">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b4866-209">NuGet package.</span></span> <span data-ttu-id="b4866-210">Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b4866-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b4866-211">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="b4866-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b4866-212">Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b4866-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="b4866-213">Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="b4866-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="b4866-214">Możesz pobrać [kompletny projekt](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestować go zamiast go tworzyć.</span><span class="sxs-lookup"><span data-stu-id="b4866-214">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="b4866-215">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="b4866-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="b4866-216">Możesz wystawić opinię na temat [tego problemu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) w usłudze GitHub, korzystając z komentarzy dotyczących pobierania próbek, a także instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="b4866-216">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="b4866-217">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="b4866-217">Test the download app</span></span>

<span data-ttu-id="b4866-218">Jeśli nie pobrano ukończonej aplikacji i wolisz utworzyć projekt przewodnika, przejdź do [następnej sekcji](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="b4866-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4866-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4866-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b4866-220">Otwórz plik *sln* w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4866-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="b4866-221">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b4866-221">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4866-222">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4866-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4866-223">Z wiersza polecenia w katalogu *CLI* Utwórz aplikację RCL i sieć Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="b4866-224">Przejdź do katalogu *WebApp1* i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="b4866-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="b4866-225">Postępuj zgodnie z instrukcjami w temacie [test WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="b4866-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="b4866-226">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="b4866-226">Create an RCL</span></span>

<span data-ttu-id="b4866-227">W tej sekcji jest tworzony RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-227">In this section, an RCL is created.</span></span> <span data-ttu-id="b4866-228">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4866-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4866-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b4866-230">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="b4866-230">Create the RCL project:</span></span>

* <span data-ttu-id="b4866-231">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="b4866-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b4866-232">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b4866-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b4866-233">Nadaj aplikacji nazwę **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="b4866-234">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="b4866-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b4866-235">Wybierz **bibliotekę klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="b4866-236">Dodaj plik widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4866-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4866-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4866-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4866-238">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b4866-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b4866-239">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b4866-239">The preceding commands:</span></span>

* <span data-ttu-id="b4866-240">Tworzy `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="b4866-241">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="b4866-242">Parametr `-np` tworzy stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b4866-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="b4866-243">Tworzy plik [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="b4866-244">Plik *_ViewStart. cshtml* jest wymagany do użycia układu Razor Pages projektu (który jest dodawany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="b4866-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="b4866-245">Dodawanie Razor plików i folderów do projektu</span><span class="sxs-lookup"><span data-stu-id="b4866-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="b4866-246">Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b4866-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="b4866-247">Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Strona1. cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b4866-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="b4866-248">do korzystania z widoku częściowego (`<partial name="_Message" />`) jest wymagane `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="b4866-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="b4866-249">Zamiast dyrektywy `@addTagHelper` można dodać plik *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="b4866-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b4866-250">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b4866-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="b4866-251">Aby uzyskać więcej informacji na temat *_ViewImports. cshtml*, zobacz [Importowanie wspólnych dyrektyw](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="b4866-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="b4866-252">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="b4866-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="b4866-253">Dane wyjściowe kompilacji zawierają *RazorUIClassLib. dll* i *RazorUIClassLib. views. dll*.</span><span class="sxs-lookup"><span data-stu-id="b4866-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="b4866-254">*RazorUIClassLib. views. dll* zawiera skompilowaną zawartość Razor.</span><span class="sxs-lookup"><span data-stu-id="b4866-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="b4866-255">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="b4866-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4866-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4866-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b4866-257">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="b4866-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="b4866-258">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >**Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b4866-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="b4866-259">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b4866-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b4866-260">Nadaj aplikacji nazwę **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b4866-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="b4866-261">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="b4866-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b4866-262">Wybierz pozycję **aplikacja sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="b4866-263">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="b4866-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="b4866-264">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="b4866-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="b4866-265">Sprawdź **RazorUIClassLib** jako zależność **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b4866-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="b4866-266">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Dodaj** > **odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="b4866-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="b4866-267">W oknie dialogowym **Menedżer odwołań** sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4866-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="b4866-268">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b4866-268">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b4866-269">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b4866-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b4866-270">Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:</span><span class="sxs-lookup"><span data-stu-id="b4866-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="b4866-271">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="b4866-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="b4866-272">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="b4866-272">Test WebApp1</span></span>

<span data-ttu-id="b4866-273">Przejdź do `/MyFeature/Page1`, aby sprawdzić, czy biblioteka klas interfejsu użytkownika Razor jest używana.</span><span class="sxs-lookup"><span data-stu-id="b4866-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b4866-274">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="b4866-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="b4866-275">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b4866-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b4866-276">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="b4866-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="b4866-277">W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.</span><span class="sxs-lookup"><span data-stu-id="b4866-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b4866-278">Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* częściowo do *WebApp1/Areas/Webfeature/Pages/Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4866-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b4866-279">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b4866-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b4866-280">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="b4866-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="b4866-281">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="b4866-281">RCL Pages layout</span></span>

<span data-ttu-id="b4866-282">Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:</span><span class="sxs-lookup"><span data-stu-id="b4866-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="b4866-283">*RazorUIClassLib/strony*</span><span class="sxs-lookup"><span data-stu-id="b4866-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="b4866-284">*RazorUIClassLib/strony/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="b4866-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="b4866-285">Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4866-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="b4866-286">Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b4866-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b4866-287">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b4866-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
