---
uid: web-pages/overview/data/5-working-with-data
title: "Wprowadzenie do pracy z bazą danych w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym rozdziale opisano, jak uzyskać dostęp do danych z bazy danych i wyświetlić je za pomocą stron sieci Web programu ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 460af471a1b0650f8d782d582ce6cd9a06664d5c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Wprowadzenie do pracy z bazą danych w sieci Web ASP.NET stron witryny (Razor)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano, jak za pomocą narzędzi Microsoft WebMatrix tworzenia bazy danych w witrynie sieci Web platformy ASP.NET Web Pages (Razor) oraz sposobu tworzenia stron, które pozwalają na wyświetlanie, dodawanie, edytowanie i usuwanie danych.
> 
> **Zawartość:** 
> 
> - Jak utworzyć bazę danych.
> - Sposób nawiązywania połączenia z bazą danych.
> - Jak wyświetlać dane na stronie sieci web.
> - Jak wstawianie, aktualizowanie i usuwanie rekordów bazy danych.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule:
> 
> - Praca z bazy danych programu Microsoft SQL Server Compact Edition.
> - Praca z zapytania SQL.
> - `Database` Klasy.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Program WebMatrix 2
>   
> 
> W tym samouczku współdziała również z 3 programu WebMatrix. Możesz użyć 3 stron sieci Web ASP.NET i Visual Studio 2013 (lub programu Visual Studio Express 2013 for Web); jednak interfejsu użytkownika będą inne.


## <a name="introduction-to-databases"></a>Wprowadzenie do bazy danych

Wyobraź sobie książki adresowej typowych. Dla każdego wpisu w książce adresowej (to znaczy, że dla każdej osoby) ma kilka rodzajów informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

Typowy sposób dane obrazu takie jest jako tabelę z wierszy i kolumn. Każdy wiersz w terminologii baz danych jest często określany jako rekord. Każda kolumna (czasem określana jako pola) zawiera wartość dla każdego typu danych: imię, nazwa ostatniego i tak dalej.

| **IDENTYFIKATOR** | **Imię** | **Nazwisko** | **Adres** | **Poczta e-mail** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Jakub | Zawadzki | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Dla większości tabel bazy danych Tabela musi mieć kolumnę, która zawiera unikatowy identyfikator, takich jak numer klienta, numer konta itp. Jest to nazywane tabeli *klucz podstawowy*, i używa ich do identyfikacji każdego wiersza w tabeli. W tym przykładzie w kolumnie identyfikator jest kluczem podstawowym w książce adresowej.

Z tym podstawową wiedzę na temat baz danych, możesz dowiedzieć się, jak utworzyć prosty bazy danych i wykonywanie operacji takich jak dodawanie, modyfikowanie i usuwanie danych.

> [!TIP] 
> 
> **Relacyjnych baz danych**
> 
> Dane można przechowywać w wiele sposobów, łącznie z plików tekstowych i arkuszy kalkulacyjnych. Na potrzeby większości firm jednak dane są przechowywane w relacyjnej bazie danych.
> 
> W tym artykule nie bardzo głęboko przejdź do bazy danych. Jednak użytkownik może być bardzo przydatne do zrozumienia nieco o nich. Relacyjnej bazy danych informacje są logicznie podzielone na osobne tabele. Na przykład bazy danych dla Szkoła może zawierać oddzielnych tabelach dla uczniów lub studentów i ofert klasy. Bazy danych oprogramowania (takich jak SQL Server) obsługuje zaawansowane polecenia, które pozwalają dynamicznie ustanawiania relacji między tabelami. Na przykład służy relacyjnej bazy danych do ustanawiania relacji logiczne między studenci i grupy, aby utworzyć harmonogram. Przechowywanie danych w oddzielnych tabelach zmniejsza się złożoność struktury tabeli i ogranicza potrzebę nadmiarowego dane są przechowywane w tabelach.


## <a name="creating-a-database"></a>Tworzenie bazy danych

Ta procedura przedstawia sposób tworzenia bazy danych o nazwie SmallBakery za pomocą narzędzia projekt bazy danych programu SQL Server Compact, które znajduje się w programie WebMatrix. Można utworzyć bazy danych przy użyciu kodu, ale jest bardziej typowego do tworzenia bazy danych i tabele bazy danych przy użyciu narzędzia projektowania, takiego jak program WebMatrix.

1. Uruchom program WebMatrix, a na stronie Szybki Start kliknij **szablonu z witryny**.
2. Wybierz **pusta witryna**, a następnie w **nazwa witryny** polu wprowadź "SmallBakery", a następnie kliknij przycisk **OK**. Ta witryna jest utworzone i wyświetlane w programie WebMatrix.
3. W okienku po lewej stronie kliknij **baz danych** obszaru roboczego.
4. Na wstążce kliknij **nową bazę danych**. Pusta baza danych została utworzona z taką samą nazwę jak witryny.
5. W okienku po lewej stronie rozwiń **SmallBakery.sdf** węzeł, a następnie kliknij przycisk **tabel**.
6. Na wstążce kliknij **nową tabelę**. Program WebMatrix otwiera projektanta tabel.

    ![[Obraz]](5-working-with-data/_static/image1.jpg)
7. Kliknij **nazwa** kolumny, a następnie wprowadź &quot;identyfikator&quot;.
8. W **— typ danych** kolumny wybierz **int**.
9. Ustaw **jest klucz podstawowy?** i **jest określenie?** opcji **tak**.

    Jak wynika z nazwy, **jest kluczem podstawowym** bazy danych informuje, że są to klucz podstawowy tabeli. **Tożsamość jest** informuje bazy danych, aby automatycznie utworzyć identyfikator dla każdego nowego rekordu i przypisać kolejny numer sekwencyjny (rozpoczyna się od 1).
10. Kliknij w następnym wierszu. Edytor nowych definicji kolumny.
11. Wprowadź wartość nazwy &quot;nazwa&quot;.
12. Aby uzyskać **— typ danych**, wybierz &quot;nvarchar&quot; i ustawianie długości do 50. *Var* częścią `nvarchar` bazy danych informuje, że dane dla tej kolumny zostaną ciąg, którego rozmiar może się różnić od rekordu do rekordu. (  *n*  Prefiksu reprezentuje *national*, wskazującą, czy pole może przechowywać dane znakowe reprezentujący wszystkie alfabetu lub zapisywanie systemu &#8212; która jest, że pole posiada Unicode danych).
13. Ustaw **Zezwalaj na wartości null** opcji w celu **nr**. To będzie wymuszać, który *nazwa* kolumna nie jest puste.
14. Za pomocą tego samego procesu, Utwórz kolumnę o nazwie *opis*. Ustaw **— typ danych** "nvarchar" i 50 długość oraz zestaw **Zezwalaj na wartości null** o wartości false.
15. Utwórz kolumnę o nazwie *cen*. Ustaw **typu danych "pieniędzy"** i ustaw **Zezwalaj na wartości null** o wartości false.
16. W oknie dialogowym u góry nazwy tabeli &quot;produktu&quot;.

    Gdy wszystko będzie gotowe, definicja będzie wyglądać następująco:

    ![[Obraz]](5-working-with-data/_static/image2.jpg)
17. Naciśnij klawisze Ctrl + S, aby zapisać w tabeli.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

Teraz można dodać przykładowych danych do bazy danych, które będą współpracować w dalszej części tego artykułu.

1. W okienku po lewej stronie rozwiń **SmallBakery.sdf** węzeł, a następnie kliknij przycisk **tabel**.
2. Kliknij prawym przyciskiem myszy tabelę produktu, a następnie kliknij przycisk **danych**.
3. W okienku edycji wprowadź następujące rekordy:

    | **Nazwa** | **Opis** | **Ceny** |
    | --- | --- | --- |
    | Chleb | Rozszerzania świeże każdego dnia. | 2.99 |
    | Shortcake truskawkowe | Wprowadzone w organicznym truskawki z naszych ogrodu. | 9.99 |
    | Kołowy firmy Apple | Drugi tylko na Twój Tato koło. | 12.99 |
    | Orzech pekan kołowy | Jeśli chcesz Orzeszki pekan jest dla Ciebie. | 10.99 |
    | Kołowy cytrynowa | Wykonane przy użyciu najlepszych cytryn na świecie. | 11.99 |
    | Babeczki | Dziecko i kid w przypadku pokochają je. | 7.99 |

    Należy pamiętać, że nie trzeba wprowadzać dla *identyfikator* kolumny. Podczas tworzenia *identyfikator* zestawu kolumn, jego **tożsamości jest** właściwości na wartość true, co powoduje, że jej automatycznie wypełnić.

    Po zakończeniu wprowadzania danych przez projektanta tabel będzie wyglądać następująco:

    ![[Obraz]](5-working-with-data/_static/image3.jpg)
4. Zamknij kartę zawierającą danych w bazie danych.

## <a name="displaying-data-from-a-database"></a>Wyświetlanie danych z bazy danych

Po skonfigurowaniu bazy danych z danymi w nim, dane można wyświetlić strony sieci web ASP.NET. Aby wybrać wiersze tabeli, aby wyświetlić, należy użyć instrukcji SQL, który jest polecenie, które są przekazywane do bazy danych.

1. W okienku po lewej stronie kliknij **pliki** obszaru roboczego.
2. W katalogu głównym witryny sieci Web, Utwórz nową stronę CSHTML o nazwie *ListProducts.cshtml*.
3. Zastąp istniejący kod znaczników następujące czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    W pierwszym bloku kodu, możesz otworzyć *SmallBakery.sdf* pliku (baza danych), który został utworzony wcześniej. `Database.Open` Metody, przy założeniu, że *sdf* plik znajduje się w witryny sieci Web *aplikacji\_danych* folderu. (Zwróć uwagę, że nie trzeba określać *sdf* rozszerzenia &#8212; w rzeczywistości, jeśli to zrobisz, `Open` — metoda nie będzie działać.)

    > [!NOTE]
    > *Aplikacji\_danych* jest folderem specjalne w programie ASP.NET, który jest używany do przechowywania plików danych. Aby uzyskać więcej informacji, zobacz [połączenia z bazą danych](#SB_ConnectingToADatabase) dalszej części tego artykułu.

    Następnie wprowadź żądanie przy użyciu następujących SQL w bazie danych `Select` instrukcji:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    W instrukcji `Product` identyfikuje tabeli w zapytaniu. `*` Znaku określa zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Możesz można również lista kolumn oddzielnie, oddzielonych przecinkami, jeśli chcesz wyświetlić tylko niektóre kolumny.) `Order By` Klauzuli wskazuje sposób sortowania danych &#8212; w takim przypadku przez *nazwa* kolumny. Oznacza to, że dane są sortowane w kolejności alfabetycznej na podstawie wartości z *nazwa* kolumny dla każdego wiersza.

    W treści strony znaczników tworzy tabelę HTML, który będzie używany do wyświetlania danych. Wewnątrz `<tbody>` elementu, użyj `foreach` pętli można indywidualnie uzyskać każdy wiersz danych zwracane przez zapytanie. Dla każdego wiersza danych można utworzyć wiersza tabeli HTML (`<tr>` elementu). Następnie utwórz komórek tabeli HTML (`<td>` elementy) dla każdej kolumny. Zawsze można przejść przez pętlę, następnego wiersza dostępne z bazy danych jest w `row` zmiennej (należy wybrać tę opcję `foreach` instrukcji). Aby uzyskać pojedynczej kolumny z wiersza, można użyć `row.Name` lub `row.Description` lub nazwa jest się kolumny.
4. Uruchom strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Strona wyświetlana jest lista podobne do poniższych:

    ![[Obraz]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL jest językiem, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Obejmuje on poleceń, które umożliwiają pobieranie danych i zaktualizować go i umożliwiające tworzenie, modyfikowanie i zarządzanie tabel bazy danych. SQL różni się od języka programowania (tak jak używasz programu WebMatrix) ponieważ z SQL, pomysł jest wiadomo bazy danych, co ma, czy jest zadanie bazy danych, aby dowiedzieć się, jak pobrać dane lub wykonać zadania. Poniżej przedstawiono przykłady niektórych poleceń SQL i co zrobić:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Pobiera to *identyfikator*, *nazwa*, i *cen* kolumn z rekordów *produktu* tabeli, jeśli wartość *cen* jest więcej niż 10 i zwraca wyniki w kolejności alfabetycznej na podstawie wartości z *nazwa* kolumny. To polecenie zwróci zestaw wyników, który zawiera rekordy, które spełniają kryteria lub pusty zestaw, jeśli żadne rekordy nie odpowiadają.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Wstawia nowy rekord w *produktu* tabeli ustawienie *nazwa* kolumny &quot;Croissant&quot;, *opis* kolumny &quot; Niestabilnym radością&quot;i cenę 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> To polecenie usuwa rekordy w *produktu* tabeli, którego kolumna Data wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Przy założeniu, że *produktu* Tabela oczywiście ma takie kolumny.) Tutaj podano datę w formacie MM/DD/RRRR, ale powinny być wprowadzane w formacie, który jest używany dla ustawień regionalnych użytkownika.
> 
> `Insert Into` i `Delete` poleceń nie zwracać zestawów wyników. Zamiast tego zwracają numer, informujący o tym, jak wiele rekordów wpłynęła na polecenie.
> 
> Dla niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces, który żąda operacji musi mieć odpowiednie uprawnienia w bazie danych. Jest to dlaczego dla baz danych produkcyjnych często konieczne podanie nazwy użytkownika i hasła podczas łączenia z bazą danych.
> 
> Istnieje wielu poleceń SQL, ale wszystkie one wykonać wzorzec następująco. Poleceń SQL służy do tworzenia tabel bazy danych, liczbę rekordów w tabeli obliczyć ceny i wykonywać wiele kolejnych operacji.


## <a name="inserting-data-in-a-database"></a>Wstawianie danych w bazie danych

W tej sekcji przedstawiono sposób tworzenia strony, który pozwala użytkownikom na dodawanie nowego produktu do *produktu* tabeli bazy danych. Po wstawieniu nowego rekordu produktu na stronie są wyświetlane w tabeli zaktualizowane przy użyciu *ListProducts.cshtml* strony, który został utworzony w poprzedniej sekcji.

Strona zawiera weryfikacji, aby upewnić się, że danych wprowadzonych przez użytkownika jest prawidłowy dla bazy danych. Na przykład kodu na stronie upewnia się, że wprowadzono wartość dla wszystkich wymaganych kolumn.

1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *InsertProducts.cshtml*.
2. Zastąp istniejący kod znaczników następujące czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Treść strony zawiera formularz HTML z trzy pola tekstowe, które pozwalają użytkownikom, wprowadź nazwę, opis i cenę. Po kliknięciu **Wstaw** przycisku kod w górnej części strony otwiera połączenie *SmallBakery.sdf* bazy danych. Następnie Pobierz wartości, które użytkownik zostało przesłane za pomocą `Request` obiektu i przypisać te wartości do zmiennych lokalnych.

    Aby sprawdzić, czy użytkownik wprowadził wartość dla każdej kolumny wymagane, zarejestrować każdy `<input>` element, który chcesz zweryfikować:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Pomocnika sprawdza, czy istnieje wartość we wszystkich polach, które zostały zarejestrowane. Można sprawdzić, czy wszystkie pola przeszły sprawdzanie poprawności, sprawdzając `Validation.IsValid()`, która zwykle jest to ustalane przed przetworzyć informacji można uzyskać od użytkownika:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    ( `&&` Oznacza, że operator AND — ten test jest *Jeśli jest to przesłanie formularza i wszystkich pól przeszły sprawdzanie poprawności*.)

    Jeśli wszystkie kolumny zweryfikowana (żaden nie był pusty), przejdź dalej i tworzenia instrukcji SQL, aby wstawić dane, a następnie wykonaj, jak pokazano w następnym:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    W przypadku wartości do wstawienia zawierają symbole zastępcze parametr (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Ze względów bezpieczeństwa zawsze wartości przekazywane do instrukcji SQL przy użyciu parametrów, jak widać w poprzednim przykładzie. Daje to możliwość sprawdzania poprawności danych użytkownika, a także pomaga zapewnić ochronę przed prób wysłania poleceń złośliwego do bazy danych (czasem określana jako ataki).

    Aby wykonać zapytanie, należy użyć tej instrukcji przekazanie do niego zmiennych, które zawierają wartości, aby zastąpić symbole zastępcze:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Po `Insert Into` instrukcji zostało wykonane, możesz wysłać do użytkownika do strony, która zawiera listę produktów, ten wiersz:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Jeśli weryfikacja nie powiodła się, możesz pominąć insert. Istnieją jednak pomocnika na stronie można wyświetlić komunikaty o skumulowany (jeśli istnieje):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Powiadomienie, że bloku stylu w znaczniku zawiera definicję klasy CSS, o nazwie `.validation-summary-errors`. Jest to nazwa klasy CSS, która jest używany domyślnie dla `<div>` element, który zawiera wszystkie błędy weryfikacji. W takim przypadku klasy CSS określa podsumowania błędy sprawdzania poprawności są wyświetlane na czerwono i pogrubioną, ale można zdefiniować `.validation-summary-errors` klasy do wyświetlenia formatowania chcesz.

### <a name="testing-the-insert-page"></a>Testowanie strony wstawiania

1. Wyświetl stronę w przeglądarce. Zostaje wyświetlona strona formularz, który jest podobny do tego, który jest wyświetlany na poniższej ilustracji.

    ![[Obraz]](5-working-with-data/_static/image5.jpg)
2. Wprowadź wartości dla wszystkich kolumn, ale upewnij się, że pozostanie *cen* puste kolumny.
3. Kliknij przycisk **Wstaw**. Strona z komunikatem o błędzie, jak pokazano na poniższej ilustracji. (Nie rekordu jest tworzony).

    ![[Obraz]](5-working-with-data/_static/image6.jpg)
4. Całkowicie Wypełnij formularz, a następnie kliknij przycisk **Wstaw**. Teraz, *ListProducts.cshtml* strona jest wyświetlana i przedstawia nowego rekordu.

## <a name="updating-data-in-a-database"></a>Aktualizowanie danych w bazie danych

Po wprowadzeniu danych do tabeli, należy go zaktualizować. Ta procedura pokazuje, jak utworzyć dwie strony, które są podobne do tych utworzone wcześniej wstawiania danych. Pierwsza strona przedstawia produkty i pozwala użytkownikom na wybranie jednej zmiany. Druga strona umożliwia faktycznie dokonaj zmian i zapisz je.

> [!NOTE] 
> 
> **Ważne** w środowisku produkcyjnym witryny sieci Web zwykle ograniczysz kto ma może wprowadzać zmiany w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *EditProducts.cshtml*.
2. Zastąp istniejące znacznika w pliku następujące czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Jedyną różnicą między tę stronę i *ListProducts.cshtml* strony z wcześniej jest że tabeli HTML na tej stronie zawiera dodatkowe kolumny, która wyświetla **Edytuj** łącza. Po kliknięciu tego łącza spowoduje to przejście do *UpdateProducts.cshtml* strony (który utworzysz obok) którym można edytować wybranego rekordu.

    Sprawdź kod, który tworzy **Edytuj** łącza:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Spowoduje to utworzenie kodu HTML `<a>` element którego `href` dynamicznie ustawiono atrybut. `href` Atrybut określa stronę do wyświetlenia, gdy użytkownik kliknie łącze. Przekazuje również `Id` wartość bieżący wiersz do łącza. Po uruchomieniu na stronie, źródło strony może zawierać łącza, takich jak te:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Zwróć uwagę, że `href` atrybut ma ustawioną `UpdateProducts/n`, gdzie  *n*  jest liczbą produktu. Po kliknięciu jednego z nich, wynikowy adres URL będzie wyglądać następująco:

    `http://localhost:18816/UpdateProducts/6`

    Innymi słowy numer produktu można edytowane zostaną przekazane w adresie URL.
3. Wyświetl stronę w przeglądarce. Strona wyświetla dane w formacie następująco:

    ![[Obraz]](5-working-with-data/_static/image7.jpg)

    Następnie utworzysz strona, która umożliwia użytkownikom faktycznie zaktualizowanie danych. Strona aktualizacji obejmuje weryfikację sprawdzania poprawności danych wprowadzonych przez użytkownika. Na przykład kodu na stronie upewnia się, że wprowadzono wartość dla wszystkich wymaganych kolumn.
4. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *UpdateProducts.cshtml*.
5. Zamień istniejące znacznika w pliku poniżej.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Ciała strony zawiera formularza HTML, którym jest wyświetlana produktu i gdzie użytkownicy mogą go edytować. Aby uzyskać produktu do wyświetlenia, należy użyć tej instrukcji SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    To spowoduje zaznaczenie produktu o identyfikatorze pasuje do wartości, który jest przekazywany w `@0` parametru. (Ponieważ *identyfikator* klucza podstawowego i dlatego musi być unikatowa, tylko jeden produkt rekordu kiedykolwiek można wybrać w ten sposób.) Można pobrać wartości Identyfikatora do przekazania do to `Select` instrukcji, możesz przeczytać wartość, która jest przekazywany do strony jako część adresu URL, używając następującej składni:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Faktycznie pobrać rekordu produktu, należy użyć `QuerySingle` metodę, która będzie zwracać tylko jeden rekord:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Pojedynczy wiersz jest zwracana do `row` zmiennej. Można pobrać danych z każdej kolumny i przypisz je do zmiennych lokalnych w następujący sposób:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    W znaczniku formularza, te wartości są wyświetlane automatycznie w poszczególnych polach przy użyciu osadzonego kodu podobne do poniższych:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Tej części kodu wyświetla rekord produktu do zaktualizowania. Po wyświetleniu rekord użytkownik może edytować poszczególnych kolumn.

    Gdy użytkownik przesyła formularz, klikając **aktualizacji** przycisk kod w `if(IsPost)` blokowanie działa. Pobiera to wartości użytkownika z `Request` obiektu, przechowuje wartości w zmiennych i sprawdza poprawność wypełnione każdej kolumny. Jeśli weryfikacja zakończy się pomyślnie, kod powoduje utworzenie następujących instrukcji SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    W programie SQL `Update` instrukcji, określ każdej kolumny do zaktualizowania oraz ustawić ją na wartość. W tym kodzie wartości są określone za pomocą parametru symbole zastępcze `@0`, `@1`, `@2`i tak dalej. (Jak wspomniano wcześniej, zabezpieczeń, należy zawsze przekazać wartości do instrukcji SQL przy użyciu parametrów.)

    Podczas wywoływania `db.Execute` metody, należy przekazać zmiennych, które zawierają wartości w kolejności odpowiadającej parametrów w instrukcji SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Po `Update` instrukcji zostało wykonane, wywołanie następującej metody w celu przekieruje użytkownika z powrotem do edycji strony:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Powoduje, że użytkownik widzi zaktualizowaną listę danych w bazie danych i edytować innego produktu.
6. Zapisz stronę.
7. Uruchom *EditProducts.cshtml* strony (nie stronę aktualizacji), a następnie kliknij przycisk **Edytuj** wybrać produkt do edycji. *UpdateProducts.cshtml* strony zostanie wyświetlone wybranego rekordu.

    ![[Obraz]](5-working-with-data/_static/image8.jpg)
8. Zmiany, a następnie kliknij przycisk **aktualizacji**. Na liście produktów jest ponownie wyświetlone zaktualizowanych danych.

## <a name="deleting-data-in-a-database"></a>Usuwanie danych z bazy danych

W tej sekcji przedstawiono sposób zezwolić użytkownikom na usuwanie produktu z *produktu* tabeli bazy danych. Przykład zawiera dwie strony. Na pierwszej stronie użytkowników wybierz rekordy do usunięcia. Usunięcie rekordu jest następnie wyświetlane w drugiej stronie, które umożliwi upewnij się, czy chcą usunąć rekord.

> [!NOTE] 
> 
> **Ważne** w środowisku produkcyjnym witryny sieci Web zwykle ograniczysz kto ma może wprowadzać zmiany w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *ListProductsForDelete.cshtml*.
2. Zastąp istniejący kod znaczników następujące czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Ta strona jest podobny do *EditProducts.cshtml* strony z wcześniej. Jednak zamiast **Edytuj** łącze dla każdego produktu Wyświetla **usunąć** łącza. **Usunąć** link jest tworzony przy użyciu następującego kodu osadzone w znaczniku:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Spowoduje to utworzenie adresu URL, który wygląda jak to, gdy użytkownik kliknie łącze:

    `http://<server>/DeleteProduct/4`

    Adres URL wywołania stronę o nazwie *DeleteProduct.cshtml* (który utworzysz obok) i przekazuje jego identyfikator produktu do usunięcia (tutaj, 4).
3. Zapisz plik, ale zostawić otwarty.
4. Utwórz plik CHTML o nazwie *DeleteProduct.cshtml*. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Ta strona jest wywoływana przez *ListProductsForDelete.cshtml* i umożliwia użytkownikom, upewnij się, że chcą usunąć produkt. Aby wyświetlić listę produktów, które mają zostać usunięte, można uzyskać Identyfikatora produktu do usunięcia z adresu URL przy użyciu następującego kodu:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Strona następnie z monitem o kliknąć przycisk, aby usunąć rekord, faktycznie. Jest to zabezpieczenie ważne: podczas wykonywania operacji poufnych w witrynie sieci Web, takich jak aktualizowania lub usuwania danych, należy zawsze wykonać te operacje przy użyciu operacji POST, nie operacji GET. Jeśli witryna jest skonfigurowana tak, aby operację usuwania można wykonać przy użyciu operacji GET, każdy użytkownik może przekazać adres URL podobny `http://<server>/DeleteProduct/4` i cokolwiek chcą usuwania z bazy danych. Dodawanie potwierdzenie i kodowania strony, dzięki czemu usunięcia można wykonać tylko przy użyciu ogłoszenie (POST), możesz dodać miarę zabezpieczeń do swojej witryny.

    Operacja usuwania rzeczywiste odbywa się przy użyciu następujący kod, który najpierw sprawdza, czy jest to operacja post i czy identyfikator nie jest pusty:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Kod działa na instrukcję SQL, która usuwa określonego rekordu, a następnie przekierowuje użytkownika do strony listy.
5. Uruchom *ListProductsForDelete.cshtml* w przeglądarce.

    ![[Obraz]](5-working-with-data/_static/image9.jpg)
6. Kliknij przycisk **usunąć** łącze do jednego z produktów. *DeleteProduct.cshtml* zostanie wyświetlona strona, aby potwierdzić usunięcie tego rekordu.
7. Kliknij przycisk **usunąć** przycisku. Rekord produkt zostanie usunięty i odświeżeniu strony z listą zaktualizowane produktu.

> [!TIP] 
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Połączenia z bazą danych
> 
> Można połączyć się z bazą danych na dwa sposoby. Pierwsza to użycie `Database.Open` — metoda i określ nazwę pliku bazy danych (mniej *sdf* rozszerzenia):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open` Metody, przy założeniu, że. *sdf* plik znajduje się w witrynie sieci Web *aplikacji\_danych* folderu. Ten folder jest zaprojektowany specjalnie z myślą przechowywania danych. Na przykład, że ma ona odpowiednie uprawnienia, aby umożliwić witryny sieci Web do odczytywania i zapisywania danych i ze względów bezpieczeństwa program WebMatrix nie zezwala na dostęp do plików z tego folderu.
> 
> Drugim sposobem jest używane są parametry połączenia. Parametry połączenia zawierają informacje na temat nawiązywania połączenia z bazą danych. Mogą to być ścieżka pliku lub może obejmować nazwę bazy danych programu SQL Server na serwerze lokalnym lub zdalnym, oraz nazwę użytkownika i hasło, aby połączyć się z serwerem. (Jeśli dane są przechowywane w centralnie zarządzanej wersji programu SQL Server, takich jak witryny dostawcy hostingu zawsze umożliwia ciąg połączenia Określ informacje o połączeniu.)
> 
> W programie WebMatrix, parametry połączenia są zwykle przechowywane w pliku XML o nazwie *Web.config*. Jak wskazuje nazwę, można użyć *Web.config* w katalogu głównym witryny sieci Web do przechowywania informacji o konfiguracji witryny, w tym wszelkie parametry połączenia, które mogą wymagać witryny. Przykładem ciągu połączenia w *Web.config* pliku może wyglądać następująco:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> W tym przykładzie wskazuje ciąg połączenia bazy danych w wystąpieniu programu SQL Server, który jest uruchomiony na innym serwerze (a nie lokalnie *sdf* pliku). Należy zastąpić odpowiednie nazwy dla `myServer` i `myDatabase`i określ wartości logowania programu SQL Server dla `username` i `password`. (Wartości nazwy użytkownika i hasła nie są zawsze taki sam jako poświadczeń systemu Windows lub wartości, które Twój dostawca hostingu podane do logowania do swoich serwerów. Skontaktuj się z administratorem, aby uzyskać dokładne wartości, które są potrzebne.)
> 
> `Database.Open` — Metoda jest elastyczny, ponieważ umożliwia ona przekazywania nazwy bazy danych *sdf* Nazwa ciągu połączenia, który jest przechowywany w pliku lub *Web.config* pliku. Poniższy przykład przedstawia sposób nawiązywania połączenia z bazą danych przy użyciu parametrów połączenia, przedstawiono w poprzednim przykładzie:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Jak wspomniano, `Database.Open` metoda pozwala przekazać nazwę bazy danych lub ciąg połączenia i jego określenie, który ma zostać użyty. Jest to bardzo przydatne podczas wdrażania (publikowanie) witryny sieci Web. Można użyć *sdf* w pliku *aplikacji\_danych* folderu, gdy w przypadku tworzenia i testowania witryny. Po przeniesieniu witryny na serwerze produkcyjnym, możesz użyć ciągu połączenia w *Web.config* pliku, który ma taką samą nazwę jak Twoje *sdf* plik, ale się do bazy danych dostawcy hostingu & # 8212; wszystko to bez konieczności zmiany kodu.
> 
> Ponadto jeśli chcesz pracować bezpośrednio z ciągu połączenia, można wywołać `Database.OpenConnectionString` — metoda i przekazać go parametrów zamiast nazwy tylko w rzeczywistych połączenia *Web.config* pliku. Może to być przydatne w sytuacjach, w którym jakiegoś powodu nie mają dostępu do ciągu połączenia (lub wartości, takich jak *sdf* nazwa pliku) do momentu strony jest uruchomiona. Jednak w przypadku większości scenariuszy należy użyć `Database.Open` zgodnie z opisem w tym artykule.


## <a name="additional-resources"></a>Dodatkowe zasoby

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Łączenie z serwerem SQL lub bazy danych MySQL w programie WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Walidacja danych wejściowych użytkownika w lokacjach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
