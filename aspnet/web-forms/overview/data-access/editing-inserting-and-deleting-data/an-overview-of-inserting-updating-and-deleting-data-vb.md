---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: "Omówienie Wstawianie, aktualizowanie i usuwanie danych (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku zajmiemy się tym, jak zamapować operacji Insert() ObjectDataSource, Update(), i Delete() metod do metod logiki warstwy Biznesowej klasy, a także sposobu konfigu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 99d6b98bb7efa2f63e0c19b8623fd42ed92bdbaf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Omówienie Wstawianie, aktualizowanie i usuwanie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) lub [pobierania plików PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> W tym samouczku zajmiemy się tym, jak zamapować operacji Insert() ObjectDataSource, Update(), metody Delete() do metod logiki warstwy Biznesowej klasy i, a także sposobu konfigurowania kontrolki GridView widoku DetailsView i FormView zapewnienie możliwości modyfikacji danych.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczki możemy zostały zbadane sposób wyświetlania danych strony ASP.NET przy użyciu kontrolki GridView widoku DetailsView i FormView. Formanty po prostu działa z dane do nich. Zazwyczaj tych kontrolek dostęp do danych za pomocą formantu źródła danych, takich jak ObjectDataSource. Firma Microsoft przedstawiono sposób ObjectDataSource działa jako serwer proxy między strony ASP.NET i danych. Element GridView potrzebne do wyświetlania danych, wywołuje jego ObjectDataSource `Select()` metodę, która z kolei wywołuje metodę z naszych firm logiki warstwy (logiki warstwy Biznesowej), która wywołuje metodę w odpowiedniej warstwie dostępu do danych firmy (DAL) TableAdapter, który z kolei wysyła `SELECT` zapytania do bazy danych Northwind.

Odwołania, który po utworzyliśmy TableAdapters w DAL w [naszym pierwszym samouczku](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automatycznie dodawane metod wstawiania, aktualizowania, i usuwanie danych z podstawowej bazy danych tabeli. Ponadto w [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) metod w logiki warstwy Biznesowej, który wywołuje w dół możemy przeznaczony do tych metod DAL modyfikacji danych.

Oprócz jego `Select()` metody ObjectDataSource ma również `Insert()`, `Update()`, i `Delete()` metody. Podobnie jak `Select()` metody te trzy metody mogą być mapowane na metodach obiektu źródłowego. W przypadku skonfigurowania do wstawiania, aktualizacji lub usuwania danych, kontrolki GridView widoku DetailsView i FormView udostępniają interfejs użytkownika do modyfikowania danych. Ten interfejs użytkownika wywołuje `Insert()`, `Update()`, i `Delete()` metody ObjectDataSource, które następnie wywołania obiektu źródłowego dla elementów powiązanych metod (patrz rysunek 1).


[![Element ObjectDataSource operacji Insert(), metody Update() oraz metody Delete() służyć jako serwer Proxy do logiki warstwy Biznesowej](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Rysunek 1**: element ObjectDataSource `Insert()`, `Update()`, i `Delete()` metody służyć jako serwer Proxy do logiki warstwy Biznesowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


W tym samouczku zajmiemy się tym, jak zamapować ObjectDataSource `Insert()`, `Update()`, i `Delete()` metod do metod klasy w logiki warstwy Biznesowej, a także sposobu konfigurowania kontrolki GridView widoku DetailsView i FormView zapewnienie modyfikacji danych możliwości.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Krok 1: Tworzenie Insert, Update i stron sieci Web samouczki Delete

Przed możemy rozpocząć eksplorowanie jak wstawianie, aktualizowanie i usuwanie danych, najpierw wytłumaczone chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dalej widocznych kilka stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `EditInsertDelete`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

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


Podobnie jak w innych folderach `Default.aspx` w `EditInsertDelete` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań do widoku projektu.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Ponadto dodanie stron jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po sformatowaniu dostosowane `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczki.


![Obecnie mapy witryny zawiera wpisy dla edytowanie, wstawianie i usuwanie samouczki](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Rysunek 4**: mapy witryny teraz zawiera wpisy dla edytowanie, wstawianie i usuwanie samouczki


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Krok 2: Dodawanie i konfigurowanie kontrolki ObjectDataSource

Ponieważ FormView każdego różnią się w ich możliwości modyfikacji danych i układ widoku GridView, widoku DetailsView i Przeanalizujmy indywidualnie każdej z nich. Zamiast każdego formantu przy użyciu własnego elementu ObjectDataSource, jednak tylko Utwórzmy pojedynczego ObjectDataSource, które można udostępniać wszystkie przykłady formantów trzy.

Otwórz `Basics.aspx` strony, przeciągnij element ObjectDataSource z przybornika do projektanta, a następnie kliknij łącze Konfigurowanie źródła danych z jego tagów inteligentnych. Ponieważ `ProductsBLL` jest to jedyna klasa logiki warstwy Biznesowej, która udostępnia edytowanie, wstawianie i usuwanie metody, należy skonfigurować element ObjectDataSource używanie tej klasy.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


Na następnym ekranie można określić jakie metody `ProductsBLL` klasy są zamapowane na element ObjectDataSource `Select()`, `Insert()`, `Update()`, i `Delete()` zaznaczając odpowiednią kartę i wybrać metodę z listy rozwijanej. Rysunek 6, który powinna wyglądać znajomo przez teraz, mapy ObjectDataSource `Select()` metodę `ProductsBLL` klasy `GetProducts()` metody. `Insert()`, `Update()`, I `Delete()` metod można skonfigurować, wybierając odpowiednią kartę z listy na górze.


[![Element ObjectDataSource powrót wszystkich produktów](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Rysunek 6**: ma element ObjectDataSource zwraca wszystkie produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Rysunki 7, 8 i 9 Pokaż UPDATE, INSERT i DELETE ObjectDataSource karty. Konfigurowanie tych kart, aby `Insert()`, `Update()`, i `Delete()` wywołania metody `ProductsBLL` klasy `UpdateProduct`, `AddProduct`, i `DeleteProduct` metod, odpowiednio.


[![Mapowanie metody Update() ObjectDataSource klasy ProductBLL UpdateProduct metody](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Rysunek 7**: mapowanie ObjectDataSource `Update()` metodę `ProductBLL` klasy `UpdateProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Map — Metoda operacji Insert() ObjectDataSource do metody AddProduct klasy ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Rysunek 8**: mapowanie ObjectDataSource `Insert()` metodę `ProductBLL` Dodaj klasy `Product` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Map — Metoda Delete() ObjectDataSource klasy ProductBLL DeleteProduct metody](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Rysunek 9**: mapowanie ObjectDataSource `Delete()` metodę `ProductBLL` klasy `DeleteProduct` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Można zauważyć, że listy rozwijanej w kartach UPDATE, INSERT i DELETE już tych metod wybrane. Jest to dzięki użyciu używanie przez firmę `DataObjectMethodAttribute` który decorates metody `ProducstBLL`. Na przykład metoda DeleteProduct ma następującą sygnaturą:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute` Atrybut wskazuje na przeznaczenie każdej z metod, czy dla zaznaczenie, wstawianie, aktualizowanie, lub usuwanie i czy jest wartością domyślną jest. Pominięcie te atrybuty podczas tworzenia klas logiki warstwy Biznesowej można będzie muszą ręcznie wybrać metody aktualizacji, Wstaw i usuwanie kart.

Po upewnieniu się, że odpowiednie `ProductsBLL` metody są zamapowane na element ObjectDataSource `Insert()`, `Update()`, i `Delete()` metod, kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

## <a name="examining-the-objectdatasources-markup"></a>Badanie znaczników ObjectDataSource

Po skonfigurowaniu ObjectDataSource jego kreatora, przejdź do widoku źródłowego, aby zbadać wygenerowanego deklaratywne znaczników. `<asp:ObjectDataSource>` Tag określa obiekt i metody do wywołania. Ponadto istnieją `DeleteParameters`, `UpdateParameters`, i `InsertParameters` mapowania parametrów wejściowych dla `ProductsBLL` klasy `AddProduct`, `UpdateProduct`, i `DeleteProduct` metod:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

Element ObjectDataSource zawiera parametr dla każdego z parametrów wejściowych skojarzonych z nim metod, podobnie jak lista `SelectParameter` s jest obecna, gdy element ObjectDataSource jest skonfigurowany do wywołania metody select, która oczekuje parametru wejściowego (takie jak `GetProductsByCategoryID(categoryID)`). Jak zajmiemy się wkrótce, wartości dla nich `DeleteParameters`, `UpdateParameters`, i `InsertParameters` są ustawiane automatycznie GridView, widoku DetailsView i FormView przed wywołaniem elementu ObjectDataSource `Insert()`, `Update()`, lub `Delete()` Metoda. Te wartości można również ustawić programowo, zgodnie z potrzebami, jak omówiono w przyszłości samouczka.

Jeden po stronie za pomocą kreatora można skonfigurować tak, aby element ObjectDataSource powoduje, że program Visual Studio ustawia [właściwości elementu OldValuesParameterFormatString](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) do `original_{0}`. Wartość tej właściwości jest używana do włączenia oryginalne wartości danych, edytowania i jest przydatne w przypadku dwóch scenariuszy:

- Jeśli podczas edytowania rekordu, użytkownicy będą mogli zmienić wartość klucza podstawowego. W takim przypadku zarówno nowe wartości klucza podstawowego, jak i oryginalną wartość klucza podstawowego należy podać rekord z oryginalną wartość klucza podstawowego można znaleźć i jego wartość odpowiednio aktualizowany.
- Korzystając z optymistycznej współbieżności. Optymistycznej współbieżności to technika, aby upewnić się, że dwa równoczesnych użytkowników nie zastępuj siebie nawzajem zmiany i jest temat przyszłych samouczek.

`OldValuesParameterFormatString` Właściwość wskazuje nazwę parametry wejściowe w podstawowego obiektu aktualizacji i usuwania metody oryginalnych wartości. Omówiono ta właściwość i jej celem szczegółowo gdy firma Microsoft Eksploruj optymistycznej współbieżności. I wyświetlić go teraz, jednak ponieważ metody naszych logiki warstwy Biznesowej nie oczekuje wartości początkowe i w związku z tym ważne jest, aby usunąć tej właściwości. Pozostawienie `OldValuesParameterFormatString` ustawiona na inny niż domyślny (`{0}`) spowoduje, że wystąpił błąd podczas próby wywołania elementu ObjectDataSource danych formantu sieci Web `Update()` lub `Delete()` metody ponieważ będzie ObjectDataSource próba przekazania zarówno `UpdateParameters` lub `DeleteParameters` określony oraz oryginalnych wartości parametrów.

Jeśli to nie jest poważny, wyczyść w tym momencie, nie martw się zajmiemy się ta właściwość i jej narzędzi w przyszłości samouczka. Teraz po prostu można niektórych tej deklaracji właściwości całkowicie usunąć z składni deklaratywnej lub ustaw wartość na wartość domyślną ({0}).

> [!NOTE]
> Jeśli po prostu wyczyszczenie `OldValuesParameterFormatString` wartości właściwości z okna właściwości w widoku Projekt, właściwość będą nadal istnieć w składni deklaratywnej, ale można ustawić na pusty ciąg. Niestety, nadal spowoduje ten sam problem opisanych wyżej. W związku z tym Usuń właściwość całkowicie z składni deklaratywnej lub, w oknie właściwości ustaw wartość domyślną, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Krok 3: Dodawanie formantu danych sieci Web i skonfigurowania go dla modyfikacji danych

Po dodany do strony i skonfigurowaniu ObjectDataSource już wszystko gotowe do dodania danych formantów sieci Web do strony, aby wyświetlić dane, a także umożliwiają użytkownikom końcowym go zmodyfikować. Przyjrzymy GridView widoku DetailsView i FormView oddzielnie, jak te formantów sieci Web danych różnią się w ich możliwości modyfikacji danych i konfiguracji.

Jak zajmiemy się w dalszej części tego artykułu, dodawania bardzo podstawowe edytowanie, wstawianie i usuwanie pomocy technicznej za pośrednictwem widoku DetailsView, w widoku GridView i kontroluje FormView jest naprawdę najprostszą sprawdzanie kilka pól wyboru. Istnieje wiele precyzyjnie i przypadków krawędzi w świecie rzeczywistym, które zapewniają takich funkcji, które są bardziej skomplikowane niż tylko polecenie, a następnie kliknij. Ten samouczek koncentruje się jednak wyłącznie na potwierdzania funkcji modyfikacji simplistic danych. Samouczki przyszłych zbada dotyczy nasuwające się bez wątpienia w ustawieniu rzeczywistych.

## <a name="deleting-data-from-the-gridview"></a>Usuwanie danych z widoku GridView

Uruchom, wystarczy przeciągnąć element GridView z przybornika do projektanta. Następnie należy powiązać ObjectDataSource widoku GridView, wybierając ją z listy rozwijanej w tagu w widoku GridView. W tym momencie deklaratywne znaczników w widoku GridView będzie:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Powiązanie widoku GridView ObjectDataSource za pośrednictwem jego tagów inteligentnych ma dwie korzyści:

- BoundFields i CheckBoxFields są tworzone automatycznie dla każdego pola zwrócony przez element ObjectDataSource. Ponadto właściwości elementu BoundField i w polu CheckBoxField jest ustawiona na podstawie metadanych pola źródłowego. Na przykład `ProductID`, `CategoryName`, i `SupplierName` pola są oznaczone jako tylko do odczytu w `ProductsDataTable` i w związku z tym nie należy zezwalać na aktualizacje podczas edycji. Aby zmieścił się w tym, te BoundFields [właściwości tylko do odczytu](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) są ustawione na `True`.
- [Właściwości DataKeyNames](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) jest przypisany do pola klucza podstawowego obiektu źródłowego. Jest to istotne, kiedy przy użyciu widoku GridView do edycji lub usuwania danych, ponieważ ta właściwość wskazuje pola (lub zestaw pól) to unikatowy identyfikuje każdego rekordu. Aby uzyskać więcej informacji na temat `DataKeyNames` właściwości, odwołaj się do [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka.

Gdy widoku GridView może być powiązana z ObjectDataSource przy użyciu okna właściwości lub składni deklaratywnej, wymaga można ręcznie dodać odpowiedniego elementu BoundField i `DataKeyNames` znaczników.

Kontrolki widoku siatki udostępnia wbudowaną obsługę na poziomie wiersza, edytowanie i usuwanie. Konfigurowanie GridView do obsługi usuwanie dodaje kolumnę przycisków Delete. Gdy użytkownik końcowy kliknie przycisk usuwania dla danego wiersza, ensues odświeżania strony i widoku GridView wykonuje następujące czynności:

1. Element ObjectDataSource `DeleteParameters` są przypisane wartości
2. Element ObjectDataSource `Delete()` wywoływana jest metoda, usuwanie określonego rekordu
3. Widoku GridView rebinds się do elementu ObjectDataSource przez wywołanie jego `Select()` — metoda

Wartości przypisane do `DeleteParameters` są wartości `DataKeyNames` następującą liczbę pól: kliknięto przycisk Usuń, którego wiersza. Dlatego istotne jest który element GridView `DataKeyNames` poprawnie można ustawić właściwości. Jeśli jest on niedostępny, `DeleteParameters` zostanie przypisana wartość `Nothing` w kroku 1, co z kolei nie spowoduje żadnych usuniętych rekordów w kroku 2.

> [!NOTE]
> `DataKeys` Kolekcji znajduje się w widoku GridView stan formantu s, oznacza to, że `DataKeys` wartości zostanie zapamiętany na ogłaszania zwrotnego nawet, jeśli stan widoku GridView s została wyłączona. Jednak jest bardzo ważne jest, że stan widoku pozostanie włączony GridViews, która obsługuje edytowania lub usuwania (domyślnie). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, edytowanie i usuwanie zachowanie będzie działać prawidłowo dla pojedynczego użytkownika, ale w przypadku równoczesnych użytkowników usuwania danych, istnieje możliwość, że te równoczesnych użytkowników mogą przypadkowo Usuń lub Edytuj rejestruje, że t ma. Zobacz Moje wpis w blogu [Ostrzeżenie: problem współbieżności z ASP.NET 2.0 GridViews/widoku DetailsView/FormViews tej edycji pomocy technicznej i/lub usunięcie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx), aby uzyskać więcej informacji.


Ten sam ostrzeżenie dotyczy również DetailsViews i FormViews.

Aby dodać element GridView możliwości usuwania, przejdź do jego tagów inteligentnych i zaznacz pole wyboru Włącz usuwanie.


![Włącz usuwanie pola wyboru Sprawdź](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Na rysunku nr 10**: Sprawdź Włącz usuwanie wyboru


Zaznaczając pole wyboru Włącz usuwanie z tagów inteligentnych dodaje CommandField do widoku GridView. CommandField renderuje kolumny w widoku GridView z przyciski umożliwiające wykonywanie co najmniej jeden z następujących zadań: Wybieranie rekordu, edytowanie rekordu i usuwania rekordu. Wcześniej widzieliśmy CommandField w akcji z wybierania rekordów w [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka.

CommandField zawiera szereg `ShowXButton` właściwości, które wskazują, jakie serii przyciski są wyświetlane w elemencie CommandField. Zaznaczając pole wyboru Włącz usuwanie CommandField których `ShowDeleteButton` jest właściwość `True` został dodany do kolekcji kolumny w widoku GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

W tym momencie believe nim lub nie skończymy o dodanie obsługi usuwania do widoku GridView! Jak pokazano na rysunku nr 11, podczas odwiedzania tej strony za pośrednictwem przeglądarki kolumnę przycisków Delete jest obecny.


[![CommandField dodaje kolumnę usuwanie przycisków](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Rysunek 11**: CommandField dodaje kolumnę o usuwanie przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Jeśli już zostały konstruowany w tym samouczku od podstaw samodzielnie, podczas testowania tej strony, klikając przycisk Usuń zgłosi wyjątek. Kontynuuj lekturę, aby dowiedzieć się, dlaczego zostały zgłoszone następujące wyjątki i sposobu ich rozwiązania.

> [!NOTE]
> Jeśli postępujesz zgodnie z wzdłuż przy użyciu pobierania towarzyszące tego samouczka, te problemy mają już uwzględniono w. Jednak I zachęca do przeczytania szczegóły wymienione poniżej, aby łatwiej zidentyfikować problemy, które mogą wystąpić podczas i odpowiedniego rozwiązania.


Jeśli podczas próby usunięcia produktu, otrzymasz wyjątek, których komunikat jest podobny do "*element ObjectDataSource"ObjectDataSource1"nie można znaleźć nieuniwersalnej metody DeleteProduct, który ma następujące parametry: productID oryginalnego\_ ProductID*, "prawdopodobnie nie pamiętasz usunąć `OldValuesParameterFormatString` właściwości w elemencie ObjectDataSource. Z `OldValuesParameterFormatString` określona właściwość ObjectDataSource podejmuje próbę przekazania zarówno `productID` i `original_ProductID` parametrów wejściowych `DeleteProduct` metody. `DeleteProduct`, jednak akceptuje tylko jeden parametr wejściowy, dlatego wyjątek. Usuwanie `OldValuesParameterFormatString` właściwości (lub ustawieniem dla niego `{0}`) powoduje, że element ObjectDataSource próby nie umożliwia przekazywanie oryginalnego parametru wejściowego.


[![Upewnij się, że właściwość elementu OldValuesParameterFormatString został wyczyszczony](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Rysunek 12**: Upewnij się, że `OldValuesParameterFormatString` właściwości ma zostać wyczyszczone limit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


Nawet jeśli były usuwane `OldValuesParameterFormatString` właściwości, nadal wystąpi wyjątek podczas próby usunięcia produktu z komunikatem: "*instrukcji DELETE konflikt z ograniczeniem odwołania" klucz OBCY\_kolejności\_szczegóły \_Produktów*. " Baza danych Northwind zawiera ograniczenie klucza obcego między `Order Details` i `Products` tabeli, co oznacza, że produkt nie można usunąć z systemu, jeśli istnieje co najmniej jeden rekord dla niego w `Order Details` tabeli. Ponieważ każdego produktu w bazie danych Northwind ma co najmniej jeden rekord `Order Details`, nie można usunąć wszystkie produkty, dopóki najpierw usunąć rekordów szczegółów skojarzone zamówienia produktu.


[![Usuwanie produkty zabrania ograniczenie klucza obcego](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Rysunek 13**: ograniczenie klucza obcego zabrania usunięcia produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


W naszym samouczku umożliwia tylko usunięcie wszystkich rekordów z `Order Details` tabeli. W rzeczywistych aplikacjach czy musimy albo:

- Ma inny ekran, aby zarządzać szczegółowe informacje o kolejności
- Rozszerzyć `DeleteProduct` metodę w celu uwzględnienia logiki można usunąć szczegółów zamówienia określonego produktu
- Zmodyfikuj zapytanie SQL używane przez TableAdapter uwzględnienie usunięcia szczegółów zamówienia określonego produktu

Umożliwia tylko usunięcie wszystkich rekordów z `Order Details` tabeli, aby obejść ograniczenie klucza obcego. Przejdź do Eksploratora serwera w programie Visual Studio, kliknij prawym przyciskiem myszy `NORTHWND.MDF` węzeł i wybierz nową kwerendę. Następnie w oknie zapytania, uruchom następującą instrukcję SQL:`DELETE FROM [Order Details]`


[![Usuń wszystkie rekordy z tabeli Szczegóły kolejności](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Rysunek 14**: Usuń wszystkie rekordy z `Order Details` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Po wyczyszczeniu `Order Details` tabeli, klikając przycisk Usuń spowoduje usunięcie produktu bez błędów. Jeśli kliknięcie przycisku Usuń nie powoduje usunięcia produktu, sprawdź, upewnij się, że w widoku GridView `DataKeyNames` właściwość jest ustawiona na pola klucza podstawowego (`ProductID`).

> [!NOTE]
> Po kliknięciu przycisku Usuń ensues odświeżania strony i usunąć rekordu. Może to być niebezpieczne, ponieważ ułatwia przypadkowo kliknij przycisk Usuń nieprawidłowy wiersz. W przyszłości samouczek będzie przedstawiono sposób dodawania potwierdzenie po stronie klienta podczas usuwania rekordu.


## <a name="editing-data-with-the-gridview"></a>Edytowanie danych z widoku GridView

Wraz z usunięciem, kontrolki widoku siatki udostępnia wbudowana obsługa edycji na poziomie wiersza. Konfigurowanie GridView do obsługi edycji dodaje kolumnę przycisków edycji. Z perspektywy użytkownika końcowego, klikając wiersz edycji przycisk przyczyn, które wiersz być edytowalny włączając komórek do pól tekstowych zawierających istniejące wartości i zastępowanie przyciski Anuluj i przycisk Edytuj z aktualizacją. Po wprowadzeniu ich odpowiednich zmian, użytkownik końcowy może kliknij przycisk Aktualizuj, aby zatwierdzić zmiany lub przycisk Anuluj, aby je odrzucić. W obu przypadkach po kliknięciu przycisku aktualizację lub Anuluj widoku GridView zwraca stan wstępnie edycji.

Z naszych perspektywy Deweloper strony gdy użytkownik końcowy kliknie przycisk edycji dla danego wiersza, ensues odświeżania strony i widoku GridView wykonuje następujące czynności:

1. W widoku GridView `EditItemIndex` właściwości jest przypisany do indeks wiersza, którego przycisk Edytuj został kliknięty
2. Widoku GridView rebinds się do elementu ObjectDataSource przez wywołanie jego `Select()` — metoda
3. Indeks wiersza, który odpowiada `EditItemIndex` jest renderowany w "trybie edycji." W tym trybie przycisk Edytuj zastępuje przyciski aktualizacji i Anuluj i BoundFields których `ReadOnly` właściwości mają wartość FAŁSZ (ustawienie domyślne) są renderowane jako kontrolki TextBox w sieci Web, którego `Text` właściwości są przypisane do wartości pola danych.

Na tym etapie kod znaczników jest zwracana w przeglądarce, dzięki czemu użytkownik końcowy wprowadzać żadnych zmian do wiersza danych. Po kliknięciu przycisku Aktualizuj występuje odświeżania strony i widoku GridView wykonuje następujące czynności:

1. Element ObjectDataSource `UpdateParameters` wartości są przypisane wartości wprowadzonej przez użytkownika końcowego w interfejsie edycji w widoku GridView
2. Element ObjectDataSource `Update()` wywoływana jest metoda, aktualizowanie określonego rekordu
3. Widoku GridView rebinds się do elementu ObjectDataSource przez wywołanie jego `Select()` — metoda

Przypisane do wartości kluczy podstawowych `UpdateParameters` w kroku 1 pochodzą z wartości określona w `DataKeyNames` właściwości, wartości klucza podstawowego z systemem innym niż pochodzić z tekst w formantach TextBox w sieci Web dla wiersza edytowanej. Zgodnie z usuwania, ważne jest, że w widoku GridView `DataKeyNames` poprawnie można ustawić właściwości. Jeśli jest on niedostępny, `UpdateParameters` wartość klucza podstawowego zostanie przypisana wartość `Nothing` w kroku 1, co z kolei nie spowoduje żadnych zaktualizowanych rekordów w kroku 2.

Funkcje edycji można aktywować, po prostu, zaznaczając pole wyboru Włącz edytowanie w tagu w widoku GridView.


![Włącz edytowanie pola wyboru Sprawdź](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Rysunek 15**: Sprawdź edycji pole wyboru Włącz


Sprawdzanie, pole wyboru Włącz edytowanie doda CommandField (w razie potrzeby) i zestaw jej `ShowEditButton` właściwości `True`. Widzieliśmy wcześniej, CommandField zawiera szereg `ShowXButton` właściwości, które wskazują, jakie serii przyciski są wyświetlane w elemencie CommandField. Zaznaczając pole wyboru Włącz edytowanie dodaje `ShowEditButton` właściwości istniejącego CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

To wszystko jest dodanie obsługi edycji podstawowe. Jak przedstawiono Figure16 edycji interfejs jest raczej surowych każdego elementu BoundField których `ReadOnly` właściwość jest ustawiona na `False` (ustawienie domyślne) jest renderowane jako pole tekstowe. Obejmuje to również pola, takie jak `CategoryID` i `SupplierID`, które są klucze do innych tabel.


[![Klikając przycisk Edytuj s Chai wierszy są wyświetlane w trybie edycji](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Rysunek 16**: s Chai, klikając przycisk Edytuj wierszy są wyświetlane w trybie edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Oprócz prosząc użytkowników o bezpośredniego edytowania wartości kluczy obcych, Brak interfejsu interfejsu edycji w następujący sposób:

- Jeśli użytkownik wprowadzi `CategoryID` lub `SupplierID` który nie istnieje w bazie danych `UPDATE` będzie narusza ograniczenie klucza obcego, powodując wyjątek do wywołania.
- Interfejs edytowania nie zawiera żadnych sprawdzania poprawności. Jeśli nie podasz wymaganej wartości (takie jak `ProductName`), lub wprowadź wartość ciągu, w którym Oczekiwano wartości numerycznej (na przykład wprowadzić "Zbyt dużo!" do `UnitPrice` pole tekstowe), zostanie wygenerowany wyjątek. Samouczek przyszłych zbada sposób Dodaj formanty walidacji do edycji interfejsu użytkownika.
- Obecnie *wszystkie* pola produktów, które nie są tylko do odczytu muszą być zawarte w widoku GridView. Jeśli można usunąć pola z widoku GridView, możemy powiedzieć `UnitPrice`, gdy nie ustawiał aktualizowanie danych widoku GridView `UnitPrice` `UpdateParameters` wartość, która zmieniłaby rekordu bazy danych `UnitPrice` do `NULL` wartości. Podobnie, jeśli to pole jest wymagane, takie jak `ProductName`, zostanie usunięty z widoku GridView, aktualizacji zakończy się niepowodzeniem z takimi samymi "*kolumny"ProductName"nie dopuszcza wartości null*" wyjątek wymienionych powyżej.
- Formatowanie edycji interfejsu pozostawia się być pożądane. `UnitPrice` Jest wyświetlany z czterech miejsc dziesiętnych. W idealnym przypadku `CategoryID` i `SupplierID` wartości zawierałoby DropDownLists, który listy kategorii i dostawców w systemie.

Są to wszystkie nieprawidłowości, które firma Microsoft będzie konieczne współdziałać z teraz, ale zostanie rozwiązany w przyszłości samouczki.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Wstawianie, edytowanie i usuwanie danych z widoku DetailsView

Jak możemy przedstawiono w starszych samouczki, w widoku DetailsView formant zawiera jeden rekord w czasie i, jak GridView, umożliwia edytowanie i usuwanie rekordu aktualnie wyświetlany. Środowisko zarówno przez użytkownika końcowego związane z edytowanie i usuwanie elementów z widoku DetailsView i przepływu pracy ze strony ASP.NET jest identyczna z widoku GridView. Gdy widoku DetailsView różni się od widoku GridView jest również obsługa Wstawianie.

Aby zademonstrować możliwości modyfikacji danych widoku GridView, Rozpocznij od dodania widoku DetailsView do `Basics.aspx` strony powyżej istniejącego widoku GridView i powiązać ją z istniejącego elementu ObjectDataSource przy użyciu tagów inteligentnych DetailsView. Następnie czyści DetailsView `Height` i `Width` właściwości i zaznacz opcję Włącz stronicowanie w tagu. Aby umożliwić edytowanie, wstawianie i usuwanie pomocy technicznej, po prostu zaznacz pola wyboru Włącz edytowanie, Włącz wstawianie i usuwanie włączyć w tagu.


![Konfigurowanie obsługi edytowanie, wstawianie i usuwanie widoku DetailsView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Rysunek 17**: Konfigurowanie obsługi edytowanie, wstawianie i usuwanie widoku DetailsView


Jako z widoku GridView, dodawanie, edytowanie, wstawiając lub usuwając obsługi dodaje CommandField do widoku DetailsView, jak przedstawiono na poniższym składni deklaratywnej:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Należy pamiętać, że dla widoku DetailsView CommandField domyślnie pojawia się na końcu kolekcji kolumn. Ponieważ w widoku DetailsView są renderowane jako wiersze, CommandField występuje jako wiersz z Insert, edytowanie i usuwanie przycisków w dolnej części widoku DetailsView.


[![Konfigurowanie obsługi edytowanie, wstawianie i usuwanie widoku DetailsView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Rysunek 18**: konfigurowanie widoku DetailsView do obsługi edytowanie, wstawianie i usuwanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Kliknij przycisk Usuń taką samą sekwencję zdarzeń, ponieważ rozpoczyna się od widoku GridView: a ogłaszania zwrotnego; następuje widoku DetailsView wypełnianie jej ObjectDataSource `DeleteParameters` na podstawie `DataKeyNames` wartości i zakończone wywołanie jego ObjectDataSource `Delete()` metodę, która faktycznie spowoduje usunięcie produktu z bazy danych. Edytowanie w widoku DetailsView działa również w sposób identyczne z widoku GridView.

Podczas wstawiania, użytkownik końcowy zobaczy nowego przycisku, po kliknięciu renderuje widoku DetailsView w "trybie wstawiania." Z "tryb wstawiania" nowy przycisk zastępuje Insert i Anuluj przycisków i tylko te BoundFields których `InsertVisible` właściwość jest ustawiona na `True` (ustawienie domyślne) są wyświetlane. Te pola danych identyfikowane jako pola automatycznego przyrostu, takich jak `ProductID`, ma ich [właściwości InsertVisible](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) ustawioną `False` podczas wiązania ze źródłem danych za pomocą tagów inteligentnych widoku DetailsView.

Podczas tworzenia wiązania DetailsView za pomocą tagów inteligentnych źródła danych, ustawia Visual Studio `InsertVisible` właściwości `False` tylko dla pól automatycznego przyrostu. Pola tylko do odczytu, takich jak `CategoryName` i `SupplierName`, będzie wyświetlana w interfejsie użytkownika "tryb wstawiania", chyba że ich `InsertVisible` właściwość jest jawnie ustawiona na `False`. Poświęć chwilę, aby ustawić tych dwóch pól `InsertVisible` właściwości `False`, albo za pomocą składni deklaratywnej DetailsView lub Edytuj pola łącze w tagu. 19 rysunek pokazuje ustawienie `InsertVisible` właściwości `False` , klikając polecenie Edytuj pola łącza.


[![Northwind Traders oferuje teraz Zepołowy xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Rysunek 19**: Northwind Traders teraz oferuje xyz Zepołowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Po ustawieniu `InsertVisible` właściwości, widok `Basics.aspx` strony w przeglądarce, a następnie kliknij przycisk Nowy. 20 rysunek przedstawia widoku DetailsView podczas dodawania nowego napój Zepołowy xyz, do wiersza naszego produktu.


[![Northwind Traders oferuje teraz Zepołowy xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Rysunek 20**: Northwind Traders teraz oferuje xyz Zepołowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Po Wprowadzanie szczegółów dotyczących Zepołowy xyz, a następnie kliknąć przycisk Wstaw, ensues odświeżania strony i dodawania nowego rekordu do `Products` tabeli bazy danych. Ponieważ tego widoku DetailsView zawiera listę produktów, w kolejności, z którym istnieje w tabeli bazy danych, firma Microsoft musi strony do ostatniego produktu Aby zobaczyć nowe produktu.


[![Szczegóły dotyczące Zepołowy xyz](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Rysunek 21**: szczegóły Zepołowy xyz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> DetailsView [właściwości CurrentMode](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) wskazuje interfejs wyświetlane i może być jedną z następujących wartości: `Edit`, `Insert`, lub `ReadOnly`. [Właściwości DefaultMode właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) wskazuje trybu DetailsView zwraca po modyfikacji lub Wstaw zostało ukończone i jest używane do wyświetlania widoku DetailsView, jest trwale edycji lub tryb wstawiania.


Punkt i kliknij przycisk wstawiania i edytowania DetailsView boryka się z te same ograniczenia co widoku GridView: użytkownik musi wprowadzić istniejące `CategoryID` i `SupplierID` wartości do pola tekstowego; nie ma interfejsu wszelka logika weryfikacji; wszystkie pola produktów, które nie zezwalają na `NULL` wartości lub nie ma wartości domyślnej wartości z określonego na poziomie bazy danych muszą być zawarte w Wstawianie interfejsu i tak dalej.

Techniki omówione zostanie rozszerzanie i udoskonalanie w widoku GridView edycji interfejsu w przyszłości artykuły mogą być stosowane do formantu widoku DetailsView edycji i wstawianie również interfejsów.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Dla bardziej elastyczne interfejsu użytkownika modyfikacji danych przy użyciu widoku FormView

FormView oferuje wbudowaną obsługę Wstawianie, edytowanie i usuwanie danych, ale ponieważ używa szablonów zamiast pól nie ma miejsce, nie można dodać BoundFields lub CommandField używany przez formant widoku GridView i widoku DetailsView do dostarczania danych Interfejs modyfikacji. Zamiast tego interfejsu formantów sieci Web do zbierania użytkownika dane wejściowe podczas dodawania nowego elementu lub edytowanie istniejących wraz z nowym, edytowania, usuwania, wstawiania, aktualizowania i przyciski "Anuluj" należy je dodać ręcznie do odpowiednich szablonów. Na szczęście usługa Visual Studio automatycznie utworzy interfejsu potrzebne podczas wiązania ze źródłem danych za pomocą listy rozwijanej w jego tagów inteligentnych FormView.

Aby zilustrować te techniki, Rozpocznij od dodania FormView do `Basics.aspx` strony, a z tagu FormView powiązać go z ObjectDataSource już utworzone. Spowoduje to wygenerowanie `EditItemTemplate`, `InsertItemTemplate`, i `ItemTemplate` dla FormView z kontrolki TextBox w sieci Web do zbierania danych wejściowych użytkownika i kontrolki przycisku w sieci Web dla nowego Edycja, usuwanie, wstawiania, aktualizowania i przyciski "Anuluj". Ponadto FormView `DataKeyNames` właściwość jest ustawiona na pola klucza podstawowego (`ProductID`) zwrócone przez element ObjectDataSource obiektu. Sprawdź też, opcja Włącz stronicowanie w widoku FormView tagów inteligentnych.

Poniżej pokazano deklaratywne znaczników dla FormView `ItemTemplate` po FormView została powiązana z ObjectDataSource. Domyślnie każde pole produktu wartość Boolean nie jest powiązany z `Text` właściwości formantu etykiety sieci Web podczas każdego pola wartość logiczna (`Discontinued`) jest powiązana z `Checked` właściwość wyłączonego formantu CheckBox w sieci Web. Aby nowy, edytowanie i usuwanie przycisków do wyzwolenia określone zachowanie FormView po kliknięciu, konieczne jest który ich `CommandName` być równa wartości `New`, `Edit`, i `Delete`odpowiednio.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

22 rysunek przedstawia FormView `ItemTemplate` podczas wyświetlania za pośrednictwem przeglądarki. Każde pole produktu znajduje się nowy, edytowanie i usuwanie przycisków u dołu.


[![ItemTemplate Defaut FormView zawiera listę pól produktu oraz nowego, edytowanie i usuwanie przycisków](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Rysunek 22**: Defaut FormView `ItemTemplate` wymieniono każdego produktu pola wzdłuż z nowej, edytowanie i usuwanie przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


Z widoku GridView i widoku DetailsView, klikając przycisk Usuń lub dowolnego przycisku, LinkButton lub ImageButton, takich jak których `CommandName` właściwość jest ustawiona na Delete powoduje odświeżenie strony, wypełnienie przez element ObjectDataSource `DeleteParameters` oparte na FormView `DataKeyNames`wartość, a następnie wywołuje element ObjectDataSource `Delete()` metody.

Po kliknięciu przycisku Edytuj ensues odświeżania strony i danych jest odbitych do `EditItemTemplate`, który jest odpowiedzialny za renderowania interfejsu edycji. Ten interfejs zawiera formanty sieci Web do edytowania danych wraz z przycisków aktualizacji i Anuluj. Wartość domyślna `EditItemTemplate` generowane przez Visual Studio zawiera etykiety pól automatycznego przyrostu (`ProductID`), a pole tekstowe dla każdego pola innego typu oraz pola wyboru dla każdego pola wartość logiczna. To zachowanie jest bardzo podobny do BoundFields automatycznie wygenerowany w formantach GridView i widoku DetailsView.

> [!NOTE]
> Jeden mały problem z FormView Autogenerowanie `EditItemTemplate` jest renderowania Web pola tekstowego kontrolki dla tych pól, które są tylko do odczytu, takich jak `CategoryName` i `SupplierName`. Zajmiemy się tym, jak to uwzględniać wkrótce.


Określa, w polu tekstowym `EditItemTemplate` ma ich `Text` właściwość powiązana do ich odpowiednich danych pola przy użyciu wartości *dwukierunkowego wiązania z danymi*. Dwukierunkowego wiązania z danymi, wskazywane przez `<%# Bind("dataField") %>`, wykonuje wiązania danych zarówno podczas wiązania danych z szablonu i podczas wypełniania parametry ObjectDataSource wstawiania lub edytowania rekordów. Oznacza to, kiedy użytkownik kliknie przycisk Edytuj z `ItemTemplate`, `Bind()` metoda zwraca wartość pola określone dane. Po ich zmienia i klika aktualizacji wartości publikowanego Wstecz, który odpowiada pola danych, określić przy użyciu `Bind()` są stosowane do ObjectDataSource `UpdateParameters`. Alternatywnie jednokierunkowe databinding wskazywane przez `<%# Eval("dataField") %>`, tylko pobiera wartości pola danych podczas wiązania danych z szablonu i jest *nie* zwraca wartości wprowadzonych przez użytkownika do parametrów źródła danych na ogłaszania zwrotnego.

Następujący kod deklaratywne przedstawia FormView `EditItemTemplate`. Należy pamiętać, że `Bind()` metoda jest używana w tym miejscu składni wiązania z danymi i formanty aktualizacji i Web przycisku Anuluj mają ich `CommandName` odpowiednio ustawić właściwości.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Nasze `EditItemTemplate`, dla tego punktu, spowoduje, że wyjątek zostanie wygenerowany, firma Microsoft podejmie próbę użycia go. Jest to problem, że `CategoryName` i `SupplierName` pola mają być renderowane jako kontrolki TextBox w sieci Web w `EditItemTemplate`. Musimy albo zmień tych pól tekstowych etykiety lub usunąć je całkowicie. Teraz wystarczy usunąć je całkowicie z `EditItemTemplate`.

23 rysunek przedstawia FormView w przeglądarce po kliknięciu przycisku Edytuj dla Chai. Należy pamiętać, że `SupplierName` i `CategoryName` wyświetlanych w polach `ItemTemplate` nie są już dostępne, ponieważ właśnie usunęliśmy je z `EditItemTemplate`. Po kliknięciu przycisku Aktualizuj FormView będzie kontynuowana przy użyciu takiej samej kolejności kroków uruchamiających GridView i widoku DetailsView.


[![Domyślnie EditItemTemplate przedstawia każdego produktu można edytować pola jako pola tekstowego lub pola wyboru](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Rysunek 23**: Domyślnie `EditItemTemplate` pokazuje każdą edycji produktu pole jako pole tekstowe lub wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Po kliknięciu przycisku Wstaw FormView `ItemTemplate` ensues odświeżania strony. Jednak żadne dane nie jest powiązany z FormView, ponieważ jest dodawany nowy rekord. `InsertItemTemplate` Interfejs zawiera formanty sieci Web do dodawania nowego rekordu wraz z przycisków Insert i Anuluj. Wartość domyślna `InsertItemTemplate` generowane przez Visual Studio zawiera pole tekstowe dla każdego pola wartości inne niż logiczne i pole wyboru dla każdego pola wartość logiczna, podobny do automatycznego generowania `EditItemTemplate`interfejsu użytkownika. Formanty pole tekstowe mają ich `Text` właściwość powiązana wartość ich odpowiednich pól danych przy użyciu dwukierunkowego wiązania z danymi.

Następujący kod deklaratywne przedstawia FormView `InsertItemTemplate`. Należy pamiętać, że `Bind()` metoda jest używana w tym miejscu składni wiązania z danymi i formanty Insert i Web przycisku Anuluj mają ich `CommandName` odpowiednio ustawić właściwości.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

Brak subtlety z FormView Autogenerowanie `InsertItemTemplate`. W szczególności kontrolki TextBox w sieci Web są tworzone nawet dla tych pól, które są tylko do odczytu, takich jak `CategoryName` i `SupplierName`. Jak `EditItemTemplate`, należy usunąć te pola tekstowe z `InsertItemTemplate`.

24 rysunek przedstawia FormView w przeglądarce podczas dodawania nowego produktu xyz kawy. Należy pamiętać, że `SupplierName` i `CategoryName` wyświetlanych w polach `ItemTemplate` nie są już dostępne, ponieważ właśnie usunęliśmy je. Po kliknięciu przycisku Wstaw przychody FormView za pomocą tej samej sekwencji kroki jako formant widoku DetailsView dodawania nowego rekordu do `Products` tabeli. 25 rysunek przedstawia kawy xyz szczegółowe w widoku FormView po został wstawiony.


[![InsertItemTemplate nakazują interfejsu Wstawianie FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Rysunek 24**: `InsertItemTemplate` nakazują FormView Wstawianie interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![Szczegóły dotyczące nowego produktu, XYZ kawy, są wyświetlane w widoku FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Rysunek 25**: w widoku FormView wyświetlane są szczegóły dotyczące nowego produktu, kawa xyz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Oddzielając się tylko do odczytu, edytowanie i wstawianie interfejsy do trzech osobnych szablonów, FormView umożliwia dokładniejszą kontrolę nad te interfejsy niż widoku DetailsView i widoku GridView.

> [!NOTE]
> DetailsView, FormView, takich jak `CurrentMode` właściwość wskazuje interfejs wyświetlane i jego `DefaultMode` właściwość wskazuje tryb zwraca FormView do po modyfikacji lub Wstaw została ukończona.


## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy zbadać podstawy Wstawianie, edytowanie i usuwanie danych przy użyciu widoku GridView widoku DetailsView i FormView. Wszystkie trzy formanty Podaj pewnego poziomu możliwości modyfikacji danych wbudowanych, które mogą zostać użyte bez pisania pojedynczy wiersz kodu na stronie platformy ASP.NET dzięki użyciu formantów sieci Web danych i ObjectDataSource. Jednak prosty i kliknij opcję dość frail renderowania technik i interfejs użytkownika modyfikacji danych prosty. W celu udostępnienia weryfikacji, wstrzyknąć programowe wartości, obsługiwać wyjątki dostosowania interfejsu użytkownika i itd., potrzebujemy polegać na bevy technik, które zostaną dokładniej omówione w następnym samouczki kilka.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](limiting-data-modification-functionality-based-on-the-user-cs.md)
[dalej](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
