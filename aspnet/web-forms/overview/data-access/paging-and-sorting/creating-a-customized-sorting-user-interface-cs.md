---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Tworzenie dostosowanego interfejsu użytkownika sortowania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas wyświetlania długą listę posortowanych danych, może być bardzo przydatny do grupowania powiązanych danych dzięki zastosowaniu separator wierszy. W tym samouczku zobaczymy, jak poświadczeń...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 7671c86d105f7d8a4ef5ce65da500250dce47db0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369557"
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Tworzenie dostosowanego interfejsu użytkownika sortowania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) lub [Pobierz plik PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Podczas wyświetlania długą listę posortowanych danych, może być bardzo przydatny do grupowania powiązanych danych dzięki zastosowaniu separator wierszy. W tym samouczku opisano sposób tworzenia takiego sortowania interfejs użytkownika.


## <a name="introduction"></a>Wprowadzenie

Podczas wyświetlania długą listę posortowanych danych w przypadku zaledwie kilku różnych wartości w kolumnie posortowany, użytkownik końcowy może okazać się trudne do wykrycia, where, dokładnie granice różnica wystąpić. Na przykład istnieją 81 produktów w bazie danych, ale tylko dziewięć inną kategorię opcji (osiem kategorii unikatowy plus `NULL` opcji). Rozważmy przypadek użytkownika, który jest zainteresowany badanie produktów, które należą do kategorii owoce morza. Ze strony zawierającej zestawienie *wszystkich* produktów w jednym widoku GridView, użytkownik może zrezygnować jej najlepiej sortować wyniki według kategorii, które zostaną zgrupowane wszystkich produktów owoce morza razem. Po sortowaniu według kategorii, użytkownik musi możliwe za pośrednictwem listy, wyszukiwanie, dla których produkty pogrupowane owoce morza rozpoczęcia i zakończenia. Ponieważ wyniki są uporządkowane alfabetycznie według nazwy kategorii znajdowanie produktów owoce morza nie jest trudne, ale nadal wymaga ściśle przeszukaniu listy elementów w siatce.

Aby wyróżnić granice między grupami posortowany, wiele witryn sieci Web używają interfejsu użytkownika, który dodaje znak oddzielający takich grup. Separatory, jak w powyższym przedstawionej na rysunku 1 umożliwia użytkownikowi szybciej znaleźć określonej grupy i zidentyfikować jego granice, a także upewnić się, jakie różne grupy istnieje w danych.


[![Każda kategoria grupa jest jasno określone](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Rysunek 1**: każdej grupy kategorii są jasno określone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


W tym samouczku opisano sposób tworzenia takiego sortowania interfejs użytkownika.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Krok 1: Tworzenie standardowych, sortowanie kontrolki GridView

Zanim omówimy sposób rozszerzyć GridView zapewnia rozszerzoną interfejs sortowania, chętnie s GridView standardowe, sortowania, która zawiera listę produktów należy najpierw utworzyć. Zacznij od otwarcia `CustomSortingUI.aspx` stronie `PagingAndSorting` folderu. Na stronie Dodaj GridView, ustaw jego `ID` właściwości `ProductList`i powiąż go do nowego elementu ObjectDataSource. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy s `GetProducts()` metoda wybierania rekordów.

Następnie skonfiguruj widoku GridView w taki sposób, że zawiera on tylko `ProductName`, `CategoryName`, `SupplierName`, i `UnitPrice` BoundFields i CheckBoxField wycofane. Na koniec skonfiguruj GridView do obsługi sortowania, zaznaczając pole wyboru Włącz sortowanie w tagu inteligentnego s kontrolki GridView (lub przez ustawienie jego `AllowSorting` właściwość `true`). Po wprowadzeniu te dodatki do `CustomSortingUI.aspx` stronie oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić nasz postęp tej pory w przeglądarce. Rysunek 2 przedstawia sortowanie GridView po jego dane są sortowane według kategorii w kolejności alfabetycznej.


[![S GridView Sortowalna Data są uporządkowane według kategorii](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Rysunek 2**: s GridView można sortować dane są uporządkowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Krok 2: Eksplorowania technik Dodawanie wierszy separatora

Za pomocą rodzajowego, sortowania widoku GridView pełną pozostaje aby można było dodać separator wierszy w widoku GridView przed każdej unikatowej grupy posortowany. Ale jak można te wiersze wstrzyknięte do kontrolki GridView? Zasadniczo firma Microsoft należy do iterowania po GridView wierszy s, określić, gdzie występują różnice między wartościami w kolumna posortowanego, a następnie dodaj odpowiedni separator wiersza. Rozważania na temat tego problemu, wygląda na to fizyczne, czy rozwiązanie znajduje się gdzieś w GridView s `RowDataBound` programu obsługi zdarzeń. Tak jak Omówiliśmy to w [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-cs.md) samouczków, ta procedura obsługi zdarzeń jest często stosowany podczas stosowania formatowanie na poziomie wierszy na podstawie wiersza s danych. Jednak `RowDataBound` program obsługi zdarzeń nie jest rozwiązanie, w tym miejscu, ponieważ wierszy nie można dodać do widoku GridView programowo z tej obsługi zdarzeń. GridView s `Rows` kolekcji, w rzeczywistości jest tylko do odczytu.

Aby dodać dodatkowe wiersze do kontrolki GridView mamy trzy opcje:

- Dodaj te wiersze separator metadanych do rzeczywistych danych, który jest powiązany z widoku GridView
- Po widoku GridView została powiązana z danymi, Dodaj dodatkowe `TableRow` wystąpień GridView s kontrolować zbieranie
- Utwórz niestandardowy formant serwera w kontrolce GridView i zastępuje te metody, które są odpowiedzialne za konstruowanie struktury s GridView

Tworzenie formantu niestandardowego serwera będzie najlepszym rozwiązaniem, w razie potrzeby ta funkcja została na wielu stronach sieci web lub w kilku witryn sieci Web. Jednak pociąga znacznej kodu i dokładnego badań do odnajdywanie ukrytych wewnętrzne działanie s GridView. W związku z tym firma Microsoft będzie należy traktować tę opcję na potrzeby tego samouczka.

Pozostałe dwie opcje Dodawanie wierszy separator do bycia w rzeczywistych danych powiązany z kontrolki GridView i manipulowanie nimi kolekcji kontrolki GridView s, po jego został powiązany — inaczej ataki problem i że opiszemy je w dyskusji.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Dodawanie wierszy do danych powiązany z widoku GridView

Gdy widoku GridView jest powiązana ze źródłem danych, tworzy `GridViewRow` dla każdego rekordu zwrócony przez źródło danych. W związku z tym firma Microsoft wstrzyknąć wierszy separator wymagane przez dodanie separator rekordów do źródła danych przed wiążące go do widoku GridView. Rysunek 3 ilustruje tę koncepcję.


![Jedna z technik polega na dodawaniu Separator wierszy do źródła danych](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Rysunek 3**: jedna z technik polega na dodawaniu Separator wierszy do źródła danych


Korzystam z rekordów separator termin w cudzysłowie, ponieważ nie istnieje rekord specjalny separator; przeciwnie firma Microsoft musi jakiś sposób flagi, które określonego rekordu w źródle danych służy jako separator, a nie wiersz zwykłych danych. Dla naszych przykładach, możemy ponownie powiązania `ProductsDataTable` wystąpienie GridView, który składa się z `ProductRows`. Firma Microsoft może Flaga rekordu jako wiersz separatora, ustawiając jego `CategoryID` właściwość `-1` (ponieważ taki wartość istnieje zazwyczaj).

Korzystanie z tej techniki d należy wykonać następujące czynności:

1. Programowe pobieranie danych do powiązania do kontrolki GridView ( `ProductsDataTable` wystąpienia)
2. Sortowanie danych w oparciu GridView s `SortExpression` i `SortDirection` właściwości
3. Iteracyjne przeglądanie `ProductsRows` w `ProductsDataTable`poszukiwaniu gdzie znajdują się różnice w posortowanej kolumny
4. Na każdej granicy grupy wstawić rekordu separator `ProductsRow` wystąpienie do elementu DataTable, taki, który ma ona s `CategoryID` równa `-1` (lub dowolnych oznaczenia została podjęta podczas do oznaczania rekordu jako separator rekordu)
5. Po wstawianie wierszy separatora, programowo powiązać dane widoku GridView

Oprócz tych pięciu krokach d również należy podać program obsługi zdarzeń dla GridView s `RowDataBound` zdarzeń. W tym miejscu d sprawdzenie każdego `DataRow` i określenie, czy separator wierszy jeden którego `CategoryID` ustawienie zostało `-1`. Jeśli tak, możemy d prawdopodobnie chcesz dostosować formatowanie lub tekst wyświetlany w komórkach.

Ta technika wstrzykiwania sortowania granicami grupy wymaga nieco więcej pracy niż opisane powyżej, jak należy również podać program obsługi zdarzeń dla GridView s `Sorting` zdarzeń i zachować prowadnicy `SortExpression` i `SortDirection` wartości.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulowanie GridView s kontrolować zbieranie po nim s została powiązana z danymi

Zamiast wiadomości dane przed wiążące go do widoku GridView, możemy dodać wiersze separator *po* danych została powiązana z kontrolki GridView. Proces wiązania danych są gromadzone hierarchii kontrolki GridView s, który w rzeczywistości jest po prostu `Table` wystąpienia składa się z kolekcji wierszy, każdy z nich składa się z kolekcji komórek. W szczególności zawiera kolekcję kontrolki GridView s `Table` obiektu w jego katalogu głównego, `GridViewRow` (który jest tworzony na podstawie `TableRow` klasy) dla każdego rekordu w `DataSource` powiązany z GridView i `TableCell` obiektu w każdym `GridViewRow` wystąpienia dla każdego pola danych w `DataSource`.

Aby dodać separator wierszy między poszczególnymi grupami sortowania, firma Microsoft bezpośrednio manipulować tej hierarchii kontroli po jego utworzeniu. Firma Microsoft mieć pewność, że utworzono hierarchii kontroli s GridView po raz ostatni przez razem, gdy strona jest renderowany. W związku z tym, ta metoda zastępuje `Page` klasy s `Render` metody, w tym momencie hierarchii kontroli końcowego s GridView została zaktualizowana do wymaganych separatora wierszy. Rysunek 4 przedstawia ten proces.


[![Technikę alternatywne manipuluje hierarchii kontroli s GridView](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Rysunek 4**: technikę alternatywne manipuluje s GridView hierarchii kontroli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


W tym samouczku użyjemy to drugie podejście do dostosowywania interfejsu użytkownika sortowania.

> [!NOTE]
> Kod I m prezentować w ramach tego samouczka jest oparta na przykład podany w [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) wpis w blogu s [odtwarzanie bitowej przy użyciu GridView sortowania grupowania](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Krok 3: Dodawanie wierszy Separator do hierarchii kontroli s GridView

Ponieważ chcemy dodać separator wierszy do hierarchii kontroli s GridView, po utworzeniu hierarchii kontroli i utworzony po raz ostatni w tym odwiedź stronę, chcemy wykonać to dodawanie na koniec cyklu życia strony, ale przed rzeczywiste c GridView Hierarchia ontrola zostało wyrenderowane w kodzie HTML. Najnowszy punkt możliwe, w którym firma Microsoft można to zrobić jest `Page` klasy s `Render` zdarzenie, które firma Microsoft może zastąpić w naszych osobna klasa kodu za pomocą następujący podpis metody:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Gdy `Page` klasy s, oryginalnym `Render` wywoływana jest metoda `base.Render(writer)` każdej kontrolki na stronie będzie renderowana, generowanie kodu znaczników na podstawie ich hierarchii kontroli. W związku z tym należy bezwzględnie o obu rozmowę `base.Render(writer)`, dzięki czemu wyrenderowaniu strony i możemy manipulować nimi GridView s kontrolować hierarchii przed wywołaniem `base.Render(writer)`, dzięki czemu separator wierszy zostały dodane do hierarchii kontroli s GridView przed nią s został renderowane.

Można wstrzyknąć nagłówków grup sortowania najpierw należy upewnić się, że użytkownik zażądał, czy dane można sortować. Domyślnie zawartość widoku GridView s nie są sortowane i w związku z tym firma Microsoft don t musisz wprowadzić dowolną grupę sortowanie nagłówków.

> [!NOTE]
> GridView ma zostać posortowana według określonej kolumny, po pierwszym załadowaniu strony należy wywołać metodę GridView `Sort` metoda pierwszej wizyty strony (ale nie kolejnych ogłaszania zwrotnego). Aby to osiągnąć, należy dodać to wywołanie w `Page_Load` programu obsługi zdarzeń w ramach `if (!Page.IsPostBack)` warunkowe. Odwołaj się do [stronicowanie i sortowanie danych raportu](paging-and-sorting-report-data-cs.md) informacji samouczka, aby uzyskać więcej informacji na temat `Sort` metody.


Przy założeniu, że dane zostały posortowane, naszym kolejnym krokiem jest określenie kolumny danych posortowano według, a następnie skanowania wierszy szukasz różnice w tej kolumnie s wartości. Poniższy kod gwarantuje, że dane została posortowana i umożliwia znalezienie kolumny za pomocą którego została posortowana danych:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Jeśli ma widoku GridView jeszcze być sortowane, GridView s `SortExpression` właściwość będzie nie ustawiono. W związku z tym tylko chcemy dodać separator wierszy, jeśli ta właściwość ma kilka wartości. Jeśli tak jest, następnie należy określić indeks kolumny za pomocą którego została posortowana danych. Jest to osiągane przez pętli GridView s `Columns` kolekcji, wyszukując kolumny, których `SortExpression` właściwości jest równa GridView s `SortExpression` właściwości. Oprócz indeksu kolumny s, możemy również Pobierz `HeaderText` właściwość, która jest używana podczas wyświetlania separatora wierszy.

Indeks kolumny, według której dane są sortowane ostatnim krokiem jest wyliczanie wierszy GridView. Dla każdego wiersza należy ustalić, czy wartość s kolumna posortowanego różni się od poprzedniej wartości s kolumny s posortowanych wierszy. Jeśli tak więc należy wstawić nową `GridViewRow` wystąpienia w hierarchii kontroli. Jest to realizowane przy użyciu następującego kodu:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Ten kod uruchamia programowo odwołując `Table` obiektu znalezione w folderze głównym hierarchii kontroli s GridView i utworzenie zmiennej ciągu o nazwie `lastValue`. `lastValue` Służy do porównywania wartości bieżącego wiersza s sortowane kolumny z poprzedniej wartości wiersza s. Następnie, GridView s `Rows` kolekcji są wyliczane dla każdego wiersza wartości kolumna posortowanego jest ona przechowywana w `currentValue` zmiennej.

> [!NOTE]
> Do określenia wartości kolumny określonego wiersza s sortowane używam komórki s `Text` właściwości. To działa dobrze w przypadku BoundFields, ale będzie nie działa zgodnie z potrzebami, w przypadku kontrolek TemplateField, CheckBoxFields i tak dalej. Omówimy sposób konta alternatywne pól GridView wkrótce.


`currentValue` i `lastValue` zmienne są porównywane. Jeśli będą się różnić musimy dodać nowy wiersz separator do hierarchii kontroli. Jest to realizowane poprzez określenie indeks `GridViewRow` w `Table` obiektu s `Rows` kolekcji, tworzenie nowych `GridViewRow` i `TableCell` wystąpień, a następnie dodając `TableCell` i `GridViewRow` do hierarchii kontroli.

Należy pamiętać, że separator wierszy s pojedynczy `TableCell` jest sformatowany w taki sposób, że obejmuje całą szerokość GridView, jest formatowana przy użyciu `SortHeaderRowStyle` klasy CSS i jego `Text` takie pokazuje grupy sortowania nazwa właściwości (na przykład kategoria) i Wartość s grupy (na przykład Beverages). Na koniec `lastValue` został zaktualizowany do wartości `currentValue`.

Klasy CSS, używany do formatowania sortowania wiersz nagłówka grupy `SortHeaderRowStyle` musi być określona w `Styles.css` pliku. Możesz skorzystać, niezależnie od ustawienia stylu odwołania się do Ciebie; Czy mogę używane następujące czynności:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Przy użyciu bieżącego kodu interfejsu sortowania dodaje nagłówków grup sortowania, gdy sortowanie według dowolnego elementu BoundField (zobacz rysunek 5, która zawiera zrzut ekranu podczas sortowania przez dostawcę). Jednak podczas sortowania przez inny typ pola (na przykład CheckBoxField lub TemplateField) sortowania grupy nagłówki są nigdzie nie można odnaleźć (patrz rysunek 6).


[![Interfejs sortowania zawiera nagłówki Sortuj i Grupuj, gdy sortowanie według BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Rysunek 5**: sortowanie interfejsu obejmuje sortowania grupy nagłówków podczas sortowania według BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Nagłówki grupy sortowania są, Brak podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Rysunek 6**: nagłówków grup sortowania są Brak podczas sortowania CheckBoxField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Przyczyną nagłówków grup sortowania występują, gdy sortowanie według CheckBoxField jest kod aktualnie używa tylko `TableCell` s `Text` właściwości w celu określenia wartości posortowanej kolumny dla każdego wiersza. Dla CheckBoxFields `TableCell` s `Text` właściwości jest ciągiem pustym; zamiast tego wartość jest dostępna za pośrednictwem formant pola wyboru w sieci Web, który znajduje się w obrębie `TableCell` s `Controls` kolekcji.

Do obsługi pola typów innych niż BoundFields, należy rozszerzyć kod gdzie `currentValue` zmienna jest przypisywana do sprawdzenia istnienia pola wyboru w `TableCell` s `Controls` kolekcji. Zamiast używania `currentValue = gvr.Cells[sortColumnIndex].Text`, Zastąp kod następującym kodem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Ten kod sprawdza, czy kolumna posortowanego `TableCell` dla bieżącego wiersza w celu określenia, czy formanty w `Controls` kolekcji. Jeśli istnieją, a pierwszy formant jest to pole wyboru, `currentValue` zmienna jest ustawiona tak lub nie, w zależności od s wyboru `Checked` właściwości. W przeciwnym razie wartość jest pobierana z `TableCell` s `Text` właściwości. Tę logikę można replikować do obsługi sortowania dla wszelkich kontrolek TemplateField, który może znajdować się w widoku GridView.

Dodając kod powyżej, nagłówków grup sortowania są teraz, gdy sortowanie według CheckBoxField zakończona (zobacz rysunek 7).


[![Nagłówki grupy sortowania są teraz obecny podczas sortowania CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Rysunek 7**: nagłówków grup sortowania są teraz obecny podczas sortowania CheckBoxField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> W przypadku produktów dzięki `NULL` bazy danych wartości `CategoryID`, `SupplierID`, lub `UnitPrice` pól, te wartości będą wyświetlane jako puste ciągi w widoku GridView domyślnie, co oznacza separator tekst wiersza s dla tych produktów z `NULL`odczyta wartości takich jak kategoria: (czyli tam s Brak nazwy po kategorii: za pomocą kategorii, takich jak: Beverages). Jeśli chcesz użyć wartości wyświetlane w tym miejscu można ustawić BoundFields [ `NullDisplayText` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) tekstu mają być wyświetlane lub Dodaj instrukcję warunkową w metodzie renderowania podczas przypisywania `currentValue` do separatora Wiersz s `Text` właściwości.


## <a name="summary"></a>Podsumowanie

Kontrolki GridView nie zawiera wiele wbudowanych opcji dostosowywania interfejsu sortowania. Jednak w przypadku znacznej liczby niskiego poziomu kodu go s umożliwia dostosowanie hierarchię GridView s kontroli w celu utworzenia bardziej dostosowanego interfejsu. Jak dodać wiersz separatora grupy sortowania dla GridView można sortować, łatwiej identyfikuje różne grupy i tych grup granic widzieliśmy w ramach tego samouczka. Aby uzyskać więcej przykładów niestandardowych interfejsów sortowania, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [kilka ASP.NET 2.0 GridView sortowanie porady i wskazówki](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) wpis w blogu.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](sorting-custom-paged-data-cs.md)
> [dalej](paging-and-sorting-report-data-vb.md)
