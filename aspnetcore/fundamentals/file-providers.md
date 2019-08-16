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
# <a name="file-providers-in-aspnet-core"></a>Dostawcy plików w ASP.NET Core

[Steve Kowalski](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

ASP.NET Core abstrakcję dostępu systemu plików przy użyciu dostawców plików. Dostawcy plików są używani w całym ASP.NET Core Framework:

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uwidacznia katalog główny zawartości aplikacji i katalogu głównego sieci Web `IFileProvider` jako typy.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawców plików do lokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawców plików do lokalizowania stron i widoków.
* Narzędzia .NET Core używają dostawców plików i wzorców globalizowania, aby określić, które pliki powinny zostać opublikowane.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Interfejs podstawowy to [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider`udostępnia metody:

* Uzyskaj informacje o pliku ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Uzyskaj informacje o katalogu ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Skonfiguruj powiadomienia o zmianach (przy użyciu [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo`zapewnia metody i właściwości do pracy z plikami:

* [Istniejący](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Nazwa](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Długość](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (w bajtach)
* Data [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)

Można odczytać z pliku za pomocą metody [IFileInfo. CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) .

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę plików w programie `Startup.ConfigureServices` do użycia w całej aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementacje dostawcy plików

Dostępne są trzy `IFileProvider` implementacje programu.

| Implementacja | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizyczny jest używany do uzyskiwania dostępu do plików fizycznych systemu. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Dostawca osadzony manifestu służy do uzyskiwania dostępu do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Dostawca złożony służy do zapewniania połączonego dostępu do plików i katalogów z jednego lub kilku innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider`używa typu [System. IO. File](/dotnet/api/system.io.file) (dla dostawcy fizycznego) i zakresy wszystkie ścieżki do katalogu i jego elementów podrzędnych. Takie Określanie zakresu uniemożliwia dostęp do systemu plików poza określonym katalogiem i jego elementami podrzędnymi. Podczas tworzenia wystąpienia tego dostawcy wymagana jest ścieżka katalogu i służy jako ścieżka podstawowa dla wszystkich żądań wysyłanych przy użyciu dostawcy. Można utworzyć wystąpienie `PhysicalFileProvider` dostawcy bezpośrednio lub można `IFileProvider` zażądać w konstruktorze poprzez iniekcję [zależności](xref:fundamentals/dependency-injection).

**Typy statyczne**

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

Przykładowa aplikacja tworzy dostawcę w `Startup.ConfigureServices` klasie aplikacji za pomocą [IHostingEnvironment. ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Uzyskiwanie typów dostawców plików z iniekcją zależności**

Wsuń dostawcę do dowolnego konstruktora klasy i przypisz go do pola lokalnego. Użyj pola w metodach klasy, aby uzyskać dostęp do plików.

W przykładowej aplikacji `IndexModel` Klasa `IFileProvider` otrzymuje wystąpienie, aby uzyskać zawartość katalogu dla ścieżki podstawowej aplikacji.

*Pages/index. cshtml. cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

Na `IDirectoryContents` stronie zostały powtórzone.

*Pages/index. cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) jest używany do uzyskiwania dostępu do plików osadzonych w zestawach. `ManifestEmbeddedFileProvider` Używa manifestu skompilowanego w zestawie, aby odtworzyć oryginalne ścieżki osadzonych plików.

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
| [ManifestEmbeddedFileProvider (zestaw, ciąg)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | Akceptuje opcjonalny `root` parametr ścieżki względnej. Określ wywołania zakresu [](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) doGetDirectoryContentsdotychzasobówwramachpodanej`root` ścieżki. |
| [ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | Akceptuje opcjonalny `root` parametr ścieżki względnej `lastModified` i parametr Date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)). Data Scopes Data ostatniej modyfikacji dla wystąpień [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) zwracanych przez IFileProvider. [](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) `lastModified` |
| [ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | Akceptuje opcjonalną `root` ścieżkę względną `lastModified` , datę i `manifestName` parametry. `manifestName` Reprezentuje nazwę zasobu osadzonego zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) łączy `IFileProvider` wystąpienia, ujawniając pojedynczy interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jedno `IFileProvider` wystąpienie do jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` a `ManifestEmbeddedFileProvider` i udostępniaj pliki `CompositeFileProvider` zarejestrowane w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Obejrzyj zmiany

Metoda [IFileProvider. Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) zawiera scenariusz, aby obejrzeć zmiany w jednym lub kilku plikach lub katalogach. `Watch`akceptuje ciąg ścieżki, który może używać [wzorców globalizowania](#glob-patterns) do określenia wielu plików. `Watch`zwraca [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). Token zmiany ujawnia:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Właściwość, którą można sprawdzić w celu ustalenia, czy wprowadzono zmianę.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Wywołuje się, gdy zostaną wykryte zmiany do określonego ciągu ścieżki. Każdy token zmiany wywołuje tylko skojarzone wywołanie zwrotne w odpowiedzi na pojedynczą zmianę. Aby włączyć monitorowanie stałe, użyj [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (pokazanego poniżej) lub Utwórz `IChangeToken` ponownie wystąpienia w odpowiedzi na zmiany.

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
