---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: Deklaracyjne parametrów (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku firma Microsoft będzie pokazano, jak użyć parametru ustawioną wartość wpisaną na stałe, aby wybrać dane do wyświetlenia w formancie widoku DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 840630852d28f49f4f4387f1d2cc6b275b468fc2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875942"
---
<a name="declarative-parameters-c"></a>Deklaracyjne parametrów (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe) lub [pobierania plików PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> W tym samouczku firma Microsoft będzie pokazano, jak użyć parametru ustawioną wartość wpisaną na stałe, aby wybrać dane do wyświetlenia w formancie widoku DetailsView.


## <a name="introduction"></a>Wprowadzenie

W [ostatniego samouczek](displaying-data-with-the-objectdatasource-cs.md) analizujemy wyświetlanie danych z widoku GridView widoku DetailsView i FormView formantów związanych z kontrolki ObjectDataSource, który wywołał `GetProducts()` metody `ProductsBLL` klasy. `GetProducts()` Metoda zwraca jednoznacznie DataTable wypełnioną wszystkie rekordy z bazy danych Northwind `Products` tabeli. `ProductsBLL` Klasa zawiera dodatkowe metody dla zwracanie tylko podzbiór produktów - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, i `GetProductsBySupplierID(supplierID)`. Te trzy metody oczekiwać, że parametr wejściowy wskazujący sposób filtrowania informacji o produkcie zwrócony.

Element ObjectDataSource może służyć do wywoływania metod, które oczekują parametrów wejściowych, ale aby zrobić, należy określić, skąd pochodzą wartości tych parametrów. Wartości parametrów, które mogą być ustalony lub mogą pochodzić z różnych źródeł dynamicznej łącznie: wartości querystring, zmienne sesji, wartość właściwości formantu sieci Web na stronie lub innym osobom.

W tym samouczku Zacznijmy od pokazujący sposób użycia parametru ustawioną wartość ustalony. W szczególności przyjrzymy dodanie do strony, która wyświetla informacje dotyczące określonego produktu, a mianowicie Jacka Chef Gumbo mieszany, który ma element DetailsView `ProductID` 5. Następnie będzie przedstawiono sposób ustawiania wartości parametru na podstawie formantu sieci Web. W szczególności użyjemy pole tekstowe użytkownikowi należy wpisać w danym kraju, po upływie którego można kliknąć przycisk, aby wyświetlić listę dostawców, które znajdują się w tym kraju.

## <a name="using-a-hard-coded-parameter-value"></a>Przy użyciu wartości parametru ustalony

Na przykład pierwszy, Rozpocznij od dodania formantu widoku DetailsView `DeclarativeParams.aspx` strony `BasicReporting` folderu. W widoku DetailsView tagów inteligentnych, wybierz &lt;nowe źródło danych&gt; z listy rozwijanej liście i wybierz opcję Dodaj element ObjectDataSource.


[![Na stronie Dodaj element ObjectDataSource](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**Rysunek 1**: na stronie Dodaj element ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image3.png))


Spowoduje to automatyczne uruchomienie kreatora wybierz źródło danych kontrolki ObjectDataSource. Wybierz `ProductsBLL` klasy na pierwszym ekranie kreatora.


[![Wybierz klasę ProductsBLL](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**Rysunek 2**: Wybierz `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image6.png))


Ponieważ chcemy wyświetlić informacje dotyczące określonego produktu chcemy użyć `GetProductByProductID(productID)` metody.


[![Wybierz metodę GetProductByProductID(productID)](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**Rysunek 3**: Wybierz `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image9.png))


Ponieważ metoda, którą wybrano zawiera parametr, jest jednym ekranie więcej kreatora, gdy system prosi o definiują wartości do użycia dla parametru. Na liście z lewej strony pokazano wszystkie parametry dla wybranej metody. Aby uzyskać `GetProductByProductID(productID)` jest tylko jeden `productID`. Po prawej stronie można określić wartość parametru wybrane. Listy rozwijanej Źródło parametru wylicza różnych źródeł możliwe wartości parametru. Ponieważ chcemy określić wartość ustalony 5 dla `productID` parametru, pozostaw źródło parametru None, a następnie wprowadź 5 w polu tekstowym DefaultValue.


[![Hard-Coded parametru z 5 zostanie użyta wartość dla parametru productID](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**Rysunek 4**: A Hard-Coded parametru z 5 zostanie użyta wartość dla `productID` parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image12.png))


Po zakończeniu pracy kreatora skonfiguruj źródło danych zawiera kod znaczników deklaratywne kontrolki ObjectDataSource `Parameter` obiektu w `SelectParameters` kolekcji dla wszystkich parametrów wejściowych oczekiwany przez metody zdefiniowanej w `SelectMethod` Właściwość. Ponieważ używamy w tym przykładzie metoda oczekuje tylko jednego parametru wejściowego, `parameterID`, jest tylko jeden wpis w tym miejscu. `SelectParameters` Kolekcja może zawierać dowolną klasę, która pochodzi z `Parameter` klasy w `System.Web.UI.WebControls` przestrzeni nazw. Dla parametru ustalony wartości podstawowym `Parameter` klasa jest używana, ale w parametrze inne opcje pochodnego źródła `Parameter` klasa jest używana; można także tworzyć własne [typy parametrów niestandardowych](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), jeśli to konieczne.

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> Jeśli postępujesz zgodnie z na własnym komputerze deklaratywne znaczników zostanie wyświetlony na tym etapie może zawierać wartości `InsertMethod`, `UpdateMethod`, i `DeleteMethod` właściwości, a także `DeleteParameters`. Element ObjectDataSource kreatora wybierz źródło danych automatycznie określa metody z `ProductBLL` do użycia podczas wstawiania, aktualizowania i usuwania, chyba że jawnie wyczyszczone tych wychodzących, zostaną one uwzględnione w znaczniku powyżej.


Podczas odwiedzania tej strony, danych formantu sieci Web zostaną wywołane przez element ObjectDataSource `Select` metodę, która wywoła `ProductsBLL` klasy `GetProductByProductID(productID)` metodę przy użyciu wartości stałe 5 `productID` parametru wejściowego. Metoda zwróci silnie typizowanego `ProductDataTable` obiekt, który zawiera pojedynczy wiersz z informacjami o Jacka Chef Gumbo mieszanego (produkt o `ProductID` 5).


[![Wyświetlane są informacje o Chef Jacka Gumbo mieszany](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**Rysunek 5**: wyświetlane są informacje o Chef Jacka Gumbo mieszanego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Ustawienie wartości parametru na wartość właściwości formantu sieci Web

Element ObjectDataSource parametr, który można ustawić wartości na podstawie wartości formantu na stronie sieci Web. Na przykład załóżmy ma element GridView, który wyświetla listę wszystkich dostawców znajdujących się w danym kraju określone przez użytkownika. Do wykonania tego uruchomienia przez dodanie pola tekstowego do strony, do którego użytkownik może wprowadzić nazwę kraju. Ustaw tego formantu TextBox `ID` właściwości `CountryName`. Również dodać kontrolkę przycisku w sieci Web.


[![Na stronie o identyfikatorze CountryName Dodaj pole tekstowe](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**Rysunek 6**: Dodaj pole tekstowe do strony z `ID` `CountryName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image18.png))


Następnie dodaj element GridView do strony i z tagów inteligentnych, wybierz można dodać nowego elementu ObjectDataSource. Ponieważ chcemy wyświetlić wybierz informacje o dostawcy `SuppliersBLL` klasy z pierwszym ekranie kreatora. Na ekranie drugi wybierz `GetSuppliersByCountry(country)` metody.


[![Wybierz metodę GetSuppliersByCountry(country)](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**Rysunek 7**: Wybierz `GetSuppliersByCountry(country)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image21.png))


Ponieważ `GetSuppliersByCountry(country)` metoda ma parametr wejściowy, Kreator ponownie zawiera ekranie końcowym dla wartości parametru. Teraz, ustawioną kontroli źródła parametru. Spowoduje to wypełnienie listy rozwijanej ControlID z nazwami formantów na stronie; Wybierz `CountryName` kontroli z listy. Po pierwsze odwiedzenia strony `CountryName` pola tekstowego będzie puste, więc żadne wyniki nie są zwracane i nie będą wyświetlane żadne. Jeśli chcesz wyświetlić wyniki niektórych domyślnie ustawiony odpowiednio pole tekstowe DefaultValue.


[![Ustaw wartość parametru na wartość formantu CountryName](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**Rysunek 8**: Ustaw wartość parametru `CountryName` wartości formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image24.png))


Znaczników deklaracyjne element ObjectDataSource różni się nieco z pierwszego przykładu, za pomocą [parametrze ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) zamiast standardowego `Parameter` obiektu. A `ControlParameter` ma dodatkowe właściwości, aby określić `ID` kontrolka sieci Web i wartości właściwości do użycia dla parametru (`PropertyName`). Kreator konfigurowania źródła danych nie inteligentne określić, że dla pola tekstowego, firma Microsoft będzie prawdopodobnie zechcesz użyć `Text` właściwości dla wartości parametru. Jeśli jednak chcesz użyć wartości różnych właściwości z formantu sieci Web można zmienić `PropertyName` wartości w tym miejscu lub klikając łącze "Pokaż zaawansowane właściwości" w kreatorze.

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

Podczas odwiedzania strony po raz pierwszy `CountryName` pole tekstowe jest pusta. Element ObjectDataSource `Select` widoku GridView, ale wartość nadal jest wywoływana metoda `null` została przekazana do `GetSuppliersByCountry(country)` metody. Konwertuje TableAdapter `null` w bazie danych `NULL` wartość (`DBNull.Value`), ale zapytania używanego przez `GetSuppliersByCountry(country)` metodę jest zapisywany w taki sposób, że nie zwraca żadnego wartości, gdy `NULL` określono wartość dla `@CategoryID`parametru. Krótko mówiąc są zwracane nie dostawców.

Po obiekt odwiedzający przechodzi w danym kraju, jednak i kliknie przycisk Pokaż dostawców powoduje odświeżenie strony, ObjectDataSource w `Select` ponowieniu metody, przekazując formantu TextBox `Text` wartość jako `country` parametru.


[![Tych dostawców z Kanady są wyświetlane.](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**Rysunek 9**: przedstawiono tych dostawców z Kanady ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Wyświetlanie wszystkich dostawców domyślnie

Zamiast niż w przypadku najpierw wyświetlania strony nie wykazuje żadnych dostawców może chęć pokazania *wszystkie* dostawców na początku, umożliwiając użytkownikowi nawiasu listę w dół, wprowadzając w polu tekstowym Nazwa kraju. Gdy pole tekstowe jest pusta, `SuppliersBLL` klasy `GetSuppliersByCountry(country)` metody jest przekazywany w `null` wartość jego *`country`* parametru wejściowego. To `null` wartości są następnie przekazywane w dół do warstwy DAL `GetSupplierByCountry(country)` metody, gdzie jest translacja do bazy danych `NULL` wartość `@Country` parametr w następującym zapytaniu:

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

Wyrażenie `Country = NULL` zawsze zwraca wartość False, nawet w przypadku rekordów którego `Country` kolumna ma `NULL` wartości; w związku z tym nie zwrócono żadnych rekordów.

Aby powrócić *wszystkie* dostawców, gdy kraju pole tekstowe jest pusta, firma Microsoft może rozszerzyć `GetSuppliersByCountry(country)` w logiki warstwy Biznesowej do wywołania metody `GetSuppliers()` metody po jej parametr kraju `null` oraz wywołanie DAL `GetSuppliersByCountry(country)` w przeciwnym razie — metoda Ma to wpływu zwracanie wszystkich dostawców, jeśli nie określono żadnych kraju i odpowiednie podzestaw dostawców, gdy parametr kraju jest dołączony.

Zmień `GetSuppliersByCountry(country)` metoda `SuppliersBLL` klasy do następującego:

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

Z tą zmianą `DeclarativeParams.aspx` są wyświetlane wszystkie dostawców po odwiedzeniu najpierw (lub po każdej zmianie `CountryName` pole tekstowe jest pusta).


[![Wszystkich dostawców są teraz wyświetlane domyślnie](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**Na rysunku nr 10**: wszystkich dostawców są teraz wyświetlane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>Podsumowanie

Aby można było używać metody z parametrami wejściowymi, należy określić wartości parametrów w elemencie ObjectDataSource `SelectParameters` kolekcji. Zezwalaj na różnych typów parametrów dla wartości parametru, które mają zostać uzyskane z różnych źródeł. Domyślny typ parametru używa stałe wartości, ale równie łatwo (i bez linii kodu) można uzyskać wartości parametrów z querystring, zmienne sesji, plików cookie i wartości z formantów sieci Web na stronie nawet wprowadzonych przez użytkownika.

Przykłady, które analizujemy w tym samouczku przedstawiono sposób użycia wartości deklaratywne parametrów. Jednak mogą wystąpić sytuacje, kiedy należy używać źródła parametr, który nie jest dostępny, takich jak bieżącą datę i godzinę lub, jeśli naszej witrynie korzystał z członkostwa, identyfikator użytkownika osoby odwiedzającej jest możliwe. W takich scenariuszach możemy ustawić wartości parametru programowo przed ObjectDataSource wywołania metody jego obiekt źródłowy. Zajmiemy się tym, jak w tym w celu [następny samouczek](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md).

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-objectdatasource-cs.md)
> [dalej](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
