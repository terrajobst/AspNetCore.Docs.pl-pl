---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: Omówienie wstawiania, aktualizowania i usuwania danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku opisano sposób mapowania Insert() ObjectDataSource, Update(), i Delete() metody do metody LOGIKI klasy, a także jak configu...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 55fab6bb7a1041a14f8734a0d2ae1238b3801149
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752884"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Omówienie wstawiania, aktualizowania i usuwania danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) lub [Pobierz plik PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> W tym samouczku opisano sposób mapowania Insert() ObjectDataSource, Update(), i Delete() metody do metody LOGIKI klasy, a także jak skonfigurować kontrolki GridView DetailsView i FormView do zapewnienia możliwości modyfikacji danych.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczki zbadaliśmy już sposób wyświetlania danych na stronie ASP.NET za pomocą kontrolki GridView DetailsView i FormView. Te kontrolki tylko do pracy dostarczane do ich danych. Zazwyczaj te kontrolki uzyskać dostęp do danych przy użyciu kontroli źródła danych, takich jak kontrolki ObjectDataSource. Zobaczyliśmy, jak kontrolki ObjectDataSource działa jako serwer proxy między strony ASP.NET i danych bazowych. GridView wymaganych, aby wyświetlić dane, wywołuje jego ObjectDataSource `Select()` metody, która z kolei wywołuje metodę z naszych firm logiki warstwy (LOGIKI), która wywołuje metodę w odpowiedni dostęp do warstwy danych (DAL) TableAdapter, która z kolei wysyła `SELECT` zapytanie do bazy danych Northwind.

Pamiętamy, podczas tworzenia elementów TableAdapter w DAL w [naszym pierwszym samouczku](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automatycznie dodawane metod wstawiania, aktualizowania, i usuwanie danych z podstawową bazą danych tabeli. Ponadto w [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) został zaprojektowany metody LOGIKI, która wywołana w dół do tych metod modyfikacji warstwy danych.

W uzupełnieniu do jego `Select()` metody kontrolki ObjectDataSource ma również `Insert()`, `Update()`, i `Delete()` metody. Podobnie jak `Select()` metody te trzy metody mogą zostać zmapowane do metody obiektu źródłowego. Po skonfigurowaniu do wstawiania, aktualizacji lub usuwania danych kontrolki GridView DetailsView i FormView udostępniają interfejs użytkownika do modyfikowania danych bazowych. Ten interfejs użytkownika wywołuje `Insert()`, `Update()`, i `Delete()` metody ObjectDataSource, które następnie wywołać obiekt źródłowy skojarzonej z metod (patrz rysunek 1).


[![ObjectDataSource Insert() Update() i metod Delete() służyć jako serwer Proxy do LOGIKI](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Rysunek 1**: ObjectDataSource `Insert()`, `Update()`, i `Delete()` metody służyć jako serwer Proxy do LOGIKI ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


W tym samouczku opisano sposób mapowania ObjectDataSource `Insert()`, `Update()`, i `Delete()` metody dla metod klas w LOGIKI, a także jak skonfigurować kontrolki GridView, DetailsView i FormView, aby zapewnić, modyfikacji danych możliwości.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Krok 1: Tworzenie Insert, Update i stron sieci Web samouczki Delete

Zanim zaczniemy, eksplorowanie jak wstawianie, aktualizowanie i usuwanie danych, najpierw poświęćmy chwilę do tworzenia stron ASP.NET w naszym projektu witryny sieci Web, który będziemy potrzebować dla tego samouczka i dalej te kilka. Rozpocznij od dodania nowy folder o nazwie `EditInsertDelete`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące modyfikacji danych](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Rysunek 2**: Dodawanie stron ASP.NET samouczki dotyczące modyfikacji danych


Podobnie jak w przypadku innych folderów `Default.aspx` w `EditInsertDelete` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań do widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Na koniec Dodaj strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po sformatowaniu dostosowane `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczków.


![Mapa witryny zawiera teraz wpisy dla edytowanie, wstawianie i usuwanie samouczki](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Rysunek 4**: mapy witryny zawiera teraz wpisy dla edytowanie, wstawianie i usuwanie samouczki


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Krok 2: Dodawanie i konfigurowanie kontrolki ObjectDataSource

Ponieważ DetailsView, GridView i FormView każdego różnią się w ich funkcji modyfikacji danych i układ Przeanalizujmy osobno każdy z nich. Zamiast każdego formantu przy użyciu własnego elementu ObjectDataSource, jednak po prostu Utwórz pojedynczego elementu ObjectDataSource, wszystkie trzy kontrolki przykłady można współdzielić.

Otwórz `Basics.aspx` stronie, przeciągnij kontrolki ObjectDataSource z przybornika w projektancie, a następnie kliknij łącze Konfiguruj źródła danych w tagu inteligentnego. Ponieważ `ProductsBLL` to jedyna klasa LOGIKI, która umożliwia edytowanie, wstawianie i usuwanie metod, skonfiguruj kontrolki ObjectDataSource, aby użyć tej klasy.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


Na następnym ekranie można określić jakie metody `ProductsBLL` klasy są zamapowane na ObjectDataSource `Select()`, `Insert()`, `Update()`, i `Delete()` , wybierając odpowiednią kartę i wybranie metody z listy rozwijanej. Rysunek 6, która powinna wyglądać znajomo już mapuje ObjectDataSource `Select()` metody `ProductsBLL` klasy `GetProducts()` metody. `Insert()`, `Update()`, I `Delete()` metod można skonfigurować, wybierając odpowiednią kartę na liście u góry.


[![Kontrolki ObjectDataSource powrót wszystkie produkty](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Rysunek 6**: mają ObjectDataSource zwrócić wszystkich produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Karty danych, 7, 8 i 9 Pokaż ObjectDataSource UPDATE, INSERT i DELETE. Konfigurowanie tych kart, aby `Insert()`, `Update()`, i `Delete()` wywołania metody `ProductsBLL` klasy `UpdateProduct`, `AddProduct`, i `DeleteProduct` metod, odpowiednio.


[![Map, Metoda Update() ObjectDataSource klasy ProductBLL UpdateProduct metody](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Rysunek 7**: mapowanie ObjectDataSource `Update()` metody `ProductBLL` klasy `UpdateProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Map, metoda Insert() ObjectDataSource klasy ProductBLL AddProduct metody](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Rysunek 8**: mapowanie ObjectDataSource `Insert()` metody `ProductBLL` Dodaj klasy `Product` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Map, Metoda Delete() ObjectDataSource klasy ProductBLL DeleteProduct metody](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Rysunek 9**: mapowanie ObjectDataSource `Delete()` metody `ProductBLL` klasy `DeleteProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Być może zauważono, że list rozwijanych w kartach UPDATE, INSERT i DELETE był już tych metod wybrane. Jest to dzięki używaniu przez nas `DataObjectMethodAttribute` , rozszerza metody `ProducstBLL`. Na przykład metoda DeleteProduct ma następujący podpis:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute` Atrybut wskazuje przeznaczenie każdej z metod, czy jest wybranie, wstawianie, aktualizowanie lub usuwanie i określa, czy jest to wartość domyślna. Pominięcie tych atrybutów podczas tworzenia klas LOGIKI będzie należy ręcznie wybrać metody z aktualizacji, Wstaw i usuwanie kart.

Po upewnieniu się, że odpowiednie `ProductsBLL` metody są mapowane na ObjectDataSource `Insert()`, `Update()`, i `Delete()` metod, kliknij przycisk Zakończ, aby zakończyć działanie kreatora.

## <a name="examining-the-objectdatasources-markup"></a>Badanie ObjectDataSource znaczników

Po skonfigurowaniu ObjectDataSource za pomocą jego kreatora, przejdź do widoku źródła do sprawdzenia wygenerowanego oznaczeniu deklaracyjnym. `<asp:ObjectDataSource>` Tag określa obiekt źródłowy i metody do wywołania. Ponadto istnieją `DeleteParameters`, `UpdateParameters`, i `InsertParameters` mapowania parametrów wejściowych dla `ProductsBLL` klasy `AddProduct`, `UpdateProduct`, i `DeleteProduct` metody:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

Kontrolki ObjectDataSource zawiera parametr dla każdego z parametrów wejściowych dla skojarzonych z nim metod, podobnie jak listy `SelectParameter` s jest obecna, gdy kontrolki ObjectDataSource jest skonfigurowany do wywołania metody select, który oczekuje, że parametr wejściowy (takie jak `GetProductsByCategoryID(categoryID)`). Jak zajmiemy się wkrótce, wartości dla tych `DeleteParameters`, `UpdateParameters`, i `InsertParameters` są ustawiane automatycznie GridView, DetailsView i FormView przed wywołaniem ObjectDataSource `Insert()`, `Update()`, lub `Delete()` Metoda. Wartości te można również ustawić programowo, zgodnie z potrzebami, jak omówiono w przyszłości zapoznać się z samouczkiem.

Jeden po stronie za pomocą kreatora można skonfigurować w celu ObjectDataSource powoduje, że program Visual Studio ustawia [właściwość elementu OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) do `original_{0}`. Wartość tej właściwości jest używana do włączenia oryginalnej wartości danych, edycji i jest przydatne w przypadku dwóch scenariuszy:

- Jeśli podczas edytowania rekordu, użytkownicy będą mogli zmienić wartość klucza podstawowego. W takim przypadku zarówno nowe wartości klucza podstawowego, jak i oryginalną wartość klucza podstawowego musi być podana tak, aby rekord z oryginalną wartość klucza podstawowego można znaleźć oraz korzystać z jej wartość odpowiednio aktualizowana.
- Korzystając z optymistycznej współbieżności. Optymistycznej współbieżności to technika, aby upewnić się, że dwa równoczesnych użytkowników nie zastępuj zmiany kogoś innego i jest temat samouczek, w przyszłości.

`OldValuesParameterFormatString` Właściwość wskazuje nazwę parametrów wejściowych w obiekt źródłowy aktualizacji i usuwania metody oryginalnych wartości. Omówimy tę właściwość i jej przeznaczenie bardziej szczegółowo, gdy będziemy eksplorować optymistycznej współbieżności. Czy mogę przenieść je teraz jednak ponieważ naszych LOGIKI metody nie powinna mieć oryginalnych wartości, dlatego ważne jest, że możemy usunąć tej właściwości. Pozostawienie `OldValuesParameterFormatString` właściwość ustawioną na coś innego niż domyślny (`{0}`) spowoduje, że wystąpił błąd podczas próby wywołania ObjectDataSource danych formantu sieci Web `Update()` lub `Delete()` metody ponieważ kontrolki ObjectDataSource będzie próba przekazania zarówno `UpdateParameters` lub `DeleteParameters` określone, a także oryginalnych wartości parametrów.

Jeśli to nie jest poważny wyczyść w tym momencie, nie martw się zajmiemy się tę właściwość i jej narzędzi w przyszłości zapoznać się z samouczkiem. Teraz, po prostu upewnij się, ta deklaracja właściwości całkowicie usunąć z składni deklaratywnej lub ustaw wartość na wartość domyślną ({0}).

> [!NOTE]
> Jeśli po prostu wyczyszczenie `OldValuesParameterFormatString` wartości właściwości z okna właściwości w widoku Projekt, a właściwość będą nadal istnieć w składni deklaratywnej, ale można ustawić na pusty ciąg. W efekcie Niestety nadal spowoduje ten sam problem omówione powyżej. W związku z tym, albo usuń właściwość całkowicie ze składni deklaratywnej, lub z poziomu okna właściwości ustaw wartość domyślną `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Krok 3: Dodawanie danych kontrolki sieci Web i skonfigurowania go dla modyfikacji danych

Po dodał do strony i skonfigurowaniu kontrolki ObjectDataSource jesteśmy gotowi dodać dane kontrolki sieci Web do strony Aby wyświetlić dane i umożliwiają użytkownikom końcowym go zmodyfikować. Omówimy GridView DetailsView i FormView oddzielnie, ponieważ te kontrolki internetowe danych różnią się w swoich funkcji modyfikacji danych i konfiguracji.

Jak zobaczymy w dalszej części tego artykułu, dodawania wykraczającego poza podstawowe edytowanie, wstawianie i usuwanie pomocy technicznej w ramach DetailsView, w widoku GridView i FormView kontroluje jest naprawdę proste i polega na sprawdzanie kilka pól wyboru. Istnieje jeszcze wiele precyzyjnych przypadki brzegowe w rzeczywistych warunkach, wchodzące w dostarczanie takich funkcji, które są bardziej skomplikowane niż po prostu wskazanie i kliknięcie. Jednak ten samouczek koncentruje się wyłącznie na udowodnienie funkcji modyfikacji danych uproszczony. Samouczki przyszłych zbada problemy, które pojawią się bez wątpienia w ustawieniu rzeczywistych.

## <a name="deleting-data-from-the-gridview"></a>Usuwanie danych z widoku GridView

Rozpocznij, przeciągając je z przybornika do projektanta w kontrolce GridView. Następnie powiązać kontrolki ObjectDataSource widoku GridView, wybierając je z listy rozwijanej w tagu inteligentnego GridView. W tym momencie GridView oznaczeniu deklaracyjnym będzie:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Powiązywanie kontrolki GridView ObjectDataSource za pośrednictwem jego tag inteligentny ma dwie zalety:

- BoundFields i CheckBoxFields są tworzone automatycznie dla każdego pola, zwracany przez kontrolki ObjectDataSource. Ponadto właściwości elementu BoundField i CheckBoxField firmy jest ustawiona na podstawie odpowiedniego pola metadanych. Na przykład `ProductID`, `CategoryName`, i `SupplierName` pola są oznaczone jako tylko do odczytu w `ProductsDataTable` i dlatego nie powinny być nadaje się do aktualizacji podczas edycji. Aby uwzględnić ten, te BoundFields [właściwości tylko do odczytu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) są ustawione na `True`.
- [Właściwości DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) jest przypisany do pola klucza podstawowego obiektu źródłowego. Jest to istotne, kiedy przy użyciu widoku GridView do edytowania lub usuwania danych, ponieważ ta właściwość wskazuje pola (lub zestaw pól) oznacza unikatowy identyfikuje każdy rekord. Aby uzyskać więcej informacji na temat `DataKeyNames` właściwości, odwołaj się do [Master/szczegółów przy użyciu GridView wzorca można wybierać z DetailView szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka.

Gdy widoku GridView może być powiązana z kontrolki ObjectDataSource za pośrednictwem oknie właściwości lub składni deklaratywnej, wymaga można ręcznie dodać odpowiednie elementu BoundField i `DataKeyNames` znaczników.

W kontrolce GridView udostępnia wbudowaną obsługę na poziomie wiersza, edytowania i usuwania. Konfigurowanie GridView umożliwiających usunięcie dodaje kolumnę przycisków Delete. Gdy użytkownik końcowy kliknie przycisk usuwania dla konkretnego wiersza, ensues odświeżenie strony i kontrolki GridView wykonuje następujące czynności:

1. ObjectDataSource `DeleteParameters` wartości są przypisane.
2. ObjectDataSource `Delete()` zostanie wywołana metoda usuwania określonego rekordu
3. Kontrolki GridView rebinds się do kontrolki ObjectDataSource przez wywołanie jego `Select()` — metoda

Wartości przypisane do `DeleteParameters` są wartości `DataKeyNames` pola dla wiersza, którego przycisk Usuń został kliknięty. Dlatego istotne jest, w widoku GridView `DataKeyNames` poprawnie można ustawić właściwości. Jeśli brakuje, `DeleteParameters` zostanie przypisana wartość `Nothing` w kroku 1, co z kolei nie spowoduje żadnych usuniętych rekordów w kroku 2.

> [!NOTE]
> `DataKeys` Kolekcji znajduje się w stan kontrolki GridView s, co oznacza, że `DataKeys` wartości zostaną zapamiętane na odświeżenie strony, nawet jeśli wyłączono stan widoku GridView s. Jest bardzo ważne jest, że stan widoku pozostanie włączony GridViews, która obsługuje edytowania lub usuwania (zachowanie domyślne). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, edytowania i usuwania zachowanie będzie działać prawidłowo dla pojedynczego użytkownika, ale w przypadku równoczesnych użytkowników, usuwaniem danych, istnieje możliwość przypadkowego może te równoczesnych użytkowników Usuń lub Edytuj rejestruje, t są planowane. Zobacz Mój wpis w blogu [Ostrzeżenie: problem współbieżności przy użyciu platformy ASP.NET 2.0 GridViews/DetailsView/FormViews tej edycji pomocy technicznej i/lub usuwanie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx), aby uzyskać więcej informacji.


Ostrzeżenie to ten sam dotyczy także DetailsViews i FormViews.

Umożliwiające dodanie funkcji usuwania w kontrolce GridView, przejdź do jej tagów inteligentnych i zaznacz pole wyboru Włącz usuwanie.


![Sprawdź Włącz usuwanie pola wyboru](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Na rysunku nr 10**: Sprawdź Włącz usuwanie pola wyboru


Zaznaczając pole wyboru Włącz usuwanie z tagu inteligentnego dodaje CommandField do kontrolki GridView. CommandField renderuje kolumny w widoku GridView za pomocą przycisków do wykonywania co najmniej jedną z następujących zadań: wybranie rekordu, edytowania rekordu i usunięcie rekordu. Widzieliśmy wcześniej CommandField in action z wybraniu rekordów w [Master/szczegółów przy użyciu GridView wzorca można wybierać z DetailView szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka.

CommandField zawiera szereg `ShowXButton` właściwości, które wskazują, jakie szeregu przycisków są wyświetlane w CommandField. Zaznaczając pole wyboru Włącz usuwanie CommandField którego `ShowDeleteButton` właściwość `True` została dodana do kolekcji kolumn GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

W tym momencie believe nim lub nie wszystko jest gotowe o dodanie obsługi usuwania do kontrolki GridView! Jak pokazano na ilustracji 11, gdy odwiedzić tę stronę za pośrednictwem przeglądarki kolumnę przycisków Delete jest obecny.


[![CommandField dodaje kolumnę przycisków Delete](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Rysunek 11**: CommandField dodaje kolumnę o usuń przyciski ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Jeśli już zostały oparte w tym samouczku od podstaw na własną rękę, podczas testowania tej strony, klikając przycisk Usuń zgłosi wyjątek. Kontynuuj czytanie, aby dowiedzieć się, dlaczego tych wyjątków zostały zgłoszone i jak je rozwiązać.

> [!NOTE]
> Jeśli piszesz przy użyciu pobierania towarzyszący w tym samouczku, te problemy zostały już została uwzględniona. Jednakże zachęcam Cię do przeczytania przez czytelników szczegóły wymienionych poniżej, aby pomagać w identyfikacji problemów, które mogą wystąpić i odpowiednie rozwiązania.


Jeśli podczas próby usunięcia produktu, otrzymujesz wyjątek którego komunikat jest podobny do "*ObjectDataSource"ObjectDataSource1"nie można odnaleźć metody nieogólnego DeleteProduct, który ma następujące parametry: productID, oryginalnym\_ ProductID*, "prawdopodobnie, że nie można usunąć `OldValuesParameterFormatString` właściwości z kontrolki ObjectDataSource. Za pomocą `OldValuesParameterFormatString` określić właściwość kontrolki ObjectDataSource próbuje przekazywania w obu `productID` i `original_ProductID` parametry wejściowe `DeleteProduct` metody. `DeleteProduct`, jednak akceptowane są tylko pojedynczy parametr wejściowy, dlatego wyjątek. Usuwanie `OldValuesParameterFormatString` właściwości (lub ustawieniem dla niego `{0}`) powoduje, że ObjectDataSource próby nie podawać oryginalnego parametru wejściowego.


[![Upewnij się, że właściwość elementu OldValuesParameterFormatString zostały wyczyszczone](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Rysunek 12**: Upewnij się, że `OldValuesParameterFormatString` właściwość ma zostały wyczyszczone Out ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


Nawet jeśli były usuwane `OldValuesParameterFormatString` właściwości nadal będą otrzymujesz wyjątek podczas próby usunięcia produktu z komunikatem: "*instrukcji DELETE konflikt z ograniczeniem odwołania" klucza Obcego\_kolejności\_szczegóły \_Produktów*. " Bazy danych Northwind zawiera ograniczenie klucza obcego między `Order Details` i `Products` tabeli, co oznacza, że produkt nie można usunąć z systemu, jeśli istnieją co najmniej jeden rekord dla niego w `Order Details` tabeli. Ponieważ każdego produktu w bazie danych Northwind ma co najmniej jeden rekord `Order Details`, nie możemy usunąć wszystkie produkty, aż najpierw usuniemy produktu skojarzonego zamówienia szczegóły rekordów.


[![Ograniczenie klucza obcego zabrania usuwania produktów](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Rysunek 13**: ograniczenie klucza obcego zabrania usuwania produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


W naszym samouczku możemy po prostu usuń wszystkie rekordy z `Order Details` tabeli. W rzeczywistych aplikacjach firma Microsoft musiałaby albo:

- Ma inny ekran, aby zarządzać informacjami szczegóły zamówienia
- Rozszerzaj `DeleteProduct` metodę, aby uwzględnić logiki, aby usunąć określony produkt szczegóły zamówienia
- Zmodyfikuj zapytanie SQL używane przez TableAdapter obejmujący usunięcie określonego produktu szczegóły zamówienia

Teraz po prostu usuń wszystkie rekordy z `Order Details` tabeli, aby obejść ograniczenie klucza obcego. Przejdź do Eksploratora serwera w programie Visual Studio, kliknij prawym przyciskiem myszy `NORTHWND.MDF` węzeł i wybierz opcję nowe zapytanie. Następnie w oknie zapytania Uruchom następującą instrukcję SQL: `DELETE FROM [Order Details]`


[![Usuń wszystkie rekordy z tabeli Szczegóły zamówienia](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Rysunek 14**: Usuń wszystkie rekordy z `Order Details` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Po wyczyszczeniu `Order Details` tabeli, klikając przycisk usuwania spowoduje usunięcie produktu bez błędów. Jeśli kliknięcie na przycisk usuwania nie spowoduje usunięcia produktu, sprawdź, upewnij się, że w widoku GridView `DataKeyNames` właściwość jest ustawiona na klucz podstawowy (`ProductID`).

> [!NOTE]
> Po kliknięciu przycisku Usuń ensues odświeżenie strony i rekord jest usuwany. Może to być niebezpieczne, ponieważ ułatwia przypadkowo klikniesz przycisk usuwania nieprawidłowy wiersz. W samouczku przyszłych Zobaczymy się, jak dodać potwierdzenia po stronie klienta podczas usuwania rekordu.


## <a name="editing-data-with-the-gridview"></a>Edytowanie danych za pomocą widoku GridView

Wraz z usunięciem, w kontrolce GridView zapewnia wbudowaną obsługą edycji na poziomie wiersza. Konfigurowanie GridView do obsługi edycji dodaje kolumnę przycisków edycji. Z perspektywy użytkownika końcowego, klikając wiersz edycji przycisk przyczyn, które wiersz, aby stać się niemożliwa Przekształcanie komórki w pola tekstowe zawierające istniejące wartości i przycisk Edytuj z aktualizacją Update i przyciski Anuluj. Po wprowadzeniu ich żądane zmiany, użytkownik końcowy może kliknij przycisk Aktualizuj, aby zatwierdzić zmiany, lub przycisk Anuluj, aby je odrzucić. W obu przypadkach po kliknięciu przycisku Aktualizuj, lub Anuluj zwraca GridView stan wstępnie edycji.

Z nasz pogląd, deweloper strony gdy użytkownik końcowy kliknie przycisk edycji dla konkretnego wiersza, ensues odświeżenie strony i widoku GridView wykonuje następujące czynności:

1. W widoku GridView `EditItemIndex` właściwość jest przypisany do indeks wiersza został kliknięty przycisk Edytuj, którego
2. Kontrolki GridView rebinds się do kontrolki ObjectDataSource przez wywołanie jego `Select()` — metoda
3. Indeks wiersza, który odpowiada `EditItemIndex` jest renderowany w "trybie edycji." W tym trybie przycisk Edytuj zastępuje przyciski aktualizacji i Anuluj i BoundFields którego `ReadOnly` właściwości mają wartość False (domyślnie) są renderowane jako pole tekstowe w sieci Web decyduje, którego `Text` właściwości są przypisywane do wartości pola danych.

W tym momencie znaczników jest zwracany do przeglądarki, umożliwiając użytkownikowi końcowemu wprowadzać żadnych zmian w danych wiersza. Gdy użytkownik kliknie przycisk Aktualizuj, występuje odświeżenie strony i kontrolki GridView wykonuje następujące czynności:

1. ObjectDataSource `UpdateParameters` wartości są przypisane wartości wprowadzone przez użytkownika końcowego do interfejsu edycji kontrolki GridView
2. ObjectDataSource `Update()` metoda jest wywoływana, aktualizowanie określony rekord
3. Kontrolki GridView rebinds się do kontrolki ObjectDataSource przez wywołanie jego `Select()` — metoda

Wartości klucza podstawowego, przypisany do `UpdateParameters` w kroku 1 pochodzą z wartości określone w `DataKeyNames` właściwości, wartości kluczy innych niż podstawowe pochodzić z tekstu w kontrolkach internetowych pole tekstowe dla wiersza edytowany. Jak usuwanie, ważne jest, że w widoku GridView `DataKeyNames` poprawnie można ustawić właściwości. Jeśli brakuje, `UpdateParameters` wartość klucza podstawowego zostanie przypisana wartość `Nothing` w kroku 1, co z kolei nie spowoduje żadnych zaktualizowanych rekordów w kroku 2.

Po prostu, zaznaczając pole wyboru Włącz edytowanie w tagu inteligentnego GridView można aktywować funkcji edycji.


![Sprawdź Włącz edytowanie pola wyboru](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Rysunek 15**: Sprawdź Włącz edytowanie pola wyboru


Sprawdzanie, zaznacz pole wyboru Włącz edytowanie doda CommandField (w razie potrzeby) i ustaw jego `ShowEditButton` właściwość `True`. Jak wcześniej widzieliśmy, CommandField zawiera szereg `ShowXButton` właściwości, które wskazują, jakie szeregu przycisków są wyświetlane w CommandField. Zaznaczając pole wyboru Włącz edytowanie dodaje `ShowEditButton` właściwości istniejących CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

To wszystko na temat dodawania obsługi edycji podstawowe. Tak jak pokazano w Figure16 zamiast surowych jest interfejs edytowania każdego elementu BoundField którego `ReadOnly` właściwość jest ustawiona na `False` (ustawienie domyślne) jest renderowany jako pole tekstowe. W tym pól, takich jak `CategoryID` i `SupplierID`, które są klucze do innych tabel.


[![Klikając przycisk Edytuj s Chai wyświetla wiersz w trybie edycji](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Rysunek 16**: s Chai, klikając przycisk Edytuj wyświetla wiersz w trybie edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Oprócz prosząc użytkowników o bezpośrednio edytować wartości klucza obcego, Brak interfejsu interfejsu edycji w następujący sposób:

- Jeśli użytkownik wprowadzi `CategoryID` lub `SupplierID` , nie istnieje w bazie danych `UPDATE` spowodują naruszenie ograniczenia klucza obcego, powoduje wyjątek.
- Edytowanie interfejsu nie zawiera wszystkich sprawdzania poprawności. Jeśli nie podasz wymaganej wartości (takie jak `ProductName`), lub wprowadź wartość ciągu, w którym Oczekiwano wartości liczbowej (na przykład wprowadzenie "Zbyt dużo!" do `UnitPrice` pole tekstowe), zostanie zgłoszony wyjątek. Samouczek przyszłych zbada sposób dodawania kontrolek weryfikacji do edytowania interfejsu użytkownika.
- Obecnie *wszystkich* pola produktów, które nie są tylko do odczytu muszą być zawarte w widoku GridView. Załóżmy, gdybyśmy wybrali usunąć pole z kontrolki GridView `UnitPrice`podczas aktualizowania danych widoku GridView ustawiał nie `UnitPrice` `UpdateParameters` wartość, która zmieniłby się w rekordzie bazy danych `UnitPrice` do `NULL` wartości. Podobnie, jeśli to pole jest wymagane, takie jak `ProductName`, zostanie usunięty z GridView aktualizacji zakończy się niepowodzeniem z takimi samymi "*kolumny"ProductName"dopuszcza wartości null*" wyjątków wymienionych powyżej.
- Formatowanie edycji interfejsu pozostawia znacznie być wskazane. `UnitPrice` Jest przedstawiana wraz z czterech separatorów dziesiętnych. W idealnym `CategoryID` i `SupplierID` wartości zawierałoby kontrolek DropDownList, który listy kategorii i dostawców w systemie.

Są to wszystkie nieprawidłowości, które odpowiemy na żywo za pomocą teraz, ale zostanie rozwiązany w przyszłej samouczków.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Wstawianie, edytowanie i usuwanie danych za pomocą DetailsView

Widzieliśmy w samouczkach wcześniej kontrolce DetailsView Wyświetla jeden rekord jednocześnie i jak kontrolki GridView, umożliwia edytowanie i usuwanie aktualnie wyświetlany rekord. Środowisko zarówno przez użytkownika końcowego za pomocą edycji i usuwania elementów z DetailsView i przepływu pracy po stronie platformy ASP.NET jest taka sama jak w przypadku widoku GridView. Gdy DetailsView różni się od widoku GridView jest również zapewnia wbudowaną obsługę Wstawianie.

Aby zademonstrować funkcji modyfikacji danych GridView, Rozpocznij, dodając DetailsView do `Basics.aspx` stronie ponad istniejących GridView i powiązać ją z istniejącej kontrolki ObjectDataSource za pośrednictwem DetailsView tagu inteligentnego. Następnie czyści DetailsView `Height` i `Width` właściwości i zaznacz opcję Włącz stronicowanie za pomocą tagu inteligentnego. Aby umożliwić edytowanie, wstawianie i usuwanie pomocy technicznej, po prostu zaznaczyć pola wyboru Włącz edytowanie, Włącz wstawianie i Włącz usuwanie w tagu inteligentnego.


![Konfigurowanie DetailsView do pomocy technicznej, edytowanie, wstawianie i usuwanie](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Rysunek 17**: Konfigurowanie DetailsView do pomocy technicznej, edytowanie, wstawianie i usuwanie


Jako przy użyciu GridView, dodawanie, edytowanie, wstawiając lub usuwając obsługi dodaje CommandField do DetailsView, tak jak pokazano w poniższej składni deklaratywnej:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Należy zauważyć, że DetailsView CommandField domyślnie pojawia się na końcu kolekcji kolumn. Ponieważ pola DetailsView są renderowane jako wiersze, CommandField występuje jako wiersz z Insert, edytowanie i usuwanie przycisków w dolnej części DetailsView.


[![Konfigurowanie DetailsView do pomocy technicznej, edytowanie, wstawianie i usuwanie](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Rysunek 18**: Konfigurowanie DetailsView do pomocy technicznej edytowanie, wstawianie i usuwanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Kliknij przycisk Usuń rozpoczyna się taką samą sekwencję zdarzeń podobnie jak w przypadku widoku GridView: a zwrotu; następuje DetailsView wypełnianie jej ObjectDataSource `DeleteParameters` na podstawie `DataKeyNames` wartości oraz zostać zakończona za pomocą wywołania jego ObjectDataSource `Delete()` metody, która faktycznie usuwa produkt z bazy danych. Edytowanie w DetailsView działa również w sposób jest taka sama jak w przypadku widoku GridView.

Do wstawiania, użytkownik końcowy zostanie wyświetlony nowy przycisk, który, po kliknięciu renderuje DetailsView w "trybie wstawiania". Za pomocą "tryb wstawiania" przycisk "nowy" zastępuje przycisków Insert i Anuluj i tylko te BoundFields którego `InsertVisible` właściwość jest ustawiona na `True` (ustawienie domyślne) są wyświetlane. Te pola danych, identyfikowany jako automatycznego przyrostu pola, takie jak `ProductID`, ma ich [właściwość InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) równa `False` podczas tworzenia powiązania DetailsView ze źródłem danych za pomocą tagu inteligentnego.

Podczas tworzenia wiązania źródła danych do DetailsView za pośrednictwem tagu, program Visual Studio ustawia `InsertVisible` właściwość `False` tylko dla pól automatycznego przyrostu. Pola tylko do odczytu, takich jak `CategoryName` i `SupplierName`, będą wyświetlane w interfejsie użytkownika "tryb wstawiania", chyba że ich `InsertVisible` jawnie ustawiono właściwość `False`. Poświęć chwilę, aby ustawić te dwa pola `InsertVisible` właściwości `False`, albo za pomocą składni deklaratywnej DetailsView lub Edytuj pola łącze w tagu inteligentnego. Rysunek 19 pokazuje ustawienie `InsertVisible` właściwości `False` , klikając polecenie Edytuj pola link.


[![Northwind Traders oferuje teraz herbaty xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Rysunek 19**: Northwind Traders teraz oferuje Acme herbaty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Po ustawieniu `InsertVisible` właściwości, Wyświetl `Basics.aspx` strony w przeglądarce, a następnie kliknij przycisk Nowy. 20 rysunek pokazuje DetailsView podczas dodawania nowego spożywczy herbaty xyz, do naszych linia produktów.


[![Northwind Traders oferuje teraz herbaty xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Rysunek 20**: Northwind Traders teraz oferuje Acme herbaty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Po wprowadzeniu szczegóły dla Acme herbaty i kliknij przycisk Wstaw, ensues odświeżenie strony i nowy rekord zostanie dodany do `Products` tabeli bazy danych. Ponieważ ta DetailsView zawiera listę produktów, które w kolejności, z którym istnieje w tabeli bazy danych, firma Microsoft musi strony do ostatniego produktu zobaczenie nowego produktu.


[![Szczegóły dotyczące herbaty xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Rysunek 21**: szczegóły herbaty Acme ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> DetailsView [właściwość CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) wskazuje interfejsu są wyświetlane i może być jedną z następujących wartości: `Edit`, `Insert`, lub `ReadOnly`. [Wartość właściwości DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) oznacza tryb DetailsView zwraca po edycji lub wstawić zostało ukończone i jest przydatny do wyświetlania DetailsView, trwale w trakcie edycji lub trybie wstawiania.


Wskaż i kliknij, wstawiania i edytowania DetailsView cechują się te same ograniczenia co widoku GridView: użytkownik musi wprowadzić istniejące `CategoryID` i `SupplierID` wartości do pola tekstowego; nie ma interfejsu wszelka logika sprawdzania poprawności; wszystkie pola produktów, które nie zezwalają na `NULL` wartości lub nie ma domyślnie wartość określonego na poziomie bazy danych muszą być zawarte w Wstawianie interfejsu i tak dalej.

Techniki firma Microsoft będzie szukać rozszerzanie i udoskonalanie GridView edycji interfejsu w przyszłości artykuły mogą być stosowane do kontrolce DetailsView edycji i wstawiania również interfejsów.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Za pomocą widoku FormView dla bardziej elastyczne interfejsu użytkownika modyfikacji danych

FormView oferuje wbudowaną obsługę wstawiania, edytowania i usuwania danych, ale używa szablonów zamiast pól nie wymaga ona umożliwiające dodawanie BoundFields lub CommandField używane przez kontrolki GridView i DetailsView do dostarczania danych Interfejs modyfikacji. Zamiast tego należy ten interfejs, kontrolki sieci Web do zbierania użytkownika wejścia podczas dodawania nowego elementu lub edycji istniejącego wraz z nowym, edytowanie, usuwanie, wstawiania, aktualizowania i przyciski "Anuluj" musi zostać dodany ręcznie do stosownych szablonów. Na szczęście programu Visual Studio automatycznie utworzy interfejsu potrzebne podczas tworzenia powiązania FormView ze źródłem danych za pomocą listy rozwijanej w jego tagu inteligentnego.

Aby zilustrować tych metod, Rozpocznij, dodając FormView do `Basics.aspx` strony, a z tagu inteligentnego FormView powiązać go ObjectDataSource już utworzony. Spowoduje to wygenerowanie `EditItemTemplate`, `InsertItemTemplate`, i `ItemTemplate` dla FormView za pomocą kontrolki TextBox w sieci Web do zbierania danych wejściowych użytkownika i kontrolki przycisku w sieci Web dla nowego, Edytuj, Usuń, wstawiania, aktualizowania i przyciski "Anuluj". Ponadto FormView `DataKeyNames` właściwość jest ustawiona na klucz podstawowy (`ProductID`) z obiektu zwróconego przez kontrolki ObjectDataSource. Na koniec zaznacz opcję Włącz stronicowania w tagu inteligentnego FormView.

Poniżej pokazano oznaczeniu deklaracyjnym dla FormView `ItemTemplate` po FormView została powiązana z kontrolki ObjectDataSource. Domyślnie każde pole produktu wartości inne niż logiczne jest powiązany z `Text` właściwości formantu etykiety w sieci Web podczas każdego pola wartość logiczna (`Discontinued`) jest powiązana z `Checked` właściwość wyłączona kontrolka pola wyboru w sieci Web. Aby przyciski nowy, edytowania i usuwania wyzwolić określone zachowanie FormView po kliknięciu, należy bezwzględnie, ich `CommandName` być równa wartości `New`, `Edit`, i `Delete`, odpowiednio.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

Rysunek 22 pokazuje FormView `ItemTemplate` podczas wyświetlania za pośrednictwem przeglądarki. Każde pole produktu znajduje się za pomocą przycisków nowy, Edit i Delete na dole.


[![Właściwości ItemTemplate Defaut FormView zawiera listę pól produktu, wraz z nowego, edytowanie i usuwanie przycisków](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Rysunek 22**: Defaut FormView `ItemTemplate` zawiera listę każdego produktu pola wraz z nowego, edytowanie i usuwanie przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


Jak z kontrolkami GridView i DetailsView, klikając przycisk Usuń lub dowolnego przycisku, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona na usunięcie powoduje odświeżenie strony, wypełnienie ObjectDataSource `DeleteParameters` oparte na FormView `DataKeyNames`wartości, a następnie wywołuje ObjectDataSource `Delete()` metody.

Po kliknięciu przycisku Edytuj ensues odświeżenie strony i danych jest odbitych do `EditItemTemplate`, który jest odpowiedzialny za renderowania interfejsu edycji. Ten interfejs zawiera kontrolki sieci Web do edycji danych wraz z przyciskami aktualizacji i Anuluj. Wartość domyślna `EditItemTemplate` generowane przez program Visual Studio zawiera etykietę dla wszystkich pól, automatycznego przyrostu (`ProductID`), w polu tekstowym dla każdego pola innego typu oraz pole wyboru dla każdego pola, wartość logiczna. To zachowanie jest bardzo podobne do BoundFields automatycznie wygenerowany w kontrolkach kontrolkami GridView i DetailsView.

> [!NOTE]
> Jeden mały problem z FormView Autogenerowanie `EditItemTemplate` jest formanty dla tych pól, które są tylko do odczytu, takich jak implementująca renderowanie sieci Web w polu tekstowym `CategoryName` i `SupplierName`. Zobaczymy, jak zostało to wkrótce.


Pole tekstowe formantów w `EditItemTemplate` ma ich `Text` właściwość powiązana wartość ich odpowiednie pola, używając *dwukierunkowego wiązania danych*. Dwukierunkowego wiązania danych, wskazywane przez `<%# Bind("dataField") %>`, wykonuje wiązanie danych zarówno podczas wiązania danych w szablonie i wypełnianie parametrów elementu ObjectDataSource wstawiania lub edytowania rekordów. Oznacza to, kiedy użytkownik kliknie przycisk Edytuj z `ItemTemplate`, `Bind()` metoda zwraca wartość pola określone dane. Po użytkownik sprawia, że ich zmiany i kliknie przycisk aktualizacji, wartości opublikowane kopii, które odnoszą się do pól danych określony za pomocą `Bind()` są stosowane do ObjectDataSource `UpdateParameters`. Alternatywnie jednokierunkowe databinding wskazywane przez `<%# Eval("dataField") %>`tylko pobiera wartości pól danych podczas wiązania danych w szablonie i jest *nie* zwrot do źródła danych parametrów zwrotu wartości wprowadzonych przez użytkownika.

Następujący kod deklaratywny przedstawia FormView `EditItemTemplate`. Należy pamiętać, że `Bind()` metoda jest używana z składnią wiązania danych i kontrolki aktualizacji i Web przycisku Anuluj mieć ich `CommandName` odpowiednio ustawić właściwości.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Nasze `EditItemTemplate`, w tym punktu, spowoduje, że wyjątek zgłoszony, jeśli możemy spróbować z niej korzystać. Problem jest to, że `CategoryName` i `SupplierName` są renderowane pola, zgodnie z sieci Web w polu tekstowym kontrolki w `EditItemTemplate`. Musimy albo Zmień te pola tekstowe etykiety lub usunąć je całkowicie. Teraz po prostu usunąć je całkowicie `EditItemTemplate`.

Ilustracja 23 pokazuje FormView w przeglądarce po kliknięciu przycisku edycji dla Chai. Należy pamiętać, że `SupplierName` i `CategoryName` pola wyświetlane w `ItemTemplate` nie są już dostępne, ponieważ właśnie usunęliśmy je z `EditItemTemplate`. Po kliknięciu przycisku Aktualizuj FormView przechodzi przez tę samą sekwencję kroków, co kontrolki GridView i DetailsView.


[![Domyślnie EditItemTemplate wszystkie pola są wyświetlane produktu można edytować jako pole tekstowe lub zaznacz pole wyboru](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Ilustracja 23**: Domyślnie `EditItemTemplate` pokazuje każde edytowalne produktu pole jako pole tekstowe lub zaznacz pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Po kliknięciu przycisku Wstaw FormView `ItemTemplate` ensues ogłaszania zwrotnego. Jednak żadne dane nie jest powiązany z FormView, ponieważ jest dodawany nowy rekord. `InsertItemTemplate` Interfejs zawiera kontrolki sieci Web do dodawania nowego rekordu wraz z przycisków Insert i Anuluj. Wartość domyślna `InsertItemTemplate` generowane przez program Visual Studio zawiera pole tekstowe dla każdego pola wartości inne niż logiczne i pola wyboru dla każdego pola wartość logiczna, podobne do automatycznego generowania `EditItemTemplate`interfejsu użytkownika. Formanty TextBox mieć ich `Text` właściwość powiązana wartość ich odpowiednich pól danych przy użyciu dwukierunkowego wiązania danych.

Następujący kod deklaratywny przedstawia FormView `InsertItemTemplate`. Należy pamiętać, że `Bind()` metoda jest używana z składnią wiązania danych i kontrolki Insert i Web przycisku Anuluj mieć ich `CommandName` odpowiednio ustawić właściwości.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

Brak subtlety z FormView Autogenerowanie `InsertItemTemplate`. W szczególności kontrolki TextBox w sieci Web są tworzone nawet w przypadku tych pól, które są tylko do odczytu, takich jak `CategoryName` i `SupplierName`. Za pomocą `EditItemTemplate`, należy usunąć te pola tekstowe z `InsertItemTemplate`.

Rysunek 24 pokazuje FormView w przeglądarce podczas dodawania nowego produktu Acme kawy. Należy pamiętać, że `SupplierName` i `CategoryName` pola wyświetlane w `ItemTemplate` nie są już dostępne, jak możemy po prostu one usunięte. Po kliknięciu przycisku Wstaw kontynuowane FormView przez tę samą sekwencję kroków, co w kontrolce DetailsView dodawania nowego rekordu do `Products` tabeli. Rysunek 25 zawiera szczegółowe informacje o produkcie kawy Acme w widoku FormView po został wstawiony.


[![Połączenia z opisywanym InsertItemTemplate FormView Wstawianie interfejsu](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Rysunek 24**: `InsertItemTemplate` decyduje o interfejsu Wstawianie FormView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![Szczegółowe informacje dotyczące nowego produktu, Acme kawy, są wyświetlane w widoku FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Rysunek 25**: szczegóły dotyczące nowego produktu, Acme kawy, są wyświetlane w widoku FormView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Oddzielając się tylko do odczytu, edycji i wstawiania interfejsy do trzech osobnymi szablonami, FormView umożliwia dokładniejszą kontrolę nad tych interfejsów niż DetailsView i GridView.

> [!NOTE]
> DetailsView, FormView, takich jak `CurrentMode` właściwość wskazuje interfejsu są wyświetlane i jego `DefaultMode` właściwość wskazuje tryb zwraca FormView do po edycji lub insert została ukończona.


## <a name="summary"></a>Podsumowanie

W tym samouczku zbadaliśmy podstawy wstawiania, edytowanie i usuwanie danych przy użyciu GridView DetailsView i FormView. Wszystkie trzy tych kontrolek zapewniać pewien poziom możliwości modyfikacji danych wbudowane, które mogą zostać użyte bez konieczności pisania nawet jednego wiersza kodu na stronie ASP.NET dzięki rozłożeniu w kontrolkach internetowych danych i kontrolki ObjectDataSource. Jednakże prostą i kliknij opcję dość frail renderowania techniki i interfejsu użytkownika modyfikacji danych naiwni. W celu udostępnienia weryfikacji, wstawić programowe wartości, bezpiecznie obsługiwać wyjątki, dostosowywanie interfejsu użytkownika i itd., będziemy potrzebować polegać na bevy technik, które zostaną dokładniej omówione za pośrednictwem dalej kilka samouczków.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [dalej](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
