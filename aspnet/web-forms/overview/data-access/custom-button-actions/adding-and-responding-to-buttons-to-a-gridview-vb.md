---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Dodawanie przycisków i reagowanie na w kontrolce GridView (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku omówimy sposób dodawania niestandardowych przycisków do szablonu i pola formantu GridView lub DetailsView. W szczególności firma Microsoft będzie lecenia...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0834d43f95bd19fffb603dcde640714bd779fd80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755467"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Dodawanie przycisków i reagowanie na w kontrolce GridView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) lub [Pobierz plik PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> W tym samouczku omówimy sposób dodawania niestandardowych przycisków do szablonu i pola formantu GridView lub DetailsView. W szczególności utworzymy interfejs, który ma FormView, która pozwala użytkownikowi na stronie za pośrednictwem dostawców.


## <a name="introduction"></a>Wprowadzenie

Gdy wiele scenariuszy raportowania obejmują dostęp tylko do odczytu do danych raportu, on s nie nietypowe w przypadku raportów uwzględnić możliwość wykonywania akcji na podstawie wyświetlanych danych. Zwykle to zaangażowane Dodawanie kontrolki przycisku, element LinkButton lub ImageButton w sieci Web z każdego rekordu w raporcie, po kliknięciu powoduje odświeżenie strony i wywołuje kodu po stronie serwera. Edytowanie i usuwanie danych na podstawie rekordu, rekord jest najbardziej typowym przykładem. W rzeczywistości, ponieważ był widoczny, począwszy od [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczek, edytowania i usuwania jest tak często, że kontrolki GridView DetailsView i FormView może obsługiwać takie funkcje bez potrzeby napisania choćby jednego wiersza kodu.

Ponadto można edytować i usuwać przycisków kontrolki GridView, DetailsView i FormView kontrolki mogą również obejmować przycisków, LinkButtons lub ImageButtons, po kliknięciu wykonania niestandardowej logiki biznesowej po stronie serwera. W tym samouczku omówimy sposób dodawania niestandardowych przycisków do szablonu i pola formantu GridView lub DetailsView. W szczególności utworzymy interfejs, który ma FormView, która pozwala użytkownikowi na stronie za pośrednictwem dostawców. Dla danego dostawcy FormView wyświetli informacje o dostawcy wraz z formantu sieci Web przycisk, który, po kliknięciu spowoduje oznaczenie wszystkich ich skojarzone produkty jak wycofana. Ponadto GridView zawiera listę tych produktów, dostarczone przez wybranego dostawcę, przy czym każdy wiersz zawierający zwiększyć ceny i rabatów cena przyciski, kliknięciu zwiększyć lub zmniejszyć produkt s `UnitPrice` przy 10% (patrz rysunek 1).


[![FormView i GridView zawiera przyciski umożliwiające wykonywanie akcji niestandardowych](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Rysunek 1**: zarówno FormView i GridView zawiera przyciski, wykonywać akcje niestandardowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Krok 1: Dodawanie stron sieci Web z samouczka przycisku

Przed przedstawiony sposób dodawania niestandardowej przyciski umożliwiają s najpierw Poświęć chwilę, aby utworzyć strony ASP.NET w projekcie naszej witryny sieci Web, które będą potrzebne w tym samouczku. Rozpocznij od dodania nowy folder o nazwie `CustomButtons`. Następnie dodaj następujące dwie strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `CustomButtons.aspx`


![Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Rysunek 2**: Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków


Podobnie jak w przypadku innych folderów `Default.aspx` w `CustomButtons` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Na koniec Dodaj strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po stronicowanie i sortowanie `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczków.


![Mapa witryny zawiera teraz wpis dla samouczka przyciski niestandardowe](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Rysunek 4**: mapy witryny zawiera teraz wpis dla samouczka przyciski niestandardowe


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Krok 2: Dodawanie FormView, zawierającego dostawców

Pozwól s Rozpoczynanie pracy z tym samouczkiem, dodając FormView, zawierającego dostawców. Tak jak to opisano we wprowadzeniu, to FormView umożliwi użytkownikowi strony za pomocą dostawców, przedstawiający produktów, dostarczone przez dostawcę w GridView. Ponadto ta FormView będzie zawierać przycisk, po kliknięciu spowoduje oznaczenie wszystkich produktów dostawcy s jak wycofana. Zanim firma dotyczą osoby za pomocą dodawania niestandardowy przycisk do widoku FormView chętnie s najpierw po prostu Utwórz FormView, tak, aby wyświetlał informacje o dostawcach.

Zacznij od otwarcia `CustomButtons.aspx` stronie `CustomButtons` folderu. Na stronie Dodaj kontrolce FormView, przeciągając go z przybornika do projektanta i ustaw jego `ID` właściwość `Suppliers`. Za pomocą tagu inteligentnego s FormView wybrać opcję utworzenia nowego elementu ObjectDataSource, o nazwie `SuppliersDataSource`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Rysunek 5**: Utwórz nowy o nazwie elementu ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Konfigurowanie tej nowej kontrolki ObjectDataSource w taki sposób, że wysyła zapytanie z `SuppliersBLL` klasy s `GetSuppliers()` — metoda (patrz rysunek 6). Ponieważ ta FormView nie zapewnia interfejs do aktualizacji dostawcy informacje, wybierz opcję (Brak) z listy rozwijanej na karcie aktualizacji.


[![Konfigurowanie źródła danych, aby użyć klasy SuppliersBLL s GetSuppliers() — metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Rysunek 6**: Konfigurowanie źródła danych, aby użyć `SuppliersBLL` klasy s `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Po skonfigurowaniu kontrolki ObjectDataSource, program Visual Studio wygeneruje `InsertItemTemplate`, `EditItemTemplate`, i `ItemTemplate` dla widoku FormView. Usuń `InsertItemTemplate` i `EditItemTemplate` i modyfikować `ItemTemplate` tak, aby wyświetlał tylko dostawcy s firmy nazwa i numer telefonu. Na koniec Włącz obsługę stronicowania w widoku FormView, zaznaczając pole wyboru Włącz stronicowania w tagu inteligentnego (lub przez ustawienie jego `AllowPaging` właściwość `True`). Po wprowadzeniu tych zmian znaczniki deklaratywne s strony powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Rysunek nr 7 przedstawia stronę CustomButtons.aspx podczas wyświetlania za pośrednictwem przeglądarki.


[![FormView wyświetla pola CompanyName i telefonu z aktualnie wybranego dostawcy](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Rysunek 7**: Wyświetla listę FormView `CompanyName` i `Phone` pola z aktualnie wybrany dostawca ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Krok 3. Dodawanie GridView, który zawiera listę produktów s wybranego dostawcy

Aby dodać przycisk przerwanie wszystkich produktów w szablonie s FormView umożliwialiśmy s najpierw dodać GridView poniżej FormView, który zawiera listę produktów, dostarczone przez wybranego dostawcę. Aby to osiągnąć, dodać GridView do strony, ustaw jego `ID` właściwości `SuppliersProducts`, i dodać nowe kontrolki ObjectDataSource, o nazwie `SuppliersProductsDataSource`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Rysunek 8**: Utwórz nowy o nazwie elementu ObjectDataSource `SuppliersProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Konfigurowanie tej kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL s `GetProductsBySupplierID(supplierID)` — metoda (patrz rysunek 9). Podczas tego widoku GridView będzie zezwalał dla cena produktu s, należy dostosować, wygrał t się przy użyciu wbudowanych, edytowanie lub usuwanie funkcji z kontrolki GridView. W związku z tym firma Microsoft może określić listy rozwijanej (Brak) ObjectDataSource s karty aktualizacji, WSTAWIANIA i usuwania.


[![Konfigurowanie źródła danych, aby użyć klasy ProductsBLL s GetProductsBySupplierID(supplierID) — metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Rysunek 9**: Konfigurowanie źródła danych, aby użyć `ProductsBLL` klasy s `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Ponieważ `GetProductsBySupplierID(supplierID)` metoda akceptuje parametr wejściowy, monituje Kreator ObjectDataSource nam źródła wartości tego parametru. Aby przekazać `SupplierID` z FormView wartości, zmień wartość na liście rozwijanej źródła parametru do kontroli i listy rozwijanej ControlID do `Suppliers` (identyfikator FormView utworzony w kroku 2).


[![Wskazuje, że IDDostawcy parametr, powinien pochodzić z kontrolą FormView dostawcy](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Na rysunku nr 10**: wskazują, że *`supplierID`* parametr powinien pochodzić z `Suppliers` kontrolki widoku FormView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Po zakończeniu pracy Kreatora ObjectDataSource widoku GridView będzie zawierać elementu BoundField lub CheckBoxField dla każdego pola danych s produktu. Umożliwiają s trim to w dół do wyświetlenia po prostu z `ProductName` i `UnitPrice` BoundFields wraz z `Discontinued` CheckBoxField; ponadto umożliwiają s format `UnitPrice` elementu BoundField taki sposób, że jego tekstu jest w formacie waluty. Twoje GridView i `SuppliersProductsDataSource` ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać podobnie do następujących znaczników:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

Na tym etapie Nasz samouczek dotyczący wyświetla raport wzorzec/szczegół, umożliwiając użytkownikowi wybrać dostawcę z FormView u góry i Wyświetl produkty dostarczonych przez tego dostawcę przy użyciu GridView u dołu. Na ilustracji 11 pokazano zrzut ekranu strony podczas wybierania dostawcy handlowców Tokio z FormView.


[![Produkty s wybrany dostawca są wyświetlane w widoku GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Rysunek 11**: wybrany dostawca s produkty są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Krok 4: Tworzenie warstwy DAL i LOGIKI metody, aby zaprzestać wszystkich produktów dla dostawcy

Zanim firma Microsoft może dodać przycisk do FormView, po kliknięciu zaprzestaje wszystkich produktów dostawcy s, najpierw trzeba dodać metodę do warstwy DAL i LOGIKI, która wykonuje tę akcję. W szczególności, ta metoda będą miały nazwę nadaną `DiscontinueAllProductsForSupplier(supplierID)`. Po kliknięciu s FormView przycisk firma Microsoft będzie wywołać tę metodę w warstwy logiki biznesowej, przekazywanie wybranego dostawcy s `SupplierID`; LOGIKI będzie wywoływać w dół do odpowiedniej metody warstwy dostępu do danych, która będzie wystawiać `UPDATE` instrukcję Baza danych zaprzestaje produkty określonego dostawcy s.

Jak wykonaliśmy w naszych poprzednich samouczkach użyjemy podejście od dołu do góry, począwszy od tworzenia metody DAL, a następnie metoda LOGIKI i na koniec wdrażanie funkcji na stronie ASP.NET. Otwórz `Northwind.xsd` wpisana zestawu danych w `App_Code/DAL` folderze i Dodaj nową metodę `ProductsTableAdapter` (kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie). Ten sposób pojawi się Kreator konfiguracji zapytania TableAdapter, który przedstawia nam historię proces dodawania nowej metody. Rozpocznij, wskazując, że metoda nasze warstwy DAL będzie używać instrukcji SQL zapytań ad-hoc.


[![Utwórz metodę DAL przy użyciu instrukcji SQL zapytań Ad-Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Rysunek 12**: Utwórz za pomocą metody DAL instrukcji SQL zapytań Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Następnie kreator wyświetli nam tego rodzaju zapytanie w celu utworzenia. Ponieważ `DiscontinueAllProductsForSupplier(supplierID)` metody będą musieli zaktualizować `Products` tabeli bazy danych, ustawienie `Discontinued` pola na wartość 1 dla wszystkich produktów, dostarczone przez określony *`supplierID`*, należy utworzyć kwerendę, która aktualizuje dane.


[![Wybierz typ zapytania aktualizacji](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Rysunek 13**: Wybierz typ zapytania aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


Na następnym ekranie kreatora zawiera istniejące s TableAdapter `UPDATE` instrukcję, która aktualizuje wszystkie pola zdefiniowane w `Products` DataTable. Zastąp ten tekst zapytania następującą instrukcję:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Po wprowadzeniu tego zapytania i klikając przycisk Dalej, ostatnim ekranie Kreator poprosi o podanie nowej nazwy metody w s Użyj `DiscontinueAllProductsForSupplier`. Ukończ pracę kreatora, klikając przycisk Zakończ. Po powrocie do Projektanta obiektów DataSet powinna zostać wyświetlona nowa metoda w `ProductsTableAdapter` o nazwie `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nazwa nowego DiscontinueAllProductsForSupplier DAL — metoda](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Rysunek 14**: nadaj nazwę nowej metody DAL `DiscontinueAllProductsForSupplier` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Za pomocą `DiscontinueAllProductsForSupplier(supplierID)` metodą utworzone w warstwie dostępu do danych, naszym kolejnym krokiem jest utworzenie `DiscontinueAllProductsForSupplier(supplierID)` metody w warstwy logiki biznesowej. Aby to zrobić, otwórz `ProductsBLL` klasy plików i Dodaj następujący kod:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Ta metoda po prostu wywołuje w dół do `DiscontinueAllProductsForSupplier(supplierID)` metody w DAL, przekazując wzdłuż podane *`supplierID`* wartość parametru. Gdyby wszystkich reguł biznesowych, które dozwolone tylko dostawcy s produktów jako nieobsługiwane w pewnych okolicznościach, zasady te powinny być zrealizowane w tym miejscu w LOGIKI.

> [!NOTE]
> W odróżnieniu od `UpdateProduct` przeciążeń w `ProductsBLL` klasy `DiscontinueAllProductsForSupplier(supplierID)` nie ma sygnatury metody `DataObjectMethodAttribute` atrybutu (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Wyklucza to `DiscontinueAllProductsForSupplier(supplierID)` metodę z listy rozwijanej kontrolki ObjectDataSource s s kreatora Konfigurowanie źródła danych, na karcie aktualizacji. Czy mogę ve pominięto tego atrybutu, ponieważ firma Microsoft będzie wywoływać `DiscontinueAllProductsForSupplier(supplierID)` metody bezpośrednio z programu obsługi zdarzeń w naszej stronie ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Krok 5. Dodawanie przerwanie wszystkich produktów przycisku FormView

Za pomocą `DiscontinueAllProductsForSupplier(supplierID)` ukończyć metoda w LOGIKI i warstwy DAL, ostatnim krokiem dodawania możliwość przerwanie wszystkich produktów dla wybranego dostawcy jest dodać kontrolkę przycisku w sieci Web do FormView s `ItemTemplate`. S umożliwiają dodawanie przycisku poniżej numer telefonu dostawcy s tekstem przycisku przerwanie wszystkich produktów i `ID` wartość właściwości `DiscontinueAllProductsForSupplier`. Można dodać tej kontrolki przycisku w sieci Web za pomocą projektanta, klikając link Edytuj szablony w tagu inteligentnego s FormView (zobacz rysunek 15), lub bezpośrednio za pomocą składni deklaratywnej.


[![Dodaj przerwanie wszystkich kontrolki sieci Web do właściwości ItemTemplate s FormView produktów przycisku](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Rysunek 15**: Dodaj przerwanie wszystkich produktów w sieci Web kontrolkę przycisku do FormView s `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Po kliknięciu przycisku, odwiedzając użytkownika, ensues strony odświeżenie strony i FormView s [ `ItemCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) uruchamiany. Aby wykonać kod niestandardowy w odpowiedzi do tego przycisku, w momencie kliknięcia, możemy utworzyć program obsługi zdarzeń dla tego zdarzenia. Zrozumienie, jednak, że `ItemCommand` zdarzenia generowane, gdy *wszelkie* kliknięciu formantu przycisku, element LinkButton lub ImageButton w sieci Web w widoku FormView. Oznacza to, że gdy użytkownik przesuwa z jednej strony do drugiego w FormView `ItemCommand` generowane zdarzenie; ten sam efekt, gdy użytkownik kliknie nowy, edytować lub usunąć w kontrolce FormView, który obsługuje wstawiania, aktualizowania lub usuwania.

Ponieważ `ItemCommand` wyzwalane niezależnie od tego, jakie przycisku, w przypadku obsługi potrzebujemy możliwość określenia, jeśli został kliknięty przycisk przerwanie wszystkich produktów lub był niektóre inny przycisk. Aby to osiągnąć, możemy ustawić kontrolki przycisku w sieci Web s `CommandName` właściwości na wartość identyfikacyjne. Po kliknięciu przycisku, to `CommandName` wartość jest przekazywana do `ItemCommand` program obsługi zdarzeń, co pozwala na określenie, czy przycisk przerwanie wszystkich produktów kliknięto przycisk. Ustaw s przerwanie wszystkich przycisku produkty `CommandName` właściwość DiscontinueProducts.

Ponadto poinformuj s użyć okna dialogowego Potwierdź po stronie klienta, aby upewnić się, że użytkownik chce naprawdę przestać produktów wybranego dostawcy s. Jak widzieliśmy w [dodawanie potwierdzenia po stronie klienta podczas usuwania](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) samouczków, można to zrobić przy użyciu bitowego języka JavaScript. W szczególności ustaw właściwość OnClientClick s kontrolki przycisku w sieci Web `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Po wprowadzeniu tych zmian, składni deklaratywnej s FormView powinien wyglądać następująco:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla FormView s `ItemCommand` zdarzeń. W tej obsługi zdarzeń, musimy najpierw ustalić, czy został kliknięty przycisk przerwanie wszystkich produktów. Jeśli tak, chcemy utworzyć wystąpienie `ProductsBLL` klasy i wywoływanie jej `DiscontinueAllProductsForSupplier(supplierID)` metody, przekazując `SupplierID` wybranego FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Należy pamiętać, że `SupplierID` z bieżącego dostawcy wybranego w widoku FormView jest możliwy za pomocą FormView s [ `SelectedValue` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Właściwość zwraca pierwsze dane klucza dla rekordu, są wyświetlane w widoku FormView. FormView s [ `DataKeyNames` właściwość](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), co oznacza danych, pola, z których dane wartości klucza są ściągane z, została automatycznie ustawiona na `SupplierID` przez program Visual Studio podczas tworzenia wiązania kontrolki ObjectDataSource do tyłu FormView w kroku 2.

Za pomocą `ItemCommand` utworzony, program obsługi zdarzeń Poświęć chwilę, aby przetestować stronę. Przejdź do Cooperativa de Quesos dostawcy "Las Cabras" (go s piąty dostawcy w FormView dla mnie). Ten dostawca zawiera dwa produkty, Queso Cabrales i Queso Manchego La Pastora, które są *nie* wycofane.

Wyobraź sobie, że Cooperativa de Quesos "Las Cabras" stała się działalność i dlatego jego produkty mają zostać zakończona. Kliknij przycisk przerwanie przycisk wszystkie produkty. Spowoduje to wyświetlenie okna dialogowego Potwierdź po stronie klienta (zobacz rysunek 16).


[![Cooperativa de Quesos Las Cabras dostarcza dwóch aktywne produkty](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Rysunek 16**: Cooperativa de Quesos Las Cabras dostarcza dwóch aktywne produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Po kliknięciu przycisku OK w oknie dialogowym po stronie klienta Potwierdź przesyłania formularza będzie kontynuowana, co powoduje odświeżenie strony, w którym FormView s `ItemCommand` zdarzenia będą uruchamiane. Program obsługi zdarzeń, utworzyliśmy następnie wykona, wywołując `DiscontinueAllProductsForSupplier(supplierID)` metody i zaprzestanie Queso Cabrales i Queso Manchego La Pastora produktów.

Wyłączenie stan widoku GridView s widoku GridView jest on odbitych źródłowy magazyn danych na każdy odświeżenie strony i w związku z tym zostanie natychmiast zaktualizowany, aby odzwierciedlić, że te dwa produkty są już obsługiwany (zobacz rysunek 17). Jeśli jednak nie wyłączono stan widoku w widoku GridView, należy ręcznie ponownie powiązać dane do widoku GridView po wprowadzeniu tej zmiany. W tym celu po prostu wywołania GridView s `DataBind()` metoda natychmiast po wywołaniu `DiscontinueAllProductsForSupplier(supplierID)` metody.


[![Po kliknięciu przycisku przerwanie wszystkich produktów produkty dostawcy s są odpowiednio aktualizowane](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Rysunek 17**: po kliknięciu przycisku przerwanie wszystkich produktów produkty dostawcy s są aktualizowane w związku z tym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Krok 6: Tworzenie przeciążenia UpdateProduct w warstwy logiki biznesowej dostosowywania cena s produktu

Podobnie jak przerwanie wszystkich produktów klikając przycisk w widoku FormView, aby można było dodać przyciski do zwiększania i zmniejszania cena produktu w widoku GridView należy najpierw dodać odpowiednie metody warstwy dostępu do danych i warstwy logiki biznesowej. Ponieważ mamy już metodę, która aktualizuje wiersz jednego produktu w warstwy DAL, firma Microsoft zapewnia takie funkcje, tworząc nowe przeciążenia dla `UpdateProduct` metody w LOGIKI.

Nasze przeszłości `UpdateProduct` przeciążenia miały w niektórych kombinacji pola produktów jako wartości skalarne danych wejściowych, a następnie zostały zaktualizowane tylko te pola dla określonego produktu. Dla tego przeciążenia utworzymy się nieco różnić od tego standardu i zamiast tego Przekaż produkt s `ProductID` i wartość procentowa o którą należy dostosować `UnitPrice` (w przeciwieństwie do przekazywania w nowym, dostosowane `UnitPrice` sam). To podejście upraszcza kod, należy napisać w klasie CodeBehind strony ASP.NET, ponieważ firma Microsoft don było odblokowane za pomocą określania bieżącego produktu s `UnitPrice`.

`UpdateProduct` Przeciążenia dla tego samouczka znajdują się poniżej:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

To przeciążenie pobiera informacje o określony produkt za pośrednictwem DAL s `GetProductByProductID(productID)` metody. Następnie sprawdza, czy produkt s `UnitPrice` przypisano bazę danych `NULL` wartość. Jeśli tak jest, cena pozostanie niezmieniona. Jeśli jednak ma wartość inną niż`NULL` `UnitPrice` wartość metody aktualizacji produktu s `UnitPrice` o określony procent (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Krok 7: Dodawanie przycisków spadek i zwiększenia widoku GridView

Kontrolki GridView (i DetailsView) zarówno składają się z kolekcji pól. Oprócz BoundFields, CheckBoxFields i kontrolek TemplateField program ASP.NET zawiera ButtonField, który, jak sama nazwa wskazuje, renderowany jako kolumny za pomocą przycisku, element LinkButton lub ImageButton dla każdego wiersza. Podobnie jak FormView, klikając *wszelkie* znajdującego się w GridView przyciski stronicowania, Edytuj lub usuń przycisków, przyciski sortowania i tak dalej powoduje odświeżenie strony i zgłasza GridView s [ `RowCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

Ma ButtonField `CommandName` właściwość, która przypisuje określoną wartość do wszystkich jej przyciski `CommandName` właściwości. Za pomocą FormView `CommandName` wartość jest używana przez `RowCommand` program obsługi zdarzeń w celu określenia, który przycisk został kliknięty.

Umożliwiają s Dodaj dwa nowe ButtonFields do kontrolki GridView, jeden z tekstu przycisku Cena 10%, a druga z tekstem Cena -10%. Aby dodać te ButtonFields, kliknij link Edytuj kolumny z tagu inteligentnego s GridView, wybierz typ pola ButtonField z listy w lewym górnym rogu i kliknij przycisk Dodaj.


![Dodaj dwa ButtonFields do widoku GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Rysunek 18**: dodanie dwóch ButtonFields do widoku GridView


Przenieś dwóch ButtonFields, aby były wyświetlane jako pierwsze dwa pola GridView. Następnym etapem jest skonfigurowanie `Text` właściwości tych dwóch ButtonFields do wyceny 10% i Cena -10% i `CommandName` właściwości IncreasePrice i DecreasePrice, odpowiednio. Domyślnie ButtonField renderuje kolumny przycisków jako LinkButtons. Można to zmienić, jednak za pośrednictwem ButtonField s [ `ButtonType` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Pozwól mają te dwa ButtonFields renderowane jako przyciski regularne; s w związku z tym, ustaw `ButtonType` właściwość `Button`. Rysunek 19 znajdują się pola dialogowego po dokonaniu zmiany; Po jest oznaczeniu deklaracyjnym s GridView.


![Skonfiguruj tekst ButtonFields, CommandName i właściwości ButtonType właściwości](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Rysunek 19**: Konfigurowanie ButtonFields `Text`, `CommandName`, i `ButtonType` właściwości


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Za pomocą tych ButtonFields utworzone, ostatnim krokiem jest utworzyć program obsługi zdarzeń dla GridView s `RowCommand` zdarzeń. Ta procedura obsługi zdarzeń, jeśli jest uruchamiany, ponieważ albo Cena 10% lub Cena -10% przyciski kliknięty, trzeba określić `ProductID` dla wiersza, w której przycisk został kliknięty, a następnie wywołaj `ProductsBLL` klasy s `UpdateProduct` metody, przekazując odpowiednie `UnitPrice` korekty procent wraz z `ProductID`. Poniższy kod wykonuje następujące zadania:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

W celu określenia `ProductID` wiersza których cena 10% lub Cena -10% przycisk został kliknięty, należy zapoznać się z tym s GridView `DataKeys` kolekcji. Ta kolekcja przechowuje wartości pola określone w parametrze `DataKeyNames` właściwości dla każdego wiersza w widoku GridView. Ponieważ GridView s `DataKeyNames` właściwość została ustawiona na ProductID przez program Visual Studio, podczas tworzenia wiązania kontrolki ObjectDataSource do kontrolki GridView, `DataKeys(rowIndex).Value` zapewnia `ProductID` dla określonego *rowIndex*.

ButtonField automatycznie przekazuje *rowIndex* wiersza, w której przycisk został kliknięty za pośrednictwem `e.CommandArgument` parametru. W związku z tym aby określić `ProductID` wiersza których cena 10% lub Cena -10% przycisk został kliknięty, używamy: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Jako za pomocą przycisku przerwanie wszystkich produktów, wyłączenie stan widoku GridView s, widoku GridView jest on odbitych źródłowy magazyn danych na każdym zwrotu i w związku z tym zostanie natychmiast zaktualizowany w celu odzwierciedlenia zmiany cen, który występuje od kliknięcia jeden z przycisków. Jeśli jednak nie wyłączono stan widoku w widoku GridView, należy ręcznie ponownie powiązać dane do widoku GridView po wprowadzeniu tej zmiany. W tym celu po prostu wywołania GridView s `DataBind()` metoda natychmiast po wywołaniu `UpdateProduct` metody.

Rysunek 20 wyświetla stronę, podczas wyświetlania produktów, dostarczone przez Homestead Kelly babcia. Rysunek 21 przedstawiono wyniki po cenie 10% kliknięto przycisk dwa razy dla firmy babcia Boysenberry rozwijania i przycisku % cenę -10 raz dla Northwoods Cranberry sos.


[![Kontrolki GridView zawiera cenę 10% i Cena -10% przycisków](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Rysunek 20**: cena obejmuje GridView 10% i Cena -10% przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Ceny dla produktu pierwszy i trzeci zostały zaktualizowane przy użyciu Cena 10% i Cena -10% przycisków](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Rysunek 21**: ceny w pierwszy i trzeci produktu zostały zaktualizowane przy użyciu Cena 10% i Cena -10% przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Kontrolki GridView (i DetailsView) może mieć również przyciski LinkButtons i dodane do ich kontrolek TemplateField ImageButtons. Jak za pomocą elementu BoundField, po kliknięciu tych przycisków będzie wywoływać odświeżenie strony, wywoływanie GridView s `RowCommand` zdarzeń. Podczas dodawania przycisków w TemplateField, jednak przycisk s `CommandArgument` nie jest automatycznie zestawiona na indeks wiersza się przy użyciu ButtonFields. Jeśli zachodzi potrzeba określenia indeks wiersza przycisku, który został kliknięty w ramach `RowCommand` procedura obsługi zdarzeń, należy ręcznie skonfigurować przycisk s `CommandArgument` właściwości w jego składni deklaratywnej w ramach TemplateField, przy użyciu kodu, takich jak:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Podsumowanie

Wszystkie kontrolki GridView DetailsView i FormView mogą obejmować przycisków, LinkButtons lub ImageButtons. Przyciski, po kliknięciu powoduje odświeżenie strony i zgłosić `ItemCommand` zdarzeń formantów FormView i DetailsView i `RowCommand` zdarzenia w widoku GridView. Te kontrolki internetowe danych ma wbudowane funkcje, aby obsłużyć typowe akcje związane z polecenia, takie jak usuwanie i edytowanie rekordów. Jednak firma Microsoft może również użyj przycisków, po kliknięciu, elastyczniejsze wykonywania swój własny kod niestandardowy.

W tym celu należy utworzyć program obsługi zdarzeń dla `ItemCommand` lub `RowCommand` zdarzeń. W tej obsługi zdarzeń najpierw sprawdzenie przychodzącego `CommandName` wartość do określenia, który przycisk został kliknięty i wtedy podjąć odpowiednie działania niestandardowe. W tym samouczku widzieliśmy, jak za pomocą przycisków i ButtonFields przestać wszystkich produktów dla określonego dostawcy lub zwiększyć lub zmniejszyć cena konkretnego produktu o 10%.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-and-responding-to-buttons-to-a-gridview-cs.md)
