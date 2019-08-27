---
title: Dostawcy plików w ASP.NET Core
author: guardrex
description: Dowiedz się, jak ASP.NET Core abstrakcji dostępu do systemu plików przy użyciu dostawców plików.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 44c439dce893d486668bf8ac3f20cdf7952c5186
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059099"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="519d3-103">Dostawcy plików w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="519d3-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="519d3-104">[Steve Kowalski](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="519d3-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="519d3-105">ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="519d3-106">Dostawcy plików są używani w całym ASP.NET Core Framework:</span><span class="sxs-lookup"><span data-stu-id="519d3-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="519d3-107">`IWebHostEnvironment`udostępnia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.</span><span class="sxs-lookup"><span data-stu-id="519d3-107">`IWebHostEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="519d3-108">[Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="519d3-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="519d3-109">[Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="519d3-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="519d3-110">Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="519d3-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="519d3-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="519d3-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="519d3-112">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="519d3-112">File Provider interfaces</span></span>

<span data-ttu-id="519d3-113">Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="519d3-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="519d3-114">`IFileProvider`udostępnia metody:</span><span class="sxs-lookup"><span data-stu-id="519d3-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="519d3-115">Uzyskaj informacje o pliku<xref:Microsoft.Extensions.FileProviders.IFileInfo>().</span><span class="sxs-lookup"><span data-stu-id="519d3-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="519d3-116">Uzyskaj informacje o katalogu<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>().</span><span class="sxs-lookup"><span data-stu-id="519d3-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="519d3-117">Skonfiguruj powiadomienia o zmianach (przy <xref:Microsoft.Extensions.Primitives.IChangeToken>użyciu).</span><span class="sxs-lookup"><span data-stu-id="519d3-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="519d3-118">`IFileInfo`zapewnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="519d3-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="519d3-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(w bajtach)</span><span class="sxs-lookup"><span data-stu-id="519d3-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="519d3-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>dniu</span><span class="sxs-lookup"><span data-stu-id="519d3-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="519d3-121">Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .</span><span class="sxs-lookup"><span data-stu-id="519d3-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="519d3-122">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="519d3-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="519d3-123">Implementacje dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="519d3-123">File Provider implementations</span></span>

<span data-ttu-id="519d3-124">Dostępne są trzy `IFileProvider` implementacje programu.</span><span class="sxs-lookup"><span data-stu-id="519d3-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="519d3-125">Implementacja</span><span class="sxs-lookup"><span data-stu-id="519d3-125">Implementation</span></span> | <span data-ttu-id="519d3-126">Opis</span><span class="sxs-lookup"><span data-stu-id="519d3-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="519d3-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="519d3-128">Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu.</span><span class="sxs-lookup"><span data-stu-id="519d3-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="519d3-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="519d3-130">Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="519d3-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="519d3-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="519d3-132">Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="519d3-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="519d3-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-133">PhysicalFileProvider</span></span>

<span data-ttu-id="519d3-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> Zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="519d3-135">`PhysicalFileProvider`<xref:System.IO.File?displayProperty=fullName> używa typu (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="519d3-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="519d3-136">Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="519d3-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="519d3-137">Najbardziej typowym scenariuszem tworzenia i używania elementu `PhysicalFileProvider` jest `IFileProvider` zażądanie w konstruktorze przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="519d3-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="519d3-138">W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="519d3-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="519d3-139">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:</span><span class="sxs-lookup"><span data-stu-id="519d3-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="519d3-140">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="519d3-140">Types in the preceding example:</span></span>

* <span data-ttu-id="519d3-141">`provider``IFileProvider`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="519d3-142">`contents``IDirectoryContents`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="519d3-143">`fileInfo``IFileInfo`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="519d3-144">Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołanie `GetFileInfo` w celu uzyskania informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="519d3-145">Dostawca plików nie ma dostępu poza `applicationRoot` katalogiem.</span><span class="sxs-lookup"><span data-stu-id="519d3-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="519d3-146">Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="519d3-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="519d3-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="519d3-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="519d3-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="519d3-149">`ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="519d3-150">Dodaj odwołanie do pakietu do projektu dla pakietu [Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) .</span><span class="sxs-lookup"><span data-stu-id="519d3-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="519d3-151">Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość na. `true`</span><span class="sxs-lookup"><span data-stu-id="519d3-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="519d3-152">Określ pliki do osadzenia przy użyciu [ \<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="519d3-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="519d3-153">Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="519d3-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="519d3-154">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="519d3-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="519d3-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="519d3-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="519d3-156">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="519d3-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="519d3-157">Określ względną ścieżkę pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-157">Specify a relative file path.</span></span>
* <span data-ttu-id="519d3-158">Zakres plików do daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="519d3-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="519d3-159">Nazwij osadzony zasób zawierający manifest pliku osadzonego.</span><span class="sxs-lookup"><span data-stu-id="519d3-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="519d3-160">Występują</span><span class="sxs-lookup"><span data-stu-id="519d3-160">Overload</span></span> | <span data-ttu-id="519d3-161">Opis</span><span class="sxs-lookup"><span data-stu-id="519d3-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="519d3-162">Akceptuje opcjonalny `root` parametr ścieżki względnej.</span><span class="sxs-lookup"><span data-stu-id="519d3-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="519d3-163">Określ do zakresu <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> wywołania do tych zasobów w ramach podanej ścieżki. `root`</span><span class="sxs-lookup"><span data-stu-id="519d3-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="519d3-164">Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="519d3-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="519d3-165">Data zakresy daty ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> dla wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `lastModified`</span><span class="sxs-lookup"><span data-stu-id="519d3-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="519d3-166">Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry.</span><span class="sxs-lookup"><span data-stu-id="519d3-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="519d3-167">`manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="519d3-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="519d3-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-168">CompositeFileProvider</span></span>

<span data-ttu-id="519d3-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Łączy`IFileProvider` wystąpienia, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="519d3-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="519d3-170">Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="519d3-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="519d3-171">W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="519d3-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="519d3-172">Obejrzyj zmiany</span><span class="sxs-lookup"><span data-stu-id="519d3-172">Watch for changes</span></span>

<span data-ttu-id="519d3-173">Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach.</span><span class="sxs-lookup"><span data-stu-id="519d3-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="519d3-174">`Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="519d3-175">`Watch`Zwraca wartość <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="519d3-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="519d3-176">Token zmiany ujawnia:</span><span class="sxs-lookup"><span data-stu-id="519d3-176">The change token exposes:</span></span>

* <span data-ttu-id="519d3-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="519d3-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="519d3-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="519d3-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="519d3-179">Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę.</span><span class="sxs-lookup"><span data-stu-id="519d3-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="519d3-180">Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie Utwórz `IChangeToken` wystąpienia w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="519d3-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="519d3-181">W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:</span><span class="sxs-lookup"><span data-stu-id="519d3-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="519d3-182">Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="519d3-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="519d3-183">Ustaw zmienną `1` środowiskową na lub `true` , aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="519d3-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="519d3-184">Wzorce globalizowania</span><span class="sxs-lookup"><span data-stu-id="519d3-184">Glob patterns</span></span>

<span data-ttu-id="519d3-185">Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* .</span><span class="sxs-lookup"><span data-stu-id="519d3-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="519d3-186">Określ grupy plików z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="519d3-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="519d3-187">Dwa symbole wieloznaczne to `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="519d3-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="519d3-188">Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="519d3-189">Dopasowania są kończone przez `/` znaki `.` i w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="519d3-190">Dopasowuje wszystko na wielu poziomach katalogów.</span><span class="sxs-lookup"><span data-stu-id="519d3-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="519d3-191">Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="519d3-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="519d3-192">**Przykłady wzorców globalizowania**</span><span class="sxs-lookup"><span data-stu-id="519d3-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="519d3-193">Dopasowuje określony plik w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="519d3-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="519d3-194">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="519d3-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="519d3-195">Dopasowuje `appsettings.json` wszystkie pliki w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="519d3-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="519d3-196">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="519d3-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="519d3-197">ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="519d3-198">Dostawcy plików są używani w całym ASP.NET Core Framework:</span><span class="sxs-lookup"><span data-stu-id="519d3-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="519d3-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>udostępnia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.</span><span class="sxs-lookup"><span data-stu-id="519d3-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="519d3-200">[Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="519d3-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="519d3-201">[Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="519d3-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="519d3-202">Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="519d3-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="519d3-203">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="519d3-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="519d3-204">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="519d3-204">File Provider interfaces</span></span>

<span data-ttu-id="519d3-205">Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="519d3-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="519d3-206">`IFileProvider`udostępnia metody:</span><span class="sxs-lookup"><span data-stu-id="519d3-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="519d3-207">Uzyskaj informacje o pliku<xref:Microsoft.Extensions.FileProviders.IFileInfo>().</span><span class="sxs-lookup"><span data-stu-id="519d3-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="519d3-208">Uzyskaj informacje o katalogu<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>().</span><span class="sxs-lookup"><span data-stu-id="519d3-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="519d3-209">Skonfiguruj powiadomienia o zmianach (przy <xref:Microsoft.Extensions.Primitives.IChangeToken>użyciu).</span><span class="sxs-lookup"><span data-stu-id="519d3-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="519d3-210">`IFileInfo`zapewnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="519d3-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="519d3-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(w bajtach)</span><span class="sxs-lookup"><span data-stu-id="519d3-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="519d3-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>dniu</span><span class="sxs-lookup"><span data-stu-id="519d3-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="519d3-213">Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .</span><span class="sxs-lookup"><span data-stu-id="519d3-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="519d3-214">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="519d3-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="519d3-215">Implementacje dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="519d3-215">File Provider implementations</span></span>

<span data-ttu-id="519d3-216">Dostępne są trzy `IFileProvider` implementacje programu.</span><span class="sxs-lookup"><span data-stu-id="519d3-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="519d3-217">Implementacja</span><span class="sxs-lookup"><span data-stu-id="519d3-217">Implementation</span></span> | <span data-ttu-id="519d3-218">Opis</span><span class="sxs-lookup"><span data-stu-id="519d3-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="519d3-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="519d3-220">Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu.</span><span class="sxs-lookup"><span data-stu-id="519d3-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="519d3-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="519d3-222">Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="519d3-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="519d3-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="519d3-224">Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="519d3-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="519d3-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-225">PhysicalFileProvider</span></span>

<span data-ttu-id="519d3-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> Zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="519d3-227">`PhysicalFileProvider`<xref:System.IO.File?displayProperty=fullName> używa typu (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="519d3-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="519d3-228">Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="519d3-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="519d3-229">Najbardziej typowym scenariuszem tworzenia i używania elementu `PhysicalFileProvider` jest `IFileProvider` zażądanie w konstruktorze przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="519d3-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="519d3-230">W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="519d3-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="519d3-231">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:</span><span class="sxs-lookup"><span data-stu-id="519d3-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="519d3-232">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="519d3-232">Types in the preceding example:</span></span>

* <span data-ttu-id="519d3-233">`provider``IFileProvider`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="519d3-234">`contents``IDirectoryContents`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="519d3-235">`fileInfo``IFileInfo`jest.</span><span class="sxs-lookup"><span data-stu-id="519d3-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="519d3-236">Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołanie `GetFileInfo` w celu uzyskania informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="519d3-237">Dostawca plików nie ma dostępu poza `applicationRoot` katalogiem.</span><span class="sxs-lookup"><span data-stu-id="519d3-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="519d3-238">Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="519d3-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="519d3-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="519d3-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="519d3-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="519d3-241">`ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="519d3-242">Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość na. `true`</span><span class="sxs-lookup"><span data-stu-id="519d3-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="519d3-243">Określ pliki do osadzenia przy użyciu [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="519d3-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="519d3-244">Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="519d3-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="519d3-245">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="519d3-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="519d3-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="519d3-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="519d3-247">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="519d3-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="519d3-248">Określ względną ścieżkę pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-248">Specify a relative file path.</span></span>
* <span data-ttu-id="519d3-249">Zakres plików do daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="519d3-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="519d3-250">Nazwij osadzony zasób zawierający manifest pliku osadzonego.</span><span class="sxs-lookup"><span data-stu-id="519d3-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="519d3-251">Występują</span><span class="sxs-lookup"><span data-stu-id="519d3-251">Overload</span></span> | <span data-ttu-id="519d3-252">Opis</span><span class="sxs-lookup"><span data-stu-id="519d3-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="519d3-253">Akceptuje opcjonalny `root` parametr ścieżki względnej.</span><span class="sxs-lookup"><span data-stu-id="519d3-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="519d3-254">Określ do zakresu <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> wywołania do tych zasobów w ramach podanej ścieżki. `root`</span><span class="sxs-lookup"><span data-stu-id="519d3-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="519d3-255">Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="519d3-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="519d3-256">Data zakresy daty ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> dla wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `lastModified`</span><span class="sxs-lookup"><span data-stu-id="519d3-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="519d3-257">Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry.</span><span class="sxs-lookup"><span data-stu-id="519d3-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="519d3-258">`manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="519d3-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="519d3-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="519d3-259">CompositeFileProvider</span></span>

<span data-ttu-id="519d3-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Łączy`IFileProvider` wystąpienia, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="519d3-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="519d3-261">Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="519d3-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="519d3-262">W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="519d3-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="519d3-263">Obejrzyj zmiany</span><span class="sxs-lookup"><span data-stu-id="519d3-263">Watch for changes</span></span>

<span data-ttu-id="519d3-264">Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach.</span><span class="sxs-lookup"><span data-stu-id="519d3-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="519d3-265">`Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="519d3-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="519d3-266">`Watch`Zwraca wartość <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="519d3-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="519d3-267">Token zmiany ujawnia:</span><span class="sxs-lookup"><span data-stu-id="519d3-267">The change token exposes:</span></span>

* <span data-ttu-id="519d3-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="519d3-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="519d3-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="519d3-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="519d3-270">Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę.</span><span class="sxs-lookup"><span data-stu-id="519d3-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="519d3-271">Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie Utwórz `IChangeToken` wystąpienia w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="519d3-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="519d3-272">W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:</span><span class="sxs-lookup"><span data-stu-id="519d3-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="519d3-273">Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="519d3-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="519d3-274">Ustaw zmienną `1` środowiskową na lub `true` , aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="519d3-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="519d3-275">Wzorce globalizowania</span><span class="sxs-lookup"><span data-stu-id="519d3-275">Glob patterns</span></span>

<span data-ttu-id="519d3-276">Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* .</span><span class="sxs-lookup"><span data-stu-id="519d3-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="519d3-277">Określ grupy plików z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="519d3-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="519d3-278">Dwa symbole wieloznaczne to `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="519d3-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="519d3-279">Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="519d3-280">Dopasowania są kończone przez `/` znaki `.` i w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="519d3-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="519d3-281">Dopasowuje wszystko na wielu poziomach katalogów.</span><span class="sxs-lookup"><span data-stu-id="519d3-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="519d3-282">Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="519d3-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="519d3-283">**Przykłady wzorców globalizowania**</span><span class="sxs-lookup"><span data-stu-id="519d3-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="519d3-284">Dopasowuje określony plik w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="519d3-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="519d3-285">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="519d3-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="519d3-286">Dopasowuje `appsettings.json` wszystkie pliki w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="519d3-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="519d3-287">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="519d3-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
