---
uid: web-pages/overview/data/working-with-files
title: Praca z plikami w witryny ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano sposób odczytu, zapisu, Dołącz, Usuń i przekazywania plików.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "28040226"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Praca z plikami w witrynie sieci Web ASP.NET (Razor) stron
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak odczytu, zapisu, dołączanie, usuwanie i przekazać pliki w witrynie stron sieci Web platformy ASP.NET (Razor).
> 
> > [!NOTE]
> > Jeśli chcesz przekazać obrazów i manipulowania nimi (na przykład przerzucić lub zmienić ich rozmiar), zobacz [Praca z obrazami w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Zawartość:** 
> 
> - Jak utworzyć plik tekstowy i zapisanie w nim danych.
> - Jak dołączyć dane do istniejącego pliku.
> - Jak można odczytać pliku i wyświetlać z niej.
> - Jak usunąć pliki z witryny sieci Web.
> - Jak umożliwić użytkownikom przekazywanie jednego lub wielu plików.
> 
> Są to programowania funkcje dodane w artykule programu ASP.NET:
> 
> - `File` Obiektu, który umożliwia zarządzanie plikami.
> - `FileUpload` Pomocnika.
> - `Path` Obiektu, który udostępnia metody, które umożliwiają manipulowania nazwę pliku i ścieżkę.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> W tym samouczku współdziała również z 3 programu WebMatrix.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Tworzenie pliku tekstowego i zapisywania danych

Oprócz korzystanie z bazy danych w witrynie sieci Web, może pracować z plikami. Na przykład można użyć plików tekstowych jako prosty sposób przechowywania danych dla lokacji. (Plik tekstowy, który jest używany do przechowywania danych jest niekiedy nazywany *pliku prostego*.) Pliki tekstowe może być w różnych formatach, na przykład *.txt*, *.xml*, lub *CSV* (wartości rozdzielane przecinkami).

Jeśli chcesz przechowywać dane w pliku tekstowym, możesz użyć `File.WriteAllText` metodę, aby określić plik, aby utworzyć i zapisanie w nim danych. W tej procedurze utworzysz strona zawierająca prostego formularza z trzema `input` elementy (imię, nazwisko i adres e-mail) i **przesyłania** przycisku. Gdy użytkownik przesyła formularz, danych wejściowych użytkownika będą przechowywane w pliku tekstowym.

1. Utwórz nowy folder o nazwie *aplikacji\_danych*, jeśli jest ona już nie istnieje.
2. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *UserData.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Kod znaczników HTML tworzy formularz z trzech polach. W kodzie, użyj `IsPost` właściwości w celu określenia, czy strony zostało przesłane przed rozpoczęciem przetwarzania.

    Pierwszym zadaniem jest pobrać dane wejściowe użytkownika i przypisz je do zmiennych. Kod następnie łączy wartości zmiennych oddzielne w jeden ciąg rozdzielany przecinkami, które następnie są przechowywane w innej zmiennej. Zwróć uwagę, że przecinka jako separatora jest ciągiem zawarta w cudzysłowie (","), ponieważ jest dosłownie osadzanie przecinka w dużych ciąg, który tworzysz. Na koniec dane, które można łączyć ze sobą, możesz dodać `Environment.NewLine`. Spowoduje to dodanie podział wiersza (znak nowego wiersza). Tworzysz z wszystkich to łączenia jest ciągiem, który wygląda następująco:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Za pomocą niewidocznej linii podziału na końcu.)

    Następnie można utworzyć zmiennej (`dataFile`) zawierający lokalizację i nazwę pliku do przechowania danych. Ustawianie lokalizacji wymaga niektórych specjalnej obsługi. W witrynach sieci Web, jest rozwiązaniem zły w odwołaniu w kodzie do ścieżki bezwzględne, takich jak *C:\Folder\File.txt* plików na serwerze sieci web. Jeśli witryna sieci Web zostanie przeniesiony, ścieżką bezwzględną będą nieprawidłowe. Ponadto hostowanej witryny (a nie na własnym komputerze) można zwykle nawet poprawną ścieżkę jest nieznana podczas pisania kodu.

    Jednak czasami (na przykład teraz do zapisu pliku) trzeba pełną ścieżkę. Rozwiązaniem jest użycie `MapPath` metody `Server` obiektu. To zwraca pełną ścieżkę do witryny sieci Web. Można uzyskać ścieżki do katalogu głównego witryny sieci Web, możesz użytkownika `~` — operator (do represen lokacji do wirtualnego katalogu głównego) do `MapPath`. (Można również przekazać nazwę podfolderu, takich jak *~/App\_danych /*, można uzyskać ścieżki dla tego podfolderu.) Można następnie łączyć ze sobą informacje dodatkowe na zwraca niezależnie od metody, aby można było utworzyć pełną ścieżkę. W tym przykładzie należy dodać nazwę pliku. (Więcej o tym, jak pracować z ścieżek plików i folderów w [wprowadzenie do platformy ASP.NET Web Pages programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Plik jest zapisywany w *aplikacji\_danych* folderu. Ten folder jest specjalne w programie ASP.NET, który służy do przechowywania plików danych, zgodnie z opisem w [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    `WriteAllText` Metody `File` obiektu zapisuje dane do pliku. Ta metoda przyjmuje dwa parametry: Nazwa pliku do zapisu i rzeczywiste dane do zapisania (ze ścieżką). Należy zauważyć, że nazwa pierwszy parametr ma `@` znak jako prefiksu. To informuje ASP.NET, która jest zapewnienie literał ciągu dosłownego wyrażenia i znaków, takich jak "/" nie mogą być traktowane w specjalny sposób. (Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Aby swój kod, aby zapisać pliki w *aplikacji\_danych* folderu, aplikacja musi odczytu i zapisu uprawnienia dla tego folderu. Na komputerze deweloperskim to nie jest zazwyczaj problem. Jednak opublikowanie witryny dostawcy hostingu serwera sieci web, należy jawnie ustawić te uprawnienia. Jeśli uruchomienie tego kodu na serwerze dostawcy hostingu i występują błędy, skontaktuj się z dostawcy hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

- Uruchom strony w przeglądarce. 

    ![](working-with-files/_static/image1.jpg)
- Wprowadź wartości w polach, a następnie kliknij przycisk **przesyłania**.
- Zamknij przeglądarkę.
- Wróć do projektu i Odśwież widok.
- Otwórz *data.txt* pliku. Dane przesłane w formularzu jest w pliku. 

    ![[Obraz]](working-with-files/_static/image2.jpg)
- Zamknij *data.txt* pliku.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Dołączanie danych do istniejącego pliku

W poprzednim przykładzie użyto `WriteAllText` umożliwiający utworzenie pliku tekstowego, który otrzymano w nim tylko jeden element danych. Jeśli ponownie wywołaj metodę i przekaż go tą samą nazwą pliku, całkowicie zastąpić istniejącego pliku. Jednak po utworzeniu pliku często chcesz dodać nowe dane do końca pliku. Możesz zrobić tego za pomocą `AppendAllText` metody `File` obiektu.

1. W witrynie sieci Web, należy utworzyć kopię *UserData.cshtml* pliku i nazwa kopii *UserDataMultiple.cshtml*.
2. Zastąp blok kodu przed rozpoczęciem `<!DOCTYPE html>` tag o następujący blok kodu: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Ten kod zawiera zmiana jednego z poprzedniego przykładu. Zamiast `WriteAllText`, używa `the AppendAllText` metody. Metody są podobne, z wyjątkiem `AppendAllText` dodaje dane na końcu pliku. Jak `WriteAllText`, `AppendAllText` tworzy plik, jeśli jeszcze nie istnieje.
3. Uruchom strony w przeglądarce.
4. Wprowadź wartości w polach, a następnie kliknij przycisk **przesyłania**.
5. Dodaj więcej danych i ponownie Prześlij formularz.
6. Wróć do projektu, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij przycisk **Odśwież**.
7. Otwórz *data.txt* pliku. Zawiera teraz nowe dane, po prostu wprowadzony. 

    ![[Obraz]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Odczytywanie i wyświetlanie danych z pliku

Nawet jeśli nie trzeba zapisać danych do pliku tekstowego, prawdopodobnie czasami należy odczytać danych z jednego. Aby to zrobić, można ponownie użyć `File` obiektu. Można użyć `File` obiektu do odczytu każdego wiersza indywidualnie (oddzielone podziały wiersza) lub odczytać pojedynczy element niezależnie od tego, jak są rozdzielone.

W tej procedurze przedstawiono odczytywanie i wyświetlić dane utworzonego w poprzednim przykładzie.

1. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *DisplayData.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Kod, który rozpoczyna się od odczytu pliku, który został utworzony w poprzednim przykładzie do zmiennej o nazwie `userData`, przy użyciu wywołanie tej metody:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Kod w tym celu znajduje się wewnątrz `if` instrukcji. Umożliwia odczytywanie pliku, to warto użyć `File.Exists` metodę, aby najpierw ustalić, czy plik jest dostępny. Kod sprawdza również, czy plik jest pusty.

    Treść strony zawiera dwa `foreach` pętli jedną zagnieżdżone w innych. Zewnętrznego `foreach` pętli pobiera jeden wiersz jednocześnie z pliku danych. W takim przypadku wiersze są definiowane przez podziały wierszy w pliku &#8212; każdy element danych jest w osobnym wierszu. Zewnętrzne pętli tworzy nowy element (`<li>` element) wewnątrz listy uporządkowanej (`<ol>` elementu).

    Fragment pętli dzielony elementów (pól) za pomocą przecinka jako ogranicznik każdego wiersza danych. (Na podstawie poprzedniego przykładu, oznacza to, że każdy wiersz zawiera trzy pola &#8212; imię, nazwisko i adres e-mail, oddzielonych przecinkami.) Tworzy również pętli wewnętrznej `<ul>` elementu listy i wyświetla listę jednej dla każdego pola w wierszu danych.

    Kod ilustruje sposób używania dwa typy danych, tablicy i `char` — typ danych. Tablica jest wymagane, ponieważ `File.ReadAllLines` metoda zwraca dane w postaci tablicy. `char` Wymagany jest typ danych, ponieważ `Split` metoda zwraca `array` , w której każdy element jest typu `char`. (Aby uzyskać informacje dotyczące tablic, zobacz [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Uruchom strony w przeglądarce. Wyświetlania danych wprowadzonych w poprzednich przykładach. 

    ![[Obraz]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Wyświetlanie danych z pliku rozdzielanego przecinkami programu Microsoft Excel**
> 
> Można użyć programu Microsoft Excel można zapisać danych zawartych w arkuszu kalkulacyjnym jako pliku rozdzielanego przecinkami (*CSV* pliku). Po wykonaniu, plik jest zapisany w postaci zwykłego tekstu, a nie w formacie programu Excel. Każdego wiersza w arkuszu kalkulacyjnym są oddzielane podział wiersza w pliku tekstowym, a każdy element danych jest rozdzielonych przecinkami. Kodzie pokazanym w poprzednim przykładzie służy do odczytu z pliku rozdzielanego przecinkami programu Excel po prostu, zmieniając nazwę pliku danych w kodzie.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Usuwanie plików

Aby usunąć pliki z witryny sieci Web, można użyć `File.Delete` metody. Za pomocą poniższej procedury umożliwić użytkownikom Usuń obraz (*.jpg* plik) z *obrazów* folderu, jeśli znają nazwę pliku.

> [!NOTE] 
> 
> **Ważne** w środowisku produkcyjnym witryny sieci Web zwykle ograniczysz kto ma może wprowadzać zmiany w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, utwórz podfolder o nazwie *obrazów*.
2. Kopiuj co najmniej jeden *.jpg* pliki do *obrazów* folderu.
3. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *FileDelete.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Ta strona zawiera formularza, w którym użytkownicy mogą wprowadzać nazwy pliku obrazu. Użytkownik nie podał *.jpg* rozszerzenia nazwy pliku; ograniczając nazwę pliku, jak możesz pomocy uniemożliwia użytkownikom usuwanie dowolne pliki w witrynie.

    Ten kod odczytuje nazwę pliku, czy użytkownik wprowadził i następnie tworzy pełną ścieżkę. Aby utworzyć ścieżkę, kod używa bieżącej ścieżki witryny sieci Web (zwrócony przez `Server.MapPath` — metoda), *obrazów* nazwę folderu, nazwa zapewnianej przez użytkownika oraz ".jpg" jako literału ciągu.

    Można usunąć pliku, kod wywołuje `File.Delete` metody przekazanie jej pełną ścieżkę, którą właśnie utworzone. Na koniec kod znaczników kod wyświetla komunikat potwierdzenia, że plik został usunięty.
5. Uruchom strony w przeglądarce. 

    ![[Obraz]](working-with-files/_static/image5.jpg)
6. Wprowadź nazwę pliku, Usuń, a następnie kliknij przycisk **przesyłania**. Jeśli plik został usunięty, nazwa pliku jest wyświetlana w dolnej części strony.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Dzięki czemu użytkownicy przekazywania pliku

`FileUpload` Pomocnika pozwala użytkownikom na przekazywanie plików do witryny sieci Web. Poniższa procedura przedstawia sposób zezwolić użytkownikom na przekazywanie pojedynczy plik.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli użytkownik nie zostały dodane wcześniej.
2. W *aplikacji\_danych* , Utwórz nowy folder i nadaj mu nazwę *UploadedFiles*.
3. W folderze głównym, Utwórz nowy plik o nazwie *FileUpload.cshtml*.
4. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Korzysta z części treści strony `FileUpload` pomocnika można utworzyć pole przekazywania i przyciski, który znasz prawdopodobnie:

    ![[Obraz]](working-with-files/_static/image6.jpg)

    Właściwości, które można ustawić dla `FileUpload` pomocnika Określ, które mają pojedyncze pole pliku do przekazania i ma przycisku przesyłania, aby odczytać **przekazać**. (Dodasz więcej pól w dalszej części tego artykułu.)

    Po kliknięciu przez użytkownika **przekazać**, kod w górnej części strony pobiera pliku i zapisanie go. `Request` Obiekt, który pobiera wartości z pola formularza zwykle jest używana również ma `Files` tablicy, która zawiera plik (lub pliki) zostały przekazane. Możesz uzyskać poszczególnych plików określonych pozycji w tablicy &#8212; na przykład, aby uzyskać pierwszy przekazany plik, możesz uzyskać `Request.Files[0]`, aby uzyskać drugi plik otrzymasz `Request.Files[1]`i tak dalej. (Należy pamiętać, że w programowaniu zliczania zwykle zaczyna się od zera).

    Podczas pobierania przekazany plik, należy umieścić w zmiennej (w tym miejscu `uploadedFile`), dzięki czemu można manipulować go. Aby określić nazwę przekazany plik, po prostu otrzymasz jego `FileName` właściwości. Jednak gdy użytkownik przekazuje plik, `FileName` zawiera oryginalna nazwa użytkownika, który zawiera pełną ścieżkę. Go może wyglądać następująco:

    *C:\Users\Public\Sample.txt*

    Nie ma tych informacji ścieżki, ponieważ jest to ścieżka na komputerze użytkownika, nie dla serwera. Chcesz rzeczywiste nazwy plików (*przykład.txt*). Tylko plik ze ścieżki można usuwają przy użyciu `Path.GetFileName` metody następująco:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path` Obiektu to narzędzie, które ma wiele metod takie używanego do paska ścieżki, łączenia ścieżek i tak dalej.

    Po ich zaakceptujesz nazwę przekazany plik, można utworzyć nową ścieżkę, dla której chcesz przechowywać przekazanego pliku w witrynie sieci Web. W takim przypadku połączyć `Server.MapPath`, nazwy folderów (*aplikacji\_danych/UploadedFiles*) oraz nazwę pliku nowo pozbawionego włókien, aby utworzyć nową ścieżkę. Rozmiar przesłanego pliku może wywoływać `SaveAs` metodę, aby rzeczywiście Zapisz plik.
5. Uruchom strony w przeglądarce. 

    ![[Obraz]](working-with-files/_static/image7.jpg)
6. Kliknij przycisk **Przeglądaj** , a następnie wybierz plik do przekazania. 

    ![[Obraz]](working-with-files/_static/image8.jpg)

    Pole tekstowe dalej, aby **Przeglądaj** przycisk będzie zawierać lokalizacji i ścieżka pliku.

    ![[Obraz]](working-with-files/_static/image9.jpg)
7. Kliknij przycisk **przekazać**.
8. W witrynie sieci Web, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij przycisk **Odśwież**.
9. Otwórz *UploadedFiles* folderu. Przesłany plik znajduje się w folderze. 

    ![[Obraz]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Umożliwienie użytkownikom przekazywania wielu plików

W poprzednim przykładzie można zezwolić użytkownikom na przekazywanie jednego pliku. Jednak można użyć `FileUpload` pomocnika, aby przekazać więcej niż jeden plik w czasie. Jest to przydatne w scenariuszach, takich jak przekazywanie fotografii, gdzie jest niewygodny przekazywania jednego pliku w czasie. (Możesz przeczytać temat przekazywania zdjęć w [Praca z obrazami w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).) Ten przykład przedstawia sposób zezwolić użytkownikom na przekazywanie dwa jednocześnie, chociaż można używać tę samą metodę, aby przekazać więcej niż.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
2. Utwórz nową stronę o nazwie *FileUploadMultiple.cshtml*.
3. Zastąp istniejącą zawartość na stronie następujące czynności:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    W tym przykładzie `FileUpload` pomocnika w treści strony jest domyślnie konfigurowana umożliwi użytkownikom przekazać dwóch plików. Ponieważ `allowMoreFilesToBeAdded` ma ustawioną wartość `true`, pomocnika renderuje łącze, które pozwala użytkownikowi na dodanie więcej pól przekazywania:

    ![[Obraz]](working-with-files/_static/image11.jpg)

    Aby przetwarzać pliki, które przekazuje użytkownika, w kodzie użyto tę samą metodę podstawowego, który został użyty w poprzednim przykładzie &#8212; pobierania pliku z `Request.Files` i zapisać go. (Łącznie z różnych rzeczy należy zrobić, aby uzyskać prawidłowy plik nazwę i ścieżkę.) Innowacji teraz jest, czy użytkownik może przekazać wielu plików i nie można ustalić wiele. Aby dowiedzieć się, możesz uzyskać `Request.Files.Count`.

    Ten numer strony, można przeglądać `Request.Files`, pobrać z kolei każdy plik i zapisać go. Chcąc pętli znane wiele razy w kolekcji, można użyć `for` pętli następująco:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Zmienna `i` jest tylko tymczasowe licznika, który przejdzie od zera do niezależnie od górnego limitu w ustawieniu. W takim przypadku górny limit jest to liczba plików. Ale ponieważ licznik zaczyna się od zera, podobnie jak w typowej inwentaryzacji scenariuszy w programie ASP.NET, górny limit jest jeden mniejsza niż liczba plików. (Jeśli trzy pliki są przekazywane, wartość licznika jest zero, aby 2).

    `uploadedCount` Zmiennej sumy wszystkie pliki, które są przekazywane i pomyślnie zapisane. Ten kod kont możliwość, że oczekiwany plik nie może istnieć możliwość załadowania.
4. Uruchom strony w przeglądarce. W przeglądarce pojawi się Strona i jego przekazywania dwa pola.
5. Wybierz dwóch plików do przekazania.
6. Kliknij przycisk **dodać inny plik**. Zostaje wyświetlona strona nowe pole przekazywania. 

    ![[Obraz]](working-with-files/_static/image12.jpg)
7. Kliknij przycisk **przekazać**.
8. W witrynie sieci Web, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij przycisk **Odśwież**.
9. Otwórz *UploadedFiles* folderu, aby wyświetlić pliki, które pomyślnie przekazano.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Praca z obrazami w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Eksportowanie do pliku CSV](https://msdn.microsoft.com/library/ms155919.aspx)
