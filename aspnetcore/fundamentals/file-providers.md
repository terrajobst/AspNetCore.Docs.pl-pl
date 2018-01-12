---
title: "Plik dostawców w platformy ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak platformy ASP.NET Core abstracts dostępu do systemu plików przy użyciu dostawcy plików."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 1e35d362-0005-4f84-a187-274ca203a787
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: fd847db992b20ab096b54378418d2b9bccff67be
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="file-providers-in-aspnet-core"></a>Plik dostawców w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Platformy ASP.NET Core abstracts dostępu do systemu plików przy użyciu dostawcy plików.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Plik abstrakcje dostawcy

Dostawców w pliku są abstrakcję przez systemy plików. Interfejs głównego jest `IFileProvider`. `IFileProvider`udostępnia metody, aby uzyskać informacje o pliku (`IFileInfo`), informacji katalogowych (`IDirectoryContents`) i aby skonfigurować powiadomienia o zmianie (przy użyciu `IChangeToken`).

`IFileInfo`udostępnia metody i właściwości o poszczególnych plików lub katalogów. Ma dwie właściwości boolean, `Exists` i `IsDirectory`, a także właściwości, które opisują pliku `Name`, `Length` (w bajtach) i `LastModified` daty. Można odczytać z pliku przy użyciu jego `CreateReadStream` metody.

## <a name="file-provider-implementations"></a>Plik implementacji dostawcy

Trzy implementacje `IFileProvider` są dostępne: fizycznych, osadzone i złożone. Fizyczny dostawca jest używany do dostępu do rzeczywistego systemu plików. Osadzony dostawca jest używany do dostępu do plików osadzonych w zestawach. Złożone dostawcy służy do zapewnienia Scalonej dostępu do plików i katalogów z co najmniej jednego dostawcy.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` Zapewnia dostęp do fizycznego systemu plików. Jest zawijany `System.IO.File` typ (w przypadku fizycznego dostawcy), zakresu wszystkie ścieżki do katalogu i jego elementów podrzędnych. Ten zakres ogranicza dostęp do niektórych katalogu i jego elementów podrzędnych uniemożliwienia dostępu do systemu plików, poza tę granicę. Podczas tworzenia wystąpienia tego dostawcy, należy go podać ze ścieżką katalogu, który służy jako ścieżki bazowej dla wszystkich żądań wprowadzone do tego dostawcy (i który ogranicza dostęp spoza tej ścieżki). W aplikacji platformy ASP.NET Core, można utworzyć wystąpienia `PhysicalFileProvider` poprosić dostawcę bezpośrednio, lub `IFileProvider` w kontrolerze lub konstruktora usługi za pośrednictwem [iniekcji zależności](dependency-injection.md). Drugie podejście zwykle umożliwia uzyskanie bardziej elastyczne i testować rozwiązanie.

Poniższy przykład przedstawia sposób tworzenia `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Możesz iterację jego zawartość katalogu lub pobrać informacji o określonego pliku, podając ścieżkę podrzędną.

Aby poprosić dostawcę z kontrolera, określ go w Konstruktorze kontrolera i przypisz je do lokalnego pola. Należy użyć lokalnego wystąpienia z metody akcji:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Następnie utwórz w aplikacji dostawcy `Startup` klasy:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

W *Index.cshtml* wyświetlić, iterację `IDirectoryContents` podane:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Wynik:

![Plik dostawcy przykładowej aplikacji listy fizycznych plików i folderów](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` Umożliwia dostęp do plików osadzonych w zestawach. W .NET Core osadzanie plików w zestawie z `<EmbeddedResource>` element *.csproj* pliku:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Można użyć [wzorce globbing](#globbing-patterns) podczas określania plików do osadzenia w zestawie. Te wzorce może służyć do dopasowania jeden lub więcej plików.

> [!NOTE]
> Jest mało prawdopodobne, czy kiedykolwiek zajdzie potrzeba faktycznie osadzić każdego pliku .js w projekcie w jego zestaw; w powyższym przykładzie jest dla celów demonstracyjnych tylko.

Podczas tworzenia `EmbeddedFileProvider`, Przekaż zestawu będzie odczytywał dla jego konstruktora.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Fragment kodu powyżej przedstawia sposób tworzenia `EmbeddedFileProvider` z dostępem do obecnie wykonywany zestaw.

Aktualizowanie przykładową aplikację do używania `EmbeddedFileProvider` powoduje następujące dane wyjściowe:

![Plik dostawcy przykładowej aplikacji wyświetlanie listy plików osadzonych](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Zasobów osadzonych nie ujawniaj katalogów. Zamiast ścieżki do zasobu (za pośrednictwem jego przestrzeni nazw) jest osadzony w jego przy użyciu nazwy pliku `.` separatorów.

> [!TIP]
> `EmbeddedFileProvider` Konstruktor akceptuje opcjonalny `baseNamespace` parametru. Określenie tej będzie zakresu wywołań `GetDirectoryContents` do tych zasobów w ramach podanego obszaru nazw.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` Łączy `IFileProvider` wystąpień udostępnia jeden interfejs do pracy z plikami z wielu dostawców. Podczas tworzenia `CompositeFileProvider`, należy przekazać co najmniej jeden `IFileProvider` wystąpień dla jego konstruktora:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Aktualizowanie przykładową aplikację do używania `CompositeFileProvider` który obejmuje zarówno fizyczne i osadzone dostawców został wcześniej skonfigurowany, wynikiem następujące dane wyjściowe:

![Plik dostawcy przykładowej aplikacji wyświetlanie listy plików fizycznych i folderów i plików osadzonych](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Monitorowanie zmian

`IFileProvider` `Watch` Metody umożliwia Obejrzyj pliki lub katalogi zmian. Ta metoda przyjmuje ciąg ścieżki, której można użyć [wzorce globbing](#globbing-patterns) do określenia wielu plików i zwraca `IChangeToken`. Token ten przedstawia `HasChanged` właściwości, które mogą być kontrolowane i `RegisterChangeCallback` metodę, która jest wywoływana, gdy zostaną wykryte zmiany w ciągu określonej ścieżki. Należy pamiętać, że każdy token zmiany tylko wywołuje wywołania zwrotnego jego skojarzonego w odpowiedzi na pojedynczej zmiany. Aby włączyć monitorowanie stałą, można użyć `TaskCompletionSource` w sposób przedstawiony poniżej, lub Utwórz ponownie `IChangeToken` wystąpień w odpowiedzi na zmiany.

W tym artykule przykładowym aplikacji konsoli jest skonfigurowany do wyświetla komunikat, gdy zostanie zmodyfikowany plik tekstowy:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Wynik po zapisaniu pliku kilka razy:

![Okno polecenia po wykonywania dotnet Uruchom przedstawia zmiany w pliku o quotes.txt monitorowania aplikacji i plik został zmieniony pięć razy.](file-providers/_static/watch-console.png)

> [!NOTE]
> Niektóre systemy plików, takich jak kontenery Docker i udziały sieciowe i niezawodnie nie może wysłać powiadomienia o zmianie. Ustaw `DOTNET_USE_POLLINGFILEWATCHER` zmienną środowiskową `1` lub `true` do sondowania zmian systemu plików, co 4 sekundy.

## <a name="globbing-patterns"></a>Wzorce globbing

Ścieżki systemu plików, użyj wzorców symboli wieloznacznych o nazwie *wzorce globbing*. Te proste wzorce, można określić grupy plików. Są dwa znaki symboli wieloznacznych `*` i `**`.

**`*`**

   Pasuje do wszystkiego na bieżącym poziomie folderu lub nazwy pliku lub dowolnym rozszerzeniem pliku. Zakończono dopasowań w `/` i `.` znaki w ścieżce.

<strong><code>**</code></strong>

   Pasuje do wszystkiego na wielu poziomach katalogu. Może służyć do rekursywnie pasuje wiele plików w ramach hierarchii katalogów.

### <a name="globbing-pattern-examples"></a>Przykłady wzorzec globbing

**`directory/file.txt`**

   Pasuje do określonego pliku z określonego katalogu.

**<code>directory/*.txt</code>**

   Dopasowanie wszystkich plików z `.txt` rozszerzenia z określonego katalogu.

**`directory/*/bower.json`**

   Pasuje do wszystkich `bower.json` plików w katalogach dokładnie jeden poziom poniżej `directory` katalogu.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Dopasowanie wszystkich plików z `.txt` rozszerzenia można znaleźć w dowolnym miejscu w `directory` katalogu.

## <a name="file-provider-usage-in-aspnet-core"></a>Użycie dostawcy File w ASP.NET Core

Kilka części platformy ASP.NET Core korzystać z dostawcy plików. `IHostingEnvironment`przedstawia zawartość katalogu głównego aplikacji i głównego sieci web jako `IFileProvider` typów. Oprogramowanie pośredniczące plików statycznych korzysta z dostawców plików do lokalizacji plików statycznych. Razor sprawia, że intensywnie korzysta z `IFileProvider` lokalizowanie widoków. Dla platformy DotNet publikowania funkcji używa pliku dostawców i wzorce globbing, aby określić pliki, które powinny zostać opublikowane.

## <a name="recommendations-for-use-in-apps"></a>Zalecenia dotyczące użycia w aplikacji

Jeśli aplikacja platformy ASP.NET Core wymaga dostępu do systemu plików, możesz poprosić wystąpienia `IFileProvider` za pomocą iniekcji zależności, a następnie za pomocą jej metod dostęp, jak pokazano w przykładzie. Dzięki temu można skonfigurować dostawcę raz, podczas uruchamiania aplikacji i zmniejsza liczbę typów wdrożenia, które tworzy aplikację.
