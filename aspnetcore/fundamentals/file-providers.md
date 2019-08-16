---
title: Dostawcy plików w ASP.NET Core
author: guardrex
description: Dowiedz się, jak ASP.NET Core abstrakcji dostępu do systemu plików przy użyciu dostawców plików.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487574"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="0a510-103">Dostawcy plików w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a510-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="0a510-104">[Steve Kowalski](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a510-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a510-105">ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików.</span><span class="sxs-lookup"><span data-stu-id="0a510-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="0a510-106">Dostawcy plików są używani w całym ASP.NET Core Framework:</span><span class="sxs-lookup"><span data-stu-id="0a510-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="0a510-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uwidacznia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.</span><span class="sxs-lookup"><span data-stu-id="0a510-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="0a510-108">[Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="0a510-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="0a510-109">[Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.</span><span class="sxs-lookup"><span data-stu-id="0a510-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="0a510-110">Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="0a510-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="0a510-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a510-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="0a510-112">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="0a510-112">File Provider interfaces</span></span>

<span data-ttu-id="0a510-113">Interfejs podstawowy to [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="0a510-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="0a510-114">`IFileProvider`udostępnia metody:</span><span class="sxs-lookup"><span data-stu-id="0a510-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="0a510-115">Uzyskaj informacje o pliku ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="0a510-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="0a510-116">Uzyskaj informacje o katalogu ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="0a510-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="0a510-117">Skonfiguruj powiadomienia o zmianach (przy użyciu [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="0a510-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="0a510-118">`IFileInfo`zapewnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="0a510-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="0a510-119">Istniejący</span><span class="sxs-lookup"><span data-stu-id="0a510-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="0a510-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="0a510-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="0a510-121">Nazwa</span><span class="sxs-lookup"><span data-stu-id="0a510-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="0a510-122">[Długość](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (w bajtach)</span><span class="sxs-lookup"><span data-stu-id="0a510-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="0a510-123">Data [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="0a510-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="0a510-124">Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) .</span><span class="sxs-lookup"><span data-stu-id="0a510-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="0a510-125">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0a510-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="0a510-126">Implementacje dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="0a510-126">File Provider implementations</span></span>

<span data-ttu-id="0a510-127">Dostępne są trzy `IFileProvider` implementacje programu.</span><span class="sxs-lookup"><span data-stu-id="0a510-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="0a510-128">Implementacja</span><span class="sxs-lookup"><span data-stu-id="0a510-128">Implementation</span></span> | <span data-ttu-id="0a510-129">Opis</span><span class="sxs-lookup"><span data-stu-id="0a510-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="0a510-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="0a510-131">Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu.</span><span class="sxs-lookup"><span data-stu-id="0a510-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="0a510-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="0a510-133">Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="0a510-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="0a510-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="0a510-135">Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="0a510-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="0a510-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-136">PhysicalFileProvider</span></span>

<span data-ttu-id="0a510-137">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="0a510-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="0a510-138">`PhysicalFileProvider`używa typu [System. IO. File](/dotnet/api/system.io.file) (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="0a510-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="0a510-139">Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="0a510-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="0a510-140">Podczas tworzenia wystąpienia tego dostawcy wymagana jest ścieżka katalogu i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="0a510-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="0a510-141">Można utworzyć wystąpienie `PhysicalFileProvider` dostawcy bezpośrednio lub można `IFileProvider` zażądać w konstruktorze poprzez iniekcję [zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0a510-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="0a510-142">**Typy statyczne**</span><span class="sxs-lookup"><span data-stu-id="0a510-142">**Static types**</span></span>

<span data-ttu-id="0a510-143">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:</span><span class="sxs-lookup"><span data-stu-id="0a510-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="0a510-144">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0a510-144">Types in the preceding example:</span></span>

* <span data-ttu-id="0a510-145">`provider``IFileProvider`jest.</span><span class="sxs-lookup"><span data-stu-id="0a510-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="0a510-146">`contents``IDirectoryContents`jest.</span><span class="sxs-lookup"><span data-stu-id="0a510-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="0a510-147">`fileInfo``IFileInfo`jest.</span><span class="sxs-lookup"><span data-stu-id="0a510-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="0a510-148">Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołanie `GetFileInfo` w celu uzyskania informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="0a510-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="0a510-149">Dostawca plików nie ma dostępu poza `applicationRoot` katalogiem.</span><span class="sxs-lookup"><span data-stu-id="0a510-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="0a510-150">Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="0a510-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="0a510-151">**Uzyskiwanie typów dostawców plików z iniekcją zależności**</span><span class="sxs-lookup"><span data-stu-id="0a510-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="0a510-152">Wsuń dostawcę do dowolnego konstruktora klasy i przypisz go do pola lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0a510-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="0a510-153">Użyj pola w metodach klasy, aby uzyskać dostęp do plików.</span><span class="sxs-lookup"><span data-stu-id="0a510-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="0a510-154">W przykładowej aplikacji `IndexModel` Klasa `IFileProvider` otrzymuje wystąpienie, aby uzyskać zawartość katalogu dla ścieżki podstawowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0a510-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="0a510-155">*Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="0a510-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0a510-156">Na `IDirectoryContents` stronie zostały powtórzone.</span><span class="sxs-lookup"><span data-stu-id="0a510-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="0a510-157">*Pages/index. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a510-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="0a510-158">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="0a510-159">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) jest używany do uzyskiwania dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="0a510-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="0a510-160">`ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.</span><span class="sxs-lookup"><span data-stu-id="0a510-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="0a510-161">Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość na. `true`</span><span class="sxs-lookup"><span data-stu-id="0a510-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="0a510-162">Określ pliki do osadzenia przy użyciu [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="0a510-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="0a510-163">Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="0a510-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="0a510-164">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0a510-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="0a510-165">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a510-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="0a510-166">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="0a510-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="0a510-167">Określ względną ścieżkę pliku.</span><span class="sxs-lookup"><span data-stu-id="0a510-167">Specify a relative file path.</span></span>
* <span data-ttu-id="0a510-168">Zakres plików do daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="0a510-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="0a510-169">Nazwij osadzony zasób zawierający manifest pliku osadzonego.</span><span class="sxs-lookup"><span data-stu-id="0a510-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="0a510-170">Występują</span><span class="sxs-lookup"><span data-stu-id="0a510-170">Overload</span></span> | <span data-ttu-id="0a510-171">Opis</span><span class="sxs-lookup"><span data-stu-id="0a510-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="0a510-172">ManifestEmbeddedFileProvider (zestaw, ciąg)</span><span class="sxs-lookup"><span data-stu-id="0a510-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="0a510-173">Akceptuje opcjonalny `root` parametr ścieżki względnej.</span><span class="sxs-lookup"><span data-stu-id="0a510-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="0a510-174">Określ wywołania zakresu [](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) doGetDirectoryContentsdotychzasobówwramachpodanej`root` ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0a510-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="0a510-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="0a510-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="0a510-176">Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="0a510-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="0a510-177">Data Scopes Data ostatniej modyfikacji dla wystąpień [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) zwracanych przez IFileProvider. [](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) `lastModified`</span><span class="sxs-lookup"><span data-stu-id="0a510-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="0a510-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="0a510-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="0a510-179">Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry.</span><span class="sxs-lookup"><span data-stu-id="0a510-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="0a510-180">`manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="0a510-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="0a510-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0a510-181">CompositeFileProvider</span></span>

<span data-ttu-id="0a510-182">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) łączy `IFileProvider` wystąpienia, ujawniając pojedynczy interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="0a510-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="0a510-183">Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0a510-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="0a510-184">W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0a510-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="0a510-185">Obejrzyj zmiany</span><span class="sxs-lookup"><span data-stu-id="0a510-185">Watch for changes</span></span>

<span data-ttu-id="0a510-186">Metoda [IFileProvider. Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach.</span><span class="sxs-lookup"><span data-stu-id="0a510-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="0a510-187">`Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="0a510-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="0a510-188">`Watch`zwraca [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="0a510-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="0a510-189">Token zmiany ujawnia:</span><span class="sxs-lookup"><span data-stu-id="0a510-189">The change token exposes:</span></span>

* <span data-ttu-id="0a510-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.</span><span class="sxs-lookup"><span data-stu-id="0a510-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="0a510-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0a510-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="0a510-192">Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę.</span><span class="sxs-lookup"><span data-stu-id="0a510-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="0a510-193">Aby włączyć monitorowanie stałe, użyj [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (pokazanego poniżej) lub Utwórz `IChangeToken` ponownie wystąpienia w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a510-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="0a510-194">W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:</span><span class="sxs-lookup"><span data-stu-id="0a510-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="0a510-195">Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="0a510-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="0a510-196">Ustaw zmienną `1` środowiskową na lub `true` , aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="0a510-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="0a510-197">Wzorce globalizowania</span><span class="sxs-lookup"><span data-stu-id="0a510-197">Glob patterns</span></span>

<span data-ttu-id="0a510-198">Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* .</span><span class="sxs-lookup"><span data-stu-id="0a510-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="0a510-199">Określ grupy plików z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="0a510-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="0a510-200">Dwa symbole wieloznaczne to `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="0a510-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="0a510-201">Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku.</span><span class="sxs-lookup"><span data-stu-id="0a510-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="0a510-202">Dopasowania są kończone przez `/` znaki `.` i w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="0a510-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="0a510-203">Dopasowuje wszystko na wielu poziomach katalogów.</span><span class="sxs-lookup"><span data-stu-id="0a510-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="0a510-204">Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="0a510-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="0a510-205">**Przykłady wzorców globalizowania**</span><span class="sxs-lookup"><span data-stu-id="0a510-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="0a510-206">Dopasowuje określony plik w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="0a510-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="0a510-207">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="0a510-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="0a510-208">Dopasowuje `appsettings.json` wszystkie pliki w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="0a510-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="0a510-209">Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .</span><span class="sxs-lookup"><span data-stu-id="0a510-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
