---
title: "Plik dostawców w platformy ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak platformy ASP.NET Core abstracts dostępu do systemu plików przy użyciu dostawcy plików."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: db207f19b7ddc24dea36009138840be6efebdb84
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="aae9b-103">Plik dostawców w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aae9b-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="aae9b-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="aae9b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="aae9b-105">Platformy ASP.NET Core abstracts dostępu do systemu plików przy użyciu dostawcy plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="aae9b-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aae9b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="aae9b-107">Plik abstrakcje dostawcy</span><span class="sxs-lookup"><span data-stu-id="aae9b-107">File Provider abstractions</span></span>

<span data-ttu-id="aae9b-108">Dostawców w pliku są abstrakcję przez systemy plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="aae9b-109">Interfejs głównego jest `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="aae9b-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="aae9b-110">`IFileProvider`udostępnia metody, aby uzyskać informacje o pliku (`IFileInfo`), informacji katalogowych (`IDirectoryContents`) i aby skonfigurować powiadomienia o zmianie (przy użyciu `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="aae9b-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="aae9b-111">`IFileInfo`udostępnia metody i właściwości o poszczególnych plików lub katalogów.</span><span class="sxs-lookup"><span data-stu-id="aae9b-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="aae9b-112">Ma dwie właściwości boolean, `Exists` i `IsDirectory`, a także właściwości, które opisują pliku `Name`, `Length` (w bajtach) i `LastModified` daty.</span><span class="sxs-lookup"><span data-stu-id="aae9b-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="aae9b-113">Można odczytać z pliku przy użyciu jego `CreateReadStream` metody.</span><span class="sxs-lookup"><span data-stu-id="aae9b-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="aae9b-114">Plik implementacji dostawcy</span><span class="sxs-lookup"><span data-stu-id="aae9b-114">File Provider implementations</span></span>

<span data-ttu-id="aae9b-115">Trzy implementacje `IFileProvider` są dostępne: fizycznych, osadzone i złożone.</span><span class="sxs-lookup"><span data-stu-id="aae9b-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="aae9b-116">Fizyczny dostawca jest używany do dostępu do rzeczywistego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="aae9b-117">Osadzony dostawca jest używany do dostępu do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="aae9b-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="aae9b-118">Złożone dostawcy służy do zapewnienia Scalonej dostępu do plików i katalogów z co najmniej jednego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aae9b-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="aae9b-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="aae9b-119">PhysicalFileProvider</span></span>

<span data-ttu-id="aae9b-120">`PhysicalFileProvider` Zapewnia dostęp do fizycznego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="aae9b-121">Jest zawijany `System.IO.File` typ (w przypadku fizycznego dostawcy), zakresu wszystkie ścieżki do katalogu i jego elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="aae9b-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="aae9b-122">Ten zakres ogranicza dostęp do niektórych katalogu i jego elementów podrzędnych uniemożliwienia dostępu do systemu plików, poza tę granicę.</span><span class="sxs-lookup"><span data-stu-id="aae9b-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="aae9b-123">Podczas tworzenia wystąpienia tego dostawcy, należy go podać ze ścieżką katalogu, który służy jako ścieżki bazowej dla wszystkich żądań wprowadzone do tego dostawcy (i który ogranicza dostęp spoza tej ścieżki).</span><span class="sxs-lookup"><span data-stu-id="aae9b-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="aae9b-124">W aplikacji platformy ASP.NET Core, można utworzyć wystąpienia `PhysicalFileProvider` poprosić dostawcę bezpośrednio, lub `IFileProvider` w kontrolerze lub konstruktora usługi za pośrednictwem [iniekcji zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="aae9b-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="aae9b-125">Drugie podejście zwykle umożliwia uzyskanie bardziej elastyczne i testować rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="aae9b-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="aae9b-126">Poniższy przykład przedstawia sposób tworzenia `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="aae9b-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="aae9b-127">Możesz iterację jego zawartość katalogu lub pobrać informacji o określonego pliku, podając ścieżkę podrzędną.</span><span class="sxs-lookup"><span data-stu-id="aae9b-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="aae9b-128">Aby poprosić dostawcę z kontrolera, określ go w Konstruktorze kontrolera i przypisz je do lokalnego pola.</span><span class="sxs-lookup"><span data-stu-id="aae9b-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="aae9b-129">Należy użyć lokalnego wystąpienia z metody akcji:</span><span class="sxs-lookup"><span data-stu-id="aae9b-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="aae9b-130">Następnie utwórz w aplikacji dostawcy `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="aae9b-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="aae9b-131">W *Index.cshtml* wyświetlić, iterację `IDirectoryContents` podane:</span><span class="sxs-lookup"><span data-stu-id="aae9b-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="aae9b-132">Wynik:</span><span class="sxs-lookup"><span data-stu-id="aae9b-132">The result:</span></span>

![Plik dostawcy przykładowej aplikacji listy fizycznych plików i folderów](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="aae9b-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="aae9b-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="aae9b-135">`EmbeddedFileProvider` Umożliwia dostęp do plików osadzonych w zestawach.</span><span class="sxs-lookup"><span data-stu-id="aae9b-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="aae9b-136">W .NET Core osadzanie plików w zestawie z `<EmbeddedResource>` element *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="aae9b-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="aae9b-137">Można użyć [wzorce globbing](#globbing-patterns) podczas określania plików do osadzenia w zestawie.</span><span class="sxs-lookup"><span data-stu-id="aae9b-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="aae9b-138">Te wzorce może służyć do dopasowania jeden lub więcej plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="aae9b-139">Jest mało prawdopodobne, czy kiedykolwiek zajdzie potrzeba faktycznie osadzić każdego pliku .js w projekcie w jego zestaw; w powyższym przykładzie jest dla celów demonstracyjnych tylko.</span><span class="sxs-lookup"><span data-stu-id="aae9b-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="aae9b-140">Podczas tworzenia `EmbeddedFileProvider`, Przekaż zestawu będzie odczytywał dla jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="aae9b-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="aae9b-141">Fragment kodu powyżej przedstawia sposób tworzenia `EmbeddedFileProvider` z dostępem do obecnie wykonywany zestaw.</span><span class="sxs-lookup"><span data-stu-id="aae9b-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="aae9b-142">Aktualizowanie przykładową aplikację do używania `EmbeddedFileProvider` powoduje następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="aae9b-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Plik dostawcy przykładowej aplikacji wyświetlanie listy plików osadzonych](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="aae9b-144">Zasobów osadzonych nie ujawniaj katalogów.</span><span class="sxs-lookup"><span data-stu-id="aae9b-144">Embedded resources do not expose directories.</span></span> <span data-ttu-id="aae9b-145">Zamiast ścieżki do zasobu (za pośrednictwem jego przestrzeni nazw) jest osadzony w jego przy użyciu nazwy pliku `.` separatorów.</span><span class="sxs-lookup"><span data-stu-id="aae9b-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="aae9b-146">`EmbeddedFileProvider` Konstruktor akceptuje opcjonalny `baseNamespace` parametru.</span><span class="sxs-lookup"><span data-stu-id="aae9b-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="aae9b-147">Określenie tej będzie zakresu wywołań `GetDirectoryContents` do tych zasobów w ramach podanego obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="aae9b-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="aae9b-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="aae9b-148">CompositeFileProvider</span></span>

<span data-ttu-id="aae9b-149">`CompositeFileProvider` Łączy `IFileProvider` wystąpień udostępnia jeden interfejs do pracy z plikami z wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="aae9b-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="aae9b-150">Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jeden `IFileProvider` wystąpień dla jego konstruktora:</span><span class="sxs-lookup"><span data-stu-id="aae9b-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="aae9b-151">Aktualizowanie przykładową aplikację do używania `CompositeFileProvider` który obejmuje zarówno fizyczne i osadzone dostawców został wcześniej skonfigurowany, wynikiem następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="aae9b-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Plik dostawcy przykładowej aplikacji wyświetlanie listy plików fizycznych i folderów i plików osadzonych](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="aae9b-153">Monitorowanie zmian</span><span class="sxs-lookup"><span data-stu-id="aae9b-153">Watching for changes</span></span>

<span data-ttu-id="aae9b-154">`IFileProvider` `Watch` Metody umożliwia Obejrzyj pliki lub katalogi zmian.</span><span class="sxs-lookup"><span data-stu-id="aae9b-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="aae9b-155">Ta metoda przyjmuje ciąg ścieżki, której można użyć [wzorce globbing](#globbing-patterns) do określenia wielu plików i zwraca `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="aae9b-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="aae9b-156">Token ten przedstawia `HasChanged` właściwości, które mogą być kontrolowane i `RegisterChangeCallback` metodę, która jest wywoływana, gdy zostaną wykryte zmiany w ciągu określonej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="aae9b-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="aae9b-157">Należy pamiętać, że każdy token zmiany tylko wywołuje wywołania zwrotnego jego skojarzonego w odpowiedzi na pojedynczej zmiany.</span><span class="sxs-lookup"><span data-stu-id="aae9b-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="aae9b-158">Aby włączyć monitorowanie stałą, można użyć `TaskCompletionSource` w sposób przedstawiony poniżej, lub Utwórz ponownie `IChangeToken` wystąpień w odpowiedzi na zmiany.</span><span class="sxs-lookup"><span data-stu-id="aae9b-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="aae9b-159">W tym artykule przykładowym aplikacji konsoli jest skonfigurowany do wyświetla komunikat, gdy zostanie zmodyfikowany plik tekstowy:</span><span class="sxs-lookup"><span data-stu-id="aae9b-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="aae9b-160">Wynik po zapisaniu pliku kilka razy:</span><span class="sxs-lookup"><span data-stu-id="aae9b-160">The result, after saving the file several times:</span></span>

![Okno polecenia po wykonywania dotnet Uruchom przedstawia zmiany w pliku o quotes.txt monitorowania aplikacji i plik został zmieniony pięć razy.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="aae9b-162">Niektóre systemy plików, takich jak kontenery Docker i udziały sieciowe i niezawodnie nie może wysłać powiadomienia o zmianie.</span><span class="sxs-lookup"><span data-stu-id="aae9b-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="aae9b-163">Ustaw `DOTNET_USE_POLLINGFILEWATCHER` zmienną środowiskową `1` lub `true` do sondowania zmian systemu plików, co 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="aae9b-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="aae9b-164">Wzorce globbing</span><span class="sxs-lookup"><span data-stu-id="aae9b-164">Globbing patterns</span></span>

<span data-ttu-id="aae9b-165">Ścieżki systemu plików, użyj wzorców symboli wieloznacznych o nazwie *wzorce globbing*.</span><span class="sxs-lookup"><span data-stu-id="aae9b-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="aae9b-166">Te proste wzorce, można określić grupy plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="aae9b-167">Są dwa znaki symboli wieloznacznych `*` i `**`.</span><span class="sxs-lookup"><span data-stu-id="aae9b-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="aae9b-168">Pasuje do wszystkiego na bieżącym poziomie folderu lub nazwy pliku lub dowolnym rozszerzeniem pliku.</span><span class="sxs-lookup"><span data-stu-id="aae9b-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="aae9b-169">Zakończono dopasowań w `/` i `.` znaki w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="aae9b-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="aae9b-170">Pasuje do wszystkiego na wielu poziomach katalogu.</span><span class="sxs-lookup"><span data-stu-id="aae9b-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="aae9b-171">Może służyć do rekursywnie pasuje wiele plików w ramach hierarchii katalogów.</span><span class="sxs-lookup"><span data-stu-id="aae9b-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="aae9b-172">Przykłady wzorzec globbing</span><span class="sxs-lookup"><span data-stu-id="aae9b-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="aae9b-173">Pasuje do określonego pliku z określonego katalogu.</span><span class="sxs-lookup"><span data-stu-id="aae9b-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="aae9b-174">Dopasowanie wszystkich plików z `.txt` rozszerzenia z określonego katalogu.</span><span class="sxs-lookup"><span data-stu-id="aae9b-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="aae9b-175">Pasuje do wszystkich `bower.json` plików w katalogach dokładnie jeden poziom poniżej `directory` katalogu.</span><span class="sxs-lookup"><span data-stu-id="aae9b-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="aae9b-176">Dopasowanie wszystkich plików z `.txt` rozszerzenia można znaleźć w dowolnym miejscu w `directory` katalogu.</span><span class="sxs-lookup"><span data-stu-id="aae9b-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="aae9b-177">Użycie dostawcy File w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aae9b-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="aae9b-178">Kilka części platformy ASP.NET Core korzystać z dostawcy plików.</span><span class="sxs-lookup"><span data-stu-id="aae9b-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="aae9b-179">`IHostingEnvironment`przedstawia zawartość katalogu głównego aplikacji i głównego sieci web jako `IFileProvider` typów.</span><span class="sxs-lookup"><span data-stu-id="aae9b-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="aae9b-180">Oprogramowanie pośredniczące plików statycznych korzysta z dostawców plików do lokalizacji plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="aae9b-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="aae9b-181">Razor sprawia, że intensywnie korzysta z `IFileProvider` lokalizowanie widoków.</span><span class="sxs-lookup"><span data-stu-id="aae9b-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="aae9b-182">Dla platformy DotNet publikowania funkcji używa pliku dostawców i wzorce globbing, aby określić pliki, które powinny zostać opublikowane.</span><span class="sxs-lookup"><span data-stu-id="aae9b-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="aae9b-183">Zalecenia dotyczące użycia w aplikacji</span><span class="sxs-lookup"><span data-stu-id="aae9b-183">Recommendations for use in apps</span></span>

<span data-ttu-id="aae9b-184">Jeśli aplikacja platformy ASP.NET Core wymaga dostępu do systemu plików, możesz poprosić wystąpienia `IFileProvider` za pomocą iniekcji zależności, a następnie za pomocą jej metod dostęp, jak pokazano w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="aae9b-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="aae9b-185">Dzięki temu można skonfigurować dostawcę raz, podczas uruchamiania aplikacji i zmniejsza liczbę typów wdrożenia, które tworzy aplikację.</span><span class="sxs-lookup"><span data-stu-id="aae9b-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
