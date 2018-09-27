---
title: LibMan interfejsu wiersza polecenia (CLI) za pomocą platformy ASP.NET Core
author: scottaddie
description: Dowiedz się, jak używać interfejsu wiersza polecenia (CLI) LibMan w projektach programu ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402088"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>LibMan interfejsu wiersza polecenia (CLI) za pomocą platformy ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) interfejs wiersza polecenia to narzędzie dla wielu platform, które ma obsługiwane wszędzie platformy .NET Core jest obsługiwane.

## <a name="prerequisites"></a>Wymagania wstępne

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Instalacja

Aby zainstalować interfejs wiersza polecenia LibMan:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

A [narzędzia programu .NET Core globalnego](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowany z [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) pakietu NuGet.

Aby zainstalować interfejs wiersza polecenia LibMan z określonego źródła pakietu NuGet:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

W powyższym przykładzie narzędzie globalne Core .NET jest zainstalowany na lokalnym komputerze Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* pliku.

## <a name="usage"></a>Użycie

Po pomyślnej instalacji interfejsu wiersza polecenia służy następujące polecenie:

```console
libman
```

Aby wyświetlić zainstalowanej wersji interfejsu wiersza polecenia:

```console
libman --version
```

Aby wyświetlić dostępne polecenia interfejsu wiersza polecenia:

```console
libman --help
```

Poprzednie polecenie wyświetli dane wyjściowe podobne do następujących:

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

W poniższych sekcjach opisano dostępne polecenia interfejsu wiersza polecenia.

## <a name="initialize-libman-in-the-project"></a>Inicjowanie LibMan w projekcie

`libman init` Polecenie tworzy *libman.json* pliku, jeśli nie istnieje. Plik jest tworzony przy użyciu domyślnej zawartości szablonu elementu.

### <a name="synopsis"></a>Streszczenie

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman init` polecenia:

* `-d|--default-destination <PATH>`

  Ścieżka względną do bieżącego folderu. Pliki bibliotek są instalowane w tej lokalizacji, jeśli nie `destination` właściwość jest zdefiniowana dla biblioteki w *libman.json*. `<PATH>` Wartości są zapisywane w `defaultDestination` właściwość *libman.json*.

* `-p|--default-provider <PROVIDER>`

  Dostawcę, który ma być używany, jeśli nie zdefiniowano żadnego dostawcy dla danej biblioteki. `<PROVIDER>` Wartości są zapisywane w `defaultProvider` właściwość *libman.json*. Zastąp `<PROVIDER>` przy użyciu jednego z następujących wartości:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

Aby utworzyć *libman.json* pliku w projektach programu ASP.NET Core:

* Przejdź do katalogu głównego projektu.
* Uruchom następujące polecenie:

  ```console
  libman init
  ```

* Wpisz nazwę domyślnego dostawcę lub naciśnij klawisz `Enter` używać domyślnego dostawcę CDNJS. Prawidłowe wartości to:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![polecenie init libman — domyślny dostawca](_static/libman-init-provider.png)

A *libman.json* plik zostanie dodany do katalogu głównego projektu o następującej zawartości:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Dodaj pliki bibliotek

`libman install` Polecenie pobiera i instaluje pliki biblioteki do projektu. A *libman.json* plik zostanie dodany, jeśli nie istnieje. *Libman.json* plik zostanie zmodyfikowany na potrzeby przechowywania szczegóły konfiguracji dla plików biblioteki.

### <a name="synopsis"></a>Streszczenie

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Argumenty

`LIBRARY`

Nazwa biblioteki do zainstalowania. Ta nazwa może zawierać notacji numeru wersji (na przykład `@1.2.0`).

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman install` polecenia:

* `-d|--destination <PATH>`

  Lokalizacja, aby zainstalować bibliotekę. Jeśli nie zostanie określony, domyślna lokalizacja jest używana. Jeśli nie `defaultDestination` właściwość jest określona w *libman.json*, ta opcja jest wymagana.

* `--files <FILE>`

  Określ nazwę pliku do zainstalowania z biblioteki. Jeśli nie zostanie określony, wszystkie pliki z biblioteki są instalowane. Podaj jeden `--files` opcji na plik do zainstalowania. Obsługiwane są zbyt ścieżek względnych. Na przykład: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Nazwa dostawcy na potrzeby przejęcia biblioteki. Zastąp `<PROVIDER>` przy użyciu jednego z następujących wartości:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Jeśli nie zostanie określony, `defaultProvider` właściwość *libman.json* jest używany. Jeśli nie `defaultProvider` właściwość jest określona w *libman.json*, ta opcja jest wymagana.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

Należy wziąć pod uwagę następujące *libman.json* pliku:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Aby zainstalować wersję jQuery 3.2.1 *jquery.min.js* plik *skryptów/wwwroot/jquery* folder przy użyciu dostawcy CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

*Libman.json* pliku jest podobny do następującego:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

Aby zainstalować *calendar.js* i *calendar.css* plików ze *C:\\temp\\contosoCalendar\\*  przy użyciu systemu plików Dostawca:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Następujący monit pojawia się z dwóch przyczyn:

* *Libman.json* plik nie zawiera `defaultDestination` właściwości.
* `libman install` Nie zawiera polecenia `-d|--destination` opcji.

![polecenie - instalacji libman docelowego](_static/libman-install-destination.png)

Po zaakceptowaniu domyślnej lokalizacji docelowej *libman.json* pliku jest podobny do następującego:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>Przywróć pliki bibliotek

`libman restore` Polecenie powoduje zainstalowanie plików biblioteki zdefiniowanych w *libman.json*. Mają zastosowanie następujące zasady:

* Jeśli nie *libman.json* plik istnieje w katalogu głównym projektu, zostanie zwrócony błąd.
* Jeśli biblioteka określa dostawcę, `defaultProvider` właściwość *libman.json* jest ignorowana.
* Jeśli biblioteka określa miejsce docelowe `defaultDestination` właściwość *libman.json* jest ignorowana.

### <a name="synopsis"></a>Streszczenie

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman restore` polecenia:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

Do przywrócenia plików biblioteki zdefiniowanych w *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Usuń pliki bibliotek

`libman clean` Polecenie usuwa wcześniej przywrócić za pośrednictwem LibMan pliki biblioteki. Foldery, które stać się puste po wykonaniu tej czynności zostaną usunięte. Plików bibliotek skojarzone konfiguracje w `libraries` właściwość *libman.json* nie zostaną usunięte.

### <a name="synopsis"></a>Streszczenie

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman clean` polecenia:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

Aby usunąć pliki biblioteki zainstalowanych za pomocą LibMan:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Odinstaluj pliki biblioteki

`libman uninstall` Polecenia:

* Usuwa wszystkie pliki skojarzone z określonej biblioteki z miejsca docelowego w *libman.json*.
* Usuwa konfigurację biblioteki skojarzony z *libman.json*.

Błąd występuje, gdy:

* Nie *libman.json* plik istnieje w katalogu głównym projektu.
* Określonej biblioteki nie istnieje.

Jeśli jest zainstalowana więcej niż jedna biblioteka o tej samej nazwie, zostanie wyświetlony monit wybierz jedną.

### <a name="synopsis"></a>Streszczenie

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Argumenty

`LIBRARY`

Nazwa biblioteki do odinstalowania. Ta nazwa może zawierać notacji numeru wersji (na przykład `@1.2.0`).

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman uninstall` polecenia:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

Należy wziąć pod uwagę następujące *libman.json* pliku:

[!code-json[](samples/LibManSample/libman.json)]

* Aby odinstalować jQuery, powiedzie się jedną z następujących poleceń:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Aby odinstalować plików Lodash zainstalowanych za pomocą `filesystem` dostawcy:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Zaktualizuj wersję biblioteki

`libman update` Polecenie aktualizuje bibliotekę zainstalowanych za pomocą LibMan do wybranej wersji.

Błąd występuje, gdy:

* Nie *libman.json* plik istnieje w katalogu głównym projektu.
* Określonej biblioteki nie istnieje.

Jeśli jest zainstalowana więcej niż jedna biblioteka o tej samej nazwie, zostanie wyświetlony monit wybierz jedną.

### <a name="synopsis"></a>Streszczenie

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Argumenty

`LIBRARY`

Nazwa biblioteki, aby zaktualizować.

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman update` polecenia:

* `-pre`

  Uzyskaj najnowszą wersję wstępną wersję biblioteki.

* `--to <VERSION>`

  Uzyskać określoną wersję biblioteki.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

* Aby zaktualizować jQuery do najnowszej wersji:

  ```console
  libman update jquery
  ```

* Aby zaktualizować jQuery do wersji 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* Aby zaktualizować jQuery do najnowszej wersji wstępnej:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Zarządzaj pamięcią podręczną biblioteki

`libman cache` Polecenia zarządza w pamięci podręcznej biblioteki LibMan. `filesystem` Dostawcy nie korzysta z pamięci podręcznej biblioteki.

### <a name="synopsis"></a>Streszczenie

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Argumenty

`PROVIDER`

Używana tylko z `clean` polecenia. Określa pamięci podręcznej dostawcy, aby wyczyścić. Prawidłowe wartości to:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Opcje

Poniższe opcje są dostępne dla `libman cache` polecenia:

* `--files`

  Lista nazw plików, które są buforowane.

* `--libraries`

  Wyświetl listę nazw bibliotek, które są buforowane.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Przykłady

* Aby wyświetlić nazwy pamięci podręcznej bibliotek na dostawcy, użyj jednej z następujących poleceń:

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Wyświetlone dane wyjściowe podobne do następujących:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* Aby wyświetlić nazwy plików w pamięci podręcznej biblioteki na dostawcy:

  ```console
  libman cache list --files
  ```

  Wyświetlone dane wyjściowe podobne do następujących:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  Powiadomienia poprzedzającego dane wyjściowe pokazuje tej wersji 3.2.1 i 3.3.1 są buforowane w ramach dostawcy CDNJS jQuery.

* Aby opróżnić pamięć podręczną biblioteki dostawcy CDNJS:

  ```console
  libman cache clean cdnjs
  ```

  Po opróżnienia pamięci podręcznej dostawcy CDNJS `libman cache list` polecenie wyświetla następujące czynności:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* Opróżnienia pamięci podręcznej dla wszystkich obsługiwanych dostawców:

  ```console
  libman cache clean
  ```

  Po opróżnieniu wszystkich dostawcy pamięci podręcznych, `libman cache list` polecenie wyświetla następujące czynności:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zainstaluj narzędzie globalne](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Repozytorium LibMan GitHub](https://github.com/aspnet/LibraryManager)
