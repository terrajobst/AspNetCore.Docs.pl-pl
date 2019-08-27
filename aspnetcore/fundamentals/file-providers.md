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
# <a name="file-providers-in-aspnet-core"></a>Dostawcy plików w ASP.NET Core

[Steve Kowalski](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików. Dostawcy plików są używani w całym ASP.NET Core Framework:

* `IWebHostEnvironment`udostępnia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.
* Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider`udostępnia metody:

* Uzyskaj informacje o pliku<xref:Microsoft.Extensions.FileProviders.IFileInfo>().
* Uzyskaj informacje o katalogu<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>().
* Skonfiguruj powiadomienia o zmianach (przy <xref:Microsoft.Extensions.Primitives.IChangeToken>użyciu).

`IFileInfo`zapewnia metody i właściwości do pracy z plikami:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(w bajtach)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>dniu

Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementacje dostawcy plików

Dostępne są trzy `IFileProvider` implementacje programu.

| Implementacja | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> Zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider`<xref:System.IO.File?displayProperty=fullName> używa typu (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych. Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi. Najbardziej typowym scenariuszem tworzenia i używania elementu `PhysicalFileProvider` jest `IFileProvider` zażądanie w konstruktorze przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).

W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.

Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Typy w poprzednim przykładzie:

* `provider``IFileProvider`jest.
* `contents``IDirectoryContents`jest.
* `fileInfo``IFileInfo`jest.

Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołanie `GetFileInfo` w celu uzyskania informacji o pliku. Dostawca plików nie ma dostępu poza `applicationRoot` katalogiem.

Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Służy do uzyskiwania dostępu do plików osadzonych w zestawach. `ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.

Dodaj odwołanie do pakietu do projektu dla pakietu [Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) .

Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość na. `true` Określ pliki do osadzenia przy użyciu [ \<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.

Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Dodatkowe przeciążenia umożliwiają:

* Określ względną ścieżkę pliku.
* Zakres plików do daty ostatniej modyfikacji.
* Nazwij osadzony zasób zawierający manifest pliku osadzonego.

| Występują | Opis |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Akceptuje opcjonalny `root` parametr ścieżki względnej. Określ do zakresu <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> wywołania do tych zasobów w ramach podanej ścieżki. `root` |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date (<xref:System.DateTimeOffset>). Data zakresy daty ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> dla wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `lastModified` |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry. `manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Łączy`IFileProvider` wystąpienia, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Obejrzyj zmiany

Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach. `Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików. `Watch`Zwraca wartość <xref:Microsoft.Extensions.Primitives.IChangeToken>. Token zmiany ujawnia:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki. Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę. Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie Utwórz `IChangeToken` wystąpienia w odpowiedzi na zmiany.

W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach. Ustaw zmienną `1` środowiskową na lub `true` , aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować). `DOTNET_USE_POLLING_FILE_WATCHER`

## <a name="glob-patterns"></a>Wzorce globalizowania

Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* . Określ grupy plików z tymi wzorcami. Dwa symbole wieloznaczne to `*`: `**`

**`*`**  
Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku. Dopasowania są kończone przez `/` znaki `.` i w ścieżce pliku.

**`**`**  
Dopasowuje wszystko na wielu poziomach katalogów. Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.

**Przykłady wzorców globalizowania**

**`directory/file.txt`**  
Dopasowuje określony plik w określonym katalogu.

**`directory/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.

**`directory/*/appsettings.json`**  
Dopasowuje `appsettings.json` wszystkie pliki w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .

**`directory/**/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików. Dostawcy plików są używani w całym ASP.NET Core Framework:

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>udostępnia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.
* Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Podstawowy interfejs to <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider`udostępnia metody:

* Uzyskaj informacje o pliku<xref:Microsoft.Extensions.FileProviders.IFileInfo>().
* Uzyskaj informacje o katalogu<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>().
* Skonfiguruj powiadomienia o zmianach (przy <xref:Microsoft.Extensions.Primitives.IChangeToken>użyciu).

`IFileInfo`zapewnia metody i właściwości do pracy z plikami:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(w bajtach)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>dniu

Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) .

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementacje dostawcy plików

Dostępne są trzy `IFileProvider` implementacje programu.

| Implementacja | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> Zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider`<xref:System.IO.File?displayProperty=fullName> używa typu (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych. Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi. Najbardziej typowym scenariuszem tworzenia i używania elementu `PhysicalFileProvider` jest `IFileProvider` zażądanie w konstruktorze przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).

W przypadku bezpośredniego tworzenia wystąpienia tego dostawcy ścieżka katalogu jest wymagana i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy.

Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i używania go do uzyskiwania informacji o zawartości i pliku katalogu:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Typy w poprzednim przykładzie:

* `provider``IFileProvider`jest.
* `contents``IDirectoryContents`jest.
* `fileInfo``IFileInfo`jest.

Dostawca plików może służyć do iteracji przez katalog określony przez `applicationRoot` lub wywołanie `GetFileInfo` w celu uzyskania informacji o pliku. Dostawca plików nie ma dostępu poza `applicationRoot` katalogiem.

Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Służy do uzyskiwania dostępu do plików osadzonych w zestawach. `ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.

Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość na. `true` Określ pliki do osadzenia przy użyciu [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Użyj [wzorców globalizowania](#glob-patterns) , aby określić jeden lub więcej plików do osadzenia w zestawie.

Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje aktualnie wykonywany zestaw do jego konstruktora.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Dodatkowe przeciążenia umożliwiają:

* Określ względną ścieżkę pliku.
* Zakres plików do daty ostatniej modyfikacji.
* Nazwij osadzony zasób zawierający manifest pliku osadzonego.

| Występują | Opis |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | Akceptuje opcjonalny `root` parametr ścieżki względnej. Określ do zakresu <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> wywołania do tych zasobów w ramach podanej ścieżki. `root` |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date (<xref:System.DateTimeOffset>). Data zakresy daty ostatniej modyfikacji <xref:Microsoft.Extensions.FileProviders.IFileInfo> dla wystąpień zwracanych przez <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `lastModified` |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry. `manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Łączy`IFileProvider` wystąpienia, uwidaczniając pojedynczy interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Obejrzyj zmiany

Metoda [IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach. `Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików. `Watch`Zwraca wartość <xref:Microsoft.Extensions.Primitives.IChangeToken>. Token zmiany ujawnia:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki. Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę. Aby włączyć monitorowanie stałe, użyj <xref:System.Threading.Tasks.TaskCompletionSource`1> (pokazanego poniżej) lub ponownie Utwórz `IChangeToken` wystąpienia w odpowiedzi na zmiany.

W aplikacji przykładowej Aplikacja konsolowa *WatchConsole* jest skonfigurowana do wyświetlania komunikatu za każdym razem, gdy plik tekstowy zostanie zmodyfikowany:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Niektóre systemy plików, takie jak kontenery platformy Docker i udziały sieciowe, mogą niezawodnie wysyłać powiadomienia o zmianach. Ustaw zmienną `1` środowiskową na lub `true` , aby sondować system plików pod kątem zmian co cztery sekundy (nie można skonfigurować). `DOTNET_USE_POLLING_FILE_WATCHER`

## <a name="glob-patterns"></a>Wzorce globalizowania

Ścieżki systemu plików używają wzorców symboli wieloznacznych o nazwie *globalizowania (lub obsługi symboli wieloznacznych)* . Określ grupy plików z tymi wzorcami. Dwa symbole wieloznaczne to `*`: `**`

**`*`**  
Dopasowuje wszystko na bieżącym poziomie folderu, dowolnej nazwie pliku lub dowolnym rozszerzeniu pliku. Dopasowania są kończone przez `/` znaki `.` i w ścieżce pliku.

**`**`**  
Dopasowuje wszystko na wielu poziomach katalogów. Może służyć do rekursywnego dopasowania wielu plików w hierarchii katalogów.

**Przykłady wzorców globalizowania**

**`directory/file.txt`**  
Dopasowuje określony plik w określonym katalogu.

**`directory/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* w określonym katalogu.

**`directory/*/appsettings.json`**  
Dopasowuje `appsettings.json` wszystkie pliki w katalogach dokładnie o jeden poziom poniżej folderu *katalogu* .

**`directory/**/*.txt`**  
Dopasowuje wszystkie pliki z rozszerzeniem *. txt* , które znajdują się w dowolnym miejscu w folderze *katalogu* .

::: moniker-end
