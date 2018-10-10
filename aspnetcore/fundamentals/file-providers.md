---
title: Dostawcy plików w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: a0d326f5fc995cb903380315879d39a8ce851d06
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913219"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="202fa-103">Dostawcy plików w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="202fa-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="202fa-104">Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="202fa-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="202fa-105">Platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików.</span><span class="sxs-lookup"><span data-stu-id="202fa-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="202fa-106">Dostawcy plików są używane w ramach platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="202fa-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="202fa-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) udostępnia zawartość katalogu głównego aplikacji i katalog główny sieci web jako `IFileProvider` typów.</span><span class="sxs-lookup"><span data-stu-id="202fa-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="202fa-108">[Statyczne pliki oprogramowania pośredniczącego](xref:fundamentals/static-files) używa dostawcy plików do zlokalizowania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="202fa-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="202fa-109">[Razor](xref:mvc/views/razor) używa dostawcy plików do zlokalizowania, widoków i stron.</span><span class="sxs-lookup"><span data-stu-id="202fa-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="202fa-110">Zestaw narzędzi .NET core korzysta z dostawcy plików i glob wzorce, aby określić, które pliki powinny być publikowane.</span><span class="sxs-lookup"><span data-stu-id="202fa-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="202fa-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="202fa-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="202fa-112">Interfejsy dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="202fa-112">File Provider interfaces</span></span>

<span data-ttu-id="202fa-113">Podstawowy interfejs jest [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="202fa-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="202fa-114">`IFileProvider` udostępnia metody do:</span><span class="sxs-lookup"><span data-stu-id="202fa-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="202fa-115">Uzyskaj informacje o pliku ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="202fa-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="202fa-116">Uzyskaj informacje o katalogu ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="202fa-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="202fa-117">Konfigurowanie powiadomień o zmianie (przy użyciu [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="202fa-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="202fa-118">`IFileInfo` udostępnia metody i właściwości do pracy z plikami:</span><span class="sxs-lookup"><span data-stu-id="202fa-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="202fa-119">Istnieje</span><span class="sxs-lookup"><span data-stu-id="202fa-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="202fa-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="202fa-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="202fa-121">Nazwa</span><span class="sxs-lookup"><span data-stu-id="202fa-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="202fa-122">[Długość](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (w bajtach)</span><span class="sxs-lookup"><span data-stu-id="202fa-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="202fa-123">[Ostatnia modyfikacja](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) daty</span><span class="sxs-lookup"><span data-stu-id="202fa-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="202fa-124">Można czytać z pliku przy użyciu [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) metody.</span><span class="sxs-lookup"><span data-stu-id="202fa-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="202fa-125">Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę pliku w programie `Startup.ConfigureServices` używanych w całej aplikacji za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="202fa-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="202fa-126">Plik implementacji dostawcy</span><span class="sxs-lookup"><span data-stu-id="202fa-126">File Provider implementations</span></span>

<span data-ttu-id="202fa-127">Trzy implementacje `IFileProvider` są dostępne.</span><span class="sxs-lookup"><span data-stu-id="202fa-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="202fa-128">Implementacja</span><span class="sxs-lookup"><span data-stu-id="202fa-128">Implementation</span></span> | <span data-ttu-id="202fa-129">Opis</span><span class="sxs-lookup"><span data-stu-id="202fa-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="202fa-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="202fa-131">Dostawca fizycznych umożliwia dostęp do plików fizycznych w systemie.</span><span class="sxs-lookup"><span data-stu-id="202fa-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="202fa-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="202fa-133">Manifestu osadzonego dostawcy umożliwia dostęp do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="202fa-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="202fa-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="202fa-135">Złożony dostawca jest używany do udostępnienia połączone pliki i katalogi z jednego lub więcej innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="202fa-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="202fa-136">Implementacja</span><span class="sxs-lookup"><span data-stu-id="202fa-136">Implementation</span></span> | <span data-ttu-id="202fa-137">Opis</span><span class="sxs-lookup"><span data-stu-id="202fa-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="202fa-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="202fa-139">Dostawca fizycznych umożliwia dostęp do plików fizycznych w systemie.</span><span class="sxs-lookup"><span data-stu-id="202fa-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="202fa-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="202fa-141">Osadzony dostawcy umożliwia dostęp do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="202fa-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="202fa-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="202fa-143">Złożony dostawca jest używany do udostępnienia połączone pliki i katalogi z jednego lub więcej innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="202fa-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="202fa-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-144">PhysicalFileProvider</span></span>

<span data-ttu-id="202fa-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="202fa-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="202fa-146">`PhysicalFileProvider` używa [System.IO.File](/dotnet/api/system.io.file) typu (w przypadku fizycznego provider) i zakresów wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="202fa-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="202fa-147">Tego zakresu, które uniemożliwia dostęp do systemu plików spoza określonego katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="202fa-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="202fa-148">Podczas tworzenia wystąpienia tego dostawcy, ścieżka katalogu jest wymagana i stanowi podstawową ścieżkę dla wszystkich żądań przy użyciu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="202fa-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="202fa-149">Można utworzyć wystąpienie `PhysicalFileProvider` poprosić dostawcę bezpośrednio lub `IFileProvider` w Konstruktorze za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="202fa-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="202fa-150">**Typy statyczne**</span><span class="sxs-lookup"><span data-stu-id="202fa-150">**Static types**</span></span>

<span data-ttu-id="202fa-151">Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i użyć go do uzyskania zawartości katalogu i informacje o pliku:</span><span class="sxs-lookup"><span data-stu-id="202fa-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="202fa-152">Typy w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="202fa-152">Types in the preceding example:</span></span>

* <span data-ttu-id="202fa-153">`provider` jest `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="202fa-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="202fa-154">`contents` jest `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="202fa-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="202fa-155">`fileInfo` jest `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="202fa-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="202fa-156">Dostawcy plików może służyć do iterowania po katalogu określonego przez `applicationRoot` lub zadzwoń `GetFileInfo` można uzyskać informacji o pliku.</span><span class="sxs-lookup"><span data-stu-id="202fa-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="202fa-157">Dostawcy plików nie ma dostępu poza `applicationRoot` katalogu.</span><span class="sxs-lookup"><span data-stu-id="202fa-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="202fa-158">Przykładowa aplikacja tworzy dostawcę aplikacji `Startup.ConfigureServices` przy użyciu [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="202fa-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="202fa-159">**Uzyskaj typy dostawcy plików za pomocą iniekcji zależności**</span><span class="sxs-lookup"><span data-stu-id="202fa-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="202fa-160">Wstrzyknięcie dostawcy do dowolnego konstruktora klasy i przypisz go do lokalnego pola.</span><span class="sxs-lookup"><span data-stu-id="202fa-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="202fa-161">Dostęp do plików, należy użyć pola w całej metody tej klasy.</span><span class="sxs-lookup"><span data-stu-id="202fa-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="202fa-162">W przykładowej aplikacji `IndexModel` klasa otrzymuje `IFileProvider` wystąpienia, aby uzyskać zawartość katalogu dla ścieżki podstawowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="202fa-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="202fa-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="202fa-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="202fa-164">`IDirectoryContents` Są postanowiliśmy na stronie.</span><span class="sxs-lookup"><span data-stu-id="202fa-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="202fa-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="202fa-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="202fa-166">W przykładowej aplikacji `HomeController` klasa otrzymuje `IFileProvider` wystąpienia, aby uzyskać zawartość katalogu dla ścieżki podstawowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="202fa-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="202fa-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="202fa-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="202fa-168">`IDirectoryContents` Są postanowiliśmy w widoku.</span><span class="sxs-lookup"><span data-stu-id="202fa-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="202fa-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="202fa-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="202fa-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="202fa-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) umożliwia dostęp do plików osadzonych w ramach zestawów.</span><span class="sxs-lookup"><span data-stu-id="202fa-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="202fa-172">`ManifestEmbeddedFileProvider` Używa manifestu kompilowane do zestawu odtworzenie oryginalnej ścieżki plików osadzonych.</span><span class="sxs-lookup"><span data-stu-id="202fa-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="202fa-173">`ManifestEmbeddedFileProvider` Jest dostępna w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="202fa-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="202fa-174">Dostępu do plików osadzonych w zestawach platformy ASP.NET Core w wersji 2.0 lub wcześniej, zobacz [platformy ASP.NET Core 1.x wersję tego tematu](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="202fa-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="202fa-175">Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość `true`.</span><span class="sxs-lookup"><span data-stu-id="202fa-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="202fa-176">Określ pliki, aby osadzić przy użyciu [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="202fa-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="202fa-177">Użyj [wzorców glob](#glob-patterns) do określenia co najmniej jeden plik do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="202fa-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="202fa-178">Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje zawierający obecnie wykonywany zestaw do jej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="202fa-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="202fa-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="202fa-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="202fa-180">Dodatkowe przeciążenia umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="202fa-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="202fa-181">Określ względną ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="202fa-181">Specify a relative file path.</span></span>
* <span data-ttu-id="202fa-182">Pliki zakresu daty ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="202fa-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="202fa-183">Nazwa osadzonego zasobu zawierającego manifestu osadzonego pliku.</span><span class="sxs-lookup"><span data-stu-id="202fa-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="202fa-184">przeciążenie</span><span class="sxs-lookup"><span data-stu-id="202fa-184">Overload</span></span> | <span data-ttu-id="202fa-185">Opis</span><span class="sxs-lookup"><span data-stu-id="202fa-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="202fa-186">ManifestEmbeddedFileProvider (Assembly, ciąg)</span><span class="sxs-lookup"><span data-stu-id="202fa-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="202fa-187">Akceptuje opcjonalny `root` parametr ścieżki względnej.</span><span class="sxs-lookup"><span data-stu-id="202fa-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="202fa-188">Określ `root` do zakresu wywołań [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) do tych zasobów w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="202fa-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="202fa-189">ManifestEmbeddedFileProvider (DateTimeOffset zestawu, ciąg)</span><span class="sxs-lookup"><span data-stu-id="202fa-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="202fa-190">Akceptuje opcjonalny `root` parametr ścieżki względnej i `lastModified` daty ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametr.</span><span class="sxs-lookup"><span data-stu-id="202fa-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="202fa-191">`lastModified` Data zakresów daty ostatniej modyfikacji [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) wystąpień zwrócona przez [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="202fa-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="202fa-192">ManifestEmbeddedFileProvider (zestawu, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="202fa-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="202fa-193">Akceptuje opcjonalny `root` ścieżki względnej, `lastModified` datę i `manifestName` parametrów.</span><span class="sxs-lookup"><span data-stu-id="202fa-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="202fa-194">`manifestName` Reprezentuje nazwę osadzonego zasobu zawierającego manifest.</span><span class="sxs-lookup"><span data-stu-id="202fa-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="202fa-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="202fa-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) umożliwia dostęp do plików osadzonych w ramach zestawów.</span><span class="sxs-lookup"><span data-stu-id="202fa-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="202fa-197">Określ pliki, aby osadzić przy użyciu [ &lt;EmbeddedResource&gt; ](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) właściwość w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="202fa-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="202fa-198">Użyj [wzorców glob](#glob-patterns) do określenia co najmniej jeden plik do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="202fa-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="202fa-199">Przykładowa aplikacja tworzy `EmbeddedFileProvider` i przekazuje zawierający obecnie wykonywany zestaw do jej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="202fa-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="202fa-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="202fa-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="202fa-201">Zasobów osadzonych nie ujawniają katalogów.</span><span class="sxs-lookup"><span data-stu-id="202fa-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="202fa-202">Zamiast ścieżki do zasobów (za pośrednictwem jego przestrzeń nazw) jest osadzony w jego nazwy pliku za pomocą `.` separatorów.</span><span class="sxs-lookup"><span data-stu-id="202fa-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="202fa-203">W przykładowej aplikacji `baseNamespace` jest `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="202fa-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="202fa-204">[EmbeddedFileProvider (Assembly, ciąg)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) Konstruktor akceptuje opcjonalny `baseNamespace` parametru.</span><span class="sxs-lookup"><span data-stu-id="202fa-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="202fa-205">Określ podstawowej przestrzeni nazw do zakresu wywołań [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) do tych zasobów w podanej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="202fa-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="202fa-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="202fa-206">CompositeFileProvider</span></span>

<span data-ttu-id="202fa-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) łączy `IFileProvider` wystąpień, udostępnia jeden interfejs do pracy z plikami od wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="202fa-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="202fa-208">Podczas tworzenia `CompositeFileProvider`, — dostęp próbny, co najmniej jeden `IFileProvider` wystąpień dla jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="202fa-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="202fa-209">W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępnienia plików do `CompositeFileProvider` zarejestrowany w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="202fa-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="202fa-210">W przykładowej aplikacji `PhysicalFileProvider` i `EmbeddedFileProvider` udostępnienia plików do `CompositeFileProvider` zarejestrowany w kontenerze usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="202fa-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="202fa-211">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="202fa-211">Watch for changes</span></span>

<span data-ttu-id="202fa-212">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda zapewnia scenariusz, aby obejrzeć plików lub katalogów do zmiany.</span><span class="sxs-lookup"><span data-stu-id="202fa-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="202fa-213">`Watch` akceptuje ciąg ścieżki, która może używać [wzorców glob](#glob-patterns) do określenia wielu plików.</span><span class="sxs-lookup"><span data-stu-id="202fa-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="202fa-214">`Watch` Zwraca [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="202fa-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="202fa-215">Udostępnia tokenu zmiany:</span><span class="sxs-lookup"><span data-stu-id="202fa-215">The change token exposes:</span></span>

* <span data-ttu-id="202fa-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): właściwości, które można przeprowadzić inspekcji, aby określić, jeśli nastąpiła zmiana.</span><span class="sxs-lookup"><span data-stu-id="202fa-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="202fa-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): wywoływane, gdy zostaną wykryte zmiany w ciągu określonej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="202fa-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="202fa-218">Każdy token zmiany tylko wywołuje jego wywołania zwrotnego skojarzonego w odpowiedzi na pojedynczej zmiany.</span><span class="sxs-lookup"><span data-stu-id="202fa-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="202fa-219">Aby włączyć monitorowanie stałe, należy użyć [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (pokazana poniżej) lub Utwórz ponownie `IChangeToken` wystąpień w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="202fa-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="202fa-220">W przykładowej aplikacji *WatchConsole* konsoli aplikacja jest skonfigurowana do wyświetlania komunikatu przy każdej modyfikacji pliku tekstowego:</span><span class="sxs-lookup"><span data-stu-id="202fa-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="202fa-221">Niektóre systemy plików, takich jak kontenery platformy Docker i udziały sieciowe i niezawodnie nie może wysłać powiadomienia o zmianach.</span><span class="sxs-lookup"><span data-stu-id="202fa-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="202fa-222">Ustaw `DOTNET_USE_POLLING_FILE_WATCHER` zmiennej środowiskowej, aby `1` lub `true` sondowanie systemu plików w celu zmiany co cztery sekundy (nie można konfigurować).</span><span class="sxs-lookup"><span data-stu-id="202fa-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="202fa-223">Glob wzorców</span><span class="sxs-lookup"><span data-stu-id="202fa-223">Glob patterns</span></span>

<span data-ttu-id="202fa-224">Ścieżki systemu plików, użyj wzorców symboli wieloznacznych, o nazwie *wzorców glob (lub symboli wieloznacznych)*.</span><span class="sxs-lookup"><span data-stu-id="202fa-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="202fa-225">Określ grupy plików z tych wzorców.</span><span class="sxs-lookup"><span data-stu-id="202fa-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="202fa-226">Są dwa znaki symboli wieloznacznych `*` i `**`:</span><span class="sxs-lookup"><span data-stu-id="202fa-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="202fa-227">Pasuje do wszystkiego na bieżącym poziomie folderu, nazwy pliku lub dowolnego rozszerzenia pliku.</span><span class="sxs-lookup"><span data-stu-id="202fa-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="202fa-228">Dopasowania są kończone przez `/` i `.` znaki w ścieżce pliku.</span><span class="sxs-lookup"><span data-stu-id="202fa-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="202fa-229">Pasuje do wszystkiego na wielu poziomach katalogu.</span><span class="sxs-lookup"><span data-stu-id="202fa-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="202fa-230">Może służyć do rekursywnie dopasować wiele plików w ramach hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="202fa-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="202fa-231">**Glob wzorzec przykłady**</span><span class="sxs-lookup"><span data-stu-id="202fa-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="202fa-232">Pasuje do określonego pliku w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="202fa-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="202fa-233">Uwzględnia wszystkie pliki z *.txt* rozszerzenie w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="202fa-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="202fa-234">Pasuje do wszystkich `appsettings.json` plików w katalogach dokładnie jeden poziom poniżej *katalogu* folderu.</span><span class="sxs-lookup"><span data-stu-id="202fa-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="202fa-235">Uwzględnia wszystkie pliki z *.txt* znaleźć rozszerzenia w dowolnym miejscu w ramach *katalogu* folderu.</span><span class="sxs-lookup"><span data-stu-id="202fa-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
