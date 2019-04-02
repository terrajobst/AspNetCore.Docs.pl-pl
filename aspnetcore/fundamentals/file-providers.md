---
title: Dostawcy plików w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 2ce40ea0d576d08a6b42c3eb6693754f2a0bddce
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809224"
---
# <a name="file-providers-in-aspnet-core"></a>Dostawcy plików w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

Platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików. Dostawcy plików są używane w ramach platformy ASP.NET Core:

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) udostępnia zawartość katalogu głównego aplikacji i katalog główny sieci web jako `IFileProvider` typów.
* [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) używa dostawcy plików do zlokalizowania plików statycznych.
* [Razor](xref:mvc/views/razor) używa dostawcy plików do zlokalizowania, widoków i stron.
* Zestaw narzędzi .NET core korzysta z dostawcy plików i glob wzorce, aby określić, które pliki powinny być publikowane.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfejsy dostawcy plików

Podstawowy interfejs jest [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider` udostępnia metody do:

* Uzyskaj informacje o pliku ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Uzyskaj informacje o katalogu ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Konfigurowanie powiadomień o zmianie (przy użyciu [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo` udostępnia metody i właściwości do pracy z plikami:

* [Istnieje](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Nazwa](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Długość](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (w bajtach)
* [Ostatnia modyfikacja](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) daty

Można czytać z pliku przy użyciu [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) metody.

Przykładowa aplikacja pokazuje, jak skonfigurować dostawcę pliku w programie `Startup.ConfigureServices` używanych w całej aplikacji za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Plik implementacji dostawcy

Trzy implementacje `IFileProvider` są dostępne.

| Implementacja | Opis |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Dostawca fizycznych umożliwia dostęp do plików fizycznych w systemie. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Manifestu osadzonego dostawcy umożliwia dostęp do plików osadzonych w zestawach. |
| [CompositeFileProvider](#compositefileprovider) | Złożony dostawca jest używany do udostępnienia połączone pliki i katalogi z jednego lub więcej innych dostawców. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) zapewnia dostęp do fizycznego systemu plików. `PhysicalFileProvider` używa [System.IO.File](/dotnet/api/system.io.file) typu (w przypadku fizycznego provider) i zakresów wszystkie ścieżki do katalogu i jego elementów podrzędnych. Tego zakresu, które uniemożliwia dostęp do systemu plików spoza określonego katalogu i jego elementów podrzędnych. Podczas tworzenia wystąpienia tego dostawcy, ścieżka katalogu jest wymagana i stanowi podstawową ścieżkę dla wszystkich żądań przy użyciu dostawcy. Można utworzyć wystąpienie `PhysicalFileProvider` poprosić dostawcę bezpośrednio lub `IFileProvider` w Konstruktorze za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

**Typy statyczne**

Poniższy kod przedstawia sposób tworzenia `PhysicalFileProvider` i użyć go do uzyskania zawartości katalogu i informacje o pliku:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Typy w poprzednim przykładzie:

* `provider` jest `IFileProvider`.
* `contents` jest `IDirectoryContents`.
* `fileInfo` jest `IFileInfo`.

Dostawcy plików może służyć do iterowania po katalogu określonego przez `applicationRoot` lub zadzwoń `GetFileInfo` można uzyskać informacji o pliku. Dostawcy plików nie ma dostępu poza `applicationRoot` katalogu.

Przykładowa aplikacja tworzy dostawcę aplikacji `Startup.ConfigureServices` przy użyciu [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Uzyskaj typy dostawcy plików za pomocą iniekcji zależności**

Wstrzyknięcie dostawcy do dowolnego konstruktora klasy i przypisz go do lokalnego pola. Dostęp do plików, należy użyć pola w całej metody tej klasy.

W przykładowej aplikacji `IndexModel` klasa otrzymuje `IFileProvider` wystąpienia, aby uzyskać zawartość katalogu dla ścieżki podstawowej aplikacji.

*Pages/Index.cshtml.cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

`IDirectoryContents` Są postanowiliśmy na stronie.

*Pages/Index.cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) umożliwia dostęp do plików osadzonych w ramach zestawów. `ManifestEmbeddedFileProvider` Używa manifestu kompilowane do zestawu odtworzenie oryginalnej ścieżki plików osadzonych.

Aby wygenerować manifest osadzonych plików, należy ustawić `<GenerateEmbeddedFilesManifest>` właściwość `true`. Określ pliki, aby osadzić przy użyciu [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Użyj [wzorców glob](#glob-patterns) do określenia co najmniej jeden plik do osadzenia w zestawie.

Przykładowa aplikacja tworzy `ManifestEmbeddedFileProvider` i przekazuje zawierający obecnie wykonywany zestaw do jej konstruktora.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Dodatkowe przeciążenia umożliwiają:

* Określ względną ścieżkę.
* Pliki zakresu daty ostatniej modyfikacji.
* Nazwa osadzonego zasobu zawierającego manifestu osadzonego pliku.

| przeciążenie | Opis |
| -------- | ----------- |
| [ManifestEmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | Akceptuje opcjonalny `root` parametr ścieżki względnej. Określ `root` do zakresu wywołań [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) do tych zasobów w podanej ścieżce. |
| [ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | Akceptuje opcjonalny `root` parametr ścieżki względnej i `lastModified` daty ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametr. `lastModified` Data zakresów daty ostatniej modyfikacji [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) wystąpień zwrócona przez [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). |
| [ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | Akceptuje opcjonalny `root` ścieżki względnej, `lastModified` datę i `manifestName` parametrów. `manifestName` Reprezentuje nazwę osadzonego zasobu zawierającego manifest. |

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) łączy `IFileProvider` wystąpień, udostępnia jeden interfejs do pracy z plikami od wielu dostawców. Podczas tworzenia `CompositeFileProvider`, — dostęp próbny, co najmniej jeden `IFileProvider` wystąpień dla jego konstruktora.

W przykładowej aplikacji `PhysicalFileProvider` i `ManifestEmbeddedFileProvider` udostępnienia plików do `CompositeFileProvider` zarejestrowany w kontenerze usługi aplikacji:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Śledzenie zmian

[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda zapewnia scenariusz, aby obejrzeć plików lub katalogów do zmiany. `Watch` akceptuje ciąg ścieżki, która może używać [wzorców glob](#glob-patterns) do określenia wielu plików. `Watch` Zwraca [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). Udostępnia tokenu zmiany:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Właściwość, która można przeprowadzić inspekcji, aby określić, jeśli nastąpiła zmiana.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Wywołuje się, gdy zostaną wykryte zmiany w ciągu określonej ścieżki. Każdy token zmiany tylko wywołuje jego wywołania zwrotnego skojarzonego w odpowiedzi na pojedynczej zmiany. Aby włączyć monitorowanie stałe, należy użyć [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (pokazana poniżej) lub Utwórz ponownie `IChangeToken` wystąpień w odpowiedzi na zmiany.

W przykładowej aplikacji *WatchConsole* konsoli aplikacja jest skonfigurowana do wyświetlania komunikatu przy każdej modyfikacji pliku tekstowego:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Niektóre systemy plików, takich jak kontenery platformy Docker i udziały sieciowe i niezawodnie nie może wysłać powiadomienia o zmianach. Ustaw `DOTNET_USE_POLLING_FILE_WATCHER` zmiennej środowiskowej, aby `1` lub `true` sondowanie systemu plików w celu zmiany co cztery sekundy (nie można konfigurować).

## <a name="glob-patterns"></a>Glob wzorców

Ścieżki systemu plików, użyj wzorców symboli wieloznacznych, o nazwie *wzorców glob (lub symboli wieloznacznych)*. Określ grupy plików z tych wzorców. Są dwa znaki symboli wieloznacznych `*` i `**`:

**`*`**  
Pasuje do wszystkiego na bieżącym poziomie folderu, nazwy pliku lub dowolnego rozszerzenia pliku. Dopasowania są kończone przez `/` i `.` znaki w ścieżce pliku.

**`**`**  
Pasuje do wszystkiego na wielu poziomach katalogu. Może służyć do rekursywnie dopasować wiele plików w ramach hierarchii katalogów.

**Glob wzorzec przykłady**

**`directory/file.txt`**  
Pasuje do określonego pliku w określonym katalogu.

**`directory/*.txt`**  
Uwzględnia wszystkie pliki z *.txt* rozszerzenie w określonym katalogu.

**`directory/*/appsettings.json`**  
Pasuje do wszystkich `appsettings.json` plików w katalogach dokładnie jeden poziom poniżej *katalogu* folderu.

**`directory/**/*.txt`**  
Uwzględnia wszystkie pliki z *.txt* znaleźć rozszerzenia w dowolnym miejscu w ramach *katalogu* folderu.
