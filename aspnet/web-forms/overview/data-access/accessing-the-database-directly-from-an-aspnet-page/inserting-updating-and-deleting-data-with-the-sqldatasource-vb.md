---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Wstawianie, aktualizowanie i usuwanie danych z SqlDataSource (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W samouczkach poprzedniej dowiedzieliśmy się, jak kontrolki ObjectDataSource dozwolone Wstawianie, aktualizowanie i usuwanie danych. Formant SqlDataSource obsługuje t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 92d195c3e1e349cd82e0625cf9a6c5a82644b5db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Wstawianie, aktualizowanie i usuwanie danych z SqlDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) lub [pobierania plików PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> W samouczkach poprzedniej dowiedzieliśmy się, jak kontrolki ObjectDataSource dozwolone Wstawianie, aktualizowanie i usuwanie danych. Formant SqlDataSource obsługuje te same operacje, ale podejście jest inne, a ten samouczek pokazuje, jak skonfigurować SqlDataSource do wstawiania, aktualizacji i usuwania danych.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w [omówienie z wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), kontrolki widoku siatki udostępnia wbudowane aktualizowania i usuwania możliwości, podczas gdy formant widoku DetailsView i FormView obejmuje Wstawianie obsługuje wraz z programem edytowanie i usuwanie funkcji. Te możliwości modyfikacji danych można podłączyć bezpośrednio do kontroli źródła danych bez wiersz kodu musi zostać zapisany. [Omówienie programu wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) przy ObjectDataSource ułatwiające Wstawianie, aktualizowanie i usuwanie z formantami GridView widoku DetailsView i FormView. Alternatywnie można użyć zamiast ObjectDataSource SqlDataSource.

Odwołania, który do obsługi Wstawianie, aktualizowanie i usuwanie z elementu ObjectDataSource możemy potrzebne do określenia metody warstwy obiektów do wywołania, aby wykonać wstawiania, aktualizowania lub usuwania akcji. Z SqlDataSource, należy podać `INSERT`, `UPDATE`, i `DELETE` SQL instrukcji (lub procedur składowanych) do wykonania. Zajmiemy się tym, w tym samouczku, te instrukcje można tworzyć ręcznie lub automatycznie generowane przez kreatora SqlDataSource s Konfigurowanie źródła danych.

> [!NOTE]
> Od nas kolejnych już omówione, wstawianie, edytowanie i usuwanie funkcji widoku DetailsView, w widoku GridView i formanty FormView, ten samouczek koncentruje się na temat konfigurowania kontroli SqlDataSource do obsługi tych operacji. Jeśli potrzebujesz lepiej poznać te funkcje w widoku GridView widoku DetailsView i FormView, wróć do samouczki edytowanie, wstawianie i usuwanie danych wykonawczych, począwszy od [omówienie z wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Krok 1: Określanie`INSERT`,`UPDATE`, i`DELETE`— instrukcje

Jak możemy występuje w ciągu ostatnich dwóch samouczki, można pobrać danych z formantu SqlDataSource potrzebnych do kolejnych Ustaw dwie właściwości:

1. `ConnectionString`, który określa, jakie bazy danych do wysyłania zapytań, i
2. `SelectCommand`, który określa ad hoc instrukcji SQL lub nazwa procedury składowanej do wykonania do zwracania wyników.

Dla `SelectCommand` wartości parametrów, parametr wartości są określane za pomocą SqlDataSource s `SelectParameters` kolekcji i mogą zawierać stałe wartości wspólnych parametrów źródła wartości (querystring pola Zmienne sesji wartości formantu sieci Web, i itd.), lub można przypisać programowo. Gdy SqlDataSource kontrolować s `Select()` wywoływana jest metoda programowo lub automatycznie z danych formantu sieci Web jest nawiązywane połączenie z bazą danych, wartości parametrów, które są przypisane do zapytania i polecenia jest shuttled poza do Baza danych. Wyniki są następnie zwracane jako zestawu danych lub element DataReader, w zależności od wartości kontrolki s `DataSourceMode` właściwości.

Wraz z wybór danych, sterowania SqlDataSource może służyć do wstawiania, aktualizacji i usuwania danych, podając `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL w podobny sposób. Po prostu przypisać `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL do wykonania. Jeśli oświadczenia mają parametry (zgodnie z ich najbardziej zawsze będzie), należy je uwzględnić w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji.

Raz `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` określono wartość, zostanie udostępniona opcja Włącz wstawianie, Włącz edytowanie lub usuwanie włączyć w odpowiednich danych tagów inteligentnych kontroli s sieci Web. Na przykład umożliwiają s zająć przykład z `Querying.aspx` strony utworzone [badanie danych z formantem SqlDataSource](querying-data-with-the-sqldatasource-control-vb.md) samouczka i rozszerzyć, usuń go, aby uwzględnić możliwości.

Uruchamianie przez otwarcie `InsertUpdateDelete.aspx` i `Querying.aspx` strony z `SqlDataSource` folderu. Przy użyciu projektanta na `Querying.aspx` wybierz SqlDataSource i GridView z pierwszego przykładu ( `ProductsDataSource` i `GridView1` formantów). Po wybraniu dwóch formantów, przejdź do menu Edycja i wybierz polecenie Kopiuj (lub po prostu kliknij przycisk klawisze Ctrl + C). Następnie należy przejść do projektanta `InsertUpdateDelete.aspx` i Wklej w formantach. Po dwóch formantów za pośrednictwem zostały przeniesione do `InsertUpdateDelete.aspx`, przetestować strony w przeglądarce. Powinny pojawić się wartości `ProductID`, `ProductName`, i `UnitPrice` kolumn dla wszystkich rekordów `Products` tabeli bazy danych.


[![Wszystkie te produkty zostaną wyświetlone, uporządkowanych według ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Rysunek 1**: wszystkie te produkty zostaną wyświetlone, uporządkowanych według `ProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Dodawanie SqlDataSource s`DeleteCommand`i`DeleteParameters`właściwości

W tym momencie mamy SqlDataSource, która po prostu zwraca wszystkie rekordy z `Products` tabeli i GridView, który renderuje tych danych. Naszym celem jest rozszerzenia tego przykładu, aby umożliwić użytkownikowi na usuwanie produktów za pośrednictwem widoku GridView. Należy określić wartości dla formantu SqlDataSource s w tym celu `DeleteCommand` i `DeleteParameters` właściwości, a następnie skonfiguruj GridView do obsługi usuwania.

`DeleteCommand` i `DeleteParameters` właściwości można określić wiele sposobów:

- Za pomocą składni deklaratywnej
- W oknie właściwości w Projektancie
- Określ niestandardową instrukcję SQL lub procedurę składowaną ekranie Kreatora konfigurowania źródła danych
- Za pomocą przycisku Zaawansowane w Określ kolumny z tabeli ekran Kreatora konfigurowania źródła danych, które faktycznie automatycznie wygeneruje `DELETE` zbierania instrukcji i parametru SQL używane w `DeleteCommand` i `DeleteParameters` właściwości

Zajmiemy się jak automatycznie `DELETE` instrukcji utworzony w kroku 2. Na razie umożliwiają s Użyj okna właściwości w projektancie, mimo że Kreator konfigurowania źródła danych lub opcji składni deklaratywnej będzie działać równie dobrze.

Przy użyciu projektanta w `InsertUpdateDelete.aspx`, kliknij `ProductsDataSource` SqlDataSource, a następnie wywołaj okno właściwości (w menu Widok wybierz polecenie Właściwości okno lub po prostu kliknij przycisk F4). Wybierz właściwość DeleteQuery, która pojawi się zestaw elipsy.


![Wybierz właściwość DeleteQuery w oknie właściwości](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Rysunek 2**: Wybierz właściwość DeleteQuery w oknie właściwości


> [!NOTE]
> SqlDataSource t ma właściwości DeleteQuery. Zamiast DeleteQuery jest kombinacją `DeleteCommand` i `DeleteParameters` właściwości i jest wyświetlany tylko w oknie właściwości, podczas wyświetlania okna poprzez projektanta. Jeśli szukasz w oknie właściwości w widoku źródła, można znaleźć `DeleteCommand` właściwości zamiast tego.


Kliknij przycisk wielokropka we właściwości DeleteQuery, aby wyświetlić okno dialogowe Edytor poleceń i parametrów polu (patrz rysunek 3). W tym oknie dialogowym można określić `DELETE` instrukcji SQL i określ parametry. Wpisz poniższe zapytanie do `DELETE` polecenia pola tekstowego (albo ręcznie lub za pomocą konstruktora zapytań, jeśli wolisz):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Następnie kliknij przycisk Odśwież parametrów, aby dodać `@ProductID` parametru do listy parametrów poniżej.


[![Wybierz właściwość DeleteQuery w oknie właściwości](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Rysunek 3**: Wybierz właściwość DeleteQuery z okna właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Czy *nie* podać wartość tego parametru (pozostaw jej parametr źródła na None). Po usunięcie pomocy technicznej możemy dodać do widoku GridView, widoku GridView będzie automatyczne określanie wartości tego parametru, przy użyciu wartości jego `DataKeys` kolekcji dla wiersza kliknięto przycisk Usuń, którego.

> [!NOTE]
> Nazwa parametru używane w `DELETE` zapytania *musi* być taka sama jak nazwa `DataKeyNames` wartość w widoku GridView, w widoku DetailsView lub w widoku FormView. Oznacza to, że parametr w `DELETE` celowo nosi nazwę instrukcji `@ProductID` (zamiast, co oznacza, `@ID`), ponieważ nazwa kolumny klucza podstawowego w tabeli Produkty (i w związku z tym wartości DataKeyNames w widoku GridView) jest `ProductID`.


Jeśli nazwa parametru i `DataKeyNames` wartości są zgodne, widoku GridView nie można automatycznie przypisać parametru wartości z `DataKeys` kolekcji.

Po wprowadzeniu informacji dotyczących usuwania w oknie dialogowym Edytor poleceń i parametrów kliknij przycisk OK, a następnie przejdź do widoku źródłowego, aby zbadać wynikowy deklaratywne znaczników:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Należy pamiętać, dodanie `DeleteCommand` właściwości, jak również `<DeleteParameters>` sekcji i obiekt parametr o nazwie `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurowanie widoku GridView usuwania

Z `DeleteCommand` dodana właściwość tagów inteligentnych s GridView zawiera teraz opcję Włącz usuwanie. Przejdź dalej i zaznacz to pole wyboru. Zgodnie z opisem w [omówienie z wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), powoduje to widoku GridView dodać CommandField z jego `ShowDeleteButton` ustawioną właściwość `True`. Jak rysunek 4 przedstawia po odwiedzeniu strony za pośrednictwem przeglądarki przycisk usuwania jest dołączony. Przetestuj tę stronę limit przez usunięcie niektórych produktów.


[![Przycisk Usuń zawiera teraz każdego wiersza w widoku GridView](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Rysunek 4**: każdego wiersza w widoku GridView teraz zawiera przycisk Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Po kliknięciu przycisk Usuń, wystąpi odświeżenie strony, przypisuje widoku GridView `ProductID` wartość parametru z `DataKeys` wartość kolekcji dla wiersza, którego przycisk Usuń został kliknięty, a następnie wywołuje SqlDataSource s `Delete()` metody. Następnie nawiązuje połączenie z bazą danych sterowania SqlDataSource i wykonuje `DELETE` instrukcji. Widoku GridView jest następnie rebinds do SqlDataSource odzyskać i wyświetlanie bieżącego zestawu produktów (który nie zawiera rekordu tylko usunięte).

> [!NOTE]
> Ponieważ w widoku GridView używane jego `DataKeys` kolekcji, aby wypełnić parametry SqlDataSource go s istotne który GridView s `DataKeyNames` z kolumnami, który stanowi klucza podstawowego i że można ustawić właściwości SqlDataSource s `SelectCommand` zwraca te kolumny. Ponadto go s ważne jest, że nazwa parametru w elemencie SqlDataSource s `DeleteCommand` ma ustawioną wartość `@ProductID`. Jeśli `DataKeyNames` właściwość nie jest ustawiona lub nie ma nazwy parametru `@ProductsID`, kliknięcie przycisku Usuń powoduje odświeżenie strony, ale wykorzystanej t usuwa żadnych rekordów.


Rysunek 5 przedstawia graficznie interakcji. Odwołaj się do [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczka bardziej szczegółowe omówienie w łańcuchu zdarzeń związanych z Wstawianie, aktualizowanie i usuwanie danych formantu sieci Web.


![Kliknięcie przycisku Usuń w widoku GridView wywołuje metodę Delete() SqlDataSource s](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Rysunek 5**: kliknięcie przycisku Usuń w widoku GridView wywołuje SqlDataSource s `Delete()` — metoda


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Krok 2: Automatyczne generowanie`INSERT`,`UPDATE`, i`DELETE`— instrukcje

Krok 1 zbadane `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL można określić za pomocą okna właściwości lub składni deklaratywnej kontroli s. Ta metoda wymaga jednak czy możemy ręcznie zapisać instrukcje SQL ręcznie, które mogą być monotonii i podatne na błędy. Na szczęście Kreator konfigurowania źródła danych udostępnia opcję, aby `INSERT`, `UPDATE`, i `DELETE` instrukcje generowana automatycznie po użyciu Określ kolumny z tabeli ekran.

Let s Eksploruj ta opcja automatycznego generowania. Dodaj element DetailsView do projektanta w `InsertUpdateDelete.aspx` i ustawić jej `ID` właściwości `ManageProducts`. Następnie wybierz z tagów inteligentnych s widoku DetailsView do tworzenia nowego źródła danych i tworzenie SqlDataSource o nazwie `ManageProductsDataSource`.


[![Utwórz nowy SqlDataSource, o nazwie ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Rysunek 6**: Utwórz nowy składnik o nazwie SqlDataSource `ManageProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


W Kreatorze skonfiguruj źródło danych zdecydować się na użycie `NORTHWINDConnectionString` połączenia ciągu, a następnie kliknij przycisk Dalej. Z konfiguracji ekranu instrukcji Select, pozostaw Określ kolumny z tabeli lub widoku przycisku radiowego zaznaczone, a następnie wybierz `Products` tabelę z listy rozwijanej. Wybierz `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn na liście wyboru.


[![Przy użyciu tabeli Produkty, zwróć ProductID, ProductName, UnitPrice i wycofane kolumn](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Rysunek 7**: przy użyciu `Products` tabeli, aby uzyskać `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Automatyczne generowanie `INSERT`, `UPDATE`, i `DELETE` instrukcje na podstawie wybranych tabel i kolumn, kliknij przycisk Zaawansowane i Sprawdź generowanie `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru.


![Sprawdź generowanie INSERT, UPDATE i DELETE instrukcje wyboru](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Rysunek 8**: Sprawdź generowanie `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru


Generuj `INSERT`, `UPDATE`, i `DELETE` wyboru instrukcje będą tylko dostępne do kontroli, jeśli wybrana tabela ma klucz podstawowy, a kolumna klucza podstawowego (lub kolumny) znajdują się na liście kolumn zwracanych. Użyj wyboru optymistycznej współbieżności, które staje się możliwy raz Generuj `INSERT`, `UPDATE`, i `DELETE` zaznaczone pole wyboru instrukcje, będzie rozszerzyć `WHERE` klauzule powstałe w ten sposób `UPDATE` i `DELETE` instrukcje w celu zapewnienia kontroli optymistycznej współbieżności. Na razie nie zaznaczaj tego pola wyboru wyboru; w następnym samouczku zajmiemy się optymistycznej współbieżności za pomocą formantu SqlDataSource.

Po sprawdzeniu Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru, kliknij przycisk OK, aby powrócić do ekranu Konfiguruj instrukcję Select, a następnie kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć pracę Kreatora konfigurowania źródła danych. Po zakończeniu działania kreatora programu Visual Studio spowoduje dodanie BoundFields do widoku DetailsView dla `ProductID`, `ProductName`, i `UnitPrice` kolumn i CheckBoxField dla `Discontinued` kolumny. Z tagów inteligentnych s widoku DetailsView zaznacz opcję Włącz stronicowanie, dzięki czemu użytkownik odwiedzający tej stronie można przejrzeć te produkty. Również wyczyszczenie s widoku DetailsView `Width` i `Height` właściwości.

Zwróć uwagę, tagów inteligentnych nie ma dostępnych opcji Włącz wstawianie, Włącz edytowanie i usuwanie włączyć. Wynika to z faktu SqlDataSource zawiera wartości dla jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand`, jak pokazano na poniższej składni deklaratywnej:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Należy zwrócić uwagę, jak kontrolka SqlDataSource miał wartości ustawiane automatycznie jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. W zestawie kolumn, do którego odwołuje się `InsertCommand` i `UpdateCommand` właściwości są na podstawie `SELECT` instrukcji. Oznacza to zamiast czekać, aż *co* kolumny produktów w `InsertCommand` i `UpdateCommand`, istnieją tylko kolumn określonych w `SelectCommand` (mniej `ProductID`, który został pominięty, ponieważ jego s [ `IDENTITY` kolumny](http://www.sqlteam.com/item.asp?ItemID=102), którego wartość nie można zmienić podczas edycji i który jest automatycznie przypisany podczas wstawiania). Ponadto dla każdego parametru w `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości ma odpowiednich parametrów w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji.

Aby włączyć funkcje modyfikacji danych widoku DetailsView s, sprawdź Włącz wstawianie, Włącz edytowanie i usuwanie Włącz opcje w jego tagów inteligentnych. Spowoduje to dodanie CommandField z jego `ShowInsertButton`, `ShowEditButton`, i `ShowDeleteButton` właściwości `True`.

Odwiedź stronę w przeglądarce i zanotuj edycji, usuwania i nowe przyciski zawarte w widoku DetailsView. Klikając przycisk Edytuj zmieni się w tryb edycji, który wyświetla każdego elementu BoundField widoku DetailsView których `ReadOnly` właściwość jest ustawiona na `False` (ustawienie domyślne) jako pole tekstowe i w polu CheckBoxField jako pole wyboru.


[![Interfejs edytowania domyślne s widoku DetailsView](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Rysunek 9**: s widoku DetailsView domyślny interfejs edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


Podobnie można usunąć aktualnie zaznaczonego produktu lub dodanie nowego produktu do systemu. Ponieważ `InsertCommand` instrukcji działa tylko z `ProductName`, `UnitPrice`, i `Discontinued` kolumny, innych kolumn mają albo `NULL` lub ich wartość domyślną przypisane przez bazę danych podczas wstawiania. Podobnie jak z elementu ObjectDataSource, jeśli `InsertCommand` nie ma żadnej tabeli bazy danych, Zezwalaj na kolumny, które ADAM t `NULL` s i ADAM t miała wartość domyślną, wystąpi błąd SQL podczas próby wykonania `INSERT` instrukcji.

> [!NOTE]
> S widoku DetailsView Wstawianie i Edycja interfejsy brakuje dowolny rodzaj dostosowanie lub sprawdzania poprawności. Dodaj formanty walidacji lub dostosować interfejsy, należy przekonwertować BoundFields TemplateFields. Zapoznaj się [dodawanie formantów weryfikacji do edycji i wstawianie interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) i [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczki, aby uzyskać więcej informacji.


Ponadto należy pamiętać, aktualizowania i usuwania, widoku DetailsView używa bieżącego produktu s `DataKey` wartość, która jest obecna tylko w przypadku `DataKeyNames` właściwość jest skonfigurowana. Jeśli edytowania lub usuwania nie mają żadnego skutku, upewnij się, że `DataKeyNames` właściwość jest ustawiona.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Ograniczenia automatycznego generowania instrukcje SQL

Ponieważ Generuj `INSERT`, `UPDATE`, i `DELETE` opcja instrukcji jest dostępna tylko, gdy pobrania kolumn z tabeli, dla bardziej złożone kwerendy, użytkownik będzie musiał napisać własny `INSERT`, `UPDATE`, i `DELETE` instrukcje, jak robiliśmy w kroku 1. Zazwyczaj SQL `SELECT` użyj instrukcji `JOIN` s, aby przywrócić wyświetlania danych z co najmniej jednej tabeli odnośników dla celów (takich jak przywracanie z powrotem `Categories` tabeli s `CategoryName` podczas wyświetlania informacji o produkcie). W tym samym czasie, chcemy może zezwolić użytkownikowi na edytowanie, aktualizowanie lub wstawiania danych do tabeli podstawowej (`Products`, w tym przypadku).

Gdy `INSERT`, `UPDATE`, i `DELETE` instrukcji można wprowadzić ręcznie, należy wziąć pod uwagę następujące porada. Wstępnie skonfigurować SqlDataSource tak, aby ponownie pobiera dane tylko z `Products` tabeli. Użyj kolumn skonfiguruj źródło danych Kreatora s Określ z ekranu tabeli lub widoku, dzięki czemu można automatycznie generować `INSERT`, `UPDATE`, i `DELETE` instrukcje. Następnie wybierz po zakończeniu pracy kreatora skonfigurować SelectQuery z okna właściwości (lub też, wróć do Kreatora konfigurowania źródła danych, ale użyj Określ niestandardową instrukcję SQL lub procedurę składowaną opcji). Następnie zaktualizuj `SELECT` instrukcji, aby uwzględnić `JOIN` składni. Ta metoda zapewnia korzyści zaoszczędzić czas automatycznie generowanych instrukcji SQL i umożliwia bardziej dostosowanego `SELECT` instrukcji.

Innym ograniczeniem automatycznie generować `INSERT`, `UPDATE`, i `DELETE` instrukcje jest kolumn w `INSERT` i `UPDATE` instrukcje są oparte na kolumny zwracane przez `SELECT` instrukcji. Firma Microsoft może być konieczne zaktualizować ani wstawić pola więcej lub mniej, natomiast. Na przykład w przykładzie z kroku nr 2 może być chcemy mieć `UnitPrice` elementu BoundField się tylko do odczytu. W takim przypadku go t nie powinien wystąpić w `UpdateCommand`. Możemy też wartość pola w tabeli, które nie są wyświetlane w widoku GridView. Na przykład podczas dodawania nowego rekordu może chcemy `QuantityPerUnit` wartość TODO.

Jeśli takie dostosowania są wymagane, należy to zrobić ręcznie, korzystając z okna właściwości, Określ niestandardową instrukcję SQL lub procedurę składowaną opcji za pomocą kreatora lub za pomocą składni deklaratywnej.

> [!NOTE]
> Podczas dodawania parametrów, które nie mają odpowiednich pól danych formant sieci Web, należy pamiętać, wymagającym tych wartości parametrów przypisywane wartości w jakikolwiek sposób. Te wartości mogą być: ustalony bezpośrednio w `InsertCommand` lub `UpdateCommand`; może pochodzić z określonego źródła wstępnie zdefiniowane (querystring, stan sesji, formantów sieci Web na stronie i tak dalej); lub można przypisać programowo, jak widzieliśmy w powyższych instrukcji.


## <a name="summary"></a>Podsumowanie

W kolejności danych czy formanty sieci Web mogą korzystać z ich Wstawianie wbudowanych, edytowanie i usuwanie możliwości kontroli źródła danych, które są powiązane musi oferować takich funkcji. Dla SqlDataSource, oznacza to, że `INSERT`, `UPDATE`, i `DELETE` instrukcji SQL muszą być przypisane do `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. Te właściwości, a odpowiedni kolekcji parametrów, można dodać ręcznie lub automatycznie wygenerowana za pomocą Kreatora konfigurowania źródła danych. W tym samouczku będziemy zbadać obie techniki.

Firma Microsoft bada optymistycznej współbieżności z ObjectDataSource w [implementacja optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) samouczka. Formant SqlDataSource oferuje również obsługę optymistycznej współbieżności. Zgodnie z opisem w kroku 2, podczas automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcje, Kreator oferuje użycie opcji optymistycznej współbieżności. Jak zajmiemy się w następnym samouczku, przy użyciu optymistycznej współbieżności z SqlDataSource modyfikuje `WHERE` klauzule `UPDATE` i `DELETE` instrukcje, aby upewnić się, że wartości innych kolumn informacjami o klientach t zmienione od ostatniego danych wyświetlane na stronie.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [dalej](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
