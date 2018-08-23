---
uid: web-pages/overview/data/working-with-files
title: Praca z plikami w witrynie ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale wyjaśniono, jak do odczytu, zapisu, dołączania, usuwania i przekazywania plików.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 20fdbfda7d3e4b214a8e201c4a815a29c5bb7061
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756892"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Praca z plikami w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak do odczytu, zapisu, dołączania, usuwania i przekazywanie plików w witrynie ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Jeśli chcesz przekazywać obrazy i manipulować nimi (na przykład przerzucić lub zmieniać ich rozmiar), zobacz [Praca z obrazami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Zawartość:** 
> 
> - Jak utworzyć plik tekstowy i zapisanie w nim danych.
> - Jak dołączyć dane do istniejącego pliku.
> - Jak odczytać plik i wyświetlić z niego.
> - Jak usunąć pliki z witryny sieci Web.
> - Jak umożliwić użytkownikom przekazywanie jednego lub wielu plików.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule programowania programu ASP.NET:
> 
> - `File` Obiektu, który umożliwia zarządzanie plikami.
> - `FileUpload` Pomocnika.
> - `Path` Obiektu, który udostępnia metody, które pozwala manipulować ścieżkę i nazwy plików.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> W tym samouczku współpracuje również z programu WebMatrix 3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Tworzenie pliku tekstowego i zapisywania danych

Oprócz używania bazy danych w witrynie sieci Web, mogą pracować z plikami. Na przykład może używać plików tekstowych jako prosty sposób przechowywania danych dla tej witryny. (Plik tekstowy, który jest używany do przechowywania danych jest czasami nazywane *pliku prostego*.) Pliki tekstowe mogą być w różnych formatach, na przykład *.txt*, *.xml*, lub *CSV* (wartości rozdzielane przecinkami).

Jeśli chcesz przechowywać dane w pliku tekstowym, możesz użyć `File.WriteAllText` metodę, aby określić plik, aby utworzyć i zapisanie w nim dane. W tej procedurze utworzysz strony, który zawiera prosty formularz z trzema `input` elementów (pól Imię, nazwisko i adres e-mail) i **przesyłania** przycisku. Gdy użytkownik przesyła formularz, będzie przechowywać dane wejściowe użytkownika w pliku tekstowym.

1. Utwórz nowy folder o nazwie *aplikacji\_danych*, jeśli nie już istnieje.
2. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *UserData.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Kod znaczników HTML tworzy formularz z pól tekstowych trzy. W kodzie, należy użyć `IsPost` właściwości w celu określenia, czy strony zostały przesłane, przed rozpoczęciem przetwarzania.

    Pierwsze zadanie jest pobrać dane wejściowe użytkownika i przypisz je do zmiennych. Kod następnie łączy wartości zmiennych oddzielne w jeden ciąg rozdzielonych przecinkami, który następnie jest przechowywany w innej zmiennej. Należy zauważyć, że przecinka jako separatora jest ciągiem zawarte w znaki cudzysłowu (","), ponieważ przecinek dosłownie osadzasz w dużych ciąg, który tworzysz. Na końcu danych, które mają zostać połączone, możesz dodać `Environment.NewLine`. Spowoduje to dodanie podział wiersza (znak nowego wiersza). Tworzysz przy użyciu wszystkich to połączenie jest ciągiem, który wygląda w następujący sposób:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Przy użyciu niewidocznej podziału wiersza na końcu.)

    Następnie Utwórz zmienną (`dataFile`) zawierający lokalizację i nazwę pliku do przechowywania danych. Ustawianie lokalizacji wymaga pewnych specjalnej obsługi. W witrynach sieci Web, jest złym zwyczajem się w kodzie do ścieżki bezwzględnej, jak *C:\Folder\File.txt* dla plików na serwerze sieci web. Jeśli witryna sieci Web zostanie przeniesiony, ścieżka bezwzględna będą nieprawidłowe. Ponadto hostowanej witryny (a nie na swoim komputerze) należy zwykle nawet prawidłowej ścieżki jest nieznana podczas pisania kodu.

    Jednak czasami (np. teraz do zapisywania pliku) należy pełną ścieżkę. Rozwiązaniem jest użycie `MapPath` metody `Server` obiektu. Spowoduje to zwrócenie pełnej ścieżki do witryny sieci Web. Aby pobrać ścieżkę do katalogu głównego witryny sieci Web, użytkownik zostanie `~` — operator (do represen lokacji użytkownika wirtualnego katalogu głównego) do `MapPath`. (Można również przekazać nazwę podfolderu, takich jak *~/App\_danych /*, aby pobrać ścieżkę do tego podfolderu.) Można następnie łączyć ze sobą dodatkowe informacje na zwraca niezależnie od metody, aby można było utworzyć pełną ścieżkę. W tym przykładzie należy dodać nazwę pliku. (Możesz dowiedzieć się więcej o tym, jak pracować z ścieżek plików i folderów w [wprowadzenie do platformy ASP.NET Web Pages programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Plik jest zapisywany w *aplikacji\_danych* folderu. Ten folder jest na platformie ASP.NET, który jest używany do przechowywania plików danych, zgodnie z opisem w folderze specjalnym [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    `WriteAllText` Metody `File` obiekt zapisuje dane do pliku. Ta metoda przyjmuje dwa parametry: Nazwa (ze ścieżką) pliku do zapisu i rzeczywistego dane do zapisania. Należy zauważyć, że nazwa pierwszego parametru ma `@` znak jako prefiksu. To informuje program ASP.NET, że udostępniasz verbatim literał ciągu znaków i znaków, takich jak "/" nie mogą być traktowane w specjalny sposób. (Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > W kolejności, w kodzie zapisać plik w *aplikacji\_danych* folderu, aplikacja musi odczytu i zapisu uprawnienia do tego folderu. Na komputerze deweloperskim to nie jest zazwyczaj problem. Jednak podczas publikowania witryny dostawcy hostingu serwera sieci web, konieczne może być jawnie ustawić te uprawnienia. Jeśli możesz uruchomić ten kod na serwerze dostawcy hostingu i występują błędy, skontaktuj się z dostawcy hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

- Uruchom stronę w przeglądarce. 

    ![](working-with-files/_static/image1.jpg)
- Wprowadź wartości w polach, a następnie kliknij przycisk **przesyłania**.
- Zamknij przeglądarkę.
- Wróć do projektu, a następnie Odśwież widok.
- Otwórz *data.txt* pliku. Dane, przesłanych w formularzu znajdują się w pliku. 

    ![[image]](working-with-files/_static/image2.jpg)
- Zamknij *data.txt* pliku.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Dołączanie danych do istniejącego pliku

W poprzednim przykładzie użyto `WriteAllText` utworzyć plik tekstowy, który ma stało się w niej tylko jednego elementu danych. Jeśli ponownie wywołaj metodę i przekazać go taką samą nazwę, istniejący plik jest całkowicie zastąpiony. Jednak po utworzeniu pliku często chcesz dodać nowe dane do końca pliku. Możesz zrobić, przy użyciu `AppendAllText` metody `File` obiektu.

1. W witrynie sieci Web, należy utworzyć kopię *UserData.cshtml* pliku i nazwę kopii *UserDataMultiple.cshtml*.
2. Zastąp blok kodu, przed otwarciem `<!DOCTYPE html>` tag z następującym fragmencie kodu: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Ten kod zawiera jedną zmianę w poprzednim przykładzie. Zamiast używania `WriteAllText`, używa ona `the AppendAllText` metody. Metody są podobne, z tą różnicą, że `AppendAllText` dodaje dane na końcu pliku. Podobnie jak w przypadku `WriteAllText`, `AppendAllText` tworzy plik jeśli jeszcze nie istnieje.
3. Uruchom stronę w przeglądarce.
4. Wprowadź wartości w polach, a następnie kliknij przycisk **przesyłania**.
5. Dodawanie większej ilości danych i ponownie Prześlij formularz.
6. Wróć do projektu, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij **Odśwież**.
7. Otwórz *data.txt* pliku. Zawiera teraz nowe dane, które wprowadziłeś. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Odczytywanie i wyświetlanie danych z pliku

Nawet wtedy, gdy nie ma potrzeby zapisywania danych do pliku tekstowego, należy prawdopodobnie czasami odczytywać dane z jednego. Aby to zrobić, należy ponownie użyć `File` obiektu. Możesz użyć `File` obiektu do odczytania indywidualnie każdy wiersz (rozdzielonych przez podziały wiersza) lub do odczytania pojedynczych elementów niezależnie od tego, w jaki sposób są oddzielone.

Ta procedura pokazuje, jak odczytywać i wyświetlić dane, który został utworzony w poprzednim przykładzie.

1. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *DisplayData.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Kod, który rozpoczyna się od odczytu pliku, który został utworzony w poprzednim przykładzie w zmiennej o nazwie `userData`, za pomocą wywołanie tej metody:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Kod w tym celu znajduje się wewnątrz `if` instrukcji. Jeśli chcesz odczytać plik jest dobry pomysł, aby użyć `File.Exists` metodę, aby najpierw sprawdzić, czy plik jest dostępny. Kod sprawdza również, czy plik jest pusty.

    Treść strony zawiera dwa `foreach` w pętli, jeden zagnieżdżone wewnątrz innych. Zewnętrzny `foreach` pętla przechodzi jeden wiersz jednocześnie z pliku danych. W tym przypadku wiersze są definiowane przez podziały wierszy w pliku &#8212; każdy element danych jest w osobnym wierszu. Zewnętrzna Pętla tworzy nowy element (`<li>` elementu) wewnątrz listy uporządkowanej (`<ol>` elementu).

    Wewnętrzną pętlę dzieli każdy wiersz danych na elementy (pola), za pomocą przecinka jako ogranicznik. (Na podstawie poprzedniego przykładu, oznacza to, że każdy wiersz zawiera trzy pola &#8212; imię, nazwisko i adres e-mail, każda jest oddzielona przecinkiem.) Tworzy również wewnętrzną pętlę `<ul>` listy i wyświetla listę jednego elementu dla każdego pola w wierszu danych.

    Kod ilustruje sposób użycia dwa typy danych, tablicy i `char` typu danych. Tablica jest wymagana, ponieważ `File.ReadAllLines` metoda zwraca dane jako tabelę. `char` Typ danych jest wymagany, ponieważ `Split` metoda zwraca `array` w której każdy element jest typu `char`. (Aby uzyskać informacje na temat tablic, zobacz [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Uruchom stronę w przeglądarce. Dane, wprowadzonych w poprzednich przykładach są wyświetlane. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Wyświetlanie danych z pliku rozdzielanego przecinkami programu Microsoft Excel**
> 
> Można użyć programu Microsoft Excel, można zapisać danych zawartych w arkuszu kalkulacyjnym jako pliku rozdzielanego przecinkami (*CSV* pliku). Po wykonaniu, plik jest zapisywany w postaci zwykłego tekstu, a nie w formacie programu Excel. Każdy wiersz w arkuszu kalkulacyjnym jest rozdzielonych przez podziały wiersza w pliku tekstowym, a każdy element danych jest oddzielona przecinkiem. Kod przedstawiony w poprzednim przykładzie służy do odczytu z pliku rozdzielanego przecinkami programu Excel po prostu zmieniając nazwę pliku danych w kodzie.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Usuwanie plików

Aby usunąć pliki z witryny sieci Web, można użyć `File.Delete` metody. Za pomocą poniższej procedury zezwolić użytkownikom na usuwanie obrazu (*.jpg* plik) z *obrazów* folderu, jeśli znają nazwę pliku.

> [!NOTE] 
> 
> **Ważne** w produkcyjnej witrynie internetowej można zwykle ograniczają kto ma prawo do wprowadzania zmian w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, utwórz podfolder o nazwie *obrazów*.
2. Kopiuj co najmniej jeden *.jpg* pliki do *obrazów* folderu.
3. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *FileDelete.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Ta strona zawiera formularza, w którym użytkownik może wprowadzić nazwę pliku obrazu. Użytkownik nie podał *.jpg* rozszerzenia nazwy pliku; ograniczając nazwę pliku, tak, możesz uzyskać pomoc uniemożliwia użytkownikom usuwanie dowolnych plików w witrynie.

    Kod odczytuje nazwę pliku, że użytkownik wprowadził i następnie tworzy pełną ścieżkę. Aby utworzyć ścieżkę, kod używa bieżącej ścieżki witryny sieci Web (zwrócone przez `Server.MapPath` metoda), *obrazów* nazwę folderu, nazwę, która użytkownik udostępnił i ".jpg" jako ciąg literału.

    Można usunąć pliku, kod wywołuje `File.Delete` metody przekazanie jej pełną ścieżkę, która po prostu skonstruowany. Na końcu znaczników kod wyświetla komunikat potwierdzający, że plik został usunięty.
5. Uruchom stronę w przeglądarce. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Wprowadź nazwę pliku do usunięcia, a następnie kliknij przycisk **przesyłania**. Jeśli plik został usunięty, nazwa pliku jest wyświetlane w dolnej części strony.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Dzięki czemu użytkownicy przekazywania pliku

`FileUpload` Pomocnika umożliwia użytkownikom przekazywanie plików do witryny sieci Web. Poniższa procedura pokazuje, jak umożliwić użytkownikom przekazać jeden plik.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli użytkownik nie został dodany wcześniej.
2. W *aplikacji\_danych* folderu, Utwórz nowy folder i nadaj mu nazwę *UploadedFiles*.
3. W katalogu głównym, Utwórz nowy plik o nazwie *FileUpload.cshtml*.
4. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Korzysta z części treści strony `FileUpload` element pomocniczy służący do tworzenia okno przekazywania i przyciski, które znasz prawdopodobnie:

    ![[image]](working-with-files/_static/image6.jpg)

    Właściwości, które można ustawić dla `FileUpload` pomocnika określić, czy pojedyncze pole, aby uzyskać plik do przekazania i mają przycisk Prześlij, aby przeczytać **przekazywanie**. (Należy dodać więcej pól w dalszej części tego artykułu.)

    Kiedy użytkownik kliknie **przekazywanie**, kod w górnej części strony pobiera plik i zapisuje go. `Request` Obiekt, który umożliwia zwykle pobierają wartości z pól formularza również ma `Files` tablicę, która zawiera plik (lub pliki) zostały przekazane. Możesz uzyskać pojedyncze pliki z określonych pozycji w tablicy &#8212; na przykład, aby uzyskać pierwszy przekazany plik, możesz pobrać `Request.Files[0]`, aby uzyskać drugi plik otrzymasz `Request.Files[1]`i tak dalej. (Należy pamiętać, że w programowaniu zliczanie zwykle zaczyna się od zera).

    Po pobraniu przekazany plik, można umieścić w zmiennej (w tym miejscu `uploadedFile`), dzięki czemu można manipulować go. Aby określić nazwę przekazanego pliku, po prostu otrzymasz jego `FileName` właściwości. Jednak po użytkownik służy do przekazywania pliku, `FileName` zawiera oryginalna nazwa użytkownika, który zawiera pełną ścieżkę. Jego może wyglądać następująco:

    *C:\Users\Public\Sample.txt*

    Nie chcesz tych informacji ścieżki, ponieważ jest to ścieżka na komputerze użytkownika, nie dla serwera. Po prostu chcesz rzeczywiste nazwy plików (*przykład.txt*). Po prostu pliku ze ścieżki mogą odłączenia przy użyciu `Path.GetFileName` metoda następująco:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path` Obiektu to narzędzie, które ma kilka metod, takich jak ta, używanej do paska ścieżki, łączenia ścieżek i tak dalej.

    Po trafiła do Ciebie nazwa przekazany plik, możesz utworzyć nową ścieżkę dla której chcesz przechowywać przekazanego pliku w witrynie sieci Web. W takim przypadku możesz połączyć `Server.MapPath`, nazwy folderów (*aplikacji\_danych/UploadedFiles*) i nazwę nowo usuniętych plików, aby utworzyć nową ścieżkę. Następnie możesz wywołać przekazanego pliku `SaveAs` metodę, aby faktycznie Zapisz plik.
5. Uruchom stronę w przeglądarce. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Kliknij przycisk **Przeglądaj** a następnie wybierz plik do przekazania. 

    ![[image]](working-with-files/_static/image8.jpg)

    Pole tekstowe obok **Przeglądaj** przycisk będzie zawierać ścieżkę i lokalizacji.

    ![[image]](working-with-files/_static/image9.jpg)
7. Kliknij przycisk **przekazywanie**.
8. W witrynie sieci Web, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij przycisk **Odśwież**.
9. Otwórz *UploadedFiles* folderu. Przekazany plik znajduje się w folderze. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Umożliwienie użytkownikom przekazywanie wielu plików

W poprzednim przykładzie możesz zezwolić użytkownikom przekazać jeden plik. Ale można użyć `FileUpload` element pomocniczy służący do przekazywania więcej niż jeden plik jednocześnie. Jest to przydatne w scenariuszach, takich jak przekazywanie zdjęć, gdzie jest niewygodna przekazywania jeden plik jednocześnie. (Możesz przeczytać o przekazywania zdjęć w [Praca z obrazami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) Ten przykład pokazuje, jak umożliwić użytkownikom przekazywanie dwóch naraz, chociaż można używać tej samej techniki, można przekazać więcej niż.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
2. Utwórz nową stronę o nazwie *FileUploadMultiple.cshtml*.
3. Zastąp istniejącą zawartość na stronie następujących czynności:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    W tym przykładzie `FileUpload` pomocnika w treści strony jest domyślnie skonfigurowana do zezwala użytkownikom na przekazywanie dwa pliki. Ponieważ `allowMoreFilesToBeAdded` ustawiono `true`, pomocnika renderuje łącze, które pozwala użytkownikowi na dodanie więcej pól przekazywania:

    ![[image]](working-with-files/_static/image11.jpg)

    Aby przetwarzać pliki, które przekazuje użytkownika, w kodzie za pomocą tej samej techniki podstawowe, które było używane w poprzednim przykładzie &#8212; pobieranie pliku z `Request.Files` i zapisz go. (W tym różne elementy należy zrobić, aby uzyskać prawo nazwę i ścieżkę.) Wprowadzanie innowacji w tej chwili jest, czy użytkownik może przekazać wiele plików i nie wiesz, wiele. Aby dowiedzieć się, możesz uzyskać `Request.Files.Count`.

    Ten numer strony, można pętli `Request.Files`, z kolei pobierania każdego pliku i zapisz go. Sprzężenie znanych kilka razy w kolekcji, należy można użyć `for` pętli następująco:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Zmienna `i` to tylko tymczasowy licznik, który przechodzi od zera do dowolnych górny limit zostanie ustawiona. W tym przypadku górny limit jest to liczba plików. Jednak ponieważ licznik rozpoczyna się od zera, podobnie jak w typowej do zliczania scenariuszy dla platformy ASP.NET, górny limit jest faktycznie jeden mniejsza niż liczba plików. (Jeśli trzy pliki są przekazywane, liczba jest zero, 2).

    `uploadedCount` Zmiennej sumuje wszystkie pliki, które zostaną pomyślnie przekazane i zapisane. Ten kod kont możliwość, że oczekiwany plik może okazać się niemożliwe do przekazania.
4. Uruchom stronę w przeglądarce. W przeglądarce pojawi się Strona i jego przekazywanie dwóch polach.
5. Wybierz dwa pliki do przekazania.
6. Kliknij przycisk **dodać inny plik**. Strony wyświetli nowe okno przekazywania. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Kliknij przycisk **przekazywanie**.
8. W witrynie sieci Web, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij przycisk **Odśwież**.
9. Otwórz *UploadedFiles* folder, aby wyświetlić pliki, które pomyślnie przekazano.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Praca z obrazami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)

[Eksportowanie do pliku CSV](https://msdn.microsoft.com/library/ms155919.aspx)
