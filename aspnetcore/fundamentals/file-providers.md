---
title: Dostawcy plików w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ASP.NET Core abstrakcji dostępu do systemu plików przy użyciu dostawców plików.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 34a48bbcf9ffb20bb61f89c80adedc1cc4783988
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658789"
---
# <a name="file-providers-in-aspnet-core"></a>Dostawcy plików w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików. Dostawcy plików są używani w całym ASP.NET Core Framework:

* `IWebHostEnvironment` uwidacznia [katalog główny zawartości](xref:fundamentals/index#content-root) aplikacji i [katalogu głównego sieci web](xref:fundamentals/index#web-root) jako typy `IFileProvider`.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.
* Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Interfejs podstawowy jest <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` uwidacznia metody:

* Uzyskaj informacje o pliku (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Uzyskaj informacje o katalogu (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Skonfiguruj powiadomienia o zmianach (przy użyciu <xref:Microsoft.Extensions.Primitives.IChangeToken>).

`IFileInfo` udostępnia metody i właściwości do pracy z plikami:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (w bajtach)
* Data <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>

Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w `Startup.ConfigureServices` do użycia w całej aplikacji przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementacje dostawcy plików

Dostępne są trzy implementacje `IFileProvider`.

| Wdrażanie | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider` używa typu <xref:System.IO.File?displayProperty=fullName> (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych. Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi. Najbardziej typowym scenariuszem tworzenia i używania `PhysicalFileProvider` jest zażądanie `IFileProvider` w konstruktorze poprzez [iniekcję zależności](xref:fundamentals/dependency-injection).

W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.

Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Typy w poprzednim przykładzie:

* `provider` jest `IFileProvider`.
* `contents` jest `IDirectoryContents`.
* `fileInfo` jest `IFileInfo`.

Dostawcy plików można użyć do iteracji w katalogu określonym przez `applicationRoot` lub wywołać `GetFileInfo`, aby uzyskać informacje o pliku. Dostawca plików nie ma dostępu poza katalogiem `applicationRoot`.

Przykładowa aplikacja tworzy dostawcę w klasie `Startup.ConfigureServices` aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> służy do uzyskiwania dostępu do plików osadzonych w zestawach. `ManifestEmbeddedFileProvider` używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.

Dodaj odwołanie do pakietu do projektu dla pakietu [Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) .

Aby wygenerować manifest osadzonych plików, ustaw właściwość `<GenerateEmbeddedFilesManifest>` na `true`. Określ pliki do osadzenia przy użyciu [\<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.

Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Dodatkowe przeciążenia umożliwiają:

* Określ względną ścieżkę pliku.
* Zakres plików do daty ostatniej modyfikacji.
* Nazwij osadzony zasób zawierający manifest pliku osadzonego.

| Przeciążenie | Opis |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Akceptuje opcjonalny parametr ścieżki względnej `root`. Określ `root`, aby określić zakres wywołań <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> do tych zasobów w ramach podanej ścieżki. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Akceptuje opcjonalny parametr ścieżki względnej `root` i parametr `lastModified` Date (<xref:System.DateTimeOffset>). Data `lastModified` zakresy dat ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Akceptuje opcjonalną `root` ścieżkę względną, datę `lastModified` i parametry `manifestName`. `manifestName` reprezentuje nazwę zasobu osadzonego zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> łączy wystąpienia `IFileProvider`, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`Przekaż co najmniej jedno wystąpienie `IFileProvider` do jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępniają pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Obejrzyj zmiany

Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach. `Watch` akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików. `Watch` zwraca <xref:Microsoft.Extensions.Primitives.IChangeToken>. Token zmiany ujawnia:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; wywoływana, gdy zostaną wykryte zmiany do określonego ciągu ścieżki. Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę. Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazane poniżej) lub ponownie utwórz wystąpienia `IChangeToken` w odpowiedzi na zmiany.

W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach. Ustaw zmienną środowiskową `DOTNET_USE_POLLING_FILE_WATCHER`, aby `1` lub `true` sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować).

## <a name="glob-patterns"></a>Wzorce globalizowania

Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* . Określ grupy plików z tymi wzorcami. Dwa symbole wieloznaczne są `*` i `**`:

**`*`**  
Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku. Dopasowania są kończone przez `/` i `.` znaków w ścieżce pliku.

**`**`**  
Dopasowuje wszystko na wielu poziomach katalogów. Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.

**Przykłady wzorców globalizowania**

**`directory/file.txt`**  
Dopasowuje określony plik w określonym katalogu.

**`directory/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.

**`directory/*/appsettings.json`**  
Dopasowuje wszystkie pliki `appsettings.json` w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .

**`directory/**/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików. Dostawcy plików są używani w całym ASP.NET Core Framework:

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uwidacznia [katalog główny zawartości](xref:fundamentals/index#content-root) aplikacji i [katalogu głównego sieci web](xref:fundamentals/index#web-root) jako typy `IFileProvider`.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.
* Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Interfejs podstawowy jest <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` uwidacznia metody:

* Uzyskaj informacje o pliku (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Uzyskaj informacje o katalogu (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Skonfiguruj powiadomienia o zmianach (przy użyciu <xref:Microsoft.Extensions.Primitives.IChangeToken>).

`IFileInfo` udostępnia metody i właściwości do pracy z plikami:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (w bajtach)
* Data <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>

Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w `Startup.ConfigureServices` do użycia w całej aplikacji przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementacje dostawcy plików

Dostępne są trzy implementacje `IFileProvider`.

| Wdrażanie | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider` używa typu <xref:System.IO.File?displayProperty=fullName> (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych. Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi. Najbardziej typowym scenariuszem tworzenia i używania `PhysicalFileProvider` jest zażądanie `IFileProvider` w konstruktorze poprzez [iniekcję zależności](xref:fundamentals/dependency-injection).

W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.

Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Typy w poprzednim przykładzie:

* `provider` jest `IFileProvider`.
* `contents` jest `IDirectoryContents`.
* `fileInfo` jest `IFileInfo`.

Dostawcy plików można użyć do iteracji w katalogu określonym przez `applicationRoot` lub wywołać `GetFileInfo`, aby uzyskać informacje o pliku. Dostawca plików nie ma dostępu poza katalogiem `applicationRoot`.

Przykładowa aplikacja tworzy dostawcę w klasie `Startup.ConfigureServices` aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> służy do uzyskiwania dostępu do plików osadzonych w zestawach. `ManifestEmbeddedFileProvider` używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.

Aby wygenerować manifest osadzonych plików, ustaw właściwość `<GenerateEmbeddedFilesManifest>` na `true`. Określ pliki do osadzenia przy użyciu [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.

Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Dodatkowe przeciążenia umożliwiają:

* Określ względną ścieżkę pliku.
* Zakres plików do daty ostatniej modyfikacji.
* Nazwij osadzony zasób zawierający manifest pliku osadzonego.

| Przeciążenie | Opis |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Akceptuje opcjonalny parametr ścieżki względnej `root`. Określ `root`, aby określić zakres wywołań <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> do tych zasobów w ramach podanej ścieżki. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Akceptuje opcjonalny parametr ścieżki względnej `root` i parametr `lastModified` Date (<xref:System.DateTimeOffset>). Data `lastModified` zakresy dat ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Akceptuje opcjonalną `root` ścieżkę względną, datę `lastModified` i parametry `manifestName`. `manifestName` reprezentuje nazwę zasobu osadzonego zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> łączy wystąpienia `IFileProvider`, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`Przekaż co najmniej jedno wystąpienie `IFileProvider` do jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępniają pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Obejrzyj zmiany

Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach. `Watch` akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików. `Watch` zwraca <xref:Microsoft.Extensions.Primitives.IChangeToken>. Token zmiany ujawnia:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; wywoływana, gdy zostaną wykryte zmiany do określonego ciągu ścieżki. Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę. Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazane poniżej) lub ponownie utwórz wystąpienia `IChangeToken` w odpowiedzi na zmiany.

W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach. Ustaw zmienną środowiskową `DOTNET_USE_POLLING_FILE_WATCHER`, aby `1` lub `true` sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować).

## <a name="glob-patterns"></a>Wzorce globalizowania

Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* . Określ grupy plików z tymi wzorcami. Dwa symbole wieloznaczne są `*` i `**`:

**`*`**  
Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku. Dopasowania są kończone przez `/` i `.` znaków w ścieżce pliku.

**`**`**  
Dopasowuje wszystko na wielu poziomach katalogów. Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.

**Przykłady wzorców globalizowania**

**`directory/file.txt`**  
Dopasowuje określony plik w określonym katalogu.

**`directory/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.

**`directory/*/appsettings.json`**  
Dopasowuje wszystkie pliki `appsettings.json` w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .

**`directory/**/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .

::: moniker-end
