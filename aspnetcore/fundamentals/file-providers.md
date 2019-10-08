---
title: Dostawcy plików w ASP.NET Core
author: guardrex
description: Dowiedz się, jak ASP.NET Core abstrakcji dostępu do systemu plików przy użyciu dostawców plików.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 3a92b44efc70d156596ee9fe80b4f6a65266e73d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007167"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="4d641-103">Dostawcy plików w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d641-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="4d641-104">[Steve Kowalski](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d641-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4d641-105">ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="4d641-106">Dostawcy plików są używani w całym ASP.NET Core Framework:</span><span class="sxs-lookup"><span data-stu-id="4d641-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="4d641-107">`IWebHostEnvironment` uwidacznia [katalog główny zawartości](xref:fundamentals/index#content-root) aplikacji i [katalog główny sieci web](xref:fundamentals/index#web-root) jako typy `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="4d641-108">[Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="4d641-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="4d641-109">[Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="4d641-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="4d641-110">Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="4d641-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="4d641-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d641-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="4d641-112">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="4d641-112">File Provider interfaces</span></span>

<span data-ttu-id="4d641-113">Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d641-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="4d641-114">`IFileProvider` uwidacznia metody, aby:</span><span class="sxs-lookup"><span data-stu-id="4d641-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="4d641-115">Uzyskaj informacje o pliku (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="4d641-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="4d641-116">Uzyskaj informacje o katalogu (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="4d641-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="4d641-117">Skonfiguruj powiadomienia o zmianach (przy użyciu <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="4d641-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="4d641-118">`IFileInfo` udostępnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="4d641-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="4d641-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (w bajtach)</span><span class="sxs-lookup"><span data-stu-id="4d641-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="4d641-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> Data</span><span class="sxs-lookup"><span data-stu-id="4d641-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="4d641-121">Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .</span><span class="sxs-lookup"><span data-stu-id="4d641-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="4d641-122">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w `Startup.ConfigureServices` do użycia w całej aplikacji przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4d641-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="4d641-123">Implementacje dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="4d641-123">File Provider implementations</span></span>

<span data-ttu-id="4d641-124">Dostępne są trzy implementacje `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="4d641-125">Wdrażanie</span><span class="sxs-lookup"><span data-stu-id="4d641-125">Implementation</span></span> | <span data-ttu-id="4d641-126">Opis</span><span class="sxs-lookup"><span data-stu-id="4d641-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="4d641-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="4d641-128">Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu.</span><span class="sxs-lookup"><span data-stu-id="4d641-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="4d641-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="4d641-130">Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="4d641-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="4d641-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="4d641-132">Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="4d641-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="4d641-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-133">PhysicalFileProvider</span></span>

<span data-ttu-id="4d641-134">@No__t-0 zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="4d641-135">`PhysicalFileProvider` używa typu <xref:System.IO.File?displayProperty=fullName> (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4d641-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="4d641-136">Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="4d641-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="4d641-137">Najbardziej typowym scenariuszem tworzenia i używania `PhysicalFileProvider` jest zażądanie `IFileProvider` w konstruktorze poprzez [iniekcję zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4d641-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="4d641-138">W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="4d641-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="4d641-139">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:</span><span class="sxs-lookup"><span data-stu-id="4d641-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="4d641-140">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4d641-140">Types in the preceding example:</span></span>

* <span data-ttu-id="4d641-141">`provider` to `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="4d641-142">`contents` to `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="4d641-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="4d641-143">`fileInfo` to `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="4d641-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="4d641-144">Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołania `GetFileInfo` w celu uzyskania informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="4d641-145">Dostawca plików nie ma dostępu poza katalogiem `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="4d641-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="4d641-146">Przykładowa aplikacja tworzy dostawcę w klasie `Startup.ConfigureServices` aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="4d641-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="4d641-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="4d641-148">@No__t-0 służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="4d641-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="4d641-149">@No__t-0 używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="4d641-150">Dodaj odwołanie do pakietu do projektu dla pakietu [Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) .</span><span class="sxs-lookup"><span data-stu-id="4d641-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="4d641-151">Aby wygenerować manifest osadzonych plików, ustaw właściwość `<GenerateEmbeddedFilesManifest>` na `true`.</span><span class="sxs-lookup"><span data-stu-id="4d641-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="4d641-152">Określ pliki do osadzenia przy użyciu [\<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="4d641-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="4d641-153">Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="4d641-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="4d641-154">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4d641-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="4d641-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d641-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="4d641-156">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="4d641-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="4d641-157">Określ względną ścieżkę pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-157">Specify a relative file path.</span></span>
* <span data-ttu-id="4d641-158">Zakres plików do daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="4d641-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="4d641-159">Nazwij osadzony zasób zawierający manifest pliku osadzonego.</span><span class="sxs-lookup"><span data-stu-id="4d641-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="4d641-160">Występują</span><span class="sxs-lookup"><span data-stu-id="4d641-160">Overload</span></span> | <span data-ttu-id="4d641-161">Opis</span><span class="sxs-lookup"><span data-stu-id="4d641-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="4d641-162">Akceptuje opcjonalny parametr ścieżki względnej `root`.</span><span class="sxs-lookup"><span data-stu-id="4d641-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="4d641-163">Określ `root` do zakresu wywołań do <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> do tych zasobów w ramach podanej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="4d641-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="4d641-164">Akceptuje opcjonalny parametr ścieżki względnej `root` i `lastModified` Data (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="4d641-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="4d641-165">W przypadku zakresów `lastModified` Data ostatniej modyfikacji dla wystąpień <xref:Microsoft.Extensions.FileProviders.IFileInfo> zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d641-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="4d641-166">Akceptuje opcjonalną ścieżkę względną `root`, datę `lastModified` i parametry `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="4d641-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="4d641-167">@No__t-0 reprezentuje nazwę osadzonego zasobu zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="4d641-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="4d641-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-168">CompositeFileProvider</span></span>

<span data-ttu-id="4d641-169">@No__t-0 łączy wystąpienia `IFileProvider`, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="4d641-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="4d641-170">Podczas tworzenia `CompositeFileProvider` do jego konstruktora należy przekazać co najmniej jedno wystąpienie `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="4d641-171">W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępniają pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4d641-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="4d641-172">Obejrzyj zmiany</span><span class="sxs-lookup"><span data-stu-id="4d641-172">Watch for changes</span></span>

<span data-ttu-id="4d641-173">Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach.</span><span class="sxs-lookup"><span data-stu-id="4d641-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="4d641-174">`Watch` akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="4d641-175">`Watch` zwraca <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="4d641-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="4d641-176">Token zmiany ujawnia:</span><span class="sxs-lookup"><span data-stu-id="4d641-176">The change token exposes:</span></span>

* <span data-ttu-id="4d641-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="4d641-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="4d641-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; wywołana, gdy zostaną wykryte zmiany do określonego ciągu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="4d641-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="4d641-179">Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę.</span><span class="sxs-lookup"><span data-stu-id="4d641-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="4d641-180">Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie utwórz wystąpienia `IChangeToken` w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="4d641-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="4d641-181">W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:</span><span class="sxs-lookup"><span data-stu-id="4d641-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="4d641-182">Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="4d641-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="4d641-183">Ustaw zmienną środowiskową `DOTNET_USE_POLLING_FILE_WATCHER` na `1` lub `true`, aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować).</span><span class="sxs-lookup"><span data-stu-id="4d641-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="4d641-184">Wzorce globalizowania</span><span class="sxs-lookup"><span data-stu-id="4d641-184">Glob patterns</span></span>

<span data-ttu-id="4d641-185">Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* .</span><span class="sxs-lookup"><span data-stu-id="4d641-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="4d641-186">Określ grupy plików z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="4d641-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="4d641-187">Dwa symbole wieloznaczne są `*` i `**`:</span><span class="sxs-lookup"><span data-stu-id="4d641-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="4d641-188">Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="4d641-189">Dopasowania są kończone przez `/` i `.` znaków w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="4d641-190">Dopasowuje wszystko na wielu poziomach katalogów.</span><span class="sxs-lookup"><span data-stu-id="4d641-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="4d641-191">Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="4d641-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="4d641-192">**Przykłady wzorców globalizowania**</span><span class="sxs-lookup"><span data-stu-id="4d641-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="4d641-193">Dopasowuje określony plik w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="4d641-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="4d641-194">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="4d641-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="4d641-195">Dopasowuje wszystkie pliki `appsettings.json` w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="4d641-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="4d641-196">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="4d641-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4d641-197">ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="4d641-198">Dostawcy plików są używani w całym ASP.NET Core Framework:</span><span class="sxs-lookup"><span data-stu-id="4d641-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="4d641-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uwidacznia [katalog główny zawartości](xref:fundamentals/index#content-root) aplikacji i [katalog główny sieci web](xref:fundamentals/index#web-root) jako typy `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="4d641-200">[Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="4d641-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="4d641-201">[Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="4d641-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="4d641-202">Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="4d641-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="4d641-203">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d641-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="4d641-204">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="4d641-204">File Provider interfaces</span></span>

<span data-ttu-id="4d641-205">Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d641-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="4d641-206">`IFileProvider` uwidacznia metody, aby:</span><span class="sxs-lookup"><span data-stu-id="4d641-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="4d641-207">Uzyskaj informacje o pliku (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="4d641-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="4d641-208">Uzyskaj informacje o katalogu (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="4d641-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="4d641-209">Skonfiguruj powiadomienia o zmianach (przy użyciu <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="4d641-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="4d641-210">`IFileInfo` udostępnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="4d641-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="4d641-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (w bajtach)</span><span class="sxs-lookup"><span data-stu-id="4d641-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="4d641-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> Data</span><span class="sxs-lookup"><span data-stu-id="4d641-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="4d641-213">Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .</span><span class="sxs-lookup"><span data-stu-id="4d641-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="4d641-214">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w `Startup.ConfigureServices` do użycia w całej aplikacji przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4d641-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="4d641-215">Implementacje dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="4d641-215">File Provider implementations</span></span>

<span data-ttu-id="4d641-216">Dostępne są trzy implementacje `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="4d641-217">Wdrażanie</span><span class="sxs-lookup"><span data-stu-id="4d641-217">Implementation</span></span> | <span data-ttu-id="4d641-218">Opis</span><span class="sxs-lookup"><span data-stu-id="4d641-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="4d641-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="4d641-220">Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu.</span><span class="sxs-lookup"><span data-stu-id="4d641-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="4d641-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="4d641-222">Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="4d641-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="4d641-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="4d641-224">Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="4d641-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="4d641-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-225">PhysicalFileProvider</span></span>

<span data-ttu-id="4d641-226">@No__t-0 zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="4d641-227">`PhysicalFileProvider` używa typu <xref:System.IO.File?displayProperty=fullName> (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4d641-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="4d641-228">Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="4d641-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="4d641-229">Najbardziej typowym scenariuszem tworzenia i używania `PhysicalFileProvider` jest zażądanie `IFileProvider` w konstruktorze poprzez [iniekcję zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4d641-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="4d641-230">W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="4d641-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="4d641-231">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:</span><span class="sxs-lookup"><span data-stu-id="4d641-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="4d641-232">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4d641-232">Types in the preceding example:</span></span>

* <span data-ttu-id="4d641-233">`provider` to `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="4d641-234">`contents` to `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="4d641-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="4d641-235">`fileInfo` to `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="4d641-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="4d641-236">Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołania `GetFileInfo` w celu uzyskania informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="4d641-237">Dostawca plików nie ma dostępu poza katalogiem `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="4d641-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="4d641-238">Przykładowa aplikacja tworzy dostawcę w klasie `Startup.ConfigureServices` aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="4d641-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="4d641-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="4d641-240">@No__t-0 służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="4d641-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="4d641-241">@No__t-0 używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="4d641-242">Aby wygenerować manifest osadzonych plików, ustaw właściwość `<GenerateEmbeddedFilesManifest>` na `true`.</span><span class="sxs-lookup"><span data-stu-id="4d641-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="4d641-243">Określ pliki do osadzenia przy użyciu [&lt;EmbeddedResource @ no__t-2](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="4d641-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="4d641-244">Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="4d641-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="4d641-245">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4d641-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="4d641-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d641-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="4d641-247">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="4d641-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="4d641-248">Określ względną ścieżkę pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-248">Specify a relative file path.</span></span>
* <span data-ttu-id="4d641-249">Zakres plików do daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="4d641-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="4d641-250">Nazwij osadzony zasób zawierający manifest pliku osadzonego.</span><span class="sxs-lookup"><span data-stu-id="4d641-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="4d641-251">Występują</span><span class="sxs-lookup"><span data-stu-id="4d641-251">Overload</span></span> | <span data-ttu-id="4d641-252">Opis</span><span class="sxs-lookup"><span data-stu-id="4d641-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="4d641-253">Akceptuje opcjonalny parametr ścieżki względnej `root`.</span><span class="sxs-lookup"><span data-stu-id="4d641-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="4d641-254">Określ `root` do zakresu wywołań do <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> do tych zasobów w ramach podanej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="4d641-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="4d641-255">Akceptuje opcjonalny parametr ścieżki względnej `root` i `lastModified` Data (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="4d641-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="4d641-256">W przypadku zakresów `lastModified` Data ostatniej modyfikacji dla wystąpień <xref:Microsoft.Extensions.FileProviders.IFileInfo> zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d641-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="4d641-257">Akceptuje opcjonalną ścieżkę względną `root`, datę `lastModified` i parametry `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="4d641-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="4d641-258">@No__t-0 reprezentuje nazwę osadzonego zasobu zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="4d641-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="4d641-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4d641-259">CompositeFileProvider</span></span>

<span data-ttu-id="4d641-260">@No__t-0 łączy wystąpienia `IFileProvider`, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="4d641-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="4d641-261">Podczas tworzenia `CompositeFileProvider` do jego konstruktora należy przekazać co najmniej jedno wystąpienie `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4d641-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="4d641-262">W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępniają pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4d641-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="4d641-263">Obejrzyj zmiany</span><span class="sxs-lookup"><span data-stu-id="4d641-263">Watch for changes</span></span>

<span data-ttu-id="4d641-264">Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach.</span><span class="sxs-lookup"><span data-stu-id="4d641-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="4d641-265">`Watch` akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="4d641-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="4d641-266">`Watch` zwraca <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="4d641-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="4d641-267">Token zmiany ujawnia:</span><span class="sxs-lookup"><span data-stu-id="4d641-267">The change token exposes:</span></span>

* <span data-ttu-id="4d641-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="4d641-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="4d641-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; wywołana, gdy zostaną wykryte zmiany do określonego ciągu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="4d641-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="4d641-270">Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę.</span><span class="sxs-lookup"><span data-stu-id="4d641-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="4d641-271">Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie utwórz wystąpienia `IChangeToken` w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="4d641-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="4d641-272">W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:</span><span class="sxs-lookup"><span data-stu-id="4d641-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="4d641-273">Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="4d641-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="4d641-274">Ustaw zmienną środowiskową `DOTNET_USE_POLLING_FILE_WATCHER` na `1` lub `true`, aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować).</span><span class="sxs-lookup"><span data-stu-id="4d641-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="4d641-275">Wzorce globalizowania</span><span class="sxs-lookup"><span data-stu-id="4d641-275">Glob patterns</span></span>

<span data-ttu-id="4d641-276">Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* .</span><span class="sxs-lookup"><span data-stu-id="4d641-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="4d641-277">Określ grupy plików z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="4d641-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="4d641-278">Dwa symbole wieloznaczne są `*` i `**`:</span><span class="sxs-lookup"><span data-stu-id="4d641-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="4d641-279">Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="4d641-280">Dopasowania są kończone przez `/` i `.` znaków w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="4d641-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="4d641-281">Dopasowuje wszystko na wielu poziomach katalogów.</span><span class="sxs-lookup"><span data-stu-id="4d641-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="4d641-282">Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="4d641-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="4d641-283">**Przykłady wzorców globalizowania**</span><span class="sxs-lookup"><span data-stu-id="4d641-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="4d641-284">Dopasowuje określony plik w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="4d641-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="4d641-285">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="4d641-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="4d641-286">Dopasowuje wszystkie pliki `appsettings.json` w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="4d641-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="4d641-287">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="4d641-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
