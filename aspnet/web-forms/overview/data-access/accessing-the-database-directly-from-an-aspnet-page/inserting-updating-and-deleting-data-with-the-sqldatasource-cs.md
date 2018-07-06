---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Wstawianie, aktualizowanie i usuwanie danych przy użyciu kontrolki SqlDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednich samouczkach dowiedzieliśmy się, jak kontrolka ObjectDataSource dozwolone w przypadku wstawiania, aktualizowania i usuwania danych. Kontrolki SqlDataSource obsługuje t...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: d2958cb9cb0dd5ed89988a969663022a920a3dca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812850"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Wstawianie, aktualizowanie i usuwanie danych przy użyciu kontrolki SqlDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) lub [Pobierz plik PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> W poprzednich samouczkach dowiedzieliśmy się, jak kontrolka ObjectDataSource dozwolone w przypadku wstawiania, aktualizowania i usuwania danych. Kontrolki SqlDataSource obsługuje te same operacje, ale różni się to podejście, a w tym samouczku przedstawiono sposób konfigurowania SqlDataSource do wstawiania, aktualizacji i usuwania danych.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w [Omówienie programu Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)kontrolki GridView udostępnia wbudowane aktualizowanie i usuwanie możliwości, podczas gdy kontrolki DetailsView i FormView obejmuje Wstawianie pomocy technicznej wraz z edytowanie i usuwanie funkcji. Te możliwości modyfikacji danych można podłączyć bezpośrednio do kontroli źródła danych bez wiersza kodu, bez potrzeby zapisania. [Omówienie programu Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) zbadać za pomocą kontrolki ObjectDataSource ułatwiające Wstawianie, aktualizowanie i usuwanie z kontrolkami GridView DetailsView i FormView. Alternatywnie można użyć zamiast kontrolki ObjectDataSource SqlDataSource.

Odwołania, które umożliwiają wstawianie, aktualizowanie i usuwanie, za pomocą kontrolki ObjectDataSource możemy potrzebne do określenia metody warstwy obiektów zostać wywołana w celu wykonania insert, update lub delete akcji. Dzięki użyciu kontrolki SqlDataSource, należy podać `INSERT`, `UPDATE`, i `DELETE` SQL instrukcji (lub procedur składowanych) do wykonania. Jak opisano w tym samouczku, tych instrukcji może być tworzona ręcznie lub mogą być generowane automatycznie przez kreatora SqlDataSource s Konfigurowanie źródła danych.

> [!NOTE]
> Ponieważ firma Microsoft ve już omówione, wstawiania, edytowanie i usuwanie możliwości DetailsView, w widoku GridView i FormView kontroluje, ten samouczek koncentruje się na temat konfigurowania kontrolki SqlDataSource do obsługi tych operacji. Jeśli potrzebujesz odświeżyć informacje o implementacji tych funkcji w widoku GridView DetailsView i FormView, zwracany do samouczków, edytowanie, wstawianie i usuwanie danych, rozpoczynając od [Omówienie programu Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Krok 1: Określanie`INSERT`,`UPDATE`, i`DELETE`instrukcji

Ponieważ ve występuje w ciągu ostatnich dwóch samouczków do pobierania danych z kontrolki SqlDataSource, musimy Ustaw dwie właściwości:

1. `ConnectionString`, która określa, jakie bazy danych do wysyłania zapytań, i
2. `SelectCommand`, która określa ad-hoc instrukcji SQL lub nazwa procedury składowanej do wykonania w celu zwracania wyników.

Dla `SelectCommand` wartości parametrów, parametr wartości są określane za pomocą SqlDataSource s `SelectParameters` kolekcji i mogą obejmować zakodowanych wartości i wartości źródła typowych parametrów (querystring pola Zmienne sesji wartości formantu sieci Web, i itd.), lub można przypisać programowo. Po użyciu kontrolki SqlDataSource kontrolować s `Select()` wywoływana jest metoda programowo lub automatycznie dane formantu sieci Web jest nawiązywane połączenie z bazą danych, wartości parametrów są przypisane do zapytania i polecenia jest shuttled wyłączone do Baza danych. Wyniki są następnie zwracane jako zestawu danych lub DataReader, w zależności od wartości kontrolki s `DataSourceMode` właściwości.

Wraz z wybraniu danych, użyciu kontrolki SqlDataSource może służyć do wstawiania, aktualizacji i usuwania danych poprzez dostarczenie `INSERT`, `UPDATE`, i `DELETE` instrukcji języka SQL w taki sam sposób. Po prostu przypisać `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL do wykonania. Jeśli oświadczenia mają parametry (zgodnie z ich najbardziej zawsze będzie), należy je uwzględnić w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji.

Gdy `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` wartość została określona, opcja Włącz wstawianie, Włącz edytowanie lub Włącz usuwanie w odpowiednich danych tagu inteligentnego sterowania s sieci Web staną się dostępne. Na przykład umożliwiają s Użyj przykładu z `Querying.aspx` strony utworzonej w [zapytania danych przy użyciu kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) samouczek i rozszerzyć, usuń go, aby uwzględniał możliwości.

Zacznij od otwarcia `InsertUpdateDelete.aspx` i `Querying.aspx` strony z `SqlDataSource` folderu. Przy użyciu projektanta na `Querying.aspx` wybierz SqlDataSource i GridView z pierwszego przykładu ( `ProductsDataSource` i `GridView1` kontrolek). Po zaznaczeniu dwóch kontrolek, przejdź do menu Edycja i wybierz polecenie Kopiuj (lub po prostu naciśnij klawisze Ctrl + C). Następnie przejdź do projektanta `InsertUpdateDelete.aspx` i Wklej w kontrolkach. Po przesunięciu dwóch kontrolek celu `InsertUpdateDelete.aspx`, przetestowania strony w przeglądarce. Powinien zostać wyświetlony wartości `ProductID`, `ProductName`, i `UnitPrice` kolumn dla wszystkich rekordów w `Products` tabeli bazy danych.


[![Wszystkie produkty zostaną wyświetlone, uporządkowane według elementu ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 1**: wszystkie te produkty zostaną wyświetlone, uporządkowane według `ProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Dodawanie SqlDataSource s`DeleteCommand`i`DeleteParameters`właściwości

W tym momencie mamy SqlDataSource, który po prostu zwraca wszystkie rekordy z `Products` tabeli i GridView, który renderuje tych danych. Naszym celem jest, aby rozszerzyć ten przykład umożliwia użytkownikowi usuwanie produktów za pośrednictwem widoku GridView. W tym trzeba określić wartości dla kontrolki SqlDataSource s `DeleteCommand` i `DeleteParameters` właściwości, a następnie skonfiguruj GridView do obsługi usuwania.

`DeleteCommand` i `DeleteParameters` właściwości można określić wiele sposobów:

- Za pomocą składni deklaratywnej
- W oknie właściwości w Projektancie
- Określ niestandardową instrukcję SQL lub procedury składowanej ekranie kreatora Konfigurowanie źródła danych
- Za pomocą przycisku Zaawansowane w kolumnach Określ z tabeli ekran do wyświetlania w Kreatorze konfigurowania źródła danych, które faktycznie automatycznie wygeneruje `DELETE` SQL instrukcji i parametru kolekcji wykorzystywanej w `DeleteCommand` i `DeleteParameters` właściwości

Zajmiemy się jak automatycznie `DELETE` instrukcji, utworzona w kroku 2. Na razie umożliwiają s, użyj okna właściwości w projektancie, mimo że Kreator konfigurowania źródła danych lub opcji składni deklaratywnej będzie działać równie dobrze.

Przy użyciu projektanta w `InsertUpdateDelete.aspx`, kliknij pozycję `ProductsDataSource` SqlDataSource a następnie wywołaj okno właściwości (w menu Widok wybierz okno właściwości lub po prostu kliknij przycisk F4). Wybierz właściwość DeleteQuery, co umożliwi wyświetlenie zestawu elipsy.


![W oknie właściwości wybierz właściwość DeleteQuery](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Rysunek 2**: w oknie właściwości wybierz właściwość DeleteQuery


> [!NOTE]
> SqlDataSource t ma właściwości DeleteQuery. Zamiast DeleteQuery jest kombinacją `DeleteCommand` i `DeleteParameters` właściwości i jest wyświetlany tylko w oknie dialogowym właściwości, podczas wyświetlania okna za pomocą projektanta. Jeśli szukasz w oknie dialogowym właściwości w widoku źródła, znajdziesz `DeleteCommand` właściwości zamiast tego.


Kliknij przycisk wielokropka we właściwości DeleteQuery, aby wyświetlić okno dialogowe Edytor poleceń i parametrów polu (patrz rysunek 3). W tym oknie dialogowym można określić `DELETE` instrukcji SQL i określ parametry. Wprowadź następujące zapytanie w `DELETE` polecenie w polu tekstowym (albo ręcznie lub za pomocą konstruktora zapytań, jeśli użytkownik sobie tego życzy):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Następnie kliknij przycisk Odśwież parametry, aby dodać `@ProductID` parametru do listy poniższych parametrów.


[![W oknie właściwości wybierz właściwość DeleteQuery](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 3**: w oknie właściwości wybierz właściwość DeleteQuery ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Czy *nie* podać wartość dla tego parametru (pozostaw jako parametr źródło w None). Po Dodajemy obsługę usuwania do kontrolki GridView widoku GridView zostanie automatycznie określić wartości tego parametru przy użyciu wartości jego `DataKeys` kolekcji dla wiersza, którego przycisk Usuń został kliknięty.

> [!NOTE]
> Nazwa parametru używana w `DELETE` zapytania *musi* być taka sama jak nazwa `DataKeyNames` wartości w widoku GridView, DetailsView lub FormView. Oznacza to, że parametr w `DELETE` celowo nosi nazwę instrukcji `@ProductID` (zamiast, na przykład, `@ID`), ponieważ nazwa kolumny klucza podstawowego w tabeli Produkty (i w związku z tym wartości DataKeyNames w widoku GridView) jest `ProductID`.


Jeśli nazwa parametru i `DataKeyNames` wartości są zgodne, widoku GridView nie można automatycznie przypisać parametru wartość `DataKeys` kolekcji.

Po wprowadzeniu informacji dotyczących usuwania w oknie dialogowym Edytor poleceń i parametrów kliknij przycisk OK, a następnie przejdź do widoku źródła, aby zbadać wynikowy oznaczeniu deklaracyjnym:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Należy pamiętać, dodanie `DeleteCommand` właściwości, jak również `<DeleteParameters>` sekcji i obiektu parametr o nazwie `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurowanie kontrolki GridView usuwania

Za pomocą `DeleteCommand` dodana właściwość tagu inteligentnego s GridView zawiera teraz opcję Włącz usuwanie. Przejdź dalej i zaznacz to pole wyboru. Zgodnie z opisem w [Omówienie programu Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), powoduje to, że GridView dodać CommandField z jego `ShowDeleteButton` właściwością `true`. Rysunek 4 przedstawia, gdy strona jest kontrolowane za pośrednictwem przeglądarki przycisk Usuń jest dołączony. Przetestuj tę stronę się przez usunięcie niektórych produktów.


[![Każdy wiersz GridView zawiera teraz przycisk Usuń](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Rysunek 4**: każdego wiersza w widoku GridView zawiera teraz przycisk Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Po kliknięciu przycisku Usuń występuje odświeżenie strony, przypisuje widoku GridView `ProductID` parametru wartość z `DataKeys` wartość kolekcji dla wiersza, którego przycisk Usuń został kliknięty i wywołuje SqlDataSource s `Delete()` metody. Kontrolki SqlDataSource następnie nawiązuje połączenie z bazą danych i wykonuje `DELETE` instrukcji. Kontrolki GridView następnie rebinds do SqlDataSource powrót i wyświetlanie bieżący zestaw produktów (który nie zawiera już po prostu usunąć rekordu).

> [!NOTE]
> Ponieważ w widoku GridView używane jego `DataKeys` kolekcję, aby wypełnić parametry SqlDataSource go s istotne, GridView s `DataKeyNames` właściwości można ustawić kolumny, które tworzą klucz podstawowy oraz że SqlDataSource s `SelectCommand` zwraca te kolumny. Ponadto go s pamiętać, że parametr nazwy SqlDataSource s `DeleteCommand` ustawiono `@ProductID`. Jeśli `DataKeyNames` nie ustawiono właściwości lub parametru nie ma nazwy `@ProductsID`, klikając przycisk Usuń spowoduje odświeżenie strony, ale uzyskany t faktycznie usunąć dowolny rekord.


Rysunek 5 przedstawia ta interakcja w formie graficznej. Odwołaj się do [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczka, aby uzyskać bardziej szczegółowe omówienie dotyczące łańcuch zdarzeń powiązanych ze Wstawianie, aktualizowanie i usuwanie danych formantu sieci Web.


![Klikając przycisk Usuń w widoku GridView wywołuje metodę Delete() s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Rysunek 5**: kliknięcie przycisku Usuń w widoku GridView wywołuje SqlDataSource s `Delete()` — metoda


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Krok 2: Automatycznego generowania`INSERT`,`UPDATE`, i`DELETE`instrukcji

Jako badania, w kroku 1 `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL można określić za pomocą okna właściwości lub składni deklaratywnej formantu s. Takie podejście wymaga jednak, że firma Microsoft ręcznie zapisać instrukcji SQL ręcznie, które mogą być monotonii i obarczone ryzykiem błędów. Na szczęście Kreator skonfigurować źródło danych udostępnia opcję, aby `INSERT`, `UPDATE`, i `DELETE` instrukcji generowane automatycznie, korzystając z określ kolumn z tabeli ekran Widok.

Pozwól, zapoznaj się z tej opcji automatycznego generowania s. Dodaj element DetailsView do projektanta w `InsertUpdateDelete.aspx` i ustaw jego `ID` właściwość `ManageProducts`. Następnie wybierz za pomocą tagu inteligentnego s DetailsView utworzyć nowe źródło danych i utworzyć SqlDataSource, o nazwie `ManageProductsDataSource`.


[![Utwórz nowy SqlDataSource, o nazwie ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Rysunek 6**: Utwórz nowy o nazwie SqlDataSource `ManageProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Za pomocą Kreatora konfigurowania źródła danych zoptymalizowany pod kątem używania `NORTHWINDConnectionString` połączenia ciągu, a następnie kliknij przycisk Dalej. Z Konfiguruj ekranu instrukcji Select, pozostaw kolumn Określ z tabeli lub widoku zaznaczony przycisk radiowy, a następnie wybierz `Products` tabeli z listy rozwijanej. Wybierz `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumny z listy wyboru.


[![Korzystając z tabeli Produkty, zwracają ProductID, ProductName, UnitPrice i nieobsługiwane kolumny](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Rysunek 7**: przy użyciu `Products` tabeli, zwróć `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Aby automatycznie wygenerować `INSERT`, `UPDATE`, i `DELETE` instrukcji na podstawie wybranej tabeli i kolumn, kliknij przycisk Zaawansowane i sprawdź Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru.


![Sprawdź instrukcje Generowanie INSERT, UPDATE i DELETE, zaznacz pole wyboru](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Rysunek 8**: Sprawdź Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru


Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru będą tylko dostępne do kontroli, jeśli wybrana tabela ma klucz podstawowy, a kolumna klucza podstawowego (lub kolumny) znajdują się na liście zwrócone kolumny. Użyj optymistycznej współbieżności pole wyboru, która staje się możliwy po Generuj `INSERT`, `UPDATE`, i `DELETE` zaznaczone pole wyboru instrukcji, będzie rozszerzać `WHERE` klauzul wynikowy `UPDATE` i `DELETE` instrukcji w celu zapewnienia kontroli optymistycznej współbieżności. Na razie nie zaznaczaj tego pola wyboru wyboru; w następnym samouczku zajmiemy się optymistycznej współbieżności przy użyciu kontrolki SqlDataSource.

Po sprawdzeniu Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru kliknij przycisk OK, aby powrócić do ekranu skonfigurować instrukcji Select, a następnie kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć działanie kreatora Konfigurowanie źródła danych. Po ukończeniu kreatora, Visual Studio spowoduje dodanie BoundFields DetailsView dla `ProductID`, `ProductName`, i `UnitPrice` kolumn i CheckBoxField dla `Discontinued` kolumny. Za pomocą tagu inteligentnego s DetailsView Włączanie stronicowania zaznaczenie pola wyboru, aby przejść przez produkty użytkownika odwiedzenie tej strony. Również wyczyszczenie DetailsView s `Width` i `Height` właściwości.

Zwróć uwagę, tagu inteligentnego nie ma dostępnych opcji Włącz wstawianie, Włącz edytowanie i Włącz usuwanie. Jest to spowodowane SqlDataSource zawiera wartości dla swojej `InsertCommand`, `UpdateCommand`, i `DeleteCommand`, jak pokazano na poniższej składni deklaratywnej:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Należy zauważyć, jak kontrolki SqlDataSource miał wartości ustawiane automatycznie jej `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. Zestaw kolumn, do którego odwołuje się `InsertCommand` i `UpdateCommand` właściwości są na podstawie `SELECT` instrukcji. Oznacza to zamiast *co* kolumny produktów w `InsertCommand` i `UpdateCommand`, są wyłącznie kolumny określone w `SelectCommand` (mniej `ProductID`, który został pominięty, ponieważ jej s [ `IDENTITY` kolumny](http://www.sqlteam.com/item.asp?ItemID=102), którego wartość nie można zmienić po edycji i który jest przypisywany podczas wstawiania). Ponadto dla każdego parametru `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości, które są odpowiednie parametry w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji.

Aby włączyć funkcji modyfikacji danych s DetailsView, sprawdź Włącz wstawianie, Włącz edytowanie i Włącz usuwanie opcji w jego tagu inteligentnego. Spowoduje to dodanie CommandField z jego `ShowInsertButton`, `ShowEditButton`, i `ShowDeleteButton` właściwości ustawione na `true`.

Odwiedź stronę w przeglądarce i zwróć uwagę, edytowania, usuwania i nowe przyciski objęte DetailsView. Klikając przycisk Edytuj jest przekształcany DetailsView tryb edycji, który wyświetla każdego elementu BoundField którego `ReadOnly` właściwość jest ustawiona na `false` (ustawienie domyślne) jako pole tekstowe i CheckBoxField jako pola wyboru.


[![DetailsView s domyślny interfejs edytowania](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 9**: s DetailsView interfejsu edycji kontrolki domyślne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Podobnie można usunąć aktualnie wybranym produkcie, lub Dodaj nowy produkt do systemu. Ponieważ `InsertCommand` instrukcji działa tylko z `ProductName`, `UnitPrice`, i `Discontinued` kolumny, inne kolumny mają być `NULL` lub przywrócić wartości domyślne przypisane przez bazę danych podczas wstawiania. Podobnie jak za pomocą kontrolki ObjectDataSource, jeśli `InsertCommand` nie ma żadnej tabeli bazy danych kolumn, których w ogóle t Zezwalaj `NULL` s i don t ma wartości domyślnej, wystąpi błąd SQL podczas próby wykonania `INSERT` instrukcji.

> [!NOTE]
> S DetailsView, wstawianie i Edycja interfejsów braku dowolny rodzaj dostosowywania lub sprawdzania poprawności. Aby dodać formanty sprawdzania poprawności lub dostosować interfejsy, należy przekonwertować BoundFields kontrolek TemplateField. Zapoznaj się [Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) i [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczki, aby uzyskać więcej informacji.


Ponadto należy pamiętać o tym, aktualizowania i usuwania, DetailsView używa bieżącego produktu s `DataKey` wartość, która jest obecny tylko wtedy, gdy `DataKeyNames` właściwość jest konfigurowana. W przypadku edytowania lub usuwania wydaje się mieć żadnego efektu, upewnij się, że `DataKeyNames` właściwość jest ustawiona.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Ograniczenia dotyczące automatycznego generowania instrukcji języka SQL

Ponieważ generowanie `INSERT`, `UPDATE`, i `DELETE` opcji instrukcji jest dostępna tylko w przypadku pobrania kolumny z tabeli, dla bardziej złożone zapytania możesz będą musieli napisać własny `INSERT`, `UPDATE`, i `DELETE` instrukcje, jak zrobiliśmy w kroku 1. Często SQL `SELECT` instrukcje używają `JOIN` s, aby przywrócić dane z jednego lub więcej tabel odnośników, aby wyświetlić celów (takie jak przeniesienie ponownie `Categories` tabeli s `CategoryName` pola podczas wyświetlania informacji o produkcie). W tym samym czasie chcemy może umożliwić użytkownikowi edytowanie, aktualizowanie lub wstawić dane do tabeli podstawowej (`Products`, w tym przypadku).

Gdy `INSERT`, `UPDATE`, i `DELETE` instrukcji można wprowadzić ręcznie, należy wziąć pod uwagę następujące porada. Początkowo Instalatora SqlDataSource tak, aby ponownie ściąga dane tylko z `Products` tabeli. Użyj kolumny określ s kreatora Konfigurowanie źródła danych na ekranie tabeli lub widoku, dzięki czemu możesz automatycznie wygenerować `INSERT`, `UPDATE`, i `DELETE` instrukcji. Następnie wybierz po ukończeniu kreatora skonfigurować SelectQuery z okna właściwości (lub też, wróć do kreatora Konfigurowanie źródła danych, ale użyj Określ niestandardową instrukcję SQL lub procedury składowanej opcji). Następnie zaktualizuj `SELECT` instrukcję, aby uwzględnić `JOIN` składni. Ta metoda zapewnia korzyści zaoszczędzić czas, automatycznie generowanych instrukcji SQL i pozwala na bardziej dostosowanego `SELECT` instrukcji.

Innym ograniczeniem automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcji jest fakt, że kolumny w `INSERT` i `UPDATE` instrukcji opierają się na kolumny zwracane przez `SELECT` instrukcji. Firma Microsoft może być konieczne zaktualizować ani wstawić zwiększenie lub zmniejszenie liczby pól, jednak. Na przykład, w tym przykładzie z kroku nr 2 może chcemy mieć `UnitPrice` elementu BoundField się tylko do odczytu. W takim przypadku należy go t nie powinna występować w `UpdateCommand`. Lub chcemy ustawić wartość pola w tabeli, który nie jest widoczna w widoku GridView. Na przykład podczas dodawania nowego rekordu firma Microsoft może być `QuantityPerUnit` wartość zadań do wykonania.

Takie dostosowania są wymagane, należy to zrobić ręcznie, za pośrednictwem oknie właściwości, Określ niestandardową instrukcję SQL lub procedury składowanej opcja w kreatorze lub za pomocą składni deklaratywnej.

> [!NOTE]
> Podczas dodawania parametrów, które nie mają odpowiednich pól danych kontrolki internetowej, pamiętać, że te wartości parametrów należy można przypisać wartości w jakikolwiek sposób. Te wartości mogą być: ustaloną bezpośrednio w `InsertCommand` lub `UpdateCommand`; mogą pochodzić z wstępnie zdefiniowanych źródła (querystring, stan sesji, kontrolki sieci Web, na stronie i tak dalej), lub można przypisać programowo, zgodnie z widzieliśmy w poprzednim samouczku.


## <a name="summary"></a>Podsumowanie

W kolejności dla danych, sieci Web kontroluje wykorzystanie ich wbudowane wstawiania, edytowanie i usuwanie możliwości kontroli źródła danych, które są powiązane muszą oferować takie funkcje. Dla SqlDataSource, oznacza to, że `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL muszą być przypisane do `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. Te właściwości, a odpowiednie kolekcje parametry, można ręcznie dodawać lub generowane automatycznie za pomocą Kreatora konfigurowania źródła danych. W tym samouczku zbadaliśmy obu tych technik.

Zbadaliśmy optymistycznej współbieżności przy użyciu kontrolki ObjectDataSource w [Implementowanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) samouczka. Kontrolki SqlDataSource zapewnia również obsługę optymistycznej współbieżności. Jak wspomniano w kroku 2, podczas automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcji, Kreator oferuje użyj opcji optymistycznej współbieżności. Jak opisano w następnym samouczku, za pomocą optymistycznej współbieżności przy użyciu kontrolki SqlDataSource modyfikuje `WHERE` klauzul `UPDATE` i `DELETE` instrukcji, aby upewnić się, że wartości innych kolumn, że t zmienione od ostatniego danych wyświetlane na stronie.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [dalej](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
