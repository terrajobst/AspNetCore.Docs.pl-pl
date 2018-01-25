---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Badanie zdarzenia skojarzonego z Wstawianie, aktualizowanie i usuwanie (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku zajmiemy się za pomocą zdarzeń, które występują przed, podczas i po wstawieniu aktualizowania lub usuwania operacji danych formantu sieci Web w programie ASP.NET. W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 93da23d58d1ba73c5b97f42631d036dd364de24d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Badanie ze zdarzeniami związanymi z Wstawianie, aktualizowanie i usuwanie (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) lub [pobierania plików PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> W tym samouczku zajmiemy się za pomocą zdarzeń, które występują przed, podczas i po wstawieniu aktualizowania lub usuwania operacji danych formantu sieci Web w programie ASP.NET. Firma Microsoft widoczne jest również sposób dostosowywania interfejsu edycji można aktualizować tylko podzbiór pola produktu.


## <a name="introduction"></a>Wprowadzenie

Korzystając z wbudowanego Wstawianie, edytowanie lub usuwanie funkcji kontrolki GridView, widoku DetailsView lub FormView, różnych kroków orzeczona, gdy użytkownik kończy proces dodawania nowego rekordu, aktualizowanie lub usuwanie istniejącego rekordu. Jak wspomniano w [poprzedniego samouczek](an-overview-of-inserting-updating-and-deleting-data-cs.md), podczas edycji wiersza w widoku GridView przycisk Edytuj zastępuje przyciski aktualizacji i Anuluj i Włącz BoundFields do pól tekstowych. Po użytkownika końcowego aktualizuje dane i klika aktualizacji, strony są wykonywane następujące czynności:

1. Widoku GridView wypełnia jego ObjectDataSource `UpdateParameters` z edytowanego rekordu unikatową identyfikację następującą liczbę pól: (za pośrednictwem `DataKeyNames` właściwości) oraz wartości wprowadzonej przez użytkownika
2. Widoku GridView wywołuje jego ObjectDataSource `Update()` metodę, która z kolei wywołuje metodę odpowiednią w obiekcie źródłowym (`ProductsDAL.UpdateProduct`, w naszym samouczku poprzedniej)
3. Danych, który teraz zawiera zaktualizowane zmiany, jest odbitych do widoku GridView

Podczas tej sekwencji kroki, liczbę zdarzeń wyzwalać, co pozwala na tworzenie obsługi zdarzeń do dodanie logiki niestandardowej, w razie potrzeby. Na przykład przed kroku 1, widoku GridView `RowUpdating` generowane zdarzenie. Firma Microsoft w tym momencie anulować żądanie aktualizacji w przypadku niektórych błąd sprawdzania poprawności. Gdy `Update()` wywołaniu metody ObjectDataSource `Updating` generowane zdarzenie, zapewniając możliwość dodawania lub Dostosuj wartości któregokolwiek z `UpdateParameters`. Po elemencie ObjectDataSource podstawowej metody obiektu została ukończona wykonywania ObjectDataSource `Updated` zdarzenia. Program obsługi zdarzeń dla `Updated` zdarzeń sprawdzić szczegółowe informacje o operacji aktualizacji, takich jak zmienionych liczby wierszy i czy wystąpił wyjątek. Na koniec, po kroku 2, widoku GridView `RowUpdated` generowane zdarzenie; program obsługi zdarzeń dla tego zdarzenia może sprawdzić dodatkowe informacje na temat operacji aktualizacji po prostu wykonać.

Rysunek 1 przedstawia tę serię zdarzeń i kroki w przypadku aktualizowania Element GridView. Wzorzec zdarzeń na rysunku 1. nie jest unikatowa do aktualizowania Element GridView. Wstawianie, aktualizowanie lub usuwanie danych z widoku GridView, widoku DetailsView lub FormView wytrąca taką samą sekwencję zdarzeń przed i po poziomu dla elementu ObjectDataSource i kontroli danych w sieci Web.


[![Wstępne serii i Fire zdarzenia po podczas aktualizowania danych w widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Rysunek 1**: A serii z przed i po zdarzenia Fire podczas aktualizowania danych w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


W tym samouczku zajmiemy się przy użyciu tych zdarzeń rozszerzenie Wstawianie wbudowanych aktualizowania i usuwania możliwości danych w programie ASP.NET Web kontrolki. Firma Microsoft widoczne jest również sposób dostosowywania interfejsu edycji można aktualizować tylko podzbiór pola produktu.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Krok 1: Aktualizowanie produktu`ProductName`i`UnitPrice`pól

W interfejsach edycji z poprzednich samouczek *wszystkie* produktu pola, które nie były tylko do odczytu musiały zostać uwzględnione. Czy możemy usunąć pole z widoku GridView - powiedzieć `QuantityPerUnit` — aktualizowanie danych formantu danych w sieci Web nie może ustawić ObjectDataSource `QuantityPerUnit` `UpdateParameters` wartości. Element ObjectDataSource następnie przejdzie w `null` wartość do `UpdateProduct` logiki warstwy logiki warstwy biznesowej (Biznesowej) metodę, która zmieniłaby rekordu edytowanych bazy danych `QuantityPerUnit` kolumny do `NULL` wartość. Podobnie, jeśli to pole jest wymagane, takie jak `ProductName`, zostanie usunięty z interfejsu edycji aktualizacji zakończy się niepowodzeniem z "*kolumny"ProductName"nie dopuszcza wartości null*" wyjątek. Powodem tego zachowania było, ponieważ element ObjectDataSource został skonfigurowany do wywołania `ProductsBLL` klasy `UpdateProduct` metodę, która oczekiwanego parametru wejściowego dla każdego pola produktu. W związku z tym ObjectDataSource firmy `UpdateParameters` kolekcji zawiera parametr dla każdej metody parametrów wejściowych.

Jeśli chcemy przekazać formant sieci Web, który umożliwia użytkownikom końcowym aktualizować tylko podzbiór pola danych, a następnie musimy programowo ustawić brakujący `UpdateParameters` wartości w elemencie ObjectDataSource `Updating` obsługi zdarzeń lub Utwórz i wywołania metody logiki warstwy Biznesowej oczekuje tylko podzestaw pola. Przyjrzyjmy się tym drugie podejście.

W szczególności Utwórzmy strona, wyświetlająca tylko `ProductName` i `UnitPrice` pola edycji widoku GridView. Interfejs do edycji tego widoku GridView zezwala tylko użytkownika zaktualizować wyświetlane dwa pola, `ProductName` i `UnitPrice`. Ponieważ ten interfejs edycji zapewnia tylko podzbiór pola produktów, albo należy utworzyć ObjectDataSource używającej BLL istniejących `UpdateProduct` — metoda i brakujące wartości w polach produktu ustawił programowo w jego `Updating` zdarzeń Program obsługi lub możemy konieczne utworzenie nowej metody logiki warstwy Biznesowej, która oczekuje tylko podzbiór pola zdefiniowane w widoku GridView. W tym samouczku umożliwia tę druga opcję i utworzyć przeciążenia `UpdateProduct` metoda, który przyjmuje tylko trzy parametry wejściowe: `productName`, `unitPrice`, i `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Oryginalne, takich jak `UpdateProduct` metoda, to przeciążenie uruchamia przez sprawdzenie, czy w bazie danych z określonym jest produktem `ProductID`. Jeśli nie, zwraca `false`, co oznacza, że nie można zaktualizować informacji o produkcie żądanie. W przeciwnym razie aktualizuje istniejący rekord produktu `ProductName` i `UnitPrice` odpowiednio pól i zatwierdza aktualizacji przez wywołanie metody TableAdpater `Update()` metody, przekazując `ProductsRow` wystąpienia.

Z tego uzupełnienia naszych `ProductsBLL` klasa, jest gotowy do utworzenia uproszczony interfejs widoku GridView. Otwórz `DataModificationEvents.aspx` w `EditInsertDelete` folderu i Dodaj do strony GridView. Utwórz nowy element ObjectDataSource i skonfigurować go do używania `ProductsBLL` klasy z jego `Select()` mapowanie metody `GetProducts` i jego `Update()` mapowanie — metoda `UpdateProduct` przeciążenia, które przyjmuje tylko `productName`, `unitPrice`, i `productID` parametrów wejściowych. Na rysunku 2 przedstawiono Kreatora tworzenia źródła danych podczas mapowania ObjectDataSource `Update()` metodę `ProductsBLL` na nowe klasy `UpdateProduct` przeciążenie metody.


[![Mapowanie metody Update() ObjectDataSource nowe przeciążenia UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Rysunek 2**: mapowanie ObjectDataSource `Update()` metodę, aby nowe `UpdateProduct` przeciążenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Ponieważ w naszym przykładzie zostanie początkowo wystarczy, że możliwość do edytowania danych, ale nie wstawiania lub usuwania rekordów, Poświęć chwilę, aby jawnie wskazać, że element ObjectDataSource `Insert()` i `Delete()` metody nie powinny zostać zmapowany do `ProductsBLL` metody klasy, przechodząc do karty INSERT i DELETE i wybierając (Brak) z listy rozwijanej.


[![(Brak) wybierz z listy rozwijanej dla INSERT i usuwanie kart](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Rysunek 3**: Wybierz (Brak) z listy rozwijanej dla Wstawianie i usuwanie kart ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Po ukończeniu pracy tego kreatora pole wyboru Włącz edytowanie z tagów inteligentnych w widoku GridView.

O zakończeniu Kreatora tworzenia źródła danych i powiązanie do widoku GridView Visual Studio został utworzony składni deklaratywnej dla obu formantów. Przejdź do widoku źródłowego, aby sprawdzić ObjectDataSource deklaratywne znaczników, które są wyświetlane poniżej:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Ponieważ nie ma żadnych mapowań dla elementu ObjectDataSource `Insert()` i `Delete()` metod, istnieją nie `InsertParameters` lub `DeleteParameters` sekcje. Ponadto, ponieważ `Update()` metody jest mapowany na `UpdateProduct` przeciążenie metody, która akceptuje trzy parametry wejściowe, `UpdateParameters` sekcja zawiera tylko trzech `Parameter` wystąpień.

Należy pamiętać, że element ObjectDataSource `OldValuesParameterFormatString` właściwość jest ustawiona na `original_{0}`. Ta właściwość jest ustawiana automatycznie przez program Visual Studio podczas korzystania z Kreatora konfigurowania źródła danych. Jednak ponieważ nasze metody logiki warstwy Biznesowej oczekują, że oryginalne `ProductID` wartości, należy przesłać, całkowicie usunąć to przypisanie właściwości z składni deklaratywnej ObjectDataSource.

> [!NOTE]
> Jeśli po prostu wyczyszczenie `OldValuesParameterFormatString` wartości właściwości z okna właściwości w widoku Projekt, właściwość będą nadal istnieć w składni deklaratywnej, ale zostanie ustawiona na pusty ciąg. Usuń właściwość całkowicie z składni deklaratywnej lub, w oknie właściwości ustaw wartość domyślną, `{0}`.


Gdy zawiera tylko element ObjectDataSource `UpdateParameters` nazwy, ceny i identyfikator produktu Visual Studio dodał elementu BoundField lub CheckBoxField w widoku GridView dla każdego pola tego produktu.


[![Widoku GridView zawiera pole BoundField lub CheckBoxField dla każdego pola produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Rysunek 4**: widoku GridView zawiera pole BoundField lub CheckBoxField dla każdego pola produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Gdy użytkownik edytuje produktu i kliknięciu przycisku jej aktualizacji, w widoku GridView wylicza tych pól, które nie były tylko do odczytu. Następnie ustawia wartość odpowiadającego mu parametru w elemencie ObjectDataSource `UpdateParameters` kolekcji wartości wprowadzonej przez użytkownika. Jeśli nie ma odpowiadającego mu parametru, widoku GridView dodaje je do kolekcji. W związku z tym jeśli naszych GridView zawiera BoundFields i CheckBoxFields dla wszystkich pól produktu, ObjectDataSource zakończą się wywoływania `UpdateProduct` przeciążenia, które przyjmuje we wszystkich tych parametrów, mimo że element ObjectDataSource deklaracyjne znaczników określa tylko trzy parametry wejściowe (patrz rysunek 5). Podobnie w przypadku niektórych kombinacji nie — tylko do odczytu pola produktu w widoku GridView, który nie jest zgodny z parametrów wejściowych dla `UpdateProduct` przeciążenia, zostanie zgłoszony wyjątek podczas próby aktualizacji.


[![Widoku GridView będzie można dodać parametry do kolekcji UpdateParameters ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Rysunek 5**: widoku GridView będzie dodać parametry do elementu ObjectDataSource `UpdateParameters` kolekcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Aby upewnić się, że element ObjectDataSource wywołuje `UpdateProduct` przeciążenia, które przyjmuje tylko produktu nazwę, ceny i identyfikator, należy ograniczyć widoku GridView mającej edytowalnego pola właśnie `ProductName` i `UnitPrice`. Można to zrobić przez usunięcie BoundFields i innych CheckBoxFields, ustawiając tych innych pól `ReadOnly` właściwości `true`, lub kombinację obu. W tym samouczku umożliwia po prostu usuń wszystkie pola widoku GridView z wyjątkiem `ProductName` i `UnitPrice` BoundFields, po upływie którego będzie wyglądać w widoku GridView deklaratywne znaczników:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Mimo że `UpdateProduct` przeciążenia oczekuje trzy parametry wejściowe, mamy tylko dwa BoundFields w naszym GridView. Jest to spowodowane `productID` parametru wejściowego jest wartość klucza podstawowego i przekazany przez wartość `DataKeyNames` właściwości dla wiersza edytowanej.

Nasze GridView wraz z programem `UpdateProduct` przeciążenia, umożliwia użytkownikom edytowanie tylko nazwy i cena produktu bez utraty pola produktu.


[![Interfejs umożliwiający edytowanie tylko produktu nazwę i cen](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Rysunek 6**: umożliwia interfejsu edycji właśnie produktu nazwę i cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Zgodnie z opisem w poprzedniej samouczek, ma zasadnicze znaczenie, że GridView s stan widoku jest włączone (domyślnie). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko równoczesnych użytkowników przypadkowo usunięcie lub edytowania rekordów. Zobacz [Ostrzeżenie: problem współbieżności z ASP.NET 2.0 GridViews/widoku DetailsView/FormViews tej edycji pomocy technicznej i/lub usunięcie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) Aby uzyskać więcej informacji.


## <a name="improving-theunitpriceformatting"></a>Poprawa`UnitPrice`formatowania

Podczas GridView przykład pokazany na rysunku 6 działa, `UnitPrice` pola nie jest sformatowany w ogóle, co wyświetlania cen, który nie ma żadnych waluty symboli i ma cztery miejsc dziesiętnych. Aby zastosować waluty formatowania dla wierszy nie można edytować, po prostu ustaw `UnitPrice` dla elementu BoundField `DataFormatString` właściwości `{0:c}` i jego `HtmlEncode` właściwości `false`.


[![Ustaw właściwości HtmlEncode i DataFormatString UnitPrice odpowiednio](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Rysunek 7**: Ustaw `UnitPrice`w `DataFormatString` i `HtmlEncode` odpowiednio do właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Nie można edytować wierszy z tą zmianą formatu ceny jako walutę; wiersza edytowanej, jednak nadal wyświetlana wartość bez symbol waluty i czterech miejsc dziesiętnych.


[![Nie można edytować wiersze są teraz sformatowane jako wartości walutowe](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Rysunek 8**: nie można edytować wierszy są teraz sformatowane jako wartości waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Instrukcje dotyczące formatowania określone w `DataFormatString` właściwości można zastosować do edycji interfejsu przez ustawienie elementu BoundField `ApplyFormatInEditMode` właściwości `true` (wartość domyślna to `false`). Poświęć chwilę, aby ustawić tę właściwość na `true`.


[![Ustaw właściwość ApplyFormatInEditMode elementu UnitPrice BoundField na wartość true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Rysunek 9**: Ustaw `UnitPrice` dla elementu BoundField `ApplyFormatInEditMode` właściwości `true` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Dzięki tej zmianie wartości `UnitPrice` wyświetlane w edytowanych wiersz jest również w formacie waluty.


[![Wartość UnitPrice wiersz edytowany jest teraz sformatowane jako walutę](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Na rysunku nr 10**: edytowanie wiersza `UnitPrice` wartość jest teraz w formacie waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Jednak aktualizowania produktu o symbol waluty w polu tekstowym, takich jak $19,00 zgłasza `FormatException`. Podczas próby przypisania wartości dostarczone przez użytkownika do ObjectDataSource widoku GridView `UpdateParameters` kolekcji nie może przekonwertować `UnitPrice` na ciąg znaków "$19,00" `decimal` wymagane przez parametr (patrz rysunek 11). Aby rozwiązać ten problem można utworzyć programu obsługi zdarzeń dla w widoku GridView `RowUpdating` zdarzeń i przeanalizować podanego użytkownika `UnitPrice` jako formacie waluty `decimal`.

W widoku GridView `RowUpdating` zdarzeń akceptuje jako drugi parametr typu obiektu [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), która obejmuje `NewValues` słownika jako jeden z jego właściwości, które przechowuje dostarczone przez użytkownika wartości gotowe do przeniesienia przypisane do elementu ObjectDataSource `UpdateParameters` kolekcji. Firma Microsoft może spowodować nadpisanie istniejących `UnitPrice` wartość w `NewValues` kolekcji o wartości dziesiętnej przeanalizowany w formacie waluty następujące wiersze kodu w `RowUpdating` obsługi zdarzeń:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Jeśli użytkownik ma podany `UnitPrice` wartości (na przykład "$19,00"), ta wartość jest zastępowany wartości dziesiętnej obliczone przez [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analizy wartości jako walutę. To zostanie prawidłowo przeanalizować dziesiętnego w przypadku symbol waluty, przecinki, miejsc dziesiętnych i tak dalej, a używa [wyliczenie NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) w [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) przestrzeni nazw.

Rysunek 11 pokazuje problem spowodowany przez symbole waluty w podanego użytkownika `UnitPrice`, oraz jak w widoku GridView `RowUpdating` obsługi zdarzeń mogą zostać użyte można poprawnie przeanalizować takich danych wejściowych.


[![Wartość UnitPrice wiersz edytowany jest teraz sformatowane jako walutę](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Rysunek 11**: edytowanie wiersza `UnitPrice` wartość jest teraz w formacie waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Krok 2: zabronienia`NULL UnitPrices`

Gdy baza danych jest skonfigurowane i umożliwiają `NULL` wartości w `Products` tabeli `UnitPrice` kolumny, firma Microsoft może chcesz uniemożliwić użytkownikom odwiedzenie tej konkretnej strony z określania `NULL` `UnitPrice` wartości. Oznacza to, że jeśli użytkownik nie wprowadzi `UnitPrice` wartości podczas edycji wiersza produktu, a nie zapisane wyniki w bazie danych chcemy, aby wyświetlić komunikat, którego użytkownik dowie się, że przez tę stronę, wszystkie produkty edytowanych musi mieć cenę określony.

`GridViewUpdateEventArgs` Obiektu przekazany w widoku GridView `RowUpdating` zawiera program obsługi zdarzeń `Cancel` właściwości, jeśli ustawiono `true`, kończy proces uaktualniania. Ta funkcja pozwala rozszerzyć `RowUpdating` obsługi zdarzeń, aby ustawić `e.Cancel` do `true` i wyświetli komunikat wyjaśniający dlaczego, jeśli `UnitPrice` wartość w `NewValues` kolekcja jest `null`.

Rozpocznij od dodania formantu etykiety w sieci Web do strony o nazwie `MustProvideUnitPriceMessage`. Ta etykieta będzie wyświetlana, jeśli użytkownik nie może określić `UnitPrice` wartości podczas aktualizowania produktu. Ustaw wartość etykiety `Text` dla właściwości "Dla tego produktu musisz podać cenę". Utworzono również nową klasę CSS w `Styles.css` o nazwie `Warning` z następującej definicji:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Wreszcie, ustaw wartość etykiety `CssClass` właściwości `Warning`. W tym momencie projektanta powinien być wyświetlony komunikat ostrzegawczy w red, pogrubienie, kursywa, bardzo duży rozmiar czcionki powyżej widoku GridView, jak pokazano na rysunku 12.


[![Dodano etykietę powyżej widoku GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Rysunek 12**: A Etykieta ma został dodany powyżej widok GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Domyślnie ta etykieta powinien być ukryty, a więc ustaw jego `Visible` właściwości `false` w `Page_Load` obsługi zdarzeń:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Jeśli użytkownik próbuje zaktualizować produkt bez określania `UnitPrice`, chcemy anulować aktualizację i Wyświetl etykietę ostrzeżenie. W widoku GridView rozszerzyć `RowUpdating` obsługi zdarzeń w następujący sposób:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Jeśli użytkownik próbuje zapisać produktu bez określania cenę, aktualizacji została anulowana i zostanie wyświetlony komunikat przydatne. Połączenie bazy danych (i logiki biznesowej) umożliwia `NULL` `UnitPrice` s, tej konkretnej strony ASP.NET nie ma.


[![Użytkownik nie może opuścić UnitPrice puste](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Rysunek 13**: A użytkownik nie może opuścić `UnitPrice` puste ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Do tej pory ma pokazano sposób użycia w widoku GridView `RowUpdating` zdarzeń programowo zmienić wartości parametru dla elementu ObjectDataSource `UpdateParameters` kolekcji także jak anulować aktualizację przetworzyć całkowicie. Te pojęcia przenoszone do formantów w widoku DetailsView i FormView i dotyczą również Wstawianie i usuwanie.

Można również wykonać te zadania za pomocą obsługi zdarzeń na poziomie elementu ObjectDataSource jego `Inserting`, `Updating`, i `Deleting` zdarzenia. Te zdarzenia wyzwalać przed skojarzona metoda obiektu podstawowego i podaj ostatniej szansy możliwość modyfikowania kolekcji parametrów wejściowych, lub Anuluj operację bezpośrednich. Programy obsługi zdarzeń dla zdarzenia te trzy są przekazywane typu obiektu [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) mający dwie właściwości odsetek:

- [Anuluj](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), która, jeśli ustawioną `true`, anuluje operację
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), który jest kolekcją `InsertParameters`, `UpdateParameters`, lub `DeleteParameters`w zależności od tego, czy program obsługi zdarzeń jest dla `Inserting`, `Updating`, lub `Deleting` zdarzeń

Aby zilustrować pracy przy użyciu wartości parametrów na poziomie elementu ObjectDataSource, umożliwia dołączenie Element DetailsView naszą stronę, który umożliwia użytkownikom dodanie nowego produktu. Tego widoku DetailsView będzie służyć do zapewnienia interfejs szybkie dodanie nowego produktu do bazy danych. Aby zachować spójny interfejs użytkownika, gdy umożliwia dodanie nowego produktu umożliwia użytkownikowi tylko wprowadź wartości w polach `ProductName` i `UnitPrice` pól. Domyślnie te wartości, które nie są dostarczane w interfejsie Wstawianie DetailsView zostanie ustawiona do `NULL` bazy danych wartości. Jednak można użyć elementu ObjectDataSource `Inserting` zdarzeń iniekcję różne domyślne wartości, ponieważ zajmiemy się wkrótce.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Krok 3: Udostępnia interfejs do dodawania nowych produktów

Przeciągnij element DetailsView z przybornika do projektanta powyżej widoku GridView, wyczyść jej `Height` i `Width` właściwości i powiązanie go do elementu ObjectDataSource znajdujących się już na stronie. Spowoduje to dodanie elementu BoundField lub CheckBoxField dla każdego pola tego produktu. Ponieważ chcemy użyć tego widoku DetailsView do dodania nowych produktów, musimy zaznacz opcję Włącz wstawianie z tagów inteligentnych; jednak nie ma takich tej opcji, ponieważ element ObjectDataSource `Insert()` — metoda nie jest zamapowany do metody w `ProductsBLL` klasy (odwołania możemy ustawić tego mapowania na (Brak) podczas konfigurowania źródła danych, zobacz rysunek 3).

Aby skonfigurować ObjectDataSource, wybierz link Konfiguruj źródła danych z jego tagów inteligentnych uruchamianie kreatora. Pierwszy ekran umożliwia zmianę źródłowy obiekt, który jest powiązany element ObjectDataSource; Pozostaw ona ustawiona `ProductsBLL`. Następnym ekranie przedstawiono mapowania ObjectDataSource metody do obiektu źródłowego. Mimo że firma Microsoft wyraźnie wskazane, który `Insert()` i `Delete()` metody nie powinny być mapowane do żadnych metod, jeśli przejdź do karty INSERT i DELETE zobaczysz, że mapowanie istnieje. Jest to spowodowane `ProductsBLL`w `AddProduct` i `DeleteProduct` metody `DataObjectMethodAttribute` atrybut wskazuje, że są domyślne metody `Insert()` i `Delete()`odpowiednio. W związku z tym kreatora ObjectDataSource wybiera te zawsze, gdy należy uruchomić kreatora, chyba że istnieje wartość jawnie określony.

Pozostaw `Insert()` wskazujący — metoda `AddProduct` metody, ale ponownie ustawić listy rozwijanej karcie DELETE (Brak).


[![Ustaw kartę Wstawianie listy rozwijanej do metody AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Rysunek 14**: wartość listy rozwijanej kartę wstawianie `AddProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Ustaw kartę usuwania listy rozwijanej (Brak)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Rysunek 15**: wartość na liście rozwijanej kartę usunąć (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Po wprowadzeniu tych zmian, składni deklaratywnej ObjectDataSource zostanie rozszerzona do uwzględnienia `InsertParameters` kolekcji, jak pokazano poniżej:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Ponowne uruchomienie Kreatora dodany ponownie `OldValuesParameterFormatString` właściwości. Poświęć chwilę, aby wyczyścić tej właściwości, ustawiając wartość domyślna (`{0}`) lub usunięcie go całkowicie z składni deklaratywnej.

Element ObjectDataSource zapewnia możliwości wstawianie tagów inteligentnych DetailsView teraz obejmuje pole wyboru Włącz wstawianie; Wróć do projektanta i zaznacz tę opcję. Następnie nawiasu dół widoku DetailsView tak, aby ma tylko dwie BoundFields - `ProductName` i `UnitPrice` - i CommandField. W tym momencie składni deklaratywnej DetailsView powinien wyglądać następująco:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

16 rysunek przedstawia tę stronę podczas przeglądania w tym momencie za pośrednictwem przeglądarki. Jak widać, w widoku DetailsView Wyświetla nazwę i ceny pierwszy produktu (Chai). Firma Microsoft ma, jest jednak Wstawianie interfejs, który umożliwia użytkownikowi szybkie dodanie nowego produktu do bazy danych.


[![Widoku DetailsView jest obecnie renderowane w trybie tylko do odczytu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Rysunek 16**: widoku DetailsView jest obecnie renderowane w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Aby pokazać widoku DetailsView w trybie Wstawianie musimy ustawić `DefaultMode` właściwości `Inserting`. Renderuje widoku DetailsView w trybie wstawiania po odwiedzeniu najpierw i przechowuje ją w tym miejscu po wstawieniu nowego rekordu. Jak pokazano na rysunku 17, widoku DetailsView udostępnia szybkie interfejs na potrzeby dodawania nowego rekordu.


[![Widoku DetailsView zawiera interfejs umożliwiający szybkie dodanie nowego produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Rysunek 17**: widoku DetailsView zawiera interfejs umożliwiający szybkie dodanie nowego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Gdy użytkownik wprowadza nazwę produktu i cen (na przykład "Xyz wody" i 1.99, tak jak rysunek 17) i klika przycisk Wstaw, ensues odświeżania strony i rozpocznie się wstawianie przepływu pracy, skutkując nowy rekord produktu dodawane do bazy danych. Widoku DetailsView obsługuje interfejs Wstawianie i widoku GridView jest automatycznie odbitych ze swoim źródłem danych Aby dołączyć nowy produkt, jak pokazano na rysunku 18.


![Produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Rysunek 18**: produktu "Xyz wody" został dodany do bazy danych


Gdy GridView na rysunku 18 nie wyświetla, pól produktu nie zawiera interfejsu widoku DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`i tak dalej przypisano `NULL` bazy danych wartości. Użytkownik widzi, wykonując następujące czynności:

1. Przejdź do Eksploratora serwera w programie Visual Studio
2. Rozszerzanie `NORTHWND.MDF` węzła bazy danych
3. Kliknij prawym przyciskiem myszy `Products` węzła tabeli bazy danych
4. Wybierz opcję Pokaż dane tabeli

To spowoduje wyświetlenie listy wszystkich rekordów `Products` tabeli. Rysunek 19 ilustruje wszystkie kolumny naszego nowego produktu innej niż `ProductID`, `ProductName`, i `UnitPrice` ma `NULL` wartości.


[![Produkt pól nie podano w widoku DetailsView są przypisane wartości NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Rysunek 19**: przypisano produktu pól nie podano w widoku DetailsView `NULL` wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Chcemy podać wartość domyślną innego niż `NULL` dla co najmniej jednego z tych wartości kolumny, albo ponieważ `NULL` nie jest najlepszą opcją domyślną lub z powodu kolumny bazy danych sam nie zezwala na `NULL` s. W tym celu możemy programowane Ustawianie wartości parametrów DetailsView `InputParameters` kolekcji. To przypisanie może odbywać się albo w przypadku obsługi dla DetailsView `ItemInserting` zdarzenia lub ObjectDataSource `Inserting` zdarzeń. Ponieważ już analizujemy zostały danych sieci Web za pomocą zdarzeń przed i po poziomu sterować poziomem, Przyjrzyjmy się za pomocą zdarzeń ObjectDataSource teraz.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Krok 4: Przypisywanie wartości do`CategoryID`i`SupplierID`parametrów

W tym samouczku teraz załóżmy, że dla aplikacji podczas dodawania nowego produktu za pośrednictwem tego interfejsu powinna mieć przypisaną `CategoryID` i `SupplierID` wartość 1. Jak wspomniano wcześniej, ObjectDataSource ma para zdarzeń przed i po poziomu podczas modyfikacji danych uruchamianych. Po jego `Insert()` wywołania metody, najpierw wywołuje element ObjectDataSource jego `Inserting` zdarzeń, następnie wywołuje metodę który jego `Insert()` — metoda została zamapowana na, a na koniec zgłasza `Inserted` zdarzeń. `Inserting` Program obsługi zdarzeń zapewnia nam jeden ostatnią możliwość dostosować parametry wejściowe lub Anuluj operację bezpośrednich.

> [!NOTE]
> W rzeczywistych aplikacjach będzie prawdopodobnie zechcesz Pozwól użytkownika określ kategorii i dostawcy lub czy wybierz tę wartość na ich podstawie od określonych kryteriów lub business logiki (zamiast ślepo wybranie Identyfikatora 1). Niezależnie od tego pokazano w przykładzie programowo ustawiania wartości parametru wejściowego z ObjectDataSource wstępnie poziomu zdarzeń.


Poświęć chwilę, aby utworzyć program obsługi zdarzeń dla elementu ObjectDataSource `Inserting` zdarzeń. Zauważ, że program obsługi zdarzeń drugi parametr wejściowy jest typu obiektu `ObjectDataSourceMethodEventArgs`, który ma właściwość, aby można było uzyskać dostępu do kolekcji parametrów (`InputParameters`) i właściwości, aby anulować operację (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

W tym momencie `InputParameters` właściwość zawiera element ObjectDataSource `InsertParameters` kolekcji o wartości z widoku DetailsView. Aby zmienić wartości jednego z tych parametrów, po prostu użyj: `e.InputParameters["paramName"] = value`. W związku z tym można ustawić `CategoryID` i `SupplierID` wartości 1, Dostosuj `Inserting` program obsługi zdarzeń do wyglądać następująco:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Ten czas podczas dodawania nowego produktu (na przykład Soda xyz) `CategoryID` i `SupplierID` kolumny nowego produktu są ustawione na wartość 1 (patrz rysunek 20).


[![Nowych produktów już ich CategoryID i identyfikator wartości zestawu do 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Rysunek 20**: nowych produktów teraz ma ich `CategoryID` i `SupplierID` wartości ustawioną wartość 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Podsumowanie

Podczas edytowanie, wstawianie i usuwanie procesów zarówno kontroli danych w sieci Web, jak i element ObjectDataSource przejdź przez liczbę zdarzeń przed i po poziomu. W tym samouczku będziemy zbadane wstępnie poziomu zdarzeń i pokazano, jak skorzystać z tych Aby dostosować parametry wejściowe lub anulować operacji modyfikacji danych całkowicie zarówno z kontroli danych w sieci Web i zdarzeń w elemencie ObjectDataSource. W następnym samouczku wyjaśniono, tworzenia i używania programów obsługi zdarzeń dla zdarzenia po poziomu.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Jackie Goor i Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](an-overview-of-inserting-updating-and-deleting-data-cs.md)
[dalej](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
