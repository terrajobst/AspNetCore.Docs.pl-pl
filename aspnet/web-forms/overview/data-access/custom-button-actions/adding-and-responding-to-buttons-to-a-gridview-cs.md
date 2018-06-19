---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Dodawanie i reagowanie na przycisków GridView (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku wyjaśniono, jak dodać niestandardowe przyciski, szablon i pola formantu widoku GridView lub widoku DetailsView. W szczególności firma Microsoft będzie bui...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90648e10d5d058ea2e4aa5b3d8c4ed7448ea7166
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878298"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Dodawanie i reagowanie na przycisków GridView (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) lub [pobierania plików PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> W tym samouczku wyjaśniono, jak dodać niestandardowe przyciski, szablon i pola formantu widoku GridView lub widoku DetailsView. W szczególności firma Microsoft będzie kompilacji interfejs, który ma FormView, który umożliwia użytkownikowi przeglądanie dostawców.


## <a name="introduction"></a>Wprowadzenie

Chociaż wiele scenariuszy raportowania obejmują dostęp tylko do odczytu do danych raportu, nie ma nietypowe dla raportów, które obejmują możliwość wykonywania akcji w oparciu o dane wyświetlane. Zwykle to związane z dodaniem formantu przycisku, LinkButton lub ImageButton sieci Web z każdego rekordu wyświetlane w raporcie, który po kliknięciu powoduje odświeżenie strony i wywołuje kodu po stronie serwera. Edytowanie i usuwanie danych na podstawie rekordu przez rekordu jest najbardziej typowym przykładem. W rzeczywistości jako widzieliśmy począwszy [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek, edytowanie i usuwanie jest tak często, że kontrolki GridView widoku DetailsView i FormView może obsługiwać takich funkcji bez potrzebne do napisania jakiegokolwiek wiersza kodu.

Ponadto do edytowania i usuwania przycisków widoku GridView, widoku DetailsView i FormView formanty mogą również obejmować przycisków, LinkButtons lub ImageButtons, po kliknięciu wykonać niektóre niestandardowej logiki po stronie serwera. W tym samouczku wyjaśniono, jak dodać niestandardowe przyciski, szablon i pola formantu widoku GridView lub widoku DetailsView. W szczególności firma Microsoft będzie kompilacji interfejs, który ma FormView, który umożliwia użytkownikowi przeglądanie dostawców. Dla danego dostawcy FormView wyświetli informacje o dostawcy wraz z formantu sieci Web przycisku, który kliknięciu spowoduje oznaczenie wszystkich produktów, ich skojarzone jako zaprzestać. Ponadto Element GridView wymieniono produkty te udostępniane przez wybranego dostawcy, w której każdy wiersz zawierający zwiększyć ceny i Discount cen przycisków, kliknięciu zwiększyć lub zmniejszyć produktu `UnitPrice` przy 10% (zobacz rysunek 1).


[![Zarówno FormView i GridView zawiera przyciski umożliwiające wykonywanie akcji niestandardowych](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Rysunek 1**: zarówno FormView i GridView zawiera przyciski czy wykonywać akcje niestandardowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Krok 1: Dodawanie przycisku Samouczek stron sieci Web

Zanim przyjrzymy sposób dodawania niestandardowych przycisków umożliwia najpierw Poświęć chwilę, aby utworzyć stron ASP.NET w naszym projekt witryny sieci Web, które będą potrzebne w tym samouczku. Rozpocznij od dodania nowy folder o nazwie `CustomButtons`. Następnie dodaj następujące dwie strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strony głównej:

- `Default.aspx`
- `CustomButtons.aspx`


![Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Rysunek 2**: Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków


Podobnie jak w innych folderach `Default.aspx` w `CustomButtons` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań do widoku projektu.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Ponadto dodanie stron jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po stronicowania i sortowania `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczki.


![Mapy witryny zawiera teraz wpis samouczek przyciski niestandardowe](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Rysunek 4**: mapy witryny zawiera teraz wpis samouczek przyciski niestandardowe


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Krok 2: Dodawanie FormView, który zawiera listę dostawców

Rozpocznijmy tego samouczka, dodając FormView, który zawiera listę dostawców. Zgodnie z opisem w wprowadzenie, to FormView Zezwala użytkownikowi na stronę za pośrednictwem dostawców przedstawiający produktów dostarczone przez dostawcę w widoku GridView. Ponadto ta FormView będzie zawierać przycisk, po kliknięciu oznaczy wszystkie produkty dostawcy jako przerywane. Przed możemy dotyczą nad dodawania niestandardowych przycisku do FormView umożliwia po prostu utworzyć FormView tak, aby go Wyświetla informacje o dostawcy.

Uruchamianie przez otwarcie `CustomButtons.aspx` strony `CustomButtons` folderu. Na stronie Dodaj FormView przeciągając je z przybornika do projektanta i zestaw jej `ID` właściwości `Suppliers`. Z tagów inteligentnych FormView, wybrać do utworzenia nowego elementu ObjectDataSource o nazwie `SuppliersDataSource`.


[![Utwórz nowy element ObjectDataSource o nazwie SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Rysunek 5**: Utwórz nowy składnik o nazwie ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Skonfiguruj ten nowy element ObjectDataSource w taki sposób, że zapytanie z `SuppliersBLL` klasy `GetSuppliers()` — metoda (patrz rysunek 6). Ponieważ ten FormView nie zapewnia interfejs na zaktualizowanie dostawcy informacje, wybierz opcję (Brak) z listy rozwijanej w karcie aktualizacji.


[![Skonfiguruj źródło danych do użycia klasy SuppliersBLL s GetSuppliers() — metoda](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Rysunek 6**: Skonfiguruj źródło danych do używania `SuppliersBLL` klasy `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Po skonfigurowaniu ObjectDataSource, program Visual Studio wygeneruje `InsertItemTemplate`, `EditItemTemplate`, i `ItemTemplate` dla widoku FormView. Usuń `InsertItemTemplate` i `EditItemTemplate` i zmodyfikuj `ItemTemplate` tak, aby wyświetla tylko dostawcy firmy nazwę i numer telefonu. Na koniec, Włącz obsługę stronicowania w widoku FormView, zaznaczając pole wyboru Włącz stronicowanie w tagu inteligentnego (lub przez ustawienie jej `AllowPaging` właściwości `True`). Po wprowadzeniu tych zmian znaczników deklaratywne ze strony powinien wyglądać podobny do następującego:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Rysunek nr 7 przedstawia strony CustomButtons.aspx podczas wyświetlania za pośrednictwem przeglądarki.


[![Wyświetla FormView NazwaFirmy i pola telefonów z aktualnie wybranego dostawcy](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Rysunek 7**: Wyświetla listę FormView `CompanyName` i `Phone` pola z aktualnie wybrany dostawca ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Krok 3: Dodawanie widoku GridView, który zawiera listę produktów wybranego dostawcy

Aby dodać do szablonu FormView przycisk przerwanie wszystkich produktów umożliwia najpierw dodamy GridView poniżej FormView, który zawiera listę produktów, dostarczone przez wybranego dostawcę. Aby to osiągnąć, należy dodać element GridView do strony, ustaw jej `ID` właściwości `SuppliersProducts`, i Dodaj nowy element ObjectDataSource o nazwie `SuppliersProductsDataSource`.


[![Utwórz nowy element ObjectDataSource o nazwie SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Rysunek 8**: Utwórz nowy składnik o nazwie ObjectDataSource `SuppliersProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Skonfiguruj ten element ObjectDataSource do użycia klasy ProductsBLL `GetProductsBySupplierID(supplierID)` — metoda (patrz rysunek 9). Podczas tego widoku GridView będzie zezwalał dla cena produktu do skorygowania, nie będą korzystać z wbudowanych, edytowanie lub usuwanie funkcji z widoku GridView. Firma Microsoft w związku z tym ustawić listy rozwijanej (Brak) dla elementu ObjectDataSource na UPDATE, INSERT i usuwanie kart.


[![Skonfiguruj źródło danych do użycia klasy ProductsBLL s GetProductsBySupplierID(supplierID) — metoda](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Rysunek 9**: Skonfiguruj źródło danych do używania `ProductsBLL` klasy `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Ponieważ `GetProductsBySupplierID(supplierID)` metoda przyjmuje parametr wejściowy, monituje Kreator ObjectDataSource nam źródła wartości tego parametru. Umożliwia przekazywanie `SupplierID` wartość z FormView, ustaw parametr źródła z listy kontroli i ControlID listy rozwijanej do `Suppliers` (identyfikator FormView utworzony w kroku 2).


[![Wskazuje, że IDDostawcy parametru, powinna pochodzić z kontrolą FormView dostawcy](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Na rysunku nr 10**: wskazują, że *`supplierID`* parametru powinna pochodzić ze `Suppliers` kontroli FormView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Po zakończeniu pracy Kreatora ObjectDataSource widoku GridView będzie zawierać elementu BoundField lub CheckBoxField dla każdego pola danych tego produktu. Umożliwia trim to w dół, aby wyświetlić tylko `ProductName` i `UnitPrice` BoundFields wraz z `Discontinued` CheckBoxField; Ponadto umożliwia formatu `UnitPrice` elementu BoundField tak, aby jego tekstu jest w formacie waluty. Z widoku GridView i `SuppliersProductsDataSource` znaczników deklaratywne przez element ObjectDataSource powinien wyglądać podobnie do następującego znaczników:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

W tym momencie z naszym samouczkiem wyświetla raport głównych/szczegółów, dzięki czemu użytkownik i wybierz dostawcę z FormView u góry i wyświetlić produktów dostarczone przez tego dostawcę przy użyciu widoku GridView u dołu. Rysunek 11 przedstawiono zrzut ekranu strony podczas wybierania dostawcy Tokyo Traders z FormView.


[![Produkty s wybrane dostawcy są wyświetlane w widoku GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Rysunek 11**: wybrane produkty dostawcy są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Krok 4: Tworzenie warstwy DAL i logiki warstwy Biznesowej metody, aby przerwać wszystkich produktów dla dostawcy

Zanim dodamy przycisku do FormView, po kliknięciu zaprzestaje wszystkich produktów dostawcy, najpierw musimy dodać metodę DAL i logiki warstwy Biznesowej, które wykonuje tej akcji. W szczególności, ta metoda będzie miała nazwę `DiscontinueAllProductsForSupplier(supplierID)`. Po kliknięciu przycisku FormView, firma Microsoft będzie wywołanie tej metody, warstwy logiki biznesowej, przekazywanie wybranego dostawcy `SupplierID`; logiki warstwy Biznesowej będzie wywoływać w dół do odpowiedniej metody Warstwa dostępu do danych, które będą wystawiać `UPDATE` — instrukcja w bazie danych zaprzestaje produkty określonego dostawcy.

Jak możemy to zostało zrobione w naszych samouczków poprzedniej, użyjemy podejścia, począwszy od tworzenia metody DAL, a następnie metoda logiki warstwy Biznesowej i na koniec wdrażania funkcji na stronie platformy ASP.NET. Otwórz `Northwind.xsd` wpisane zestaw danych z `App_Code/DAL` folderu i dodać nową metodę do `ProductsTableAdapter` (kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie). W ten sposób zostanie wyświetlone okno Kreatora konfiguracji zapytania TableAdapter, w którym przedstawiono NAS proces dodawania nowej metody. Uruchom, wskazując, że nasze metody DAL będzie używać instrukcji SQL ad hoc.


[![Create — metoda DAL za pomocą instrukcji SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Rysunek 12**: tworzenie przy użyciu metody DAL instrukcji SQL Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Następnie kreator wyświetli nam dotyczące rodzaju zapytanie w celu utworzenia. Ponieważ `DiscontinueAllProductsForSupplier(supplierID)` wymaga zaktualizowania metody `Products` tabeli bazy danych, ustawienie `Discontinued` pola na wartość 1 dla wszystkich produktów dostarczonych przez określony *`supplierID`*, należy utworzyć kwerendę, która aktualizuje dane.


[![Wybierz typ zapytania aktualizacji](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Rysunek 13**: Wybierz typ zapytania aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


Na następnym ekranie kreatora zawiera jego istniejącą TableAdapter `UPDATE` instrukcja, która aktualizuje każde z pól zdefiniowane w `Products` elementu DataTable. Zastąp tekst tego zapytania następująca instrukcja:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Po wprowadzeniu tego zapytania, a następnie klikając przycisk Dalej, ostatni ekran kreatora poprosi o podanie nowej metody, użyj nazwy `DiscontinueAllProductsForSupplier`. Ukończ pracę kreatora, klikając przycisk Zakończ. Po powrocie do Projektanta obiektów DataSet powinna być widoczna nowa metoda w `ProductsTableAdapter` o nazwie `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nazwa nowej DiscontinueAllProductsForSupplier DAL — metoda](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Rysunek 14**: Nazwa nowej metody DAL `DiscontinueAllProductsForSupplier` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Z `DiscontinueAllProductsForSupplier(supplierID)` metodą utworzone w warstwie dostępu do danych, naszych następne zadanie jest utworzenie `DiscontinueAllProductsForSupplier(supplierID)` metody warstwy logiki biznesowej. W tym celu otwórz `ProductsBLL` klasy plików i Dodaj następujący kod:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Ta metoda wywołuje po prostu do `DiscontinueAllProductsForSupplier(supplierID)` metoda warstwy DAL przekazywanie wzdłuż dostarczonych *`supplierID`* wartość parametru. Gdyby wszystkie reguły biznesowe, które są dozwolone tylko produktów dostawcy wycofywane w pewnych okolicznościach, zasady te powinny być implementowane w tym miejscu w logiki warstwy Biznesowej.

> [!NOTE]
> W odróżnieniu od `UpdateProduct` overloads w `ProductsBLL` klasy `DiscontinueAllProductsForSupplier(supplierID)` podpis metody nie obejmuje `DataObjectMethodAttribute` atrybutu (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Wykluczy to `DiscontinueAllProductsForSupplier(supplierID)` metodę z listy rozwijanej Kreatora konfigurowania źródła danych przez element ObjectDataSource na karcie aktualizacji. I Zapisz pominięcia tego atrybutu, ponieważ firma Microsoft będzie mógł wywołać `DiscontinueAllProductsForSupplier(supplierID)` metody bezpośrednio z obsługi zdarzeń w naszej strony ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Krok 5: Dodawanie przerwanie przycisk wszystkie produkty, aby FormView

Z `DiscontinueAllProductsForSupplier(supplierID)` metody logiki warstwy Biznesowej i warstwy DAL pełną, ostatnim krokiem dodawania możliwość przerwanie wszystkich produktów dla wybranego dostawcy jest dodawanie formantu przycisku sieci Web do FormView `ItemTemplate`. Dodajmy przycisku poniżej numer telefonu dostawcy tekstem przycisku, przerwanie wszystkich produktów i `ID` wartość właściwości `DiscontinueAllProductsForSupplier`. Można dodać tego formantu przycisku sieci Web za pomocą projektanta, klikając łącze Edytuj szablony w tagu FormView (patrz rysunek 15), lub bezpośrednio za pomocą składni deklaratywnej.


[![Dodaj przerwanie wszystkich produktów przycisk kontrolka sieci Web do FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Rysunek 15**: Dodaj przerwanie wszystkich produktów Web kontrolkę przycisku do FormView `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Po kliknięciu przycisku wizytę użytkownika, ensues strony odświeżania strony i FormView [ `ItemCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) uruchamiany. Do wykonania kodu niestandardowego, w odpowiedzi na ten przycisk zostanie kliknięty, możemy utworzyć program obsługi zdarzeń dla tego zdarzenia. Zrozumienie, jednak, że `ItemCommand` zdarzenia generowane, gdy *żadnych* kliknięciu formantu przycisku, LinkButton lub ImageButton sieci Web w widoku FormView. Oznacza to, że gdy użytkownik przesunie się z jednej strony do innego w widoku FormView, `ItemCommand` generowane zdarzenie; samo, gdy użytkownik kliknie przycisk Nowy, Edycja, lub usunąć w widoku FormView, który obsługuje wstawiania, aktualizowania lub usuwania.

Ponieważ `ItemCommand` wyzwalane niezależnie od tego, jakie przycisku, w przypadku obsługi potrzebujemy możliwość określenia, jeśli kliknięto przycisk przerwanie wszystkich produktów lub był niektórych inny przycisk. W tym celu możemy ustawić formantu przycisku Web `CommandName` niektóre identyfikujące wartości dla właściwości. Po kliknięciu przycisku, to `CommandName` wartość została przekazana do `ItemCommand` program obsługi zdarzeń, co pozwala na określenie, czy przycisk przerwanie wszystkich produktów kliknięto element button. Ustaw przerwanie wszystkich produktów przycisku `CommandName` DiscontinueProducts dla właściwości.

Na koniec Użyjmy okno dialogowe Potwierdź po stronie klienta do zapewnienia, że użytkownik chce naprawdę zaprzestania produkty wybranego dostawcy. Jak widzieliśmy w [Dodawanie potwierdzenie po stronie klienta podczas usuwania](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) samouczek, można to zrobić z bitowego języka JavaScript. W szczególności ustawioną właściwość OnClientClick formantu przycisku sieci Web `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Po wprowadzeniu tych zmian, składni deklaratywnej FormView powinna wyglądać następująco:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla FormView `ItemCommand` zdarzeń. W tej obsłudze zdarzeń należy najpierw określić, czy został kliknięty przycisk przerwanie wszystkich produktów. Jeśli tak, jeśli chcesz utworzyć wystąpienia `ProductsBLL` klasy i Wywołaj jej `DiscontinueAllProductsForSupplier(supplierID)` metody, przekazując `SupplierID` wybranego FormView:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Należy pamiętać, że `SupplierID` z bieżącego dostawcy wybranego w widoku FormView można uzyskać dostęp za pomocą FormView [ `SelectedValue` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Właściwość zwraca pierwsze dane klucza wartości dla rekordu, będzie wyświetlany w widoku FormView. FormView [ `DataKeyNames` właściwości](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), co oznacza danych pól, z których dane wartości klucza są pobierane z, automatycznie ustawiono `SupplierID` przez program Visual Studio podczas wiązania elementu ObjectDataSource FormView w kroku 2.

Z `ItemCommand` obsługi zdarzeń utworzony, Poświęć chwilę, aby przetestować strony. Przejdź do Cooperativa de Quesos dostawcy "Las Cabras" (jest piąty dostawcy w widoku FormView dla mnie). Ten dostawca zawiera dwa produkty, Queso Cabrales i Queso Manchego La Pastora, które są *nie* przerywane.

Załóżmy, że Cooperativa de Quesos "Las Cabras" wykroczyła poza biznesowych i w związku z tym jego produkty są wycofywane. Kliknij przycisk przerwanie przycisk wszystkie produkty. Spowoduje to wyświetlenie okna dialogowego Potwierdź po stronie klienta (zobacz rysunek 16).


[![Cooperativa de Quesos Las Cabras dostaw dwóch Active produktów](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Rysunek 16**: Cooperativa de Quesos Las Cabras dostaw dwie aktywne produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Kliknięcie przycisku OK w oknie dialogowym potwierdź po stronie klienta, przesyłania formularza będzie kontynuowana, powoduje odświeżenie strony, w którym FormView `ItemCommand` będą wyzwalać zdarzeń. Program obsługi zdarzeń, utworzyliśmy następnie wykona, wywoływania `DiscontinueAllProductsForSupplier(supplierID)` — metoda i zaprzestanie Queso Cabrales i Queso Manchego La Pastora produktów.

Wyłączenie stan widoku GridView widoku GridView jest trwa odbitych odpowiedni magazyn danych na każdym ogłaszania zwrotnego i w związku z tym zostanie natychmiast zaktualizowany do uwzględnienia, że te dwa produkty są teraz zaprzestać (patrz rysunek 17). Jeśli jednak został wyłączony stan widoku w widoku GridView, należy ręcznie ponownie powiązać dane do widoku GridView po wprowadzeniu tej zmiany. W tym celu po prostu wywoływania w widoku GridView `DataBind()` metody natychmiast po wywołaniu `DiscontinueAllProductsForSupplier(supplierID)` metody.


[![Po kliknięciu przycisku przerwanie wszystkich produktów produkty dostawcy s są zaktualizowane odpowiednio](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Rysunek 17**: po kliknięciu przycisku przerwanie wszystkich produktów produkty dostawcy są zaktualizowane odpowiednio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Krok 6: Tworzenie przeciążenia UpdateProduct warstwy logiki biznesowej dostosowywania cena produktu

Podobnie jak przerwanie wszystkich produktów przycisk w formancie FormView, aby dodawanie przycisków do zwiększania i zmniejszania cen produktu w widoku GridView należy najpierw dodać odpowiednie metody Warstwa dostępu do danych i warstwy logiki biznesowej. Ponieważ firma Microsoft już metodę, która aktualizuje produktu pojedynczy wiersz w warstwę DAL, firma Microsoft udostępnia takie funkcje, tworząc nowe przeciążenie dla elementu `UpdateProduct` metoda logiki warstwy Biznesowej.

Nasze przeszłości `UpdateProduct` przeciążenia podjęte w kombinację jako wartości skalarne wejściowe pola produktów, a następnie zostały zaktualizowane tylko tych pól dla określonego produktu. Dla tego przeciążenia firma Microsoft będzie nieznacznie różnić ten standard i zamiast tego są przekazywane w ramach produktu `ProductID` i procentowo dostosować `UnitPrice` (w przeciwieństwie do przekazywania w nowym, dostosowane `UnitPrice` sam). Takie podejście uprości kod należy zapisać w klasie związanej z kodem strony ASP.NET, ponieważ nie trzeba było odblokowane określeniu bieżącego produktu `UnitPrice`.

`UpdateProduct` Przeciążenia dla tego samouczka przedstawiono poniżej:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

To przeciążenie pobiera informacje o produkcie określony przez warstwę DAL `GetProductByProductID(productID)` metody. Następnie sprawdza, czy produkt `UnitPrice` przypisano bazy danych `NULL` wartość. Jeśli tak jest, ceny pozostanie bez zmian. Jeśli jednak ma wartość inną niż`NULL` `UnitPrice` wartości, metoda aktualizacji produktu `UnitPrice` określony procent (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Krok 7: Dodawanie wzrostu i przyciski zmniejsza się do widoku GridView

Element GridView (i widoku DetailsView) zarówno składają się z kolekcji pól. Oprócz BoundFields CheckBoxFields i TemplateFields program ASP.NET zawiera ButtonField, który, jak jego nazwa wskazuje, renderuje jako kolumny za pomocą przycisku, LinkButton lub ImageButton dla każdego wiersza. Podobnie jak FormView, klikając pozycję *żadnych* przycisku w widoku GridView przycisków stronicowania, Edytuj lub usuń przycisków przycisków sortowania i itd powoduje odświeżenie strony i zgłasza w widoku GridView [ `RowCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Ma ButtonField `CommandName` właściwość, która przypisuje wartość określona dla każdego z jego przycisków `CommandName` właściwości. Jak FormView, `CommandName` wartość jest używana przez `RowCommand` obsługi zdarzeń w celu określenia, który przycisk został kliknięty.

Dodajmy dwa nowe ButtonFields do widoku GridView, jeden z tekst przycisku cen 10%, a druga z tekstem cen -10%. Aby dodać te ButtonFields, kliknięcie łącza Edytuj kolumny w widoku GridView tagów inteligentnych, wybierz typ pola ButtonField z listy w lewym górnym rogu i kliknij przycisk Dodaj.


![Dodaj dwa ButtonFields do widoku GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Rysunek 18**: Dodaj dwa ButtonFields do widoku GridView


Przenieś dwóch ButtonFields, aby były wyświetlane jako pierwsze dwa pola w widoku GridView. Następnie należy ustawić `Text` właściwości tych dwóch ButtonFields do 10% ceny i cen -10% i `CommandName` właściwości IncreasePrice i DecreasePrice, odpowiednio. Domyślnie ButtonField renderuje jego kolumnę przycisków jako LinkButtons. Można to zmienić, jednak za pomocą ButtonField [ `ButtonType` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Teraz mieć tych dwóch ButtonFields renderowane jako regularne przycisków; w związku z tym ustawić `ButtonType` właściwości `Button`. Rysunek 19 wyświetla pola okno dialogowe po dokonaniu zmiany; Po jest deklaratywne znaczników w widoku GridView.


![Skonfiguruj tekst ButtonFields, CommandName i właściwość ButtonType właściwości](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Rysunek 19**: Konfigurowanie ButtonFields `Text`, `CommandName`, i `ButtonType` właściwości


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Z tych ButtonFields utworzone, ostatnim krokiem jest tworzenie obsługi zdarzeń dla w widoku GridView `RowCommand` zdarzeń. Ten program obsługi zdarzeń, jeśli uruchamiane, ponieważ albo ceny 10% lub cen -10% przyciski kliknięty, należy określić `ProductID` dla wiersza, którego przycisk został kliknięty, a następnie wywołaj `ProductsBLL` klasy `UpdateProduct` metody, przekazując odpowiednie `UnitPrice` korekty procent wraz z programem `ProductID`. Poniższy kod wykonuje te zadania:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Aby określić `ProductID` wiersza których cena 10% lub cen -10% przycisk został kliknięty, należy zapoznać się w widoku GridView `DataKeys` kolekcji. Ta kolekcja zawiera wartości pola określone w `DataKeyNames` właściwości dla każdego wiersza w widoku GridView. Ponieważ w widoku GridView `DataKeyNames` właściwości ustawiono ProductID przez program Visual Studio podczas wiązania elementu ObjectDataSource widoku GridView, `DataKeys(rowIndex).Value` zapewnia `ProductID` dla określonego *rowIndex*.

ButtonField automatycznie przekazuje w *rowIndex* wiersza, którego przycisk został kliknięty przy użyciu `e.CommandArgument` parametru. W związku z tym aby określić `ProductID` wiersza których cena 10% lub cen -10% przycisk został kliknięty, używamy: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Jako przycisk przerwanie wszystkich produktów wyłączenie stan widoku GridView, widoku GridView jest trwa odbitych do odpowiedni magazyn danych na każdym ogłaszania zwrotnego i w związku z tym zostaną natychmiast zaktualizowane, aby odzwierciedlić zmiany ceny, która występuje z kliknięcie jeden z przycisków. Jeśli jednak został wyłączony stan widoku w widoku GridView, należy ręcznie ponownie powiązać dane do widoku GridView po wprowadzeniu tej zmiany. W tym celu po prostu wywoływania w widoku GridView `DataBind()` metody natychmiast po wywołaniu `UpdateProduct` metody.

Rysunek 20 wyświetla stronę podczas wyświetlania produktów dostarczonych przez Homestead Kelly babcia. Rysunek 21 pokazuje wyniki po cen 10% kliknięto przycisk dwukrotnie dla rozkładu Boysenberry babcia firmy i przycisku % cenę -10 raz dla Northwoods Cranberry sos.


[![Widoku GridView obejmuje cen 10% i cen -10% przycisków](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Rysunek 20**: cen obejmuje GridView 10% i cen -10% przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Ceny produktu pierwszy i trzeci zostały zaktualizowane przez cen 10% i cen -10% przycisków](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Rysunek 21**: cen w pierwszy i trzeci produktu zostały zaktualizowane przez cen 10% i cen -10% przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> Element GridView (i widoku DetailsView) ma także przyciski LinkButtons i dodane do ich TemplateFields ImageButtons. Zgodnie z elementu BoundField, po kliknięciu tych przycisków będą powodować odświeżenie strony, który wywołał w widoku GridView `RowCommand` zdarzeń. Gdy dodawanie przycisków w pole TemplateField, jednak przycisku `CommandArgument` nie jest automatycznie równa indeks wiersza, ponieważ jest on przy użyciu ButtonFields. Jeśli musisz określić indeks wiersza przycisku, który został kliknięty w `RowCommand` program obsługi zdarzeń, należy ręcznie ustawić przycisku `CommandArgument` właściwości w jego składni deklaratywnej wewnątrz TemplateField, przy użyciu kodu, takich jak:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.


## <a name="summary"></a>Podsumowanie

Formanty widoku GridView widoku DetailsView i FormView wszystkie mogą obejmować przycisków, LinkButtons lub ImageButtons. Przyciski, po kliknięciu powoduje odświeżenie strony i zgłosi `ItemCommand` zdarzeń w formantach FormView i widoku DetailsView i `RowCommand` zdarzenia w widoku GridView. Te dane formantów sieci Web ma wbudowaną funkcję obsługi typowych akcji związanych z polecenia, takich jak usuwanie lub edytowania rekordów. Jednak firma Microsoft może również użyj przycisków, po kliknięciu, odpowiadać, podając swój własny niestandardowy kod wykonywania.

W tym celu należy utworzyć programu obsługi zdarzeń dla `ItemCommand` lub `RowCommand` zdarzeń. W tej obsłudze zdarzeń najpierw sprawdzamy przychodzącego `CommandName` wartość, aby ustalić, który przycisk został kliknięty, a następnie podjąć odpowiednie działania niestandardowego. W tym samouczku widzieliśmy jak za pomocą przycisków i ButtonFields przerwanie wszystkich produktów dla określonego dostawcy lub zwiększyć lub zmniejszyć cen określonego produktu przy 10%.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](adding-and-responding-to-buttons-to-a-gridview-vb.md)
