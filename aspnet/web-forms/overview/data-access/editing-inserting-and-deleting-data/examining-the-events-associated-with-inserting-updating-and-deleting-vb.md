---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Badanie zdarzeń skojarzonych z wstawiania, aktualizowania i usuwania (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zajmiemy się przy użyciu zdarzeń występujących przed, podczas i po wstawiania aktualizowania lub usuwania operacji danych ASP.NET formantu sieci Web. W...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 548bfecc4215fbb2b36e0e2be42c7c08ee884270
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817165"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Badanie zdarzeń powiązanych ze wstawianiem, aktualizowania i usuwania (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) lub [Pobierz plik PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> W tym samouczku zajmiemy się przy użyciu zdarzeń występujących przed, podczas i po wstawiania aktualizowania lub usuwania operacji danych ASP.NET formantu sieci Web. Również zobaczymy Dostosowywanie interfejsu edycji można aktualizować tylko podzestaw pól produktu.


## <a name="introduction"></a>Wprowadzenie

Korzystając z wbudowanych Wstawianie, edytowanie lub usuwanie funkcji kontrolki GridView, DetailsView lub FormView, szereg kroków orzeczona, gdy użytkownik kończy proces dodawania nowego rekordu, aktualizowanie lub usuwanie istniejącego rekordu. Tak jak Omówiliśmy to w [poprzedniego samouczka](an-overview-of-inserting-updating-and-deleting-data-vb.md), podczas edytowania wiersza w widoku GridView przycisk Edytuj zastępuje Włącz BoundFields do pola tekstowe i przyciski aktualizacji i Anuluj. Po użytkownik końcowy aktualizuje dane i kliknie przycisk aktualizacji, poniższe kroki są wykonywane na odświeżenie strony:

1. Kontrolki GridView wypełnia jego ObjectDataSource `UpdateParameters` z unikatowe pola identyfikacyjne edytowany rekord (za pośrednictwem `DataKeyNames` właściwości) wraz z wartościami podanymi przez użytkownika
2. GridView wywołuje jego ObjectDataSource `Update()` metody, która z kolei wywołuje odpowiednią metodę obiektu źródłowego (`ProductsDAL.UpdateProduct`, w naszym poprzedni Samouczek)
3. Dane podstawowe, który teraz zawiera zaktualizowane zmiany, jest odbitych do widoku GridView

Podczas tej sekwencji kroki, liczbę zdarzeń fire, dzięki czemu możemy utworzyć procedury obsługi zdarzeń, aby dodać logikę niestandardową w razie potrzeby. Na przykład przed kroku 1, widoku GridView `RowUpdating` generowane zdarzenie. Firma Microsoft w tym momencie anulować żądanie aktualizacji w przypadku niektórych błąd sprawdzania poprawności. Gdy `Update()` metoda jest wywoływana, ObjectDataSource `Updating` generowane zdarzenie, zapewniając możliwość dodawania lub dostosowywania wartości któregokolwiek z `UpdateParameters`. Po podstawowej kontrolki ObjectDataSource metody obiektu została ukończona, wykonywanie ObjectDataSource `Updated` zdarzenie jest wywoływane. Program obsługi zdarzeń dla `Updated` zdarzenia można sprawdzić szczegółowe informacje o operacji aktualizacji, takich jak wpłynęłoby to na liczbę wierszy i określa, czy wystąpił wyjątek. Na koniec, po kroku 2, widoku GridView `RowUpdated` generowane zdarzenie; program obsługi zdarzeń dla tego zdarzenia może sprawdzić dodatkowe informacje na temat operacji aktualizacji, po prostu wykonać.

Rysunek 1 przedstawia szereg zdarzeń i kroki podczas aktualizowania GridView. Wzorzec zdarzeń na rysunku 1 nie jest unikatowa na aktualizację przy użyciu GridView. Wstawianie, aktualizowanie lub usuwanie danych z widoku GridView, DetailsView lub FormView wytrąca taką samą sekwencję przed i po poziomu zdarzeń dla formantu sieci Web danych i kontrolki ObjectDataSource.


[![Szeregu wstępnej i Fire po zdarzenia podczas aktualizowania danych w widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Rysunek 1**: serii z przed i po zdarzenia Fire podczas aktualizowania danych w GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


W tym samouczku zostaną omówione używania tych zdarzeń, aby rozszerzyć wbudowane Wstawianie aktualizowanie i usuwanie możliwości danych w programie ASP.NET sieci Web kontrolki. Również zobaczymy Dostosowywanie interfejsu edycji można aktualizować tylko podzestaw pól produktu.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Krok 1: Aktualizowanie produktu`ProductName`i`UnitPrice`pola

W polu edycji interfejsy z poprzedniego samouczka *wszystkich* pola produktów, które nie były tylko do odczytu musiały zostać uwzględnione. Załóżmy, gdybyśmy wybrali, aby usunąć pole z GridView — `QuantityPerUnit` — w przypadku aktualizowania danych kontrolki sieci Web danych nie będzie ustawienie ObjectDataSource `QuantityPerUnit` `UpdateParameters` wartości. Następnie wprowadzić wartość kontrolki ObjectDataSource `Nothing` do `UpdateProduct` warstwy logiki biznesowej (LOGIKI) metody, która zmieniłby rekordu edytowanych bazy danych `QuantityPerUnit` kolumny `NULL` wartość. Podobnie, jeśli to pole jest wymagane, takie jak `ProductName`, zostanie usunięty z interfejsu edycji aktualizacji zakończy się niepowodzeniem z "*kolumny"ProductName"dopuszcza wartości null*" wyjątku. Został przyczyna to zachowanie, ponieważ kontrolki ObjectDataSource został skonfigurowany do wywołania `ProductsBLL` klasy `UpdateProduct` metody, która oczekuje parametru wejściowego dla każdego z pól produktu. W związku z tym, ObjectDataSource `UpdateParameters` zbioru zawarte parametr, dla każdej metody dane wejściowe podane przez parametry.

Jeśli chcemy, aby dostarczać dane kontrolka sieci Web, która pozwala użytkownikowi aktualizować tylko podzestaw pól, a następnie musimy albo programowo brakujący `UpdateParameters` wartości ObjectDataSource `Updating` program obsługi zdarzeń lub Utwórz i wywołania metody LOGIKI oczekuje tylko podzbiór pól. Przyjrzyjmy się tym drugie podejście.

W szczególności Utwórzmy strona, która wyświetla tylko `ProductName` i `UnitPrice` pól w edycji kontrolki GridView. Interfejs edytowania tego widoku GridView tylko umożliwi użytkownikowi na aktualizowanie dwa pola wyświetlane, `ProductName` i `UnitPrice`. Ponieważ ten interfejs edycji zapewnia tylko podzestaw pól produktu, albo musimy utworzyć elementu ObjectDataSource, który używa istniejącej LOGIKI `UpdateProduct` metody i brakujące wartości pól produktu ustawił programowo w jego `Updating` zdarzeń Program obsługi lub firma Microsoft, należy utworzyć nową metodę LOGIKI, która oczekuje tylko podzbiór pól zdefiniowanych w widoku GridView. W tym samouczku teraz tę druga opcję i utworzyć przeciążenia `UpdateProduct` metody, która przyjmuje tylko trzy parametry wejściowe: `productName`, `unitPrice`, i `productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Takich jak oryginalne `UpdateProduct` tego przeciążenia rozpoczyna się poprzez sprawdzenie, czy produkt w bazie danych o określonej metody `ProductID`. Jeśli nie, zwraca `False`, co oznacza, że żądanie, aby zaktualizować informacje o produkcie nie powiodło się. W przeciwnym razie aktualizuje istniejący rekord produktu `ProductName` i `UnitPrice` odpowiednio pola i zatwierdzeń aktualizacji przez wywołanie metody TableAdpater `Update()` metody, przekazując `ProductsRow` wystąpienia.

Z tego uzupełnienia naszych `ProductsBLL` klasy, możemy przystąpić do tworzenia uproszczony interfejs GridView. Otwórz `DataModificationEvents.aspx` w `EditInsertDelete` folder i na stronie Dodaj GridView. Tworzenie nowego elementu ObjectDataSource i skonfigurować go do używania `ProductsBLL` klasy z jej `Select()` mapowanie metody `GetProducts` i jego `Update()` mapowanie metody `UpdateProduct` przeciążenia, które przyjmuje tylko `productName`, `unitPrice`, i `productID` parametrów wejściowych. Na rysunku 2 przedstawiono Kreatora tworzenia źródła danych, podczas mapowania ObjectDataSource `Update()` metody `ProductsBLL` klasy przez nowe `UpdateProduct` przeciążenie metody.


[![Map, Metoda Update() ObjectDataSource nowe przeciążenia UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Rysunek 2**: mapowanie ObjectDataSource `Update()` metodę, aby nowe `UpdateProduct` przeciążenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


Ponieważ w naszym przykładzie będzie początkowo, wystarczy możliwość do edycji danych, ale nie wstawiania lub usuwania rekordów, Poświęć chwilę, aby jawnie wskazać, że ObjectDataSource `Insert()` i `Delete()` metody nie powinny być zmapowany do `ProductsBLL` metody tej klasy, przechodząc do karty INSERT i DELETE i wybierając (Brak) z listy rozwijanej.


[![(Brak) wybierz z listy rozwijanej, WSTAWIANIA i usuwania karty](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Rysunek 3**: Wybierz (Brak) z listy rozwijanej dla Wstawianie i usuwanie kart ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


Po ukończeniu pracy tego kreatora sprawdź pole wyboru Włącz edytowanie, z GridView tagu inteligentnego.

Po ukończeniu Kreatora tworzenia źródła danych i powiązanie do kontrolki GridView programu Visual Studio został utworzony składni deklaratywnej dla obu kontrolek. Przejdź do widoku źródła, aby sprawdzić ObjectDataSource oznaczeniu deklaracyjnym, które znajdują się poniżej:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Ponieważ brak mapowań dla ObjectDataSource `Insert()` i `Delete()` metod, istnieją nie `InsertParameters` lub `DeleteParameters` sekcje. Ponadto, ponieważ `Update()` metody jest mapowany na `UpdateProduct` przeciążenia metody, które akceptuje tylko trzy parametry wejściowe, `UpdateParameters` sekcja zawiera tylko trzech `Parameter` wystąpień.

Należy pamiętać, że ObjectDataSource `OldValuesParameterFormatString` właściwość jest ustawiona na `original_{0}`. Ta właściwość jest ustawiana automatycznie przez program Visual Studio, korzystając z kreatora Konfigurowanie źródła danych. Jednak ponieważ naszych metody LOGIKI oczekują, że oryginalna `ProductID` wartość do przekazania w, całkowicie usunąć to przypisanie właściwości, w składni deklaratywnej ObjectDataSource.

> [!NOTE]
> Jeśli po prostu wyczyszczenie `OldValuesParameterFormatString` wartości właściwości z okna właściwości w widoku Projekt, a właściwość będą nadal istnieć w składni deklaratywnej, ale zostanie ustawiony na pusty ciąg. Albo usuń właściwość całkowicie ze składni deklaratywnej, lub z poziomu okna właściwości ustaw wartość domyślną `{0}`.


Gdy ma tylko kontrolki ObjectDataSource `UpdateParameters` na nazwę, cenę i identyfikator produktu Visual Studio został wyposażony w elementu BoundField lub CheckBoxField w widoku GridView dla każdego z pól w produkcie.


[![Kontrolki GridView zawiera elementu BoundField lub CheckBoxField dla każdego z pól w produkcie](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Rysunek 4**: widoku GridView zawiera elementu BoundField lub CheckBoxField dla każdego z pól w produkcie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


Gdy użytkownik końcowy edytuje produktu i kliknie przycisk jej aktualizacji, widoku GridView wylicza tych pól, które nie były tylko do odczytu. Następnie ustawia wartość w odpowiadającym jej parametrze w ObjectDataSource `UpdateParameters` kolekcji wartości wprowadzonej przez użytkownika. Jeśli nie ma odpowiadającego mu parametru, widoku GridView dodaje je do kolekcji. W związku z tym, jeśli nasz GridView zawiera BoundFields i CheckBoxFields dla wszystkich pól w produkcie, kontrolki ObjectDataSource prowadzi to do umożliwienia wywoływania `UpdateProduct` przeciążenia przyjmującego we wszystkich tych parametrów, mimo że ObjectDataSource oznaczeniu deklaracyjnym określa tylko dla trzech parametrów wejściowych (zobacz rysunek 5). Podobnie w przypadku niektórych kombinacji innych — tylko do odczytu pola produktu w kontrolce GridView, który nie jest zgodny z parametrami wejściowymi dla `UpdateProduct` przeciążenia, zostanie wygenerowany wyjątek podczas próby zaktualizowania.


[![Będzie GridView Dodaj parametry do elementu ObjectDataSource UpdateParameters kolekcji](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Rysunek 5**: GridView będzie Dodawanie parametrów do ObjectDataSource `UpdateParameters` kolekcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


Aby upewnić się, że kontrolki ObjectDataSource wywołuje `UpdateProduct` przeciążenia, które przyjmuje tylko w produkcie nazwę, cenę i identyfikator, należy ograniczyć widoku GridView do konieczności edytowalnego pola po prostu z `ProductName` i `UnitPrice`. Można to osiągnąć, usuwając inne BoundFields i CheckBoxFields, ustawiając tych innych pól `ReadOnly` właściwości `True`, lub kombinacji obu. W tym samouczku możemy po prostu usunąć wszystkie pola GridView, z wyjątkiem `ProductName` i `UnitPrice` BoundFields, po którym GridView oznaczeniu deklaracyjnym będzie wyglądał jak:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Mimo że `UpdateProduct` przeciążenia oczekuje trzech parametrów wejściowych, że mamy tylko dwa BoundFields w naszym GridView. Jest to spowodowane `productID` parametr wejściowy jest wartość klucza podstawowego i przekazywana wartość `DataKeyNames` właściwości edytowanego wiersza.

Nasze GridView wraz z `UpdateProduct` przeciążenia i pozwala użytkownikowi edytować tylko nazwa i cena produktu bez utraty innych pól produktu.


[![Interfejs umożliwia edytowanie po prostu produktu nazwy i ceny](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Rysunek 6**: zezwala interfejsu edycji wystarczy produktu nazwa i cena ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> Zgodnie z opisem w poprzednim samouczku, jest wszechobecne GridView można s widok stanu włączenia (zachowanie domyślne). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko, że liczba równoczesnych użytkowników przypadkowo usuwanie i edytowanie rekordów. Zobacz [Ostrzeżenie: problem współbieżności przy użyciu platformy ASP.NET 2.0 GridViews/DetailsView/FormViews tej edycji pomocy technicznej i/lub usuwanie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.


## <a name="improving-theunitpriceformatting"></a>Poprawa`UnitPrice`formatowania

Podczas gdy GridView przykładu w działa rysunek 6 `UnitPrice` pole nie jest sformatowany w ogóle, wynikiem wyświetlania ceny, który nie ma żadnych waluty symboli i ma czterech miejsc dziesiętnych. Aby zastosować formatowanie nie można edytować wierszy waluty, po prostu ustaw `UnitPrice` firmy elementu BoundField `DataFormatString` właściwości `{0:c}` i jego `HtmlEncode` właściwość `False`.


[![Odpowiednio ustawić właściwości HtmlEncode i DataFormatString zastosowanym UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Rysunek 7**: Ustaw `UnitPrice`firmy `DataFormatString` i `HtmlEncode` odpowiednio do właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


Dzięki tej zmianie nie można edytować wierszy w formacie cena waluty; edytowany wierszu, jednak nadal wyświetlana jest wartość bez symbolu waluty i czterech miejsc dziesiętnych.


[![Nie można edytować wierszy są teraz formatowane jako wartości waluty](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Rysunek 8**: nie można edytować wierszy są teraz formatowane jako wartości waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


Instrukcje formatowania, określone w `DataFormatString` właściwość może zostać zastosowana do interfejsu edycji przez ustawienie elementu BoundField `ApplyFormatInEditMode` właściwości `True` (wartość domyślna to `False`). Poświęć chwilę, aby ustawić tę właściwość na `True`.


[![Ustaw właściwość ApplyFormatInEditMode elementu UnitPrice BoundField na wartość True](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Rysunek 9**: Ustaw `UnitPrice` firmy elementu BoundField `ApplyFormatInEditMode` właściwości `True` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


Dzięki tej zmianie wartości `UnitPrice` wyświetlane w edytowanych wiersz jest również w formacie waluty.


[![Wartość UnitPrice wiersz edytowana jest teraz formatowane jako walutę](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Na rysunku nr 10**: edytować wiersza `UnitPrice` wartość jest teraz w formacie waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


Jednak aktualizowanie produktu z symbolem waluty w polu tekstowym, takich jak $19,00 zgłasza `FormatException`. Próba przypisania wartości dostarczone przez użytkownika do ObjectDataSource widoku GridView `UpdateParameters` kolekcji nie może przekonwertować `UnitPrice` ciągu "$19,00" do `Decimal` wymagane przez parametr (zobacz rysunek 11). Aby rozwiązać ten problem możesz stworzyć program obsługi zdarzeń dla GridView `RowUpdating` zdarzeń i przeanalizować podanego użytkownika `UnitPrice` jak sformatowane waluty `Decimal`.

W widoku GridView `RowUpdating` zdarzeń akceptuje jako drugi parametr obiektu typu [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), w tym `NewValues` słownika jako jeden z jego właściwości, które zawiera gotowe do przeniesienia poza wartości dostarczone przez użytkownika przypisane do ObjectDataSource `UpdateParameters` kolekcji. Firma Microsoft może spowodować zastąpienie istniejących `UnitPrice` wartość w `NewValues` kolekcji o wartości dziesiętnej analizowany, w formacie waluty z następującymi wierszami kodu w `RowUpdating` programu obsługi zdarzeń:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Jeśli użytkownik udostępnił `UnitPrice` wartości (na przykład "$19,00"), ta wartość zostanie zastąpiona dziesiętna wartość obliczona przez [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), podczas analizowania wartości jako walutę. To będzie poprawnie przeanalizować separatora dziesiętnego w przypadku symbole walut, przecinki, separatorów dziesiętnych i tak dalej i używa [wyliczenia NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) w [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) przestrzeni nazw.

Na ilustracji 11 pokazano problem spowodowany przez symbole waluty w użytkownik podał `UnitPrice`, oraz jak GridView `RowUpdating` program obsługi zdarzeń może być wykorzystywany można poprawnie przeanalizować takich danych wejściowych.


[![Wartość UnitPrice wiersz edytowana jest teraz formatowane jako walutę](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Rysunek 11**: edytować wiersza `UnitPrice` wartość jest teraz w formacie waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Krok 2: zabronienia`NULL UnitPrices`

Gdy baza danych jest skonfigurowany do zezwalania `NULL` wartości w `Products` tabeli `UnitPrice` kolumny, firma Microsoft może chcesz uniemożliwić użytkownikom odwiedzenie tej konkretnej strony z określania `NULL` `UnitPrice` wartości. Oznacza to jeśli użytkownik nie powiedzie się wprowadzić `UnitPrice` wartości podczas edytowania wierszy produktu, a nie zapisać wyniki w bazie danych, które chcemy, aby wyświetlić komunikat, którego użytkownik dowie się, że za pomocą tej strony żadnego edytowanych musi cena jest określony.

`GridViewUpdateEventArgs` Obiekt przekazany do GridView `RowUpdating` zawiera program obsługi zdarzeń `Cancel` właściwość, jeśli ustawiono `True`, kończy proces uaktualniania. Możemy rozszerzyć `RowUpdating` programu obsługi zdarzeń, aby ustawić `e.Cancel` do `True` i wyświetli komunikat wyjaśniający dlaczego, jeśli `UnitPrice` wartość w `NewValues` kolekcji ma wartość `Nothing`.

Rozpocznij, dodając kontrolkę etykiety w sieci Web do strony o nazwie `MustProvideUnitPriceMessage`. Ten formant etykiety będą wyświetlane, jeśli użytkownik nie może określić `UnitPrice` wartości podczas aktualizacji produktu. Ustaw właściwość etykiety `Text` właściwość "Musisz podać cenę produktu." Utworzono również klasę CSS z `Styles.css` o nazwie `Warning` przy użyciu następujących definicji:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Wreszcie, ustaw właściwość etykiety `CssClass` właściwość `Warning`. W tym momencie projektanta powinien być wyświetlony komunikat ostrzegawczy w czerwonym, pogrubienie, kursywa, bardzo duży rozmiar czcionki powyżej GridView, jak pokazano na rysunku 12.


[![Etykieta została dodana powyżej widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Rysunek 12**: Etykieta ma został dodany powyżej GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


Domyślnie ta etykieta powinna być ukryta, więc ustawić jej `Visible` właściwości `False` w `Page_Load` programu obsługi zdarzeń:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Jeśli użytkownik próbuje zaktualizować produkt bez określania `UnitPrice`, chcemy anulować aktualizację i wyświetlić etykiety ostrzeżenie. Rozszerzaj GridView `RowUpdating` program obsługi zdarzeń w następujący sposób:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Jeśli użytkownik próbuje zapisać podłogowej bez określania cen, aktualizacja zostanie anulowana i zostanie wyświetlony komunikat pomocne. Podczas gdy bazy danych (i logikę biznesową) umożliwia `NULL` `UnitPrice` s, tej konkretnej strony ASP.NET nie jest.


[![Użytkownik nie może opuścić UnitPrice puste](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Rysunek 13**: użytkownik nie może opuścić `UnitPrice` puste ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


Do tej pory Zaobserwowaliśmy sposób używania GridView `RowUpdating` zdarzeń programowo zmienić wartości parametru, przypisane do ObjectDataSource `UpdateParameters` kolekcji, a także jak anulować aktualizację przetwarzania całkowicie. Te pojęcia przeniosą się do formantów DetailsView i FormView i mają zastosowanie również do wstawiania i usuwania.

Można również wykonać te zadania na poziomie elementu ObjectDataSource za pośrednictwem programów obsługi zdarzeń dla jego `Inserting`, `Updating`, i `Deleting` zdarzenia. Te zdarzenia fire przed wywołaniem metody skojarzony obiekt źródłowy i zapewniają ostatniej szansy możliwość modyfikowania kolekcji parametrów wejściowych lub anulowania ostatecznego operacji. Programy obsługi zdarzeń dla tych trzech zdarzeń są przekazywane na obiekt typu [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , ma dwie właściwości zainteresowania:

- [Anuluj](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), który, jeśli wartość `True`, anuluje wykonywanej operacji
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), który jest kolekcją `InsertParameters`, `UpdateParameters`, lub `DeleteParameters`, w zależności od tego, czy program obsługi zdarzeń jest dla `Inserting`, `Updating`, lub `Deleting` zdarzeń

Aby zilustrować pracy przy użyciu wartości parametrów na poziomie elementu ObjectDataSource, możemy objąć DetailsView naszą stronę, która umożliwia użytkownikom dodawanie nowego produktu. Ta DetailsView będzie służyć do zapewniają interfejs do szybkiego dodawania nowych produktów w bazie danych. Aby zachować spójny interfejs użytkownika, gdy umożliwia dodanie nowego produktu umożliwia użytkownikowi tylko wprowadź wartości w polach `ProductName` i `UnitPrice` pola. Domyślnie te wartości, które nie są dostarczane w interfejsie Wstawianie DetailsView zostanie ustawiona do `NULL` bazy danych wartości. Jednak można użyć ObjectDataSource `Inserting` zdarzenie, aby wstawić różne domyślne wartości, ponieważ zajmiemy się wkrótce.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Krok 3: Dostarczanie interfejsu, aby dodać nowe produkty

Przeciągnij element DetailsView z przybornika w Projektancie powyżej GridView, wyczyścić jej `Height` i `Width` właściwości i powiązania go do kontrolki ObjectDataSource już istnieje na tej stronie. Spowoduje to dodanie elementu BoundField lub CheckBoxField dla każdego z pól w produkcie. Ponieważ chcemy użyć tego DetailsView, aby dodać nowe produkty, musimy sprawdzić opcję Włącz wstawianie tagu inteligentnego; jednak istnieje taka możliwość, ponieważ ObjectDataSource `Insert()` metoda nie została zamapowana do metody w `ProductsBLL` klasy (wycofaniu to mapowanie (Brak) możemy ustawić podczas konfigurowania źródła danych zobacz rysunek 3).

Aby skonfigurować kontrolki ObjectDataSource, wybierz łącze Konfiguruj źródła danych z tagu inteligentnego, uruchomić kreatora. Pierwszy ekran umożliwia zmianę podstawowego obiektu, który jest powiązany kontrolki ObjectDataSource; Pozostaw ona ustawiona na `ProductsBLL`. Następny ekran Wyświetla listę mapowań od metod ObjectDataSource do bazowego obiektu. Mimo że firma Microsoft wyraźnie wskazane, że `Insert()` i `Delete()` metody nie powinny być mapowane do żadnych metod, po przejściu do WSTAWIANIA i usuwania karty zobaczysz, że mapowanie jest. Jest to spowodowane `ProductsBLL`firmy `AddProduct` i `DeleteProduct` metody za pomocą `DataObjectMethodAttribute` atrybutu, aby wskazać, że są one domyślne metody `Insert()` i `Delete()`, odpowiednio. W związku z tym Kreator ObjectDataSource wybiera te każdym razem, gdy należy uruchomić kreatora, chyba że jest jawnie określona wartość.

Pozostaw `Insert()` metoda wskazujący `AddProduct` metody, ale ponownie ustawić Usuń kartę listy rozwijanej (Brak).


[![Ustaw karty Wstawianie listy rozwijanej metodę AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Rysunek 14**: wartość listy rozwijanej kartę wstawianie `AddProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![Zmień wartość na liście rozwijanej Usuń kartę na (Brak)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Rysunek 15**: Zmień wartość na liście rozwijanej kartę Usuń (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


Po wprowadzeniu tych zmian, składni deklaratywnej ObjectDataSource zostanie rozwinięta i obejmuje `InsertParameters` kolekcji, jak pokazano poniżej:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Ponowne uruchomienie kreatora ponownie dodane `OldValuesParameterFormatString` właściwości. Poświęć chwilę, aby wyczyścić tę właściwość, ustawiając dla niej wartość domyślną (`{0}`) lub wyjęty całkowicie składni deklaratywnej.

Za pomocą kontrolki ObjectDataSource, udostępniając funkcje Wstawianie tagu inteligentnego DetailsView będzie teraz obejmować pole wyboru Włącz wstawianie; Wróć do projektanta i zaznacz tę opcję. Następnie okrojenia DetailsView, tak aby ma tylko dwie BoundFields — `ProductName` i `UnitPrice` — i CommandField. W tym momencie powinna wyglądać składni deklaratywnej DetailsView:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Rysunek 16 pokazuje tej strony, podczas wyświetlania za pośrednictwem przeglądarki, w tym momencie. Jak widać, DetailsView Wyświetla nazwę i cena produktu pierwszy (Chai). Chcemy, jest jednak Wstawianie interfejs, który umożliwia użytkownikowi szybkie dodawanie nowego produktu w bazie danych.


[![DetailsView jest obecnie renderowane w trybie tylko do odczytu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Rysunek 16**: DetailsView jest obecnie renderowane w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


Aby pokazać DetailsView w trybie Wstawianie musimy `DefaultMode` właściwość `Inserting`. To powoduje wyświetlenie DetailsView w trybie wstawiania po raz pierwszy odwiedzony i przechowuje ją w tym miejscu po Wstawianie nowego rekordu. Jak pokazano na rysunku 17, takie DetailsView udostępnia szybki interfejs na potrzeby dodawania nowego rekordu.


[![DetailsView udostępnia interfejs do szybkiego dodawania nowego produktu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Rysunek 17**: DetailsView udostępnia interfejs do szybkiego dodawania nowego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


Gdy użytkownik wprowadza nazwę produktu i cenę (na przykład "Xyz Water" i 1,99, tak jak rysunek 17) i kliknie przycisk Wstaw, ensues odświeżenie strony i rozpoczęciem Wstawianie przepływu pracy, skutkując nowego rekordu produktu dodawane do bazy danych. DetailsView zachowuje jego Wstawianie interfejsu i widoku GridView jest automatycznie odbitych ze swoim źródłem danych Aby dołączyć nowy produkt, jak pokazano na rysunku 18.


![Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Rysunek 18**: produkt "Woda Acme" został dodany do bazy danych


Gdy GridView na rysunku 18 nie pokazuje, pola produktów, trzeba wyciągnąć z interfejsu DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`i tak dalej przypisanych `NULL` bazy danych wartości. Można to zobaczyć, wykonując następujące czynności:

1. Przejdź do Eksploratora serwera w programie Visual Studio
2. Rozwijanie `NORTHWND.MDF` węzeł bazy danych
3. Kliknij prawym przyciskiem myszy `Products` węzeł tabeli bazy danych
4. Wybierz opcję Pokaż dane tabeli

Spowoduje to wyświetlenie listy wszystkich rekordów w `Products` tabeli. Rysunek 19 pokazują, wszystkie kolumny, nasz nowy produkt innej niż `ProductID`, `ProductName`, i `UnitPrice` mają `NULL` wartości.


[![Produkt pola nie podano w DetailsView są przypisane wartości NULL](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Rysunek 19**: produkt pola nie podano w DetailsView przypisanych `NULL` wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


Warto podać inne niż wartość domyślną `NULL` dla jednego lub kilku z tych wartości kolumn, albo ponieważ `NULL` nie jest najlepszą opcją domyślną lub z powodu poziomu samej kolumny bazy danych nie zezwala na `NULL` s. W tym celu możemy programowo ustawić wartości parametrów DetailsView `InputParameters` kolekcji. To przypisanie może odbywać się albo w przypadku obsługi dla DetailsView `ItemInserting` zdarzenia lub ObjectDataSource `Inserting` zdarzeń. Ponieważ firma Microsoft została już oglądałem się na danych w sieci Web przy użyciu zdarzeń przed i po poziomu sterować poziomem, Przyjrzyjmy się przy użyciu zdarzeń ObjectDataSource, tym razem.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Krok 4: Przypisywanie wartości do`CategoryID`i`SupplierID`parametrów

W tym samouczku Załóżmy, że dla naszej aplikacji, podczas dodawania nowego produktu za pomocą tego interfejsu powinien zostać przypisany `CategoryID` i `SupplierID` wartość 1. Jak wspomniano wcześniej, kontrolki ObjectDataSource parą zdarzeń przed i po poziomu tego fire w procesie modyfikacji danych. Po jego `Insert()` metoda jest wywoływana, kontrolki ObjectDataSource najpierw wywołuje jego `Inserting` zdarzeń, następnie wywołuje metodę, jego `Insert()` metoda został zmapowany do, a na koniec zgłasza `Inserted` zdarzeń. `Inserting` Program obsługi zdarzeń, daje nam jeden ostatnią możliwość dostosowanie parametrów wejściowych lub anulowania ostatecznego operacji.

> [!NOTE]
> W rzeczywistych aplikacjach, prawdopodobnie chcesz Pozwól użytkownika określ kategorii i dostawcy lub czy wybierz tę wartość dla nich na podstawie pewnych kryteriów lub business logic (zamiast bezrefleksyjne wybranie Identyfikatora 1). Niezależnie od tego w przykładzie pokazano, jak programowo ustawić wartości parametru wejściowego od ObjectDataSource wstępnie poziomu zdarzenia.


Poświęć chwilę, aby utworzyć program obsługi zdarzeń dla ObjectDataSource `Inserting` zdarzeń. Zauważ, że program obsługi zdarzeń drugi parametr wejściowy obiekt typu `ObjectDataSourceMethodEventArgs`, która ma właściwość, aby uzyskać dostęp do kolekcji parametrów (`InputParameters`) i właściwość, aby anulować operację (`Cancel`).


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

W tym momencie `InputParameters` właściwość zawiera ObjectDataSource `InsertParameters` kolekcji przy użyciu wartości przypisane z DetailsView. Aby zmienić wartość jednego z tych parametrów, po prostu użyć: `e.InputParameters("paramName") = value`. W związku z tym aby ustawić `CategoryID` i `SupplierID` do wartości 1, Dostosuj `Inserting` procedurę obsługi zdarzeń do wyglądać podobnie do następującego:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Ten czas podczas dodawania nowego produktu (na przykład Acme Soda) `CategoryID` i `SupplierID` kolumny nowego produktu są ustawione na wartość 1 (zobacz rysunek 20).


[![Nowe produkty powstał ich CategoryID i identyfikator zestawu wartości 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Rysunek 20**: nowe produkty teraz mieć ich `CategoryID` i `SupplierID` wartości ustawione na wartość 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>Podsumowanie

Podczas edytowania, wstawianie i usuwanie procesów kontrolki sieci Web danych i kontrolki ObjectDataSource postępuj zgodnie z instrukcjami szereg zdarzeń przed i po poziomie. W tym samouczku będziemy zbadać zdarzenia wstępnie poziomu i pokazaliśmy, jak ich użyć do dostosowanie parametrów wejściowych, lub przycisk Anuluj, operacja modyfikacji danych całkowicie zarówno z poziomu kontroli danych w sieci Web i zdarzenia ObjectDataSource. W następnym samouczku Zapoznamy się tworzenia i używania programów obsługi zdarzeń dla zdarzenia po poziomu.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Jackie Goor i Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [dalej](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
