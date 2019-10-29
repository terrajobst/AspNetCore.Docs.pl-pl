---
title: Interfejs użytkownika Razor wielokrotnego użytku w bibliotekach klas z ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak utworzyć interfejs użytkownika Razor wielokrotnego użytku przy użyciu częściowych widoków w bibliotece klas w ASP.NET Core.
ms.author: riande
ms.date: 10/26/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: ff12eea5406c4f5392a466728741000e3dd16fc1
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034232"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="e5695-103">Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5695-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="e5695-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5695-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e5695-105">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="e5695-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="e5695-106">RCL można spakować i ponownie użyć.</span><span class="sxs-lookup"><span data-stu-id="e5695-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="e5695-107">Aplikacje mogą zawierać RCL i przesłonić widoki oraz strony, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="e5695-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="e5695-108">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="e5695-109">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e5695-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="e5695-110">Tworzenie biblioteki klas zawierającej interfejs użytkownika Razor</span><span class="sxs-lookup"><span data-stu-id="e5695-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5695-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5695-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e5695-112">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="e5695-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="e5695-113">Wybierz pozycję **Biblioteka klas Razor** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="e5695-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="e5695-114">Nazwij bibliotekę (na przykład "RazorClassLib"), > **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e5695-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="e5695-115">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="e5695-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="e5695-116">Wybierz **strony i widoki pomocy technicznej** , jeśli chcesz obsługiwać widoki.</span><span class="sxs-lookup"><span data-stu-id="e5695-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="e5695-117">Domyślnie obsługiwane są tylko Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e5695-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="e5695-118">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e5695-118">Select **Create**.</span></span>

<span data-ttu-id="e5695-119">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="e5695-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="e5695-120">Opcja **strony i widoki pomocy technicznej** obsługuje strony i widoki.</span><span class="sxs-lookup"><span data-stu-id="e5695-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5695-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e5695-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5695-122">W wierszu polecenia Uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="e5695-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="e5695-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5695-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="e5695-124">Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor.</span><span class="sxs-lookup"><span data-stu-id="e5695-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="e5695-125">Aby zapewnić obsługę stron i widoków, należy przekazać opcję `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`).</span><span class="sxs-lookup"><span data-stu-id="e5695-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="e5695-126">Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e5695-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="e5695-127">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="e5695-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="e5695-128">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="e5695-129">Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* .</span><span class="sxs-lookup"><span data-stu-id="e5695-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="e5695-130">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="e5695-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="e5695-131">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="e5695-131">Reference RCL content</span></span>

<span data-ttu-id="e5695-132">RCL może odwoływać się do:</span><span class="sxs-lookup"><span data-stu-id="e5695-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="e5695-133">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5695-133">NuGet package.</span></span> <span data-ttu-id="e5695-134">Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="e5695-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="e5695-135">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="e5695-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="e5695-136">Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="e5695-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="e5695-137">Zastąp widoki, częściowe widoki i strony</span><span class="sxs-lookup"><span data-stu-id="e5695-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="e5695-138">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="e5695-139">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="e5695-140">W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.</span><span class="sxs-lookup"><span data-stu-id="e5695-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="e5695-141">Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* do obszaru WebApp1/refeatures/ *Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e5695-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="e5695-142">Zaktualizuj adiustację, aby wskazać nową lokalizację.</span><span class="sxs-lookup"><span data-stu-id="e5695-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="e5695-143">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana częściowa wersja aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5695-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="e5695-144">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="e5695-144">RCL Pages layout</span></span>

<span data-ttu-id="e5695-145">Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:</span><span class="sxs-lookup"><span data-stu-id="e5695-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="e5695-146">*RazorUIClassLib/strony*</span><span class="sxs-lookup"><span data-stu-id="e5695-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="e5695-147">*RazorUIClassLib/strony/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="e5695-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="e5695-148">Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e5695-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="e5695-149">Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e5695-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="e5695-150">Tworzenie RCL przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="e5695-150">Create an RCL with static assets</span></span>

<span data-ttu-id="e5695-151">RCL może wymagać pomocnika zasobów statycznych, do których może odwoływać się aplikacja zużywana przez RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-151">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="e5695-152">ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="e5695-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="e5695-153">Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="e5695-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="e5695-154">Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.</span><span class="sxs-lookup"><span data-stu-id="e5695-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="e5695-155">Wyklucz zasoby statyczne</span><span class="sxs-lookup"><span data-stu-id="e5695-155">Exclude static assets</span></span>

<span data-ttu-id="e5695-156">Aby wykluczyć statyczne elementy zawartości, Dodaj żądaną ścieżkę wykluczenia do grupy właściwości `$(DefaultItemExcludes)` w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="e5695-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="e5695-157">Oddzielaj wpisy średnikami (`;`).</span><span class="sxs-lookup"><span data-stu-id="e5695-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="e5695-158">W poniższym przykładzie arkusz stylów *lib. css* w folderze *wwwroot* nie jest traktowany jako statyczny zasób i nie jest uwzględniony w opublikowanym RCL:</span><span class="sxs-lookup"><span data-stu-id="e5695-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="e5695-159">Integracja języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="e5695-159">Typescript integration</span></span>

<span data-ttu-id="e5695-160">Aby dołączyć pliki TypeScript do RCL:</span><span class="sxs-lookup"><span data-stu-id="e5695-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="e5695-161">Umieść pliki TypeScript ( *. TS*) poza folderem *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="e5695-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="e5695-162">Na przykład Umieść pliki w folderze *Client* .</span><span class="sxs-lookup"><span data-stu-id="e5695-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="e5695-163">Skonfiguruj dane wyjściowe kompilacji TypeScript dla folderu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="e5695-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="e5695-164">Ustaw właściwość `TypescriptOutDir` wewnątrz `PropertyGroup` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="e5695-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="e5695-165">Dołącz obiekt docelowy TypeScript jako zależność elementu docelowego `ResolveCurrentProjectStaticWebAssets`, dodając następujący element docelowy wewnątrz `PropertyGroup` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="e5695-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="e5695-166">Korzystanie z zawartości z RCL, do którego istnieje odwołanie</span><span class="sxs-lookup"><span data-stu-id="e5695-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="e5695-167">Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione dla aplikacji zużywanej pod prefiksem `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="e5695-167">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="e5695-168">Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej w `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="e5695-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="e5695-169">Aplikacja, której używa, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z `<script>`, `<style>`, `<img>` i innymi tagami HTML.</span><span class="sxs-lookup"><span data-stu-id="e5695-169">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="e5695-170">Aplikacja do konsumowania musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e5695-170">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="e5695-171">W przypadku uruchamiania aplikacji zużywanej z danych wyjściowych kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="e5695-171">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="e5695-172">Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych kompilacji, wywołaj `UseStaticWebAssets` na konstruktorze hosta w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e5695-172">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="e5695-173">Wywołanie `UseStaticWebAssets` nie jest wymagane podczas uruchamiania aplikacji z opublikowanych danych wyjściowych (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="e5695-173">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="e5695-174">Przepływ programistyczny dla wieloprojektowych</span><span class="sxs-lookup"><span data-stu-id="e5695-174">Multi-project development flow</span></span>

<span data-ttu-id="e5695-175">Po uruchomieniu aplikacji zużywanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="e5695-175">When the consuming app runs:</span></span>

* <span data-ttu-id="e5695-176">Zasoby w RCL pozostają w swoich oryginalnych folderach.</span><span class="sxs-lookup"><span data-stu-id="e5695-176">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="e5695-177">Elementy zawartości nie są przenoszone do aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="e5695-177">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="e5695-178">Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5695-178">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="e5695-179">Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-179">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="e5695-180">Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów.</span><span class="sxs-lookup"><span data-stu-id="e5695-180">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="e5695-181">Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="e5695-181">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="e5695-182">Wycofywan</span><span class="sxs-lookup"><span data-stu-id="e5695-182">Publish</span></span>

<span data-ttu-id="e5695-183">Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="e5695-183">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e5695-184">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="e5695-184">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="e5695-185">RCL można spakować i ponownie użyć.</span><span class="sxs-lookup"><span data-stu-id="e5695-185">The RCL can be packaged and reused.</span></span> <span data-ttu-id="e5695-186">Aplikacje mogą zawierać RCL i przesłonić widoki oraz strony, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="e5695-186">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="e5695-187">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-187">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="e5695-188">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e5695-188">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="e5695-189">Tworzenie biblioteki klas zawierającej interfejs użytkownika Razor</span><span class="sxs-lookup"><span data-stu-id="e5695-189">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5695-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5695-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e5695-191">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="e5695-191">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e5695-192">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e5695-192">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e5695-193">Nazwij bibliotekę (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-193">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="e5695-194">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="e5695-194">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="e5695-195">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="e5695-195">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e5695-196">Wybierz **bibliotekę klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-196">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="e5695-197">RCL ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="e5695-197">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5695-198">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e5695-198">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5695-199">W wierszu polecenia Uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="e5695-199">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="e5695-200">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5695-200">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="e5695-201">Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e5695-201">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="e5695-202">Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.</span><span class="sxs-lookup"><span data-stu-id="e5695-202">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="e5695-203">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-203">Add Razor files to the RCL.</span></span>

<span data-ttu-id="e5695-204">Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* .</span><span class="sxs-lookup"><span data-stu-id="e5695-204">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="e5695-205">Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="e5695-205">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="e5695-206">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="e5695-206">Reference RCL content</span></span>

<span data-ttu-id="e5695-207">RCL może odwoływać się do:</span><span class="sxs-lookup"><span data-stu-id="e5695-207">The RCL can be referenced by:</span></span>

* <span data-ttu-id="e5695-208">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5695-208">NuGet package.</span></span> <span data-ttu-id="e5695-209">Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="e5695-209">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="e5695-210">*{ProjectName}. csproj*.</span><span class="sxs-lookup"><span data-stu-id="e5695-210">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="e5695-211">Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="e5695-211">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="e5695-212">Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e5695-212">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="e5695-213">Możesz pobrać [kompletny projekt](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestować go zamiast go tworzyć.</span><span class="sxs-lookup"><span data-stu-id="e5695-213">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="e5695-214">Pobranie próbek zawiera dodatkowy kod i linki, które ułatwiają przetestowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="e5695-214">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="e5695-215">Możesz wystawić opinię na temat [tego problemu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) w usłudze GitHub, korzystając z komentarzy dotyczących pobierania próbek, a także instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="e5695-215">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="e5695-216">Testowanie aplikacji do pobrania</span><span class="sxs-lookup"><span data-stu-id="e5695-216">Test the download app</span></span>

<span data-ttu-id="e5695-217">Jeśli nie pobrano ukończonej aplikacji i wolisz utworzyć projekt przewodnika, przejdź do [następnej sekcji](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="e5695-217">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5695-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5695-218">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5695-219">Otwórz plik *sln* w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5695-219">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="e5695-220">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="e5695-220">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5695-221">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e5695-221">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5695-222">Z wiersza polecenia w katalogu *CLI* Utwórz aplikację RCL i sieć Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-222">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="e5695-223">Przejdź do katalogu *WebApp1* i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="e5695-223">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="e5695-224">Postępuj zgodnie z instrukcjami w temacie [test WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="e5695-224">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="e5695-225">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="e5695-225">Create an RCL</span></span>

<span data-ttu-id="e5695-226">W tej sekcji jest tworzony RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-226">In this section, an RCL is created.</span></span> <span data-ttu-id="e5695-227">Pliki Razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-227">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5695-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5695-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5695-229">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="e5695-229">Create the RCL project:</span></span>

* <span data-ttu-id="e5695-230">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="e5695-230">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e5695-231">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e5695-231">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e5695-232">Nazwij aplikację **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-232">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="e5695-233">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="e5695-233">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e5695-234">Wybierz **bibliotekę klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-234">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="e5695-235">Dodaj plik widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e5695-235">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5695-236">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e5695-236">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5695-237">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e5695-237">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="e5695-238">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="e5695-238">The preceding commands:</span></span>

* <span data-ttu-id="e5695-239">Tworzy `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-239">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="e5695-240">Tworzy stronę _Message Razor i dodaje ją do RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-240">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="e5695-241">Parametr `-np` tworzy stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="e5695-241">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="e5695-242">Tworzy plik [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-242">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="e5695-243">Plik *_ViewStart. cshtml* jest wymagany do korzystania z układu projektu Razor Pages (który jest dodawany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="e5695-243">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="e5695-244">Dodawanie plików i folderów Razor do projektu</span><span class="sxs-lookup"><span data-stu-id="e5695-244">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="e5695-245">Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e5695-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="e5695-246">Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Strona1. cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e5695-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="e5695-247">do korzystania z widoku częściowego (`<partial name="_Message" />`) jest wymagane `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="e5695-247">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="e5695-248">Zamiast dyrektywy `@addTagHelper` można dodać plik *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e5695-248">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="e5695-249">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5695-249">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="e5695-250">Aby uzyskać więcej informacji na temat *_ViewImports. cshtml*, zobacz [Importowanie wspólnych dyrektyw](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="e5695-250">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="e5695-251">Kompiluj bibliotekę klas, aby upewnić się, że nie występują błędy kompilatora:</span><span class="sxs-lookup"><span data-stu-id="e5695-251">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="e5695-252">Dane wyjściowe kompilacji zawierają *RazorUIClassLib. dll* i *RazorUIClassLib. views. dll*.</span><span class="sxs-lookup"><span data-stu-id="e5695-252">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="e5695-253">*RazorUIClassLib. views. dll* zawiera skompilowaną zawartość Razor.</span><span class="sxs-lookup"><span data-stu-id="e5695-253">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="e5695-254">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e5695-254">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5695-255">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5695-255">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5695-256">Utwórz aplikację sieci Web Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="e5695-256">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="e5695-257">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="e5695-257">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="e5695-258">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e5695-258">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e5695-259">Nadaj aplikacji nazwę **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="e5695-259">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="e5695-260">Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.</span><span class="sxs-lookup"><span data-stu-id="e5695-260">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e5695-261">Wybierz **aplikację sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-261">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="e5695-262">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="e5695-262">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="e5695-263">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** , a następnie wybierz pozycję **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="e5695-263">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="e5695-264">Sprawdź **RazorUIClassLib** jako zależność **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="e5695-264">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="e5695-265">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Dodaj** **odwołanie**>.</span><span class="sxs-lookup"><span data-stu-id="e5695-265">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="e5695-266">W oknie dialogowym **Menedżer odwołań** sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5695-266">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="e5695-267">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="e5695-267">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e5695-268">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e5695-268">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e5695-269">Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:</span><span class="sxs-lookup"><span data-stu-id="e5695-269">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="e5695-270">Kompiluj i uruchom aplikację sieci Web:</span><span class="sxs-lookup"><span data-stu-id="e5695-270">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="e5695-271">Test WebApp1</span><span class="sxs-lookup"><span data-stu-id="e5695-271">Test WebApp1</span></span>

<span data-ttu-id="e5695-272">Przejdź do `/MyFeature/Page1`, aby sprawdzić, czy biblioteka klas interfejsu użytkownika Razor jest używana.</span><span class="sxs-lookup"><span data-stu-id="e5695-272">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="e5695-273">Zastąp widoki, częściowe widoki i strony</span><span class="sxs-lookup"><span data-stu-id="e5695-273">Override views, partial views, and pages</span></span>

<span data-ttu-id="e5695-274">Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5695-274">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="e5695-275">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="e5695-275">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="e5695-276">W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.</span><span class="sxs-lookup"><span data-stu-id="e5695-276">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="e5695-277">Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* do obszaru WebApp1/refeatures/ *Shared/_Message. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e5695-277">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="e5695-278">Zaktualizuj adiustację, aby wskazać nową lokalizację.</span><span class="sxs-lookup"><span data-stu-id="e5695-278">Update the markup to indicate the new location.</span></span> <span data-ttu-id="e5695-279">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana częściowa wersja aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5695-279">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="e5695-280">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="e5695-280">RCL Pages layout</span></span>

<span data-ttu-id="e5695-281">Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:</span><span class="sxs-lookup"><span data-stu-id="e5695-281">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="e5695-282">*RazorUIClassLib/strony*</span><span class="sxs-lookup"><span data-stu-id="e5695-282">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="e5695-283">*RazorUIClassLib/strony/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="e5695-283">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="e5695-284">Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e5695-284">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="e5695-285">Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e5695-285">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
