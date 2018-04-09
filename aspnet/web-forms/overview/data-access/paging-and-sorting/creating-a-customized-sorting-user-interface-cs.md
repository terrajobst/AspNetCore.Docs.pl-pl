---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Tworzenie interfejsu użytkownika sortowania dostosowanych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas wyświetlania długą listę sortowania danych, może być bardzo przydatne do grupowania powiązanych danych dzięki zastosowaniu separator wierszy. W tym samouczku zajmiemy się tym, jak uż...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c2680f5e47883c9d5fa874a36eb666270c5e406a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Tworzenie interfejsu użytkownika sortowania dostosowanych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) lub [pobierania plików PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Podczas wyświetlania długą listę sortowania danych, może być bardzo przydatne do grupowania powiązanych danych dzięki zastosowaniu separator wierszy. W tym samouczku będzie przedstawiono sposób tworzenia takich sortowania interfejsu użytkownika.


## <a name="introduction"></a>Wprowadzenie

Podczas wyświetlania długą listę sortowania danych w przypadku, gdy istnieją tylko kilka różnych wartości w kolumnie posortowana, użytkownik końcowy może być bardzo trudne do wykrycia, dokładnie, granice różnica mieć miejsce. Na przykład istnieją 81 produktów w bazie danych, ale tylko dziewięć opcji różnych kategorii (osiem kategorii unikatowy plus `NULL` opcja). Rozważmy przypadek użytkownik chcący badanie produktów, które są objęte kategorii ryby. Ze strony, która zawiera *wszystkie* produktów w jednym widoku GridView, użytkownik może zdecydować, jej najlepiej Sortuj wyniki według kategorii, które zostaną zgrupowane wszystkich produktów ryby razem. Po posortowaniu według kategorii, następnie należy wyszukiwania za pomocą listy, wyszukiwanie gdzie produktów pogrupowane ryby rozpoczęcia i zakończenia. Ponieważ wyniki są w kolejności alfabetycznej przez nazwę kategorii produktów ryby wyszukiwanie nie jest trudne, ale nadal wymaga skanowania ściśle listę elementów w siatce.

Aby wyróżnić granice między grupami posortowane, wiele witryn sieci Web stosować interfejsu użytkownika, który dodaje znak oddzielający takich grup. Separatory, jak pokazano na rysunku 1 te umożliwia użytkownikowi szybciej znaleźć określonej grupy i zidentyfikować jego granic, a także ustalić, jakie różne grupy istnieje w danych.


[![Każda grupa kategorii jest jasno określone](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Rysunek 1**: każdy grupy kategorii jest zidentyfikowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


W tym samouczku będzie przedstawiono sposób tworzenia takich sortowania interfejsu użytkownika.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Krok 1: Tworzenie standardowych, sortowanie widoku GridView

Zanim firma Microsoft Eksploruj jak rozszerzyć GridView zapewnienie udoskonalony interfejs sortowania, chętnie s najpierw utworzyć GridView standardowe, sortowanie, który zawiera listę produktów. Uruchamianie przez otwarcie `CustomSortingUI.aspx` strony `PagingAndSorting` folderu. Na stronie Dodaj element GridView, ustaw jej `ID` właściwości `ProductList`i powiązać ją z nowego elementu ObjectDataSource. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy s `GetProducts()` metoda wybierania rekordów.

Następnie skonfiguruj widoku GridView, tak aby zawierała tylko `ProductName`, `CategoryName`, `SupplierName`, i `UnitPrice` BoundFields i CheckBoxField przerywane. Na koniec skonfiguruj GridView do obsługuje sortowanie, zaznaczając pole wyboru Włącz sortowanie w tagu s GridView (lub przez ustawienie jej `AllowSorting` właściwości `true`). Po wprowadzeniu tych dodatków do `CustomSortingUI.aspx` strony, deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić postęp naszych dotychczasowych w przeglądarce. Na rysunku 2 przedstawiono sortowanie widoku GridView, gdy jego dane są sortowane według kategorii w kolejności alfabetycznej.


[![S GridView można sortować według kategorii porządkowania danych](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Rysunek 2**: s sortowania w widoku GridView danych porządkowania według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Krok 2: Poszukiwania techniki Dodawanie wierszy separatora

Z ogólnym, sortowanie widoku GridView pełną liście nie zostanie aby można było dodać separator wierszy w widoku GridView przed każdej unikatowej grupy posortowane. Jednak jak można te wiersze można wprowadzić do widoku GridView? Zasadniczo firma Microsoft należy do iteracji w widoku GridView wierszy s, określić, gdzie wystąpić różnice między wartościami w sortowanej kolumny, a następnie dodaj odpowiedni separator wierszy. W przypadku informacje o tym problemie, prawdopodobnie fizyczne, czy rozwiązanie znajduje się gdzieś w widoku GridView s `RowDataBound` program obsługi zdarzeń. Jak wspomniano w [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-cs.md) samouczek, ten program obsługi zdarzeń jest często stosowany podczas formatowania poziomu wierszy na podstawie wiersze danych. Jednak `RowDataBound` program obsługi zdarzeń nie jest rozwiązaniem w tym miejscu wierszy można dodać do widoku GridView programowo z tej obsługi zdarzeń. GridView s `Rows` kolekcji, w rzeczywistości jest tylko do odczytu.

Aby dodać dodatkowe wiersze do widoku GridView mamy trzy opcje:

- Dodaj te metadane separator wierszy do danych rzeczywistych, który jest powiązany z widoku GridView
- Po widoku GridView został powiązany z danymi, dodać dodatkowe `TableRow` wystąpień GridView s kontrolować kolekcji
- Tworzenie formantu niestandardowego serwera kontrolki widoku siatki i zastępuje te metody odpowiedzialny za utworzenie struktury s widoku GridView

Tworzenie formantu niestandardowego serwera będzie najlepszym rozwiązaniem, jeśli ta funkcja było wymagane na wielu stronach sieci web lub w kilku witryn sieci Web. Jednak pociąga dość bit kodu i dokładne badań do głębokości wewnętrzne działanie s widoku GridView. Dlatego firma Microsoft nie przyjrzymy się tej opcji w przypadku tego samouczka.

Dodawanie wierszy separatora do trwa rzeczywiste dane innych dwie opcje powiązane z widoku GridView i operowanie nimi kolekcji formantów s GridView po jego został powiązany — inaczej ataki problemu i wymagają dyskusji.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Dodawanie wierszy do dane powiązane z widoku GridView

Gdy widoku GridView jest powiązana ze źródłem danych, tworzy `GridViewRow` dla każdego rekordu zwracanego przez źródło danych. W związku z tym można używamy wierszy separatora wymagane przez dodanie separatora rekordy w źródle danych przed jego powiązanie z widoku GridView. Rysunek 3 ilustruje tę koncepcję.


![Techniki co obejmuje dodawanie Separator wierszy w źródle danych](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Rysunek 3**: jeden technika obejmuje dodawanie Separator wierszy w źródle danych


Używać rekordów separatora termin w cudzysłowie, ponieważ nie istnieje rekord specjalny separatora; zamiast firma Microsoft musi jakiś sposób Flaga które określonego rekordu w źródle danych służy jako separator zamiast wiersz danych normalnego. Dla naszych przykładach, możemy re powiązania `ProductsDataTable` wystąpienie GridView, który składa się z `ProductRows`. Firma Microsoft może Flaga rekord jako wiersz separatora, ustawiając jego `CategoryID` właściwości `-1` (ponieważ taki wartość t włączoną istnieje zwykle).

Korzystanie z tej techniki d należy wykonać następujące czynności:

1. Programowane pobieranie danych, aby powiązać z widoku GridView ( `ProductsDataTable` wystąpienia)
2. Sortowanie danych w oparciu GridView s `SortExpression` i `SortDirection` właściwości
3. Iterowanie za pomocą `ProductsRows` w `ProductsDataTable`poszukujący gdzie znajdują się różnic w sortowanej kolumny
4. W każdej grupy granic, Wstaw rekord separatora `ProductsRow` wystąpienie do elementu DataTable, jeden zawierający s `CategoryID` ustawioną `-1` (lub niezależnie od oznaczenia została podjęta podczas zaznaczania rekord jako rekord separatora)
5. Po wstrzyknięcie separator wierszy, programowo powiązać dane z widoku GridView

Oprócz tych kroków pięć, d również potrzebujemy zapewnienie obsługi zdarzeń dla widoku GridView s `RowDataBound` zdarzeń. W tym miejscu d sprawdzamy każdego `DataRow` i określenie, czy separator wierszy jedną których `CategoryID` ustawienie zostało `-1`. Jeśli tak, d prawdopodobnie chcemy dopasować, formatowanie lub tekst wyświetlany w komórkach.

Ta technika wstrzyknięcie sortowania granice grupy wymaga nieco więcej pracy niż opisane powyżej, jak należy również podać program obsługi zdarzeń dla widoku GridView s `Sorting` zdarzeń i Zachowaj prowadnicy `SortExpression` i `SortDirection` wartości.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulowanie GridView s kontrolować kolekcji po nim s został powiązany z danymi

Zamiast wiadomości danych przed jego powiązanie z widoku GridView, można dodać separator wierszy *po* danych została powiązana z widoku GridView. Proces wiązania z danymi kompilacje w widoku GridView s kontroli hierarchii, które w rzeczywistości jest po prostu `Table` wystąpienia składa się z kolekcji wierszy, z których każdy składa się z kolekcji komórek. W szczególności zawiera kolekcji formantów s GridView `Table` obiektem głównym, `GridViewRow` (która jest pochodną `TableRow` klasy) dla każdego rekordu w `DataSource` powiązany Element GridView i `TableCell` obiektu w każdym `GridViewRow` wystąpienia dla każdego pola danych w `DataSource`.

Aby dodać separator wierszy między wszystkimi grupami sortowania, firma Microsoft bezpośrednio manipulować tej hierarchii kontroli po jego utworzeniu. Możemy mieć pewność, że utworzono hierarchii kontrolki GridView s po raz ostatni w czasie, który jest renderowany strony. W związku z tym ta metoda zastępuje `Page` klasy s `Render` metody w takim przypadku hierarchia końcowej kontroli s widoku GridView zostanie zaktualizowany wymagane separator wierszy. Rysunek 4 przedstawia tego procesu.


[![Technikę alternatywny manipuluje hierarchii kontroli s widoku GridView](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Rysunek 4**: technikę alternatywny manipuluje s GridView hierarchii formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


W tym samouczku użyjemy tego podejścia to drugie dostosowywaniu sortowania środowiska użytkownika.

> [!NOTE]
> Kod I m umożliwienie korzystania z tego samouczka jest oparta na przykładzie przedstawionym w [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) wpis w blogu s [odtwarzanie z bitowego grupowanie sortowania w widoku GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Krok 3: Dodawanie Separator wierszy w widoku GridView s kontroli hierarchii

Ponieważ chcemy tylko dodać separator wierszy w widoku GridView s kontroli hierarchii po hierarchii formantu został utworzony i jest tworzone po raz ostatni w tym odwiedź stronę, chcemy, aby wykonać to dodawanie na końcu cyklu życia strony, ale przed rzeczywiste c widoku GridView Hierarchia kontrolne zostało wyrenderowane w kodzie HTML. Najnowszy punkt możliwe, w którym możemy w tym celu jest `Page` klasy s `Render` zdarzenie, które firma Microsoft mogą zastąpić w naszej klasy związane z kodem przy użyciu następujących podpis metody:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Gdy `Page` klasy s oryginalnego `Render` wywoływana jest metoda `base.Render(writer)` każdej kontrolki na stronie są wyświetlane, generowania znaczników na podstawie ich hierarchii formantu. W związku z tym jest konieczne, że oba nazywamy `base.Render(writer)`, dzięki czemu realizacją strony hierarchii przed wywołaniem kontrolę i czy możemy manipulować GridView s `base.Render(writer)`, dzięki czemu separator wierszy zostały dodane do hierarchii formantu widoku GridView s przed s zostały renderowane.

Iniekcję nagłówków grup sortowania najpierw musimy upewnij się, że użytkownik zażądał sortowania danych. Domyślnie zawartość widoku GridView s nie są sortowane i w związku z tym możemy ADAM t trzeba wprowadzić dowolną grupę sortowania nagłówków.

> [!NOTE]
> Widoku GridView ma zostać posortowana według określonej kolumny po pierwszym załadowaniu strony należy wywołać metodę GridView `Sort` metody pierwszej wizyty strony (ale nie znajduje się w kolejnych ogłaszania zwrotnego). W tym celu należy dodać to wywołanie w `Page_Load` obsługi zdarzeń w `if (!Page.IsPostBack)` warunkowego. Odwołaj się do [stronicowania i sortowania danych raportów](paging-and-sorting-report-data-cs.md) samouczka informacji, aby uzyskać więcej informacji na temat `Sort` metody.


Przy założeniu, że dane zostały posortowane, naszych następne zadanie ma na celu określenie kolumny danych został posortowane według i następnie skanowanie wierszy wyszukiwanie różnice w tej kolumnie s wartości. Poniższy kod zapewnia, że dane zostały posortowane i znajdzie kolumny, według której zostały posortowane dane:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Jeśli w widoku GridView jest jeszcze być sortowana, s widoku GridView `SortExpression` zostanie nie ustawiono właściwości. W związku z tym będą tylko dodać separator wierszy, jeśli ta właściwość nie ma wartości. Jeśli tak, obok należy określić indeks kolumny za pomocą którego została posortowane dane. Jest to osiągane przez pętli GridView s `Columns` kolekcji wyszukiwania dla kolumny, których `SortExpression` s widoku GridView jest równa właściwości `SortExpression` właściwości. Oprócz indeks kolumny s, będziemy również pobrania `HeaderText` właściwość, która jest używana podczas wyświetlania separator wierszy.

Indeks kolumny za pomocą którego dane są sortowane ostatnim krokiem jest wyliczyć wierszach widoku GridView. Dla każdego wiersza należy ustalić, czy wartość s sortowanej kolumny różni się od poprzedniej wartości s kolumny wiersza s sortowane. Jeśli tak, musimy wprowadzić nowy `GridViewRow` wystąpienia w hierarchii formantu. Jest to realizowane przy użyciu następującego kodu:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Ten kod uruchamia programowo odwołując `Table` obiekt znalezione w głównym hierarchii kontrolki GridView s i tworzenia zmiennej ciągu o nazwie `lastValue`. `lastValue` Służy do porównywania wartości bieżącego wiersza s sortowana kolumna z poprzedniej wartości wiersza s. Następnie, w widoku GridView s `Rows` wyliczeniu kolekcji i dla każdego wiersza wartości sortowanej kolumny jest przechowywany w `currentValue` zmiennej.

> [!NOTE]
> Aby określić wartość kolumny s sortowane określonego wiersza I Użyj komórki s `Text` właściwości. To dobrze się sprawdza BoundFields, ale będzie nie działają zgodnie z potrzebami dla TemplateFields, CheckBoxFields itd. Wyjaśniono, jak konta alternatywne pól widoku GridView wkrótce.


`currentValue` i `lastValue` zmienne są porównywane. Jeśli będą się różnić musimy dodać nowy wiersz separatora do hierarchii kontroli. Jest to osiągane przez określenie indeks `GridViewRow` w `Table` obiektu s `Rows` kolekcji, tworzenie nowych `GridViewRow` i `TableCell` wystąpienia, a następnie dodanie `TableCell` i `GridViewRow` do Hierarchia formantu.

Należy pamiętać, że separator wierszy s pojedynczy `TableCell` jest sformatowany w taki sposób, że obejmuje całą szerokość GridView, jest sformatowany przy użyciu `SortHeaderRowStyle` klasy CSS i ma jego `Text` właściwości takie zawiera grupy sortowania nazwę (na przykład kategoria) i Wartość s grupy (na przykład napoje). Na koniec `lastValue` zostaną zmienione na wartość `currentValue`.

Klasa CSS używany do formatowania sortowania wiersz nagłówka grupy `SortHeaderRowStyle` musi być określone w `Styles.css` pliku. Możesz także użyć niezależnie od ustawień stylu odwołania Po użyciu następujące czynności:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Bieżący kod sortowania interfejsu dodaje nagłówki grup sortowania podczas sortowania według dowolnego elementu BoundField (patrz rysunek 5, która zawiera zrzut ekranu podczas sortowania przez dostawcę). Niemniej jednak podczas sortowania według dowolnego typu pola (na przykład CheckBoxField lub TemplateField), nagłówki grupy sortowania są nigdzie nie można odnaleźć (patrz rysunek 6).


[![Interfejs sortowania zawiera nagłówki grup sortowania podczas sortowania według BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Rysunek 5**: sortowanie interfejsu obejmuje sortowania grupy nagłówków podczas sortowania według BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Nagłówki grupy sortowania są, Brak podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Rysunek 6**: sortowanie grupy nagłówki są Brak podczas sortowania CheckBoxField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Przyczyna nagłówków grup sortowania brakuje podczas sortowania według CheckBoxField jest ponieważ kod aktualnie używa tylko `TableCell` s `Text` właściwości w celu określenia wartości sortowanej kolumny dla każdego wiersza. Dla CheckBoxFields `TableCell` s `Text` właściwości to ciąg pusty; zamiast tego wartość jest dostępna za pośrednictwem formant wyboru sieci Web, który znajduje się w `TableCell` s `Controls` kolekcji.

Aby obsłużyć pola typu innego niż BoundFields, musimy rozszerzyć kod gdzie `currentValue` zmienna jest przypisana do sprawdzenia istnienia CheckBox w `TableCell` s `Controls` kolekcji. Zamiast `currentValue = gvr.Cells[sortColumnIndex].Text`, Zastąp kod następującym kodem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Ten kod sprawdza, czy kolumna posortowana `TableCell` dla bieżącego wiersza ustalić, czy formanty w `Controls` kolekcji. Jeśli istnieje, i pierwszą kontrolkę pola wyboru, `currentValue` zmienna jest ustawiona na tak lub nie, w zależności od wyboru s `Checked` właściwości. W przeciwnym razie wartość jest pobierana z `TableCell` s `Text` właściwości. Tej logiki można replikować do obsługi sortowania dla dowolnego TemplateFields, który może znajdować się w widoku GridView.

Z uwzględnieniem powyższych kodu, nagłówki grupy sortowania występują teraz podczas sortowania w polu CheckBoxField zaprzestać (patrz rysunek 7).


[![Nagłówki grupy sortowania są teraz obecny podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Rysunek 7**: sortowanie grupy nagłówki są teraz obecny podczas sortowania CheckBoxField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Jeśli masz w produktach `NULL` bazy danych wartości `CategoryID`, `SupplierID`, lub `UnitPrice` pola, te wartości będą wyświetlane jako puste ciągi w widoku GridView domyślnie, co oznacza tekst wiersza s separatora dla tych produktów z `NULL`odczyta wartości podobnie jak kategoria: (to znaczy występują s bez nazwy po kategorii: o kategorii, takich jak: napoje). Jeśli chcesz użyć wartości wyświetlane w tym miejscu można ustawić BoundFields [ `NullDisplayText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) tekst ma być wyświetlany lub dodać instrukcji warunkowej w metodzie renderowania podczas przypisywania `currentValue` do tego separatora. Wiersz s `Text` właściwości.


## <a name="summary"></a>Podsumowanie

Widoku GridView nie zawiera wiele wbudowanych opcji Dostosowywanie sortowania interfejsu. Jednak z bitowego kodu niskiego poziomu jego s można dostosować hierarchię GridView s kontroli w celu utworzenia bardziej dostosowanego interfejsu. W tym samouczku widzieliśmy sposób dodawania sortowania grupy separatora wiersza elementu GridView sortowanie, które łatwo identyfikuje różne grupy i tych grup granic. Aby uzyskać dodatkowe przykłady interfejsów sortowania dostosowanych, zapoznaj się z [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [kilka ASP.NET 2.0 GridView sortowanie porady i wskazówki](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) wpis w blogu.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](sorting-custom-paged-data-cs.md)
> [dalej](paging-and-sorting-report-data-vb.md)
