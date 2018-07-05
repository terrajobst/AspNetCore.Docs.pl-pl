---
uid: web-pages/overview/data/5-working-with-data
title: Wprowadzenie do pracy z bazą danych we wzorcu ASP.NET Web Pages (Razor) witryn | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano, jak uzyskać dostęp do danych z bazy danych i wyświetlania ich przy użyciu stron ASP.NET Web Pages.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 1ee2f680d6088ad786022b3c590c33e4e25e04db
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363361"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Wprowadzenie do pracy z bazą danych we wzorcu ASP.NET Web Pages (Razor) witryn
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano, jak utworzyć bazę danych w witrynie internetowej ASP.NET Web Pages (Razor) za pomocą narzędzia Microsoft WebMatrix oraz sposób tworzenia stron, które pozwalają na wyświetlanie, dodawanie, edytowanie i usuwanie danych.
> 
> **Zawartość:** 
> 
> - Jak utworzyć bazę danych.
> - Jak połączyć się z bazą danych.
> - Jak wyświetlać dane na stronie sieci web.
> - Jak wstawianie, aktualizowanie i usuwanie rekordów bazy danych.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule:
> 
> - Praca z bazą danych programu Microsoft SQL Server Compact Edition.
> - Praca z zapytania SQL.
> - `Database` Klasy.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> W tym samouczku współpracuje również z programu WebMatrix 3. Można użyć 3 stron sieci Web platformy ASP.NET i programu Visual Studio 2013 (lub Visual Studio Express 2013 for Web); Jednak interfejs użytkownika może się różnić.


## <a name="introduction-to-databases"></a>Wprowadzenie do bazy danych

Wyobraź sobie książki adresowej typowy. Dla każdego wpisu w książce adresowej (oznacza to, że dla każdej osoby nawiązującej) ma kilka rodzajów informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

Typowym sposobem obraz dane, takie jak to jest tabela z wierszami i kolumnami. Każdy wiersz względem bazy danych, jest często określane jako rekord. Każda kolumna (czasami określane jako pola) zawiera wartość dla każdego typu danych: imię, ostatni nazwy i tak dalej.

| **ID** | **Imię** | **Nazwisko** | **Adres** | **Poczta e-mail** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Jakub | Adams | 1234 Main St. Seattle (Waszyngton) 99011 | terry@cohowinery.com | 555 0101 |

W przypadku większości tabel bazy danych Tabela musi mieć kolumny, która zawiera unikatowy identyfikator, takich jak numer klienta, numer konta itp. Jest to nazywane tabeli *klucz podstawowy*, i używa ich do identyfikacji każdego wiersza w tabeli. W tym przykładzie kolumna Identyfikator jest kluczem podstawowym książki adresowej.

Z tego podstawową wiedzę na temat baz danych, możesz dowiedzieć się, jak tworzenie prostej bazy danych i wykonywać operacje takie jak dodawanie, modyfikowanie i usuwanie danych.

> [!TIP] 
> 
> **Relacyjne bazy danych**
> 
> Dane można przechowywać w wiele różnych sposobów, w tym plików tekstowych i arkuszy kalkulacyjnych. Dla większości zastosowań biznesowych jednak dane są przechowywane w relacyjnej bazie danych.
> 
> W tym artykule nie działa bardzo głęboko do baz danych. Jednak może okazać grupowaniu można sprawdzić trochę informacji o nich. Informacje w relacyjnej bazie danych, logicznie odbywa się w oddzielnych tabelach. Na przykład bazę danych dla szkoły może zawierać oddzielnych tabelach dla uczniów lub studentów i ofert klasa. Bazy danych oprogramowania (np. SQL Server) obsługuje zaawansowane polecenia, które umożliwiają dynamiczne ustanawiania relacji między tabelami. Na przykład można użyć relacyjnej bazy danych można ustanowić relacji logicznych między uczniami i klasami w celu utworzenia harmonogramu. Przechowywanie danych w oddzielnych tabelach zmniejsza złożoność strukturę tabeli i zmniejsza trzeba utrzymywać nadmiarowych danych w tabelach.


## <a name="creating-a-database"></a>Tworzenie bazy danych

Ta procedura pokazuje, jak utworzyć bazę danych o nazwie SmallBakery przy użyciu narzędzia do projektowania bazy danych programu SQL Server Compact, który znajduje się w programie WebMatrix. Chociaż można utworzyć bazę danych przy użyciu kodu, jest bardziej typowego, aby utworzyć bazę danych i tabel bazy danych przy użyciu narzędzia projektowania, takiego jak program WebMatrix.

1. Uruchom program WebMatrix, a na stronie Szybki Start kliknij **lokacji za pomocą szablonu**.
2. Wybierz **pusta witryna**, a następnie w **Nazwa lokacji** polu wprowadź "SmallBakery", a następnie kliknij przycisk **OK**. Witryna jest tworzony i wyświetlany w programie WebMatrix.
3. W okienku po lewej stronie kliknij **baz danych** obszaru roboczego.
4. Na wstążce kliknij **nową bazę danych**. Pusta baza danych jest tworzony o takiej samej nazwie jako lokacji.
5. W okienku po lewej stronie rozwiń **SmallBakery.sdf** węzeł, a następnie kliknij przycisk **tabel**.
6. Na wstążce kliknij **nową tabelę**. Program WebMatrix zostanie otwarty projektant tabel.

    ![[image]](5-working-with-data/_static/image1.jpg)
7. Kliknij w **nazwa** kolumny i wprowadzić &quot;identyfikator&quot;.
8. W **— typ danych** kolumny wybierz **int**.
9. Ustaw **jest klucz podstawowy?** i **jest zidentyfikować?** opcji **tak**.

    Jak sugeruje nazwa, **jest kluczem podstawowym** bazy danych informuje, że będzie to klucz podstawowy tabeli. **Jest tożsamością** informuje bazy danych, aby automatycznie utworzyć numeru Identyfikacyjnego dla każdego nowego rekordu i przypisać ją dalej numeru sekwencyjnego (rozpoczyna się od 1).
10. Kliknij w kolejnym wierszu. Edytor rozpoczyna się nowa definicja kolumny.
11. Wartość nazwy można wprowadzić &quot;nazwa&quot;.
12. Aby uzyskać **— typ danych**, wybierz &quot;nvarchar&quot; i ustawić długość na 50. *Var* wchodzi w skład `nvarchar` bazy danych informuje, że dane dla tej kolumny będzie ciąg, którego rozmiar może się różnić między rekordami. ( *n* prefiksu reprezentuje *national*, wskazujący, że pole może zawierać dane znaków, które przedstawiają żadnych alfabetu lub systemie pisma &#8212; oznacza to, że pole zawiera dane Unicode.)
13. Ustaw **Zezwalaj na wartości null** opcję **nie**. To wymusi, *nazwa* kolumna nie jest pusty.
14. Za pomocą tego samego procesu, Utwórz kolumnę o nazwie *opis*. Ustaw **— typ danych** "nvarchar" i 50 dla długości i zestaw **Zezwalaj na wartości null** o wartości false.
15. Utwórz kolumnę o nazwie *cena*. Ustaw **typu danych "walutowy"** i ustaw **Zezwalaj na wartości null** o wartości false.
16. Nadaj tabeli nazwę w polu u góry &quot;produktu&quot;.

    Gdy wszystko będzie gotowe, definicja będzie wyglądać następująco:

    ![[image]](5-working-with-data/_static/image2.jpg)
17. Naciśnij klawisze Ctrl + S, aby zapisać w tabeli.

## <a name="adding-data-to-the-database"></a>Dodawanie danych do bazy danych

Teraz możesz dodać przykładowe dane do bazy danych, którą będziesz pracować w dalszej części tego artykułu.

1. W okienku po lewej stronie rozwiń **SmallBakery.sdf** węzeł, a następnie kliknij przycisk **tabel**.
2. Kliknij prawym przyciskiem myszy tabelę produktu, a następnie kliknij przycisk **danych**.
3. W okienku edytowania wprowadź poniższe rekordy:

    | **Nazwa** | **Opis** | **Cena** |
    | --- | --- | --- |
    | Linki | Wbudowanymi świeży każdego dnia. | 2.99 |
    | Strawberry Shortcake | Nawiązywane z organicznych strawberries z naszych ogrodzie. | 9.99 |
    | Kołowy firmy Apple | Drugi tylko na Twój Tato koło. | 12.99 |
    | Kołowy pecan | Jeśli wolisz pecans, to dla Ciebie. | 10.99 |
    | Tarta kołowy | Wprowadzone za pomocą najlepszych cytryn na całym świecie. | 11.99 |
    | Cupcakes | Twoje dzieci i dla dzieci w przypadku pokochają je. | 7.99 |

    Należy pamiętać, że nie trzeba wpisywać nic dla *identyfikator* kolumny. Podczas tworzenia *identyfikator* ustawić kolumny, jego **tożsamości jest** właściwość na true, co powoduje, że automatycznie wypełniane.

    Po zakończeniu wprowadzania danych w Projektancie tabel będzie wyglądać następująco:

    ![[image]](5-working-with-data/_static/image3.jpg)
4. Zamknij kartę która zawiera dane z bazy danych.

## <a name="displaying-data-from-a-database"></a>Wyświetlanie danych z bazy danych

Gdy już masz bazę danych z danymi w nim dane można wyświetlić strony sieci web ASP.NET. Aby wybrać wiersze tabeli, aby wyświetlić, należy użyć instrukcji SQL, który jest poleceniem, który jest przekazywany do bazy danych.

1. W okienku po lewej stronie kliknij **pliki** obszaru roboczego.
2. W katalogu głównym witryny sieci Web, Utwórz nową stronę CSHTML o nazwie *ListProducts.cshtml*.
3. Zastąp istniejący kod znaczników następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    W pierwszym bloku kodu, możesz otworzyć *SmallBakery.sdf* pliku (baza danych), który został utworzony wcześniej. `Database.Open` Metoda zakłada, że *.sdf* plik znajduje się w swojej witrynie sieci Web *aplikacji\_danych* folderu. (Zwróć uwagę, że nie trzeba określać *.sdf* rozszerzenia &#8212; w rzeczywistości, jeśli to zrobisz, `Open` metoda nie będzie działać.)

    > [!NOTE]
    > *Aplikacji\_danych* jest folderem specjalne w programie ASP.NET, który służy do przechowywania plików danych. Aby uzyskać więcej informacji, zobacz [nawiązywania połączenia z bazą danych](#SB_ConnectingToADatabase) w dalszej części tego artykułu.

    Następnie zgłosić wniosek o przy użyciu następujących SQL w bazie danych `Select` instrukcji:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    W instrukcji `Product` Określa tabelę, aby wykonać zapytanie. `*` Znak Określa, że zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Można podać również listę kolumn pojedynczo, oddzielając je średnikami, jeśli chcesz zobaczyć tylko niektóre kolumny.) `Order By` Klauzuli wskazuje, jak powinny być sortowane dane &#8212; w tym przypadku przez *nazwa* kolumny. Oznacza to, że dane są sortowane w kolejności alfabetycznej na podstawie wartości z *nazwa* kolumny dla każdego wiersza.

    W treści strony kodu znaczników tworzy tabelę HTML, która będzie służyć do wyświetlania danych. Wewnątrz `<tbody>` elementu, użyj `foreach` pętli można pobrać osobno każdy wiersz danych, który jest zwracany przez zapytanie. Dla każdego wiersza danych można utworzyć wiersza tabeli HTML (`<tr>` elementu). Następnie utwórz komórek tabeli HTML (`<td>` elementy) dla każdej kolumny. Zawsze możesz przejść za pomocą pętli, następny wiersz dostępne z bazy danych jest w `row` zmiennej (należy wybrać tę opcję `foreach` instrukcji). Aby uzyskać poszczególnych kolumn z wiersza, możesz użyć `row.Name` lub `row.Description` lub dowolną nazwę, jak to się kolumny.
4. Uruchom stronę w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Strony wyświetli listę, jak pokazano poniżej:

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Język Structured Query Language (SQL)**
> 
> SQL jest językiem, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Zawiera polecenia umożliwiające pobieranie danych i zaktualizować go i umożliwiające tworzenie, modyfikowanie i zarządzanie tabel bazy danych. SQL różni się od języka programowania (takiego jak używane w programie WebMatrix), ponieważ przy użyciu języka SQL, chodzi o to, że Poinformuj bazy danych należy, i jest zadanie bazy danych, aby dowiedzieć się, jak można pobrać dane lub wykonać zadania. Poniżej przedstawiono przykłady niektórych poleceń SQL oraz ich działania:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Pobiera to *identyfikator*, *nazwa*, i *cena* kolumn z rekordów *produktu* tabeli, jeśli wartość *cena* jest więcej niż 10 i zwraca wyniki w kolejności alfabetycznej na podstawie wartości z *nazwa* kolumny. To polecenie zwróci zestaw wyników, który zawiera rekordy, które spełniają kryteria lub pusty zestaw, jeśli żadne rekordy nie odpowiadają.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Spowoduje to wstawienie nowego rekordu w *produktu* tabeli, ustawienie *nazwa* kolumny &quot;Croissant&quot;, *opis* kolumny &quot; Niestabilne wzbudzanie&quot;i cenę 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> To polecenie usuwa rekordy w *produktu* tabeli, którego kolumna Data wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Ta zakłada, że *produktu* tabela ma kolumnę, oczywiście.) W tym miejscu podano daty w formacie MM/DD/RRRR, ale powinny być wprowadzane w formacie, który jest używany dla ustawień regionalnych.
> 
> `Insert Into` i `Delete` polecenia nie zwracają zestaw wyników. Zamiast tego zwracają liczbę, która informuje, jak wiele rekordów wpłynęła na polecenie.
> 
> W przypadku niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces, który żąda operacji musi mieć odpowiednie uprawnienia w bazie danych. To dlatego produkcyjnych baz danych, często musisz podać nazwę użytkownika i hasło, po nawiązaniu połączenia z bazą danych.
> 
> Istnieją dziesiątek poleceń SQL, ale wszystkie one wykonać wzorzec następująco. Można użyć poleceń SQL do tworzenia tabel bazy danych, liczby rekordów w tabeli, Oblicz ceny i wykonuje wiele więcej operacji.


## <a name="inserting-data-in-a-database"></a>Wstawianie danych w bazie danych

W tej sekcji pokazano, jak utworzyć stronę, która umożliwia użytkownikom dodawanie nowego produktu do *produktu* tabeli bazy danych. Po wstawieniu nowego rekordu produktu, zostanie wyświetlona strona zaktualizowane tabeli przy użyciu *ListProducts.cshtml* strony, który został utworzony w poprzedniej sekcji.

Strona zawiera sprawdzania poprawności, aby upewnić się, że dane wprowadzonych przez użytkownika są prawidłowe dla bazy danych. Na przykład kod na stronie sprawia, że się upewnić, że wartość zostanie wprowadzona dla wszystkich wymaganych kolumn.

1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *InsertProducts.cshtml*.
2. Zastąp istniejący kod znaczników następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Treść strony zawiera formularza HTML przy użyciu trzy pola tekstowe, które pozwalają użytkownikom, wprowadź nazwę, opis i cena. Gdy użytkownik kliknie **Wstaw** przycisku, kod w górnej części strony otwiera połączenie *SmallBakery.sdf* bazy danych. Następnie Pobierz wartości, które użytkownik zostało przesłane za pomocą `Request` obiektu i przypisać te wartości do zmiennych lokalnych.

    Aby sprawdzić, czy użytkownik wprowadził wartość dla każdej wymaganej kolumny, możesz zarejestrować każdy `<input>` element, który ma być sprawdzone:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Pomocnika sprawdza, czy istnieje wartość każdego z pól, które zostały zarejestrowane. Możesz sprawdzić, czy wszystkie pola przeszły sprawdzanie poprawności, sprawdzając `Validation.IsValid()`, które zwykle są przed rozpoczęciem przetwarzania informacji otrzymasz od użytkownika:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    ( `&&` Oznacza, że operator AND — ten test jest *Jeśli jest to przesłanie formularza i wszystkich pól przeszły sprawdzanie poprawności*.)

    Jeśli wszystkie kolumny zweryfikowana (żaden nie był pusty), przejdź dalej i tworzenie instrukcji SQL, aby wstawić dane, a następnie uruchomić go tak, jak pokazano dalej:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Dla wartości do wstawienia zawierają symbole zastępcze parametru (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Ze względów bezpieczeństwa zawsze przekazuj parametr wartości do instrukcji SQL przy użyciu parametrów, jak widać w powyższym przykładzie. Daje to możliwość sprawdzania poprawności danych użytkownika, a także pomaga chronić przed próbami wysyłać szkodliwe polecenia bazy danych (czasami określane jako ataki przez iniekcję SQL).

    Aby wykonać zapytanie, użyjesz tej instrukcji przekazanie do niego zmiennych, które zawierają wartości, aby zastąpić symbole zastępcze:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Po `Insert Into` instrukcji zostało wykonane, wysłać użytkownika do strony, która zawiera listę produktów, ten wiersz:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Jeśli weryfikacja nie powiodła się, możesz pominąć insert. Istnieją jednak pomocnika na stronie, który może wyświetlać komunikaty o błędach skumulowana (jeśli istnieje):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Zwróć uwagę, że bloku stylu w znaczniku zawiera definicję klasy CSS, o nazwie `.validation-summary-errors`. Jest to nazwa klasy CSS, która jest używana domyślnie `<div>` element, który zawiera wszystkie błędy weryfikacji. W tym przypadku klasy CSS określa podsumowanie błędów sprawdzania poprawności są wyświetlane w kolorze czerwonym, a pogrubioną, ale można zdefiniować `.validation-summary-errors` klasy, aby wyświetlić formatowanie chcesz.

### <a name="testing-the-insert-page"></a>Testowanie strony Insert

1. Wyświetl stronę w przeglądarce. Zostanie wyświetlona strona formularz, który jest podobny do tego, który jest wyświetlany na poniższej ilustracji.

    ![[image]](5-working-with-data/_static/image5.jpg)
2. Wprowadź wartości dla wszystkich kolumn, ale upewnij się, że opuści *cena* puste kolumny.
3. Kliknij przycisk **Wstaw**. Strony wyświetli komunikat o błędzie, jak pokazano na poniższej ilustracji. (Nie nowy rekord zostanie utworzony).

    ![[image]](5-working-with-data/_static/image6.jpg)
4. Całkowicie wypełnić formularz, a następnie kliknij przycisk **Wstaw**. Tym razem *ListProducts.cshtml* stronie jest wyświetlane i pokazuje nowy rekord.

## <a name="updating-data-in-a-database"></a>Aktualizowanie danych w bazie danych

Po wprowadzeniu danych do tabeli, należy go zaktualizować. Ta procedura pokazuje, jak utworzyć dwie strony, które są podobne do tych utworzonych dla wcześniej wstawiania danych. Pierwsza strona przedstawia produkty i pozwala użytkownikom na wybór jednego można zmienić. Druga strona umożliwia faktycznie dokonaj zmian i zapisać je.

> [!NOTE] 
> 
> **Ważne** w produkcyjnej witrynie internetowej można zwykle ograniczają kto ma prawo do wprowadzania zmian w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *EditProducts.cshtml*.
2. Zastąp istniejący kod znaczników w pliku następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Jedyną różnicą między tę stronę i *ListProducts.cshtml* strony z wcześniej to że tabela HTML na tej stronie zawiera dodatkowa kolumna, która wyświetla **Edytuj** łącza. Po kliknięciu tego łącza, spowoduje to przejście do *UpdateProducts.cshtml* stronie (które następnie utworzymy) umożliwiające edytowanie wybranego rekordu.

    Spójrz na kod, który tworzy **Edytuj** łącza:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Spowoduje to utworzenie kodu HTML `<a>` elementu którego `href` ma ustawioną wartość atrybutu dynamicznie. `href` Atrybut określa stronę, aby wyświetlić, gdy użytkownik kliknie łącze. Przekazuje także `Id` wartości bieżącego wiersza do łącza. Po uruchomieniu na stronie, źródło strony może zawierać linki, takie jak te:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Należy zauważyć, że `href` ma ustawioną wartość atrybutu `UpdateProducts/n`, gdzie *n* jest numer produktu. Po kliknięciu jednego z poniższych linków, wynikowy adres URL będzie wyglądać następująco:

    `http://localhost:18816/UpdateProducts/6`

    Innymi słowy numer produktu do edycji zostaną przekazane w adresie URL.
3. Wyświetl stronę w przeglądarce. Strona wyświetla dane w formacie następująco:

    ![[image]](5-working-with-data/_static/image7.jpg)

    Następnie utworzysz stronę która umożliwia użytkownikom faktycznie aktualizacji danych. Strona aktualizacji obejmuje sprawdzania poprawności, aby sprawdzić poprawność danych wprowadzonych przez użytkownika. Na przykład kod na stronie sprawia, że się upewnić, że wartość zostanie wprowadzona dla wszystkich wymaganych kolumn.
4. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *UpdateProducts.cshtml*.
5. Zastąp istniejący kod znaczników w pliku następujących czynności.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Ciała strony zawiera formularza HTML, którym jest wyświetlana produktu i gdzie użytkownicy mogą go edytować. Aby uzyskać produkt, aby wyświetlić, należy użyć tej instrukcji SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    To powoduje zaznaczenie produktu o identyfikatorze pasuje do wartości, które zostały przekazane `@0` parametru. (Ponieważ *identyfikator* jest klucz podstawowy i dlatego musi być unikatowa, rekord tylko jeden produkt nigdy nie można wybrać w ten sposób.) Aby uzyskać wartość identyfikator, aby przejść do tego `Select` instrukcji, można odczytać wartości, który jest przekazywany do strony jako część adresu URL, używając następującej składni:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Aby rzeczywiście pobrać rekordu produktu, należy użyć `QuerySingle` metody, która będzie zwracać tylko jeden rekord:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Pojedynczy wiersz jest zwracany do `row` zmiennej. Możesz pobrać dane poza kolumnami i przypisać ją do zmiennych lokalnych w następujący sposób:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    W znacznikach formularza, te wartości są wyświetlane automatycznie w polach tekstowych poszczególnych przy użyciu osadzonego kodu podobne do następującego:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Tę część kodu wyświetla rekord produktu do zaktualizowania. Po wyświetleniu rekord użytkownik może edytować poszczególnych kolumn.

    Gdy użytkownik przesyła formularz, klikając **aktualizacji** przycisk kod w `if(IsPost)` block przebiegów. Spowoduje pobranie wartości przez użytkownika z `Request` obiektów, wartości są przechowywane w zmiennych i sprawdza poprawność wypełnione każdej kolumny. Jeśli weryfikacja zakończy się pomyślnie, ten kod tworzy następującą instrukcję SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    W języku SQL `Update` instrukcji, należy określić każdej kolumny, aktualizacji i ustaw ją na wartość. W tym kodzie wartości są określane przy użyciu symboli zastępczych parametr `@0`, `@1`, `@2`i tak dalej. (Jak wspomniano wcześniej, aby zapewnić bezpieczeństwo, należy zawsze wartości przekazuje się do instrukcji SQL przy użyciu parametrów.)

    Gdy wywołujesz `db.Execute` metody przekazywania zmiennych, które zawierają wartości w kolejności, która odnosi się do parametrów w instrukcji SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Po `Update` została wykonana instrukcja, wywołać następującą metodę w celu przekieruje użytkownika z powrotem do strony edytowania:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Powoduje, że użytkownik widzi zaktualizowaną listę w bazie danych i edytować innego produktu.
6. Zapisz stronę.
7. Uruchom *EditProducts.cshtml* stronę (a nie strona aktualizacji), a następnie kliknij przycisk **Edytuj** wybrać produkt do edycji. *UpdateProducts.cshtml* zostanie wyświetlona strona, przedstawiający rekord wybrany.

    ![[image]](5-working-with-data/_static/image8.jpg)
8. Wprowadź zmiany, a następnie kliknij przycisk **aktualizacji**. Na liście produktów ponownie jest wyświetlany za pomocą zaktualizowanych danych.

## <a name="deleting-data-in-a-database"></a>Usuwanie danych z bazy danych

W tej sekcji pokazano, jak można zezwolić użytkownikom na usuwanie produktu z *produktu* tabeli bazy danych. Przykład składa się z dwóch stron. Na pierwszej stronie użytkowników, wybierz rekordy do usunięcia. Usunięcie rekordu są następnie wyświetlane na drugiej stronie, umożliwiającą upewnij się, że chce usunąć rekord.

> [!NOTE] 
> 
> **Ważne** w produkcyjnej witrynie internetowej można zwykle ograniczają kto ma prawo do wprowadzania zmian w danych. Aby uzyskać informacje dotyczące sposobu konfigurowania członkostwa i sposobów autoryzacji użytkowników do wykonywania zadań w lokacji, zobacz [Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. W witrynie sieci Web, Utwórz nowy plik CSHTML o nazwie *ListProductsForDelete.cshtml*.
2. Zastąp istniejący kod znaczników następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Ta strona jest podobne do *EditProducts.cshtml* strony wcześniej. Jednak zamiast wyświetlanie **Edytuj** link dla każdego produktu, wyświetla **Usuń** łącza. **Usuń** utworzeniu łącza, używając następującego kodu osadzonego w znaczniku:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Spowoduje to utworzenie adresu URL, który wygląda jak to, gdy użytkownik kliknie łącze:

    `http://<server>/DeleteProduct/4`

    Adres URL wywołania stronę o nazwie *DeleteProduct.cshtml* (które utworzysz w obok) i przekazuje je identyfikator produktu do usunięcia (tutaj 4).
3. Zapisz plik, ale pozostanie on otwarty.
4. Utwórz inny plik CHTML o nazwie *DeleteProduct.cshtml*. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Ta strona jest wywoływana przez *ListProductsForDelete.cshtml* i umożliwia użytkownikom, upewnij się, że chce usunąć produkt. Aby wyświetlić listę produktów, które mają zostać usunięte, otrzymasz identyfikator produktu do usunięcia z adresu URL przy użyciu następującego kodu:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Strona następnie prosi użytkownika o kliknij przycisk, aby rzeczywiście Usuń rekord. Jest to ważne zabezpieczenie: podczas wykonywania operacji poufnych w witrynie sieci Web, takich jak aktualizowanie i usuwanie danych, zawsze należy przeprowadzić te operacje przy użyciu operacji POST, a nie operacji pobierania. Jeśli witryna jest skonfigurowana tak, aby operacji usuwania mogą być wykonywane przy użyciu operacji pobierania, każda osoba przekazać adresu URL typu `http://<server>/DeleteProduct/4` i usuwanie czegokolwiek mają ze swojej bazy danych. Dodawanie potwierdzenia i kodowania strony, czemu usuwania można wykonać tylko przy użyciu wpisu, zabezpieczeń jest dodawanie do swojej witryny.

    Operacja usunięcia rzeczywistego jest wykonywane, używając następującego kodu, najpierw sprawdza, czy jest to operacja post i czy identyfikator nie jest pusty:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Kod uruchamia instrukcję SQL, która usuwa określony rekord i następnie przekierowuje użytkownika na stronie listy.
5. Uruchom *ListProductsForDelete.cshtml* w przeglądarce.

    ![[image]](5-working-with-data/_static/image9.jpg)
6. Kliknij przycisk **Usuń** link do jednego z produktów. *DeleteProduct.cshtml* zostanie wyświetlona strona, aby upewnić się, że chcesz usunąć tego rekordu.
7. Kliknij przycisk **Usuń** przycisku. Rekord produktu zostanie usunięty, a strona zostanie odświeżona Lista zaktualizowanych produktów.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Nawiązywanie połączenia z bazą danych
> 
> Można połączyć się z bazą danych na dwa sposoby. Pierwsza to użycie `Database.Open` metodę i określić nazwę pliku bazy danych (mniej *.sdf* rozszerzenia):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open` Metoda zakłada, że. *sdf* plik znajduje się w witrynie sieci Web firmy *aplikacji\_danych* folderu. Ten folder jest przeznaczona specjalnie do przechowywania danych. Na przykład ma ona odpowiednie uprawnienia, aby zezwolić na witrynie sieci Web do odczytu i zapisu danych, a następnie ze względów bezpieczeństwa program WebMatrix nie zezwala na dostęp do plików z tego folderu.
> 
> Druga metoda jest Użyj parametrów połączenia. Parametry połączenia zawierają informacje o tym, jak połączyć się z bazą danych. Mogą to być ścieżka pliku lub może ona zawierać nazwę bazy danych programu SQL Server na serwerze lokalnym lub zdalnym wraz z nazwą użytkownika i hasło, aby połączyć się z tym serwerem. (Jeśli dane są przechowywane w centralnie zarządzanej wersji programu SQL Server, takich jak witryny dostawcy hostingu, zawsze umożliwia parametry połączenia Określ informacje o połączeniu.)
> 
> W programie WebMatrix, parametry połączenia są zwykle przechowywane w pliku XML o nazwie *Web.config*. Jak wskazuje nazwa, można użyć *Web.config* plik w folderze głównym witryny sieci Web do przechowywania informacji o konfiguracji witryny, w tym wszelkie parametry połączenia, które mogą wymagać witryny. Przykład parametrów połączenia w *Web.config* pliku może wyglądać podobnie do poniższego:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> W tym przykładzie parametry połączenia wskazują na bazę danych w wystąpieniu programu SQL Server, który jest uruchomiony na innym serwerze (w przeciwieństwie do lokalnego *.sdf* pliku). Czy trzeba zastąpić odpowiednie nazwy dla `myServer` i `myDatabase`, a następnie określ wartości logowania programu SQL Server dla `username` i `password`. (Wartości nazwy użytkownika i hasła nie są zawsze takie same jako poświadczenia Windows lub wartości, które Twój dostawca hostingu przyznał Ci do zalogowania się do swoich serwerów. Skontaktuj się z administratorem dokładne wartości, które są potrzebne.)
> 
> `Database.Open` Metody jest elastyczny, ponieważ dzięki temu można przekazać nazwę bazy danych *.sdf* Nazwa ciągu połączenia, która jest przechowywana w pliku lub *Web.config* pliku. Poniższy przykład przedstawia sposób nawiązywania połączeń z bazą danych przy użyciu parametrów połączenia, pokazano w poprzednim przykładzie:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Jak wspomniano, `Database.Open` metoda pozwala przekazać nazwę bazy danych lub parametrów połączenia, a jej określenie, której mają zostać użyte. Jest to bardzo przydatne, gdy należy wdrożyć (opublikować) witryny sieci Web. Możesz użyć *.sdf* w pliku *aplikacji\_danych* folderu podczas tworzenia i testowania witryny. Gdy przesuniesz witryny na serwerze produkcyjnym, możesz użyć parametrów połączenia w *Web.config* pliku, który ma taką samą nazwę jak Twoje *.sdf* plików, ale wskazuje dostawcy hostingu bazy danych &#8212;wszystko to bez konieczności zmian w kodzie.
> 
> Ponadto jeśli chcesz współpracować bezpośrednio z parametrów połączenia, można wywołać `Database.OpenConnectionString` metody i przekazać go rzeczywistych parametrów połączenia zamiast tylko nazwy w *Web.config* pliku. Może to być przydatne w sytuacjach, w którym jakiegoś powodu nie mają dostępu do parametrów połączenia (lub wartości, takich jak *.sdf* nazwy pliku) dopóki strona nie zostanie uruchomiony. Jednak w przypadku większości scenariuszy, można użyć `Database.Open` zgodnie z opisem w tym artykule.


## <a name="additional-resources"></a>Dodatkowe zasoby

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Łączenie się z programu SQL Server lub bazie danych MySQL w programie WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Weryfikacja danych wejściowych użytkownika w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
