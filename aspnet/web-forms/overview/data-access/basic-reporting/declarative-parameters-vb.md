---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parametry deklaratywne (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będzie firma Microsoft pokazują, jak używać ustaloną wartość parametru, aby wybrać dane do wyświetlenia w kontrolce DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 10ed3a4f019cd78033ecdd61499b1914f403414e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755797"
---
<a name="declarative-parameters-vb"></a>Parametry deklaratywne (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) lub [Pobierz plik PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> W tym samouczku będzie firma Microsoft pokazują, jak używać ustaloną wartość parametru, aby wybrać dane do wyświetlenia w kontrolce DetailsView.


## <a name="introduction"></a>Wprowadzenie

W [ostatni samouczek](displaying-data-with-the-objectdatasource-vb.md) przyjrzeliśmy się wyświetlanie danych z kontrolkami GridView DetailsView i FormView, powiązany z kontrolka ObjectDataSource, która wywołała `GetProducts()` metody z `ProductsBLL` klasy. `GetProducts()` Metoda zwraca wszystkie rekordy z bazy danych Northwind wypełniony DataTable silnie typizowane `Products` tabeli. `ProductsBLL` Klasa zawiera dodatkowe metody dla zwracanie tylko podzbiór produktów — `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, i `GetProductsBySupplierID(supplierID)`. Te trzy metody oczekiwać, że parametr wejściowy wskazujący, jak filtrować informacje zwrócone produktu.

Kontrolki ObjectDataSource może służyć do wywołania metody, które oczekują parametrów wejściowych, ale aby to zrobić, należy określić, skąd pochodzą wartości tych parametrów. Wartości parametrów mogą być zakodowane lub mogą pochodzić z różnych źródeł dynamicznej, w tym: wartości querystring, zmienne sesji wartość właściwości formantu sieci Web, na stronie lub inne osoby.

W tym samouczku Zacznijmy od pokazujący, jak użyć ustaloną wartość parametru. W szczególności Zapoznamy się dodanie DetailsView do strony, który wyświetla informacje dotyczące określonego produktu, a mianowicie Jacka Chef Gumbo mieszanego, który ma `ProductID` 5. Następnie zobaczymy, jak ustawić wartość tego parametru na podstawie formantu sieci Web. W szczególności użyjemy pole tekstowe umożliwiające użytkownika, wpisz w kraju, po upływie którego można kliknąć przycisk, aby wyświetlić listę dostawców, które znajdują się w danym kraju.

## <a name="using-a-hard-coded-parameter-value"></a>Przy użyciu wartości parametru ustaloną

Pierwszy przykład: Rozpocznij, dodając kontrolki widoku szczegółów do `DeclarativeParams.aspx` stronie `BasicReporting` folderu. W tagu inteligentnego DetailsView, wybierz &lt;nowe źródło danych&gt; z listy rozwijanej i wybierz polecenie dodać kontrolki ObjectDataSource.


[![Dodawanie kontrolki ObjectDataSource do strony](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Rysunek 1**: na stronie Dodaj kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image3.png))


Spowoduje to automatyczne uruchomienie kreatora wybierz źródło danych formantu ObjectDataSource. Wybierz `ProductsBLL` klasy na pierwszym ekranie kreatora.


[![Wybierz klasę ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Rysunek 2**: Wybierz `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image6.png))


Ponieważ chcemy wyświetlić informacje dotyczące konkretnego produktu chcemy do użycia `GetProductByProductID(productID)` metody.


[![Wybierz metodę GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Rysunek 3**: Wybierz `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image9.png))


Ponieważ metoda, który Wybraliśmy zawiera parametr, istnieje jeden ekran więcej dla kreatora, w którym możemy się prośba o definiujące wartość ma być używany dla parametru. Na liście po lewej stronie pokazuje wszystkie parametry dla wybranej metody. Aby uzyskać `GetProductByProductID(productID)` jest tylko jedna `productID`. Po prawej stronie można określić wartość dla wybranego parametru. Listy rozwijanej źródła parametru wylicza różnych możliwych źródeł wartości parametru. Ponieważ chcemy określić ustaloną wartość 5 `productID` parametr, pozostaw źródło parametru None i wprowadź 5 w polu tekstowym DefaultValue.


[![Hard-Coded parametru z 5 zostanie użyta wartość dla elementu productID parametru](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Rysunek 4**: A Hard-Coded parametru z 5 zostanie użyta wartość dla `productID` parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image12.png))


Po zakończeniu pracy kreatora skonfiguruj źródło danych zawiera formantu ObjectDataSource oznaczeniu deklaracyjnym `Parameter` obiektu `SelectParameters` kolekcji dla każdego z parametrów wejściowych, oczekiwana przez metody zdefiniowanej w `SelectMethod` Właściwość. Ponieważ metoda używamy w tym przykładzie oczekuje tylko jednego parametr wejściowy, `parameterID`, istnieje tylko jeden wpis w tym miejscu. `SelectParameters` Kolekcja może zawierać dowolną klasę pochodzącą od `Parameter` klasy w `System.Web.UI.WebControls` przestrzeni nazw. Dla parametru ustalonych wartości podstawowej `Parameter` klasa jest używana, ale w przypadku innych parametru pochodnego Opcje źródła `Parameter` klasy jest używany; można także tworzyć własne [typy parametrów niestandardowych](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), jeśli to konieczne.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Jeśli piszesz na swoim komputerze oznaczeniu deklaracyjnym zobaczysz, w tym momencie może zawierać wartości `InsertMethod`, `UpdateMethod`, i `DeleteMethod` właściwości, a także `DeleteParameters`. Kreatora wybierz źródło danych ObjectDataSource automatycznie określa metody z `ProductBLL` na potrzeby Wstawianie, aktualizowanie i usuwanie, chyba że jawnie wyczyszczone tych się, zostaną one uwzględnione w znacznikach powyżej.


Gdy użytkownik odwiedzi tę stronę, dane formantu sieci Web będzie wywoływać ObjectDataSource `Select` metody, która będzie wywoływać `ProductsBLL` klasy `GetProductByProductID(productID)` metody przy użyciu ustaloną wartość 5 `productID` parametr wejściowy. Metoda zwraca silnie typizowanego `ProductDataTable` obiekt, który zawiera pojedynczy wiersz z informacjami o Jacka Chef Gumbo mieszanego (produktu, zapewniając `ProductID` 5).


[![Wyświetlane są informacje o Chef Jacka Gumbo mieszanego](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Rysunek 5**: wyświetlane są informacje o Chef Jacka Gumbo mieszanego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Ustawienie wartości parametru na wartość właściwości formantu sieci Web

Parametr ObjectDataSource, którego wartości można również ustawić na podstawie wartości kontrolki na stronie sieci Web. Na przykład Przyjrzyjmy się GridView, który zawiera listę wszystkich dostawców, które znajdują się w kraju, określone przez użytkownika. Aby osiągnąć ten start przez dodanie pola tekstowego do strony, do którego użytkownik może wprowadzić nazwę kraju. Ustaw ten formant TextBox `ID` właściwość `CountryName`. Również dodać kontrolkę przycisku w sieci Web.


[![Dodawanie pola tekstowego do strony zawierającej identyfikator CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Rysunek 6**: Dodaj pole tekstowe do strony zawierającej `ID` `CountryName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image18.png))


Następnie dodaj GridView do strony i za pomocą tagu inteligentnego, wybierz można dodać nowego elementu ObjectDataSource. Ponieważ chcemy wyświetlić wybierz informacje o dostawcy `SuppliersBLL` klasy z pierwszym ekranie kreatora. Na drugim ekranie Wybierz `GetSuppliersByCountry(country)` metody.


[![Wybierz metodę GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Rysunek 7**: Wybierz `GetSuppliersByCountry(country)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image21.png))


Ponieważ `GetSuppliersByCountry(country)` metoda ma parametr wejściowy, w kreatorze są dostępne ponownie ekran końcowy dotyczące wybierania wartości parametru. Tym razem Ustaw źródło parametru do formantu. Spowoduje to wypełnienie listy rozwijanej ControlID o nazwach formantów na stronie; Wybierz `CountryName` kontrolki z listy. Po pierwsze odwiedzenia strony `CountryName` pole tekstowe jest puste, dzięki czemu żadne wyniki nie są zwracane i będą wyświetlane żadne informacje. Jeśli chcesz wyświetlać pewnych wyników domyślnie, należy odpowiednio ustawić pole tekstowe DefaultValue.


[![Ustaw wartość parametru wartości kontrolki CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Rysunek 8**: Ustaw wartość parametru `CountryName` wartości formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image24.png))


ObjectDataSource oznaczeniu deklaracyjnym różni się nieco od naszym pierwszym przykładzie przy użyciu [parametrze ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) zamiast standardowego `Parameter` obiektu. A `ControlParameter` ma dodatkowe właściwości, aby określić `ID` kontrolki sieci Web i wartości właściwości, aby użyć jako parametru (`PropertyName`). W Kreatorze konfigurowania źródła danych była inteligentnego określić, że dla pola tekstowego, firma Microsoft będzie prawdopodobnie zechcesz użyć `Text` właściwości dla wartości parametru. Jeśli jednak chcesz korzystała z wartości różnych właściwości z formantu sieci Web można zmienić `PropertyName` wartości w tym miejscu lub klikając łącze "Pokaż zaawansowane właściwości" w kreatorze.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Podczas wyświetlania strony po raz pierwszy `CountryName` pole tekstowe jest puste. ObjectDataSource `Select` nadal wywoływana jest metoda widoku GridView, ale wartość `Nothing` jest przekazywana do `GetSuppliersByCountry(country)` metody. Konwertuje TableAdapter `Nothing` w bazie danych `NULL` wartość (`DBNull.Value`), ale zapytania używanego przez `GetSuppliersByCountry(country)` metoda jest napisana w taki sposób, że nie zwracać dowolne wartości, gdy `NULL` określono wartość dla `@CategoryID`parametru. Krótko mówiąc dostawców nie są zwracane.

Po użytkownik przechodzi w kraju, jednak i klika przycisk Pokaż dostawców powoduje odświeżenie strony, kontrolki ObjectDataSource firmy `Select` ponowieniu metody, przekazując w formancie TextBox `Text` wartość jako `country` parametru.


[![Tych dostawców z Kanady są wyświetlane.](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Rysunek 9**: tych dostawców z Kanady są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Wyświetlanie wszystkich dostawców domyślnie

Raczej niż Pokaż brak dostawców, gdy najpierw wyświetlanie strony warto Pokaż *wszystkich* dostawców na początku, dzięki czemu użytkownik będzie listę w dół, wprowadzając nazwę kraju, w polu tekstowym. Jeśli pole tekstowe jest puste, `SuppliersBLL` klasy `GetSuppliersByCountry(country)` metody jest przekazywany w `Nothing` dla jego *`country`* parametr wejściowy. To `Nothing` zostanie następnie przekazana wartość w dół do warstwy DAL `GetSupplierByCountry(country)` metody, gdzie jest tłumaczony na bazę danych `NULL` wartość `@Country` parametr w następującym zapytaniu:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Wyrażenie `Country = NULL` zawsze zwróci wartość False, nawet w przypadku rekordów którego `Country` kolumna ma `NULL` wartość; w związku z tym, nie zwrócono żadnych rekordów.

Do zwrócenia *wszystkich* dostawców, gdy kraju pole tekstowe jest pusta, możemy rozszerzyć `GetSuppliersByCountry(country)` LOGIKI do wywołania metody `GetSuppliers()` metody, gdy jest parametrem kraju `Nothing` oraz wywołanie DAL `GetSuppliersByCountry(country)` w przeciwnym razie metoda. Będzie to miało wpływ zwracanie wszystkich dostawców, jeśli nie określono żadnych kraju i odpowiedniego podzestawu dostawców, gdy parametr kraj jest uwzględniony.

Zmiana `GetSuppliersByCountry(country)` method in Class metoda `SuppliersBLL` klasy do następującego:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Dzięki tej zmianie `DeclarativeParams.aspx` stronie znajdują się wszyscy dostawcy, po raz pierwszy odwiedzony (lub gdy `CountryName` pole tekstowe jest puste).


[![Wszystkich dostawców są teraz wyświetlane domyślnie](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Na rysunku nr 10**: wszystkich dostawców są teraz wyświetlane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Podsumowanie

Aby użyć metody z parametrami wejściowymi, należy określić wartości parametrów w ObjectDataSource `SelectParameters` kolekcji. Różne rodzaje parametrów pozwalają na wartość parametru, które mają zostać uzyskane z innych źródeł. Domyślny typ parametru korzysta z ustaloną wartość, ale równie łatwo (i bez linii kodu) z querystring, zmienne sesji, pliki cookie i nawet wprowadzonych przez użytkownika wartości z formantów sieci Web na tej stronie można uzyskać wartości parametrów.

Przykłady, które przyjrzeliśmy się w tym samouczku przedstawiono sposób używania wartości parametrów deklaratywnego. Jednakże może występować sytuacje, gdy będziemy musieli używać źródła parametr, który nie jest dostępne, takich jak bieżącą datę i godzinę lub, jeśli naszą witrynę używano członkostwa, identyfikator użytkownika, obiekt odwiedzający. W przypadku takich scenariuszy firma Microsoft może podać wartości parametrów programowo przed kontrolki ObjectDataSource wywołania metody jej obiektu źródłowego. Zobaczymy, jak w tym w [następnego samouczka](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-objectdatasource-vb.md)
> [dalej](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
