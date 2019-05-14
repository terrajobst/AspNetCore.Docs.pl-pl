---
title: LibMan za pomocą platformy ASP.NET Core w programie Visual Studio
author: scottaddie
description: Dowiedz się, jak używać LibMan w projektach programu ASP.NET Core z programem Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: d0494b96447ad2b256acbde7748209ff430ae7c1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610187"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>LibMan za pomocą platformy ASP.NET Core w programie Visual Studio

Przez [Scott Addie](https://twitter.com/Scott_Addie)

Program Visual Studio ma wbudowaną obsługę [LibMan](xref:client-side/libman/index) w projektach ASP.NET Core, w tym:

* Obsługa konfigurowania i uruchamiania operacji przywracania LibMan podczas kompilacji.
* Elementy menu do wyzwalania przywracania LibMan i wyczyść operacji.
* Okno dialogowe Wyszukiwanie do znajdowania bibliotek i dodawanie plików do projektu.
* Edycję *libman.json*&mdash;LibMan pliku manifestu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia

## <a name="add-library-files"></a>Dodaj pliki bibliotek

Pliki biblioteki mogą być dodawane do projektu programu ASP.NET Core na dwa sposoby:

1. [Użyj okna dialogowego Dodawanie biblioteki po stronie klienta](#use-the-add-client-side-library-dialog)
1. [Ręczne konfigurowanie LibMan we wpisach w plikach manifestu](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>Użyj okna dialogowego Dodawanie biblioteki po stronie klienta

Wykonaj następujące kroki, aby zainstalować bibliotekę klienta:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder projektu, w którym można dodać plików. Wybierz **Dodaj** > **biblioteki po stronie klienta**. **Dodaj biblioteki po stronie klienta** zostanie wyświetlone okno dialogowe:

  ![Dodaj okno dialogowe biblioteki po stronie klienta](_static/add-library-dialog.png)

* Wybierz dostawcę biblioteki z **dostawcy** listy rozwijanej. CDNJS jest dostawcą domyślnym.
* Wpisz nazwę biblioteki, aby pobrać w **biblioteki** pola tekstowego. Technologia IntelliSense zawiera listę bibliotek, począwszy od podany tekst.
* Wybierz bibliotekę z listy technologii IntelliSense. Zwróć uwagę, jest sufiks nazwy biblioteki `@` symboli i najnowsza stabilna wersja znane z wybranym dostawcą.
* Zdecyduj, które pliki do uwzględnienia:
  * Wybierz **Dołącz wszystkie pliki biblioteki** przycisk radiowy, aby uwzględnić wszystkie pliki biblioteki.
  * Wybierz **wybierz konkretne pliki** przycisk radiowy, aby uwzględnić podzbiór plików biblioteki programu. Po wybraniu przycisku radiowego drzewo selektora plików jest włączona. Zaznacz pola wyboru po lewej stronie nazwy plików do pobrania.
* Określ folder projektu, do przechowywania plików w **lokalizacji docelowej** pola tekstowego. Jako zalecenie magazyn każdej biblioteki w oddzielnym folderze.

  Sugerowany **lokalizacji docelowej** folderu opiera się na lokalizację, z którego uruchomiono okno dialogowe:

  * Jeśli uruchomionego z katalogu głównego projektu:
    * *Wwwroot/lib* jest używany, gdy *wwwroot* istnieje.
    * *lib* jest używany, gdy *wwwroot* nie istnieje.
  * Jeśli uruchomionego z folderu projektu, nazwę odpowiedniego folderu jest używany.

  Sugestia folderu jest sufiks nazwy biblioteki. W poniższej tabeli przedstawiono sugestie folderu podczas instalowania jQuery w projekcie stron Razor.
  
  |Uruchom lokalizacji                           |Sugerowane folderu      |
  |------------------------------------------|----------------------|
  |katalog główny projektu (Jeśli *wwwroot* istnieje)        |*wwwroot/lib/jquery/* |
  |katalog główny projektu (Jeśli *wwwroot* nie istnieje) |*lib/jquery/*         |
  |*Strony* folderu w projekcie                 |*Pages/jquery/*       |

* Kliknij przycisk **zainstalować** przycisk, aby pobrać pliki, na konfiguracji w *libman.json*.
* Przegląd **Library Manager** kanału **dane wyjściowe** okna, aby uzyskać szczegółowe informacje dotyczące instalacji. Na przykład:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>Ręczne konfigurowanie LibMan we wpisach w plikach manifestu

Wszystkie operacje LibMan w programie Visual Studio są oparte na zawartości manifestu LibMan katalog główny projektu (*libman.json*). Możesz ręcznie edytować *libman.json* konfigurowania plików biblioteki dla projektu. Visual Studio, przywraca raz wszystkie pliki biblioteki *libman.json* jest zapisywany.

Aby otworzyć *libman.json* do edycji, istnieją następujące opcje:

* Kliknij dwukrotnie *libman.json* w pliku **Eksploratora rozwiązań**.
* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **zarządzanie bibliotekami po stronie klienta**. **&#8224;**
* Wybierz **zarządzanie bibliotekami po stronie klienta** z programu Visual Studio **projektu** menu. **&#8224;**

**&#8224;** Jeśli *libman.json* plik jeszcze nie istnieje w katalogu głównym projektu, zostanie on utworzony za pomocą szablonu elementu domyślnej zawartości.

Visual Studio oferuje bogate JSON edycję pomocy technicznej, takie jak kolorowanie, formatowanie, IntelliSense i sprawdzanie poprawności schematu. Schemat JSON manifestu LibMan znajduje się na [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).

Za pomocą następującego pliku manifestu, LibMan pobiera pliki na konfiguracją zdefiniowaną w `libraries` właściwości. Wyjaśnienie literałów obiektu zdefiniowany w ramach `libraries` poniżej:

* Podzbiór [jQuery](https://jquery.com/) w wersji 3.3.1 jest pobierana z CDNJS dostawcy. Podzbiór jest zdefiniowany w `files` właściwość&mdash;*jquery.min.js*, *jquery.js*, i *jquery.min.map*. Pliki są umieszczane w projekcie *wwwroot/lib/jquery* folderu.
* Całość [Bootstrap](https://getbootstrap.com/) wersji 4.1.3 jest pobierana i umieszczana w *wwwroot/lib/bootstrap* folderu. Literał obiektu `provider` zastąpienia właściwości `defaultProvider` wartości właściwości. LibMan pobiera ładowania plików od dostawcy unpkg.
* Podzbiór [Lodash](https://lodash.com/) została zatwierdzona przez jednostkę zarządzającej w organizacji. *Lodash.js* i *lodash.min.js* pliki są pobierane z lokalnego systemu plików w *C:\\temp\\lodash\\*. Pliki są kopiowane w projekcie *wwwroot/lib/lodash* folderu.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan obsługuje tylko jedną wersję każdego biblioteki z każdego dostawcy. *Libman.json* pliku zakończy się niepowodzeniem podczas sprawdzania poprawności schematu zawiera dwie biblioteki o takiej samej nazwie biblioteki dla danego dostawcy.

## <a name="restore-library-files"></a>Przywróć pliki bibliotek

Aby przywrócić pliki biblioteki z poziomu programu Visual Studio, musi być prawidłowym *libman.json* plik w folderze głównym projektu. Przywróconych plików są umieszczane w projekcie w lokalizacji określonej dla każdej biblioteki.

Pliki biblioteki mogą zostać przywrócone w projektach programu ASP.NET Core na dwa sposoby:

1. [Przywracanie plików podczas kompilacji](#restore-files-during-build)
1. [Przywróć pliki ręcznie](#restore-files-manually)

### <a name="restore-files-during-build"></a>Przywracanie plików podczas kompilacji

LibMan można przywrócić pliki biblioteki zdefiniowane jako część procesu kompilacji. Domyślnie *przywracania na kompilacji* zachowania jest wyłączone.

Aby włączyć i testowanie zachowania przywracania w kompilacji:

* Kliknij prawym przyciskiem myszy *libman.json* w **Eksploratora rozwiązań** i wybierz **włączyć Przywracanie biblioteki po stronie klienta, podczas kompilacji** z menu kontekstowego.
* Kliknij przycisk **tak** przycisku po wyświetleniu monitu można zainstalować pakietu NuGet. [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) pakiet NuGet zostanie dodany do projektu:

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* Skompiluj projekt, aby potwierdzić, że występuje LibMan Przywracanie plików. `Microsoft.Web.LibraryManager.Build` Pakietu wprowadza obiekt docelowy programu MSBuild, działającego LibMan podczas operacji tworzenia projektu.
* Przegląd **kompilacji** kanału **dane wyjściowe** okno Dziennik aktywności LibMan:

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

Po włączeniu zachowanie przywracania na kompilacji *libman.json* menu kontekstowe wyświetla **Wyłącz Przywracanie biblioteki po stronie klienta, podczas kompilacji** opcji. Wybranie tej opcji spowoduje usunięcie `Microsoft.Web.LibraryManager.Build` pakietu odwołania z pliku projektu. W związku z tym biblioteki po stronie klienta nie są już przywracane na każdej kompilacji.

Bez względu na ustawienie przywracania w kompilacji, można przywrócić ręcznie w dowolnym momencie z *libman.json* menu kontekstowego. Aby uzyskać więcej informacji, zobacz [ręcznie przywrócić pliki](#restore-files-manually).

### <a name="restore-files-manually"></a>Przywróć pliki ręcznie

Aby ręcznie przywrócić pliki biblioteki:

* Dla wszystkich projektów w rozwiązaniu:
  * Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań**.
  * Wybierz **przywrócenia biblioteki po stronie klienta** opcji.
* Dla określonego projektu:
  * Kliknij prawym przyciskiem myszy *libman.json* w pliku **Eksploratora rozwiązań**.
  * Wybierz **przywrócenia biblioteki po stronie klienta** opcji.

Operacja przywracania jest uruchomiona:

* Ikona Centrum stanu zadań (TSC) na pasku stanu programu Visual Studio będzie animowana i odczyta *uruchomiona operacja przywracania*. Klikając ikonę Otwiera listę zadań w tle znanych etykietka narzędzia.
* Wiadomości zostaną wysłane do paska stanu i **Library Manager** kanału **dane wyjściowe** okna. Na przykład:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>Usuń pliki bibliotek

Aby wykonać *czyste* operacji, który usuwa pliki biblioteki wcześniej przywrócona w programie Visual Studio:

* Kliknij prawym przyciskiem myszy *libman.json* w pliku **Eksploratora rozwiązań**.
* Wybierz **czystej biblioteki po stronie klienta** opcji.

Aby zapobiec przypadkowemu usunięciu plików nienależącym do biblioteki, operacji czyszczenia nie powoduje usunięcia całej katalogów. Powoduje usunięcie tylko pliki, które występowały w poprzednich przywracania.

Po uruchomieniu operacji czyszczenia:

* Ikona TSC na pasku stanu programu Visual Studio będzie animowana i odczyta *rozpoczęcia operacji biblioteki klienta*. Klikając ikonę Otwiera listę zadań w tle znanych etykietka narzędzia.
* Komunikaty są wysyłane do paska stanu i **Library Manager** kanału **dane wyjściowe** okna. Na przykład:

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

Operacji czyszczenia usuwa tylko pliki z projektu. Pliki biblioteki pozostać w pamięci podręcznej szybsze pobieranie na operacje przywracania w przyszłości. Aby zarządzać plikami biblioteki przechowywanych w pamięci podręcznej na komputerze lokalnym, użyj [LibMan interfejsu wiersza polecenia](xref:client-side/libman/libman-cli).

## <a name="uninstall-library-files"></a>Odinstaluj pliki biblioteki

Aby odinstalować plików biblioteki:

* Otwórz *libman.json*.
* Położenie karetki wewnątrz odpowiadającego `libraries` literału obiektu.
* Kliknij ikonę żarówki, która pojawia się na lewym marginesie, a następnie wybierz pozycję **Odinstaluj \<library_name > @\<library_version >**:

  ![Odinstaluj menu kontekstowego biblioteki](_static/uninstall-menu-option.png)

Można też ręcznie edytować i zapisać manifestu LibMan (*libman.json*). [Operacja przywracania](#restore-library-files) jest uruchamiany, gdy plik jest zapisywany. Pliki bibliotek, które są już zdefiniowane w *libman.json* zostaną usunięte z projektu.

## <a name="update-library-version"></a>Zaktualizuj wersję biblioteki

Aby sprawdzić wersję zaktualizowanej biblioteki:

* Otwórz *libman.json*.
* Położenie karetki wewnątrz odpowiadającego `libraries` literału obiektu.
* Kliknij ikonę żarówki, która pojawia się na lewym marginesie. Umieść kursor nad **sprawdzać dostępność aktualizacji**.

LibMan sprawdza, czy biblioteka nowsza wersja niż zainstalowana wersja. Mogą wystąpić następujące wyniki:

* A **nie odnaleziono żadnych aktualizacji** zostanie wyświetlony komunikat, jeśli jest już zainstalowana najnowsza wersja.
* Najnowsza stabilna wersja jest wyświetlana, jeśli nie jeszcze zainstalowany.

  ![Sprawdź, czy są aktualizacje menu kontekstowego](_static/update-menu-option.png)

* Jeśli wstępna jest nowsza niż zainstalowana wersja jest dostępna, jest wyświetlany w wersji wstępnej.

Zmiany na starszą starszej wersji biblioteki, należy ręcznie zmodyfikować *libman.json* pliku. Gdy plik jest zapisywany, LibMan [operacja przywracania](#restore-library-files):

* Usuwa zbędne pliki z poprzedniej wersji.
* Dodaje nowe i zaktualizowane pliki z nowej wersji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:client-side/libman/libman-cli>
* [Repozytorium LibMan GitHub](https://github.com/aspnet/LibraryManager)
